# DSCI 552 Homework 4

Name: Yang Zhao  
USC NetID: yzhao528  
Instructor: Mohammad Reza Rajati

## Files

- `DSCI552_Homework4_Yang_Zhao.ipynb`: complete notebook with saved cell outputs.
- `DSCI552_Homework4_Yang_Zhao.pdf`: English report for Questions 2(a), 2(b), 2(c), 3, and 4.
- `data/AReM/`: the 88 supplied original CSVs.
- `data/hw3_features_reference.csv`: HW3's feature table, used only to check that the unsegmented extraction matches HW3; models use the original recordings.
- `results/`: CV grids, selected features, coefficients, predictions, metrics, and figures.
- `requirements.txt` and `.gitignore`.

Homework 3 Part 1 answers and the optional ISLR 4.8.4/4.8.9 problems are not included.

## Run

Use Python 3.11 or later. From the repository root:

```sh
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m jupyterlab DSCI552_Homework4_Yang_Zhao.ipynb
```

Select the environment's Python kernel and choose Restart Kernel and Run All Cells. The notebook reads relative paths and recreates `results/`. No data download is needed. The notebook was executed top to bottom in a fresh virtual environment installed from these requirements, with zero execution errors. Model searches may take several minutes. The pinned scikit-learn 1.8 API uses `l1_ratio=1` for L1 and `C=np.inf` for unpenalized fitting.

## Data and model choices

The fixed split is 69 training and 19 test recordings. Datasets 1-2 are test data for bending1/bending2; datasets 1-3 are test data for the other activities. Three files need delimiter cleanup. `sitting/dataset8.csv` lacks timestamp 13,500 ms in the original ZIP and is kept with its 479 observations. No observed data are discarded or imputed. The original cycling/dataset1.csv and dataset15.csv are identical, as are lying/dataset1.csv and dataset15.csv. Each pair crosses the prescribed train/test split, so test accuracy is not a guarantee for fully independent new recordings.

Each recording is split into l consecutive, nonoverlapping parts with lengths differing by at most one. Seven statistics per original channel per part give 42l features; the number of recordings stays 88. The scatter plots retain HW3's mean, standard deviation, and maximum for channels 1, 2, and 6.

Model selection searches l=1,...,20. Standardization and RFE are fitted inside CV pipelines. Binary and multiclass models both use five stratified folds with seed 552. Because bending2 has four training cases, one multiclass validation fold has no bending2 case; all fitting folds contain all seven classes. Accuracy is still defined; multiclass ROC is computed on the test set, which has every class. The precise search grids, tie rules, and fold counts are in the notebook and results. Best means best on the stated finite grids, not a proof of a global optimum.

RFE is used instead of p-value screening. This is the alternative allowed in 2(a)(iii); the measured 2(b)(ii) comparison is RFE versus L1, not a separate p-value-selected baseline. If the selected ordinary logistic model has complete separation, it has no finite ordinary MLE. Nominal Wald p-values are explicitly marked invalid; supplementary Firth penalized-likelihood tests are reported separately and are not selection-adjusted. Finite solver output is used for the ordinary predictive experiment. Case-control sampling includes the prior-odds intercept correction based on the original training prevalence; no test prevalence is used.

L1 multinomial logistic uses SAGA; GaussianNB and MultinomialNB use the same multiclass folds. MultinomialNB uses nonnegative MaxAbs-scaled summaries, with its count-model approximation explained. Confusion-matrix rows are true labels and columns predicted labels. ROC/AUC use bending as positive for binary models and one-vs-rest class probabilities for multiclass models.

## Submission

Push this directory's contents to the Homework 4 GitHub repository root. Include the notebook, PDF, README, requirements, `.gitignore`, `data/`, and `results/`. Follow the instructor's announcement for repository visibility and the link to submit. Do not upload virtual environments, checkpoints, temporary files, the textbook, input assignment PDFs, or old versions.

AI tools assisted with chapter concepts, unfamiliar code, and preparing code and explanations.
