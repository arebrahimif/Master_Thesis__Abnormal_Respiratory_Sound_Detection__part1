
# Respiratory Sound Classification with Machine Learning

Master's thesis project exploring machine-learning approaches to respiratory sound classification using the ICBHI respiratory sound dataset.

**Status:** Work in progress.

## Overview

This project investigates the classification of respiratory-cycle audio as **Normal** or **Abnormal**, using classical machine-learning baselines and pretrained audio models.

The project explores:

- **Random Forest:** Statistical features extracted from log-Mel spectrograms.
- **CNN14 + Random Forest:** Pretrained PANNs/CNN14 audio embeddings followed by a Random Forest classifier.
- **CNN14 + SVM:** Pretrained CNN14 embeddings followed by a Support Vector Machine.
- **CNN14 with residual embedding adapters:** Parameter-efficient adaptation of a pretrained CNN14 backbone.

The adapter experiments use two model capacities:

- **Stage 1:** Approximately 100K trainable parameters.
- **Stage 2:** Approximately 1M trainable parameters.

Experimental results will be documented after the corresponding training and evaluation procedures are completed.

## Classification Task

The final evaluation is binary:

- **Normal:** Healthy respiratory cycles.
- **Abnormal:** Respiratory cycles containing crackles, wheezes, or both.

Some baseline models are trained using four respiratory categories before their predictions are mapped to the binary classification task.

## Dataset

The experiments use the ICBHI respiratory sound dataset and the project's split metadata.

The dataset is not included in this repository. Obtain it from its official source and follow the applicable license and terms of use.

Place the dataset in the directory expected by the notebooks, or update the paths to match your local setup.

Raw audio and dataset files should not be committed to GitHub.

## Methodology

### Audio Processing

Respiratory cycles are extracted from audio recordings using their associated start and end timestamps.

Depending on the experiment, audio is represented using log-Mel spectrogram features or embeddings extracted from a pretrained CNN14 model.

### Classical Machine Learning

The baseline experiments evaluate Random Forest and Support Vector Machine classifiers using extracted audio features.

### Parameter-Efficient Fine-Tuning

The CNN14 experiments investigate residual embedding adapters while keeping the pretrained backbone frozen.

Stage 1 uses an adapter with approximately 100K trainable parameters, while Stage 2 increases the adapter capacity to approximately 1M trainable parameters.

## Evaluation Protocol

The project uses the supplied train/test split. Validation data for model selection should be drawn only from the training portion, with patient-level separation where applicable.

The official test set is reserved for final evaluation.

The evaluation metrics include:

- **Accuracy**
- **Sensitivity (SE):** Proportion of abnormal cycles correctly identified.
- **Specificity (SP):** Proportion of normal cycles correctly identified.
- **Harmonic Score (HS):** Harmonic mean of sensitivity and specificity.

HS is calculated as:

HS = 2 × SE × SP / (SE + SP)

The implementation should handle the zero-denominator case safely.

For the adapter experiments, the best checkpoint is selected using the validation HS-score. Test-set results must not be used for checkpoint selection or hyperparameter tuning.

## Repository Structure

The following structure is illustrative. Adapt it to match the files included in the repository.

```text
.
├── src/
│   ├── audioset_tagging_cnn_master/
│   └── *.ipynb
├── datasets/       # Local dataset files; excluded from Git
├── models/         # Local checkpoints; excluded from Git
├── .gitignore
└── README.md
```

## Setup

The notebooks use Python. Exact package requirements may differ between experiments.

1. Clone the repository:

   ```bash
   git clone <YOUR_GITHUB_REPOSITORY_URL>
   cd <YOUR_REPOSITORY_NAME>
   ```

2. Create a virtual environment:

   ```bash
   python -m venv .venv
   ```

3. Activate the environment.

   **Windows PowerShell:**

   ```powershell
   .\.venv\Scripts\Activate.ps1
   ```

   **macOS/Linux:**

   ```bash
   source .venv/bin/activate
   ```

4. Install the dependencies required by your selected notebook. If a `requirements.txt` file is provided:

   ```bash
   pip install -r requirements.txt
   ```

5. Download the dataset and configure the relevant data paths.

6. Launch Jupyter:

   ```bash
   jupyter lab
   ```

## Pretrained Model Weights

The CNN14 experiments use a pretrained PANNs CNN14 AudioSet checkpoint.

The weights are not included in this repository. Obtain them from the original source, follow any applicable terms, and configure the checkpoint path in the relevant notebook.

Please cite the original PANNs/CNN14 work in academic publications.

## Reproducibility

For each experiment, document:

- Dataset split and patient-level protocol.
- Audio preprocessing and feature extraction settings.
- Model architecture and hyperparameters.
- Random seeds.
- Checkpoint-selection criterion.
- Software versions and hardware.
- Validation results and final test results, reported separately.

## Citation

Before publishing, add the official ICBHI dataset citation and the original PANNs/CNN14 paper or repository reference.

## Disclaimer

This repository is intended for academic research and educational purposes. It is not a medical device and must not be used to make clinical decisions.
