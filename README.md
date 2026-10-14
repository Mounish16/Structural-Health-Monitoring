# Structural Health Monitoring: Data-Driven Defect Detection

Team project for the University of Bristol Data Science Mini-Project (EMATM0050), Problem H: **data-driven structural health monitoring**.

The goal is to detect defects in a structure from recorded time-series signals, using only a model of what a healthy signal looks like. Several models are compared, from a classical NARX baseline to deep sequence models and graph neural networks.

## Approach

**Forecasting residuals (notebooks 01 to 03).** A model is trained on healthy signals to predict a sample 20 steps ahead from a sliding window of past samples. On a defective signal the forecast is worse, so the prediction error (residual) acts as a defect indicator. Detection quality is measured with ROC curves and AUC, using the probability of detection against the probability of false alarm.

| Notebook | Model |
| --- | --- |
| [`01_narx_baseline`](notebooks/01_narx_baseline.ipynb) | Feed-forward NARX baseline |
| [`02_lstm_forecasting`](notebooks/02_lstm_forecasting.ipynb) | LSTM forecaster |
| [`03_transformer_forecasting`](notebooks/03_transformer_forecasting.ipynb) | Conv1D + multi-head attention forecaster, pre-trained then fine-tuned with noise augmentation |

**Graph models (notebooks 04 and 05).** Signals become nodes in a similarity graph (cosine similarity or k-nearest neighbours), and a graph neural network classifies each node as healthy or defective.

| Notebook | Model |
| --- | --- |
| [`04_gnn_exploration`](notebooks/04_gnn_exploration.ipynb) | GCN prototypes |
| [`05_gcn_vs_gat`](notebooks/05_gcn_vs_gat.ipynb) | GCN vs GAT across noise levels (trained GAT in [`models/`](models)) |

## Results

AUC as reported in the notebook outputs. The synthetic defect sets range from -5 dB to -30 dB.

| Defect set | LSTM | Transformer | GCN | GAT |
| --- | --- | --- | --- | --- |
| -5 dB | 0.982 | 0.887 | 0.613 | 0.662 |
| -10 dB | 0.980 | 0.779 | 0.612 | 0.662 |
| -15 dB | 0.965 | 0.627 | 0.612 | 0.662 |
| -20 dB | 0.903 | 0.565 | 0.614 | 0.662 |
| -25 dB | 0.548 | 0.548 | 0.615 | 0.662 |
| -30 dB | 0.520 | 0.519 | 0.614 | 0.662 |

- **Sequence models detect strong defects well.** The LSTM residual detector stays above 0.90 AUC down to -20 dB, then falls to near chance (0.5) at -25 and -30 dB.
- **The transformer separates the real defect sets.** On the six real defect recordings (d1 to d6) it reaches an AUC of 1.000 for all six.
- **Graph classifiers are weaker and flat across noise levels** (GCN about 0.61, GAT about 0.66), so they were kept as exploration rather than the main method.

## Repository layout

```
.
├── notebooks/        Experiments, numbered in the order they were developed
├── data/             Normalised test signals (see data/README.md for the rest)
├── models/           Trained GAT weights
├── requirements.txt
└── LICENSE
```

## Run it

Requires Python 3.10 or newer and the data files listed in [`data/README.md`](data/README.md).

```bash
git clone https://github.com/Mounish16/structural-health-monitoring.git
cd structural-health-monitoring
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebooks
```

Notebooks read their data from `../data`, so launch Jupyter from the repository root as above.

## Limitations

- The training and defect recordings are not redistributed, so the notebooks cannot be run end to end without the course data pack.
- The NARX and LSTM evaluation on the six real defect sets printed repeated values in the saved outputs (for example 0.78125 for all six in the LSTM notebook), which points to an evaluation issue; treat those figures as inconclusive.
- The graph models were trained on very few labelled signals.
- The notebooks are research logs, not packaged code.

## Tools

Python, TensorFlow / Keras, PyTorch, PyTorch Geometric, scikit-learn, SciPy, NumPy, matplotlib.

## License

Code is released under the [MIT License](LICENSE).
