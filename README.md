# ARC SDC Rule Solver

A Knowledge-Based AI solver for ARC tasks using a **Similarity-Difference-Color** rule induction approach.

This project was developed for an ARC course project. The main idea is based on a manual puzzle-solving strategy: first identify what stays the same between input and output grids, then isolate what changed, and finally determine how colors are copied or remapped.

## Project Idea

ARC tasks provide a few training examples:

```text
input grid -> output grid
input grid -> output grid
```

The solver must infer the transformation rule and apply it to unseen test inputs.

My approach breaks this process into three stages:

1. **Similarity detection**
   - Compare input and output grids.
   - Look for preserved shapes, objects, positions, grid sizes, and colors.

2. **Difference detection**
   - Identify what changed.
   - Examples include cropping, movement, filling, scaling, tiling, or selecting an object.

3. **Color mapping**
   - After the structural transformation is identified, check whether colors stayed the same or changed consistently.

The solver implements this as an explicit rule-based system rather than a neural model.

## Implementation

The main implementation is:

```text
working/sample_submission.ipynb
```

The notebook:

1. Loads ARC challenge JSON files.
2. Represents grids as both pixel matrices and connected components.
3. Searches over a library of symbolic transformation rules.
4. Keeps rules only if they reproduce all training examples for a task.
5. Applies matching rules to the test inputs.
6. Writes `submission.json` for evaluation.

## Rule Library

The current solver includes rules for:

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

These rules are intentionally interpretable and are designed to match the manual reasoning process used to solve ARC puzzles.

## Repository Structure

```text
.
├── README.md
├── input/
│   ├── ARC-AGI-1/
│   ├── ARC-AGI-2/
│   └── problems/
└── working/
    ├── sample_submission.ipynb
    ├── submission.json
    ├── arc_solver_rule_log.json
    ├── arc_solver_summary.json
    ├── milestone2_writeup.md
    ├── milestone3_requirement_checklist.txt
    ├── project_progress_m1_m2_m3.txt
    └── README.md
```

Important files:

- `working/sample_submission.ipynb`: main solver notebook and Gradescope upload file.
- `working/submission.json`: generated output file from the notebook.
- `working/arc_solver_summary.json`: summary of which rules were used.
- `working/arc_solver_rule_log.json`: per-task rule log for analysis.
- `working/project_progress_m1_m2_m3.txt`: narrative of the project progression.
- `working/milestone3_requirement_checklist.txt`: checklist against Milestone 3 requirements.

## How To Run

From the project root:

```bash
cd working
jupyter notebook sample_submission.ipynb
```

Then run all notebook cells.

The notebook will generate:

```text
submission.json
arc_solver_rule_log.json
arc_solver_summary.json
```

For Gradescope, upload:

```text
working/sample_submission.ipynb
```

Do not upload `submission.json` unless the assignment specifically asks for it. The notebook generates that file during execution.

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

## Key Findings

The solver works best on ARC tasks that can be explained by simple symbolic transformations, such as:

- cropping an object
- selecting a component
- applying a simple geometric transformation
- scaling or tiling
- applying a consistent color map

The solver struggles with tasks requiring:

- multi-step abstract reasoning
- relational object comparison
- pattern completion
- counting
- choosing an object based on context
- complex movement rules
- flexible rule composition

The main conclusion is that explicit symbolic rules are useful and interpretable, but a small fixed rule library is not enough for broad ARC performance.

## Knowledge-Based AI Relevance

This project illustrates both the strength and weakness of Knowledge-Based AI for ARC.

Strengths:

- Predictions are interpretable.
- Rules can be inspected directly.
- Object-level representations match human reasoning.
- Failures are easier to analyze than black-box model failures.

Limitations:

- Hand-built rule libraries are brittle.
- Many ARC tasks require abstractions not currently represented.
- Better performance likely requires richer object relations, rule composition, and search over possible programs.

## Future Work

Potential next improvements:

- stronger object relation detection
- odd-object-out reasoning
- symmetry detection
- line extension to boundaries or objects
- pattern completion
- better rule ranking
- broader multi-step rule composition
- case-based retrieval from previously solved tasks

## Suggested Repository Name

Recommended GitHub repository name:

```text
arc-sdc-rule-solver
```

Suggested description:

```text
A Knowledge-Based AI ARC solver using a Similarity-Difference-Color rule induction approach.
```
