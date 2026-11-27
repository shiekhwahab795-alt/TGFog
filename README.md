[README.md](https://github.com/user-attachments/files/35509374/README.md)
# TG-Fog

Code for the paper **"TG-Fog: A Scalable and Privacy-Preserving Fog-Based Federated Graph Learning Framework for Composition-Robust Intrusion Detection in IoT Networks"** (submitted to the *IEEE Internet of Things Journal*).

The whole pipeline is a single notebook, `TGFog_Final_Code.ipynb`. It covers data download and cleaning, all experiments, and every result table. **Runtime → Run all** produces each table as CSV, as LaTeX and in one Excel workbook, and saves the figures as PNG (300 dpi) and PDF. All numbers in the paper come from these tables; none were typed by hand.

## What the notebook covers

- **Organization-aware fog federation.** Three datasets act as three organizations with different feature schemas. Each organization has a private input adapter, and only the graph body is shared.
- **TG-EGNN.** An edge-classification GNN on host-interaction window graphs, with hosts as nodes and flows as directed edges.
- **Window-purity shortcut.** Purity measurement, a mixed-window control and composition-robust training (CRT).
- **Fog-side soft-trust aggregation and privacy.** Secure aggregation (pairwise masks), client-level differential privacy with an RDP accountant, and a membership-inference audit.
- **Poisoning.** Label flipping, backdoor, ALIE and IPM attacks against Mean, Median, TrimmedMean, Multi-Krum and FLTrust.
- **Further evaluations.** Zero-day detection (leave-one-family-out), constraint-aware PGD, topology camouflage, scalability, and resource profiling on one CPU thread.

## Requirements

- Python 3 with a GPU. The experiments in the paper ran on Google Colab with an NVIDIA T4.
- `torch`, `numpy`, `pandas`, `scipy`, `scikit-learn`, `xgboost`, `matplotlib`, `openpyxl`, `kagglehub`, `kaggle`.

On Colab only `xgboost`, `kagglehub`, `kaggle` and `openpyxl` are missing, and the notebook installs them itself (cell *0b. Packages*).

## Datasets

The three public datasets are **not** included in this repository. The notebook downloads them from Kaggle.

| Organization | Dataset | File(s) used | Label column |
|---|---|---|---|
| Org-A | TON_IoT (network data) | `train_test_network.csv` or the network CSV files | `type` |
| Org-B | Edge-IIoTset | `DNN-EdgeIIoT-dataset.csv` | `Attack_type` |
| Org-C | BoT-IoT | 5% "Full5pc" CSV files | `category` |

**Kaggle access, needed once:**
1. On Kaggle, go to *Settings → API → Create New Token*. This downloads `kaggle.json`.
2. Do one of the following:
   - add the Colab secrets `KAGGLE_USERNAME` and `KAGGLE_KEY`, or
   - place `kaggle.json` in `MyDrive/`, or
   - upload the file when the first cell asks for it.
3. Some datasets require clicking *Download* once on their Kaggle page to accept the terms.

**Using your own copies:** set `CFG["DATA_SOURCE"] = "drive"` and put the files under `MyDrive/IoT_Datasets/<dataset name>/`, or give the paths in `CFG["MANUAL_PATHS"]`.

## How to run

1. Open `TGFog_Final_Code.ipynb` in Google Colab and select a GPU runtime.
2. Set up Kaggle access as described above.
3. Run **Runtime → Run all**.
   - Results are cached in `MyDrive/TGFog_outputs`. If the session disconnects, rerunning continues where it stopped.
   - `STOP_AFTER` in cell *0. Configuration* can stop the run after a given stage.
   - `SELF_TEST = True` runs a quick pipeline check on synthetic data only. Its outputs go to a separate folder and are watermarked "NOT FOR PAPER".

All settings (window sizes, seeds, number of clients, rounds, DP parameters, attacks and defenses) are in the `CFG` dictionary in cell *0. Configuration*. Hyperparameters of the proposed protocol and of FedProx are selected on validation data with a separate tuning seed (seed 7, table `T08a_tuning`). Reported results use seeds 42–46 for centralized models and seeds 42–44 for federated models.

## Where each paper result comes from

| Paper | Notebook table(s) |
|---|---|
| Table II: organizations after cleaning | `T01_datasets`, `T04_splits_graphs`, `T05_homophily` |
| Section VI-A: window purity | `T05b_window_purity`, `T07c_mixed_purity` |
| Table III: centralized results | `T06_centralised` |
| Table IV and Fig. 3: mixed-window control, CRT | `T07b_purity_stress`, `T07d_robust_training`, `T07e_purity_controlled_stats` |
| Table V: federated results | `T08_fl_main`, `T08b_fl_composition`, `T08c_fl_composition_stats`, `T09_fl_stats` |
| Table VI: ablations | `T07_ablation`, `T11_fl_ablation`, `T11b_sparsity`, `T08a_tuning` |
| Fig. 4 and Table VII: poisoning and backdoor | `T17_byzantine`, `T17b_poisoning_stats`, `T17c_poisoning_ranks`, `T18_backdoor_asr` |
| Section VI-F: secure aggregation, membership inference | `T14_secure_agg`, `T16_mia` |
| Table VIII: client-level DP | `T15a_dp_clip_tuning`, `T15_dp` |
| Table IX: zero-day detection | `T19_zero_day`, `T19b_zero_day_robust`, `T19c_zero_day_flow_level` |
| Section VI-H: constraint-aware PGD | `T20_adversarial` |
| Table X: topology camouflage | `T21_camouflage`, `T21b_camouflage_defence`, `T21c_camouflage_robustness` |
| Table XI: scalability | `T12_scalability`, `T13_fog_scaling` |
| Table XII: resource profile | `T22_resources` |

## Citation

If you use this code, please cite the paper. The full reference will be added after publication.

## License

Please contact the authors before reusing the code.
