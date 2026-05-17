# 🌿 Branch and Bound for Permutation Flowshop

> A from-scratch implementation of the **Branch and Bound** algorithm to solve the **Permutation Flowshop Scheduling** problem — minimizing the makespan across multiple machines.

---

## 📚 Table of Contents

- [Overview](#-overview)
- [Problem Definition](#-problem-definition)
- [Algorithm](#-algorithm)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Benchmark](#-benchmark-taillard-instances)
- [Results](#-results)
- [References](#-references)

---

## 🔍 Overview

This project implements a **Branch and Bound** solver for the classic **Permutation Flowshop** combinatorial optimization problem. The goal is to find the optimal job ordering that minimizes the total completion time (makespan) across all machines.

The implementation is presented as an interactive **Jupyter Notebook** with step-by-step explanations, visualizations, and benchmarks on Taillard's well-known test instances.

---

## 🏭 Problem Definition

In the **Permutation Flowshop** problem:

- There are **n jobs** and **m machines**
- Each job must be processed on every machine **in the same order** (machine 1 → machine 2 → ... → machine m)
- A machine can only process **one job at a time**
- The objective is to find the job permutation that **minimizes the makespan** (total completion time)

This is an **NP-hard** combinatorial optimization problem — brute-force search grows as `O(n!)`.

---

## 🌲 Algorithm

The solver uses a **Branch and Bound** strategy to prune the search space:

| Component | Description |
|-----------|-------------|
| 🔼 **Upper Bound** | Initialized as the sum of all processing times; updated whenever a better complete sequence is found |
| 🔽 **Lower Bound** | Computed for each partial sequence using remaining job times and machine idle analysis |
| ✂️ **Pruning** | A branch is cut whenever its lower bound exceeds the current best upper bound |
| 🔍 **Exploration** | Depth-first search over all job permutations with aggressive pruning |

The `FlowShopBB` class encapsulates the entire solver:

```python
fs = FlowShopBB(jobs_list)
fs.optim(debug=True)
print("Optimal sequence:", fs.seq_star)
print("Makespan:", fs.upper_bound)
```

---

## 📁 Project Structure

```
from_sratch_branch_and_bound_for_permutation_flowshop/
│
├── 📓 from_scratch_branch_and_bound_demo_for_permutation_flowshop.ipynb
│       Main notebook — implementation, tests, and benchmarks
│
├── 📄 from_scratch_branch_and_bound_demo_for_permutation_flowshop.html
│       Exported HTML version of the notebook
│
├── 📄 from_scratch_branch_and_bound_demo_for_permutation_flowshop.pdf
│       Exported PDF version of the notebook
│
└── 📊 ben.txt
        Taillard benchmark instances (10 instances, 20 jobs × 5 machines)
```

---

## 🚀 Getting Started

### Prerequisites

```bash
pip install matplotlib jupyter
```

### Run the notebook

```bash
jupyter notebook from_scratch_branch_and_bound_demo_for_permutation_flowshop.ipynb
```

### Quick example

```python
# Define a small flowshop instance (4 jobs × 3 machines)
jobs_list = [
    [5, 6, 4],   # Job 0: processing times on M1, M2, M3
    [8, 7, 8],   # Job 1
    [7, 2, 7],   # Job 2
    [3, 5, 9],   # Job 3
]

fs = FlowShopBB(jobs_list)
fs.optim(debug=True)
print("Optimal sequence:", fs.seq_star)
print("Makespan:", fs.upper_bound)
```

---

## 📊 Benchmark — Taillard Instances

The solver is tested against **Taillard's benchmark** (`ben.txt`), a standard set of instances widely used in scheduling research.

Each instance contains:
- **20 jobs**, **5 machines**
- Known **upper bound** and **lower bound** for comparison

| Instance | Jobs | Machines | Taillard UB | Taillard LB |
|----------|------|----------|-------------|-------------|
| 1        | 20   | 5        | 1278        | 1232        |
| 2        | 20   | 5        | 1359        | 1290        |
| 3        | 20   | 5        | 1081        | 1073        |
| 4        | 20   | 5        | 1293        | 1268        |
| 5        | 20   | 5        | 1236        | 1198        |
| 6        | 20   | 5        | 1195        | 1180        |
| 7        | 20   | 5        | 1239        | 1226        |
| 8        | 20   | 5        | 1206        | 1170        |
| 9        | 20   | 5        | 1230        | 1206        |
| 10       | 20   | 5        | 1108        | 1082        |

---

## 📈 Results

The notebook includes visualizations of:

- 📉 **Makespan evolution** — how the best-known solution improves during search
- ⏱️ **Execution time vs. number of jobs** — illustrating the exponential complexity and the impact of pruning

> ⚠️ Due to NP-hard complexity, the solver is run for gradually increasing numbers of jobs (1 to 12) before execution time becomes prohibitive for 20-job instances.

### Algorithm Results on Full 20-Job Instances

For this study, each instance was run to completion or interrupted manually when execution time became "excessive" (more than 5 minutes). When the algorithm **converges** (finishes without interruption), it proves optimality — the upper bound and lower bound collapse to the same value.

For interrupted runs, the **Lower Bound** column shows Taillard's reference LB, which remains a valid lower bound on the optimal makespan.

| Instance | Algorithm UB | Algorithm LB | Converged? |
|----------|-------------|-------------|------------|
| 1        | 1297        | 1232        | ❌          |
| 2        | 1359        | 1359        | ✅          |
| 3        | 1081        | 1081        | ✅          |
| 4        | 1293        | 1293        | ✅          |
| 5        | 1279        | 1198        | ❌          |
| 6        | 1224        | 1180        | ❌          |
| 7        | 1234        | 1234        | ✅          |
| 8        | 1222        | 1170        | ❌          |
| 9        | 1244        | 1206        | ❌          |
| 10       | 1127        | 1082        | ❌          |

> 💡 **Note:** Instance 7 converged to makespan **1234**, which is better than Taillard's reported upper bound of 1239 — confirming 1234 as a better optimal for this instance than the one found at Taillard's time.

---

## 📖 References

- 📄 [Ignall & Schrage (1965) — Application of the Branch and Bound Technique to Some Flow-Shop Scheduling Problems](https://core.ac.uk/download/pdf/234676937.pdf)
- 🎥 [YouTube — Branch and Bound for Flowshop explained](https://www.youtube.com/watch?v=Q58zRyoa4IE&t=2838s)
- The optimality of the solutions found for the converged instances can be confirmed by checking this [Zenodo](https://zenodo.org/records/17028980) report
---

<div align="center">
  Made with ❤️ as part of a combinatorial optimization challenge in the TOIA course (Technique d'Optimisation pour l'Inteligence Artificielle) at ESI (Ecole Supérieure D'informatique, Algérie).
</div>