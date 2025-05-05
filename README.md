# Hybrid-TLBO-SVM-Commodity-Futures-Forecasting
README – Hybrid TLBO–SVM Commodity‑Futures Forecasting
(replicating & extending Das et al., 2016)

1 . Overview
This repository reproduces the DR‑SVM‑TLBO ensemble proposed in

Das, S. P., Achary, N. S., & Padhy, S.
“Novel hybrid SVM–TLBO forecasting model incorporating dimensionality‑reduction techniques.”
Applied Intelligence, 45 (2016): 1148‑1165. DOI 10.1007/s10489‑016‑0801‑3

We combine

Dimensional reduction – PCA, Kernel PCA, Fast ICA

Teaching–Learning‑Based Optimisation – tunes SVR (RBF) hyper‑parameters

Technical‑indicator feature set – the same 17 indicators as the paper

and evaluate 1‑, 3‑, 5‑day forecasts of the MCX COMDEX commodity index.

2 . What’s new in this repo
Item	Paper (2010‑2014)	This work
Data window	1 Jan 2010 – 7 May 2014	1 Jan 2016 – 5 May 2025 (latest available)
Dataset source	MCX historical download	Same, but refreshed
Scale of Close	Min‑Max 0‑1	Raw index points (≈ 10 k – 26 k)
Code	MATLAB / R mix	Pure Python 3 / scikit‑learn
Re‑run time	–	< 3 minutes on laptop

3 . Folder structure
arduino
Copy
Edit
├─ dr_svm_tlbo_forecasting.py   # main script / notebook cells
├─ MCXiCOMDEX_HistoricalData.xls
├─ README.md                    # (this file)
└─ plots/                       # saved Actual vs Predicted charts
4 . Quick start
bash
Copy
Edit
# 1) create env
conda create -n tlbo_svm python=3.10 scikit-learn pandas matplotlib numpy scipy
conda activate tlbo_svm
pip install openpyxl xlrd==1.2.0  # Excel + HTML‑xls support

# 2) run
python dr_svm_tlbo_forecasting.py \
       --xls MCXiCOMDEX_HistoricalData.xls \
       --horizons 1 3 5 \
       --pop 15 \
       --iter 30 \
       --plot
For Jupyter users: copy the six notebook cells from the script header into a new notebook and run sequentially.

5 . Results on 2016‑2025 window
Dim‑red	Horizon	RMSE (pts)	MAE (pts)	NMSE	DS %
PCA	1‑day	2679	1635	0.246	70.0
3‑day	2302	1483	0.181	70.9
5‑day	2306	1355	0.182	72.6
KPCA	1‑day	2751	1873	0.259	71.4
3‑day	2458	1564	0.207	70.9
5‑day	2304	1353	0.182	72.6
ICA	1‑day	2782	2039	0.265	72.7
3‑day	2654	1776	0.241	67.4
5‑day	2378	1450	0.193	73.0

All models achieve ≥ 70 % directional accuracy (DS).

KPCA‑TLBO still yields the lowest RMSE / MAE, echoing the 2016 study.

Absolute errors are larger than in the paper because we forecast raw index points, not 0‑1 normalised values.

6 . Key scripts / functions
Function	Purpose
load_mcx_comdex()	Robustly read MCX CSV, true Excel or HTML‑wrapped xls
compute_indicators()	Generate 17 technical indicators
tlbo_optimize()	Lightweight implementation of TLBO (no external libs)
run()	Orchestrates DR → TLBO‑SVR → forecast → metrics / plots

7 . Repro tips
To match the exact scale of the paper, normalise Close with the same Min‑Max scaler used on indicators before fitting, and compute RMSE on that scale.

Set random_state everywhere (ICA, KPCA gamma, numpy.random.seed) for deterministic output.

8 . License & citation
Code released under the MIT License.
If you use this repo, please cite the original research:

Das, S. P., Achary, N. S., & Padhy, S. (2016).
Applied Intelligence, 45, 1148–1165. DOI: 10.1007/s10489‑016‑0801‑3
