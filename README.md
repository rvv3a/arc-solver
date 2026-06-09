# ARC SDC Rule Solver

A Knowledge-Based AI solver for ARC tasks using a **Similarity-Difference-Color** rule induction approach.

This repository contains the minimal files needed to run the submitted solver notebook:

- ARC problem JSON files under `input/problems/`
- the main solver notebook at `working/sample_submission.ipynb`
- this README

Generated outputs, local analysis logs, milestone writeups, and larger local data folders are not included in the GitHub repository.

## Project Idea

ARC tasks provide a few training examples:

```text
input grid -> output grid
input grid -> output grid
```

The solver must infer the transformation rule and apply it to unseen test inputs.

The approach follows a manual puzzle-solving strategy:

1. Identify what stays the same between the input and output grids.
2. Isolate what changed.
3. Determine whether colors are copied directly or remapped consistently.

The solver implements this as an explicit rule-based system rather than a neural model.

## Repository Structure

```text
.
├── README.md
├── input/
│   └── problems/
│       ├── arc-agi_evaluation_challenges.json
│       ├── arc-agi_evaluation_solutions.json
│       ├── arc-agi_test_challenges.json
│       ├── arc-agi_training_challenges.json
│       ├── arc-agi_training_solutions.json
│       └── sample_submission.json
└── working/
    └── sample_submission.ipynb
```

## Main File

The implementation is in:

```text
working/sample_submission.ipynb
```

The notebook:

1. Loads ARC challenge JSON files from `input/problems/`.
2. Represents grids using pixel matrices and connected components.
3. Searches over symbolic transformation rules.
4. Keeps rules that reproduce all training examples for a task.
5. Applies matching rules to test inputs.
6. Writes a generated `submission.json` file when run.

## Rule Library

The solver includes rules for:

- identity copying
- rotations
- horizontal and vertical flips
- transposition
- direct color mapping
- crop to non-background bounding box
- crop by color
- select largest or smallest object
- select top-left or bottom-right object
- select object unique by size, color, or shape
- object selection followed by recoloring
- object selection followed by rotation or flipping
- limited rule composition such as select -> rotate -> recolor
- tiling
- pixel scaling
- enclosed-region filling
- line completion between same-colored cells
- simple foreground movement detection

## How To Run

From the project root:

```bash
cd working
jupyter notebook sample_submission.ipynb
```

Then run all notebook cells.

The notebook generates `submission.json` during execution. That generated file is intentionally not tracked in this minimal GitHub repository.

For Gradescope, upload:

```text
working/sample_submission.ipynb
```

## Results

Milestone 2 Gradescope result:

```text
6 correct out of 100
```

Local public-evaluation checks during development:

```text
Milestone 2 local check: 8 / 419
Milestone 3 local check: 11 / 419
```

The local check is not the same as the official Gradescope score, but it was useful for comparing solver versions.

## Notes

The GitHub repository is intentionally smaller than the local working directory. Files such as generated submissions, solver logs, progress notes, milestone writeups, and expanded ARC data folders are local development artifacts and are not part of the pushed repository.
