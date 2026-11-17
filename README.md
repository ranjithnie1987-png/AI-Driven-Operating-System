# Code, data and executed notebooks — Paper B

**AI-Driven Operating-System Control Validated on the Real Linux Kernel: Learned Scheduling, Paging, DVFS and Anomaly Guarding with Factorial Shapley Attribution and a Digital Twin**
Akella S. Narasimha Raju, Kante Satyanarayana, Jinsoo Cho, Fahim Khan — submitted to *Journal of Systems Architecture (Elsevier)*.

## Contents
| Path | What it is |
|---|---|
| `osai/` | the Python library used throughout (OS simulator, ensembles incl. UAMF, evaluation, metrics, plotting) |
| `adapters/` | builders for the two real datasets, including the live-Linux collector `linux_live_collect.py` |
| `data/linux_live/` | Linux-Live: raw kernel counters for 2,600 processes from 70 programs, schema, collection metadata |
| `data/alibaba_pai/` | processed 10,000-job subset of the Alibaba PAI GPU trace (raw trace: see `source/GET_TRACE.txt`) |
| `notebooks/*.ipynb` | executed notebooks — every table and figure is visible without re-running |
| `notebooks/src/*.py` | the same notebooks as plain Python (jupytext percent format) |
| `notebooks/artifacts/` | intermediate files passed between notebooks |
| `notebooks/results/` | CSV/JSON results behind the tables |
| `notebooks/figures/` | every figure produced by the notebooks (PNG, 300 dpi) |
| `notebooks/paper_tables/` | manuscript tables as CSV and LaTeX, plus the claims ledger |
| `paper_figures/` | scripts that assemble the manuscript figures from the results (`python paper_figures/figs_B.py`) |

## Notebook → manuscript map
| Notebook | Content |
|---|---|
| 00_Architecture_and_Data_Flow | AI-OS architecture and data flow |
| 01_Data_and_EDA | the two real data sources (prerequisite: writes `artifacts/*_clean.csv`) |
| 06_Virtual_CPU_and_Scheduling | simulator, 9 schedulers, AI-SJF/SRTF/MLFQ, meta-scheduler |
| 07_Virtual_Memory_and_Storage | page replacement incl. learned Belady, TLB, swap, disk scheduling |
| 08_AI_CPU_VLSI_DVFS | CMOS power/thermal model, governors, AI-DVFS |
| 09_Digital_Twin | twin calibration, validation and decision fidelity |
| 10_OS_Data_Structures | PCB, CFS red-black tree, MLFQ, page tables, Banker's algorithm |
| 11_Integrated_AI_OS_With_vs_Without_AI | 2⁴ factorial, KPIs, Shapley attribution |
| 12_Visual_Analytics_Dashboard | interactive dashboard (`dashboard/os_ai_dashboard.html`) |
| 16_PaperB_B1_Real_Linux_Validation | real-kernel runs: CFS vs AI-nice-CFS, simulator fidelity (run on an idle machine; pinned to 2 CPUs) |
| 17_PaperB_B2_CFS_Baseline_MetaScheduler_RegimeMap | CFS-like baseline, 11 policies × 20 windows, meta-scheduler v2, regime map |
| 18_PaperB_B3_Scaled_Ablation_Shapley_Twin | 2⁴ factorial × 20 windows vs CFS, Shapley with bootstrap CIs, twin fidelity |
| 19_Paper_Evidence_Tables | `paper_tables/B*` (CSV + LaTeX) and the claims ledger |

The burst predictions used by the AI schedulers (`artifacts/*_burst_oof.csv`) and the anomaly scores
(`artifacts/linux_live_anomaly_oof.csv`) are produced by the Paper A pipeline and are included here so that every
Paper B notebook runs on its own.

## Reproducing
```bash
python -m venv .venv && source .venv/bin/activate      # Python 3.13
pip install -r requirements.txt && pip install -e .
cd notebooks
jupyter nbconvert --to notebook --execute --inplace 01_Data_and_EDA.ipynb   # then the others in numerical order
```
Each notebook's first cell embeds the `osai` library, so the notebooks also run unchanged in Google Colab; they restore
any missing artifact automatically. Seeds: 42 for single runs, 0–4 for the five CV repetitions.


## Data provenance and licence
* **Linux-Live** was collected by the authors with `adapters/linux_live_collect.py` (see `data/linux_live/collection_meta.json`).
* **Alibaba PAI** comes from the Alibaba Cluster Trace Program (`cluster-trace-gpu-v2020`), released for research and study
  use: https://github.com/alibaba/clusterdata. Please cite Weng et al., "MLaaS in the Wild: Workload Analysis and
  Scheduling in Large-Scale Heterogeneous GPU Clusters", USENIX NSDI 2022.
* Please cite the paper above when using this code.
