# Diabetes Status Classification from BRFSS 2015 Health Indicators

Classifies survey respondents as **no diabetes**, **prediabetes** or **diabetes** from 21 health indicators. Compares traditional machine learning (scikit-learn) with deep learning (TensorFlow/Keras with `tf.data`) across 15 experiments and a held-out test evaluation.

**Author:** [your name]

## Links
| Resource | Link |
|---|---|
| Report | https://docs.google.com/document/d/1nWwSUQrsAZQ5SVGPf3v8gygDqolmsWZ0t87SQofdTuc/edit?usp=sharing |
| Google Drive (dataset zip, saved models, logs) | (https://drive.google.com/drive/folders/1WTICaxd-hlayD76dvGgG38G59Mxlnz9g?usp=drive_link) |
| Video | (https://youtu.be/PlTnOthjZFU)|

## Dataset
CDC BRFSS 2015, Diabetes Health Indicators: 253,680 rows, 21 features, 3 classes (84.2% no diabetes, 13.9% diabetes, 1.8% prediabetes). Source: [add the exact page you downloaded it from].

**The raw data is not in this repository.** Download `diabetes_012_health_indicators_BRFSS2015_csv.zip` from the Drive folder above.

## How to run
1. Open `diabetes_classification.ipynb` in Google Colab (CPU is enough).
2. Locally, install dependencies with `pip install -r requirements.txt`.
3. Choose **Runtime, then Run all**, upload the dataset zip when asked, and approve the Drive popup.
4. A full run takes about 15 minutes. Seeds are fixed (`SEED = 42`), and repeated runs gave identical metrics.

## Repository contents
- `diabetes_classification_final.ipynb`: full pipeline, experiments and evaluation
- `requirements.txt`: pinned dependencies
- `experiment_results.csv`: the experiment table (settings, split, metrics and notes for all 18 runs)

Trained models, the scaler and class scales, and the TensorBoard logs are in the Drive folder.

## Key results
Duplicates were removed before the stratified 70/15/15 split. Models are compared on macro-F1, because always predicting "no diabetes" already scores about 83% accuracy.

| Model | Role | Validation macro-F1 | Test macro-F1 | Test ROC-AUC |
|---|---|---|---|---|
| E8: logistic regression + probability rescaling | best classical | 0.461 | 0.458 | 0.767 |
| D7: MLP 128-64 + probability rescaling | best deep learning | 0.464 | 0.460 | 0.765 |
| D2: MLP 128-64, class-weighted | more aggressive operating point | 0.428 | 0.430 | 0.768 |

## Main findings
- Linear, tree-based and neural models all reach a ROC-AUC of about 0.75-0.77. Larger networks, tuning and engineered features did not raise this, which points to limited information in the survey features.
- The decision rule matters more than the model: without class weights or rescaling, no model predicts prediabetes.
- Prediabetes has only about 4,600 rows (about 695 in the test set), so its metrics are noisy.

See the notebook's interpretation cells and the report for the full analysis.

## Acknowledgements
Data: CDC BRFSS 2015. No pretrained models were used. I used an AI assistant for guidance and for help with debugging and planning.
