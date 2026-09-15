# Machine Learning in R

Reference notes on machine learning and computational statistics in R, written as R
Markdown notebooks and knitted to PDF. Fourteen notebooks across eleven folders, covering
tree-based methods, SVM, neural networks including RNN and LSTM, unsupervised learning,
regularized regression, and a few numerical-methods topics.

These are study notes rather than a project. Each notebook introduces the method, gives
the R syntax in reusable form, then works a full example end to end on real data.

---

## How the notebooks are written

Most follow the same two-part structure:

1. **Syntax section** — the function signature with its important arguments explained,
   written in chunks marked `eval=FALSE, include=TRUE` so they display without running.
   These use placeholder names like `data`, `Y`, and `...`.
2. **Example section** — the same method applied to a real dataset, run for real, with
   output and plots kept inline.

Because the syntax chunks aren't meant to execute, read the notebooks or knit them
whole; running cells top to bottom out of order won't work.

---

## Contents

### Trees and ensembles

| Folder | Covers | Data |
|---|---|---|
| `CART` | `rpart()` with `method` and `rpart.control()`; `minsplit` and `minbucket`; Gini vs. entropy splits via `parms = list(split = "information")`; plotting with both `fancyRpartPlot()` and `rpart.plot()`; pruning two ways — 10-fold CV through `caret::train(tuneLength = 100)`, and manually via `printcp()` and the `cptable` minimum `xerror`; a full-vs-pruned comparison showing the TP/TN trade-off | Titanic |
| `Random Forest` | `randomForest()` and `mtry` tuning by 10-fold CV; variable importance split into mean decrease in accuracy and mean decrease in Gini, each plotted separately; worked as both a classification example and a regression example | Banknote authentication |

### Support vector machines

| Folder | Covers | Data |
|---|---|---|
| `SVM in R` | The four-step workflow — preprocess, fit, predict, confusion matrix; hyperparameter tuning with an explicit `tuneGrid` over `C`, then `tuneLength` with radial and polynomial kernels; standardizing numeric columns before fitting | Titanic (committed) |

### Neural networks

| Folder | Covers | Data |
|---|---|---|
| `Neural Network in R` | A short syntax reference for `neuralnet()`, plus a classification example comparing a no-hidden-layer perceptron under SSE and cross-entropy loss, a logistic regression baseline, and a 1×3 hidden-layer network | Banknote authentication |
| `NN Regression` | Regression with `neuralnet` — normalizing the response, keeping the mean and SD to de-scale predictions, comparing no hidden layer vs. one hidden layer vs. multiple linear regression on test MSE, and plotting predicted against observed | `Boston` (MASS) |
| `RNN and LSTM` | The longest notebook. Lagged min–max normalization, sliding-window dataset construction, converting to TensorFlow tensors, building and training both `layer_simple_rnn` and `layer_lstm` models in Keras, recursive multi-step forecasting, and rescaling predictions back to the original units. Two examples: a sine wave, then AAPL closing prices with a 10-day forecast | Sine wave, AAPL (committed) |

### Unsupervised learning

| Folder | Covers | Data |
|---|---|---|
| `Clustering` | Hierarchical clustering with `dist()` and `hclust()` across linkage methods; dendrograms via base `plot()` with `rect.hclust()` and via `fviz_dend()`; k-means with `fviz_nbclust()` for choosing k; fuzzy c-means | `USArrests` |
| `PCA` | `prcomp()` with `center` and `scale.`, reading the summary table, scree plots and biplots, and writing PC1 out as a linear combination of the original features. The example clusters iris six ways — k-means plus Ward, single, complete, average, and centroid linkage — and compares each against the true species labels | `iris` |

### Regression: diagnostics and regularization

| Folder | Covers | Data |
|---|---|---|
| `MultiColinearity:Penalized Regression` | Detecting collinearity with a correlation matrix and `corrplot` heatmap, then three regularized fits — ridge, lasso, and elastic net through `glmnet` and `caret` — each with its CV plot, best lambda, and confusion matrix, closing with PCA as a dimensionality-reduction alternative | External CSV |

### Numerical and computational statistics

| Folder | Covers |
|---|---|
| `EM Algorithm` | Monte Carlo integration — estimating definite integrals by sampling from a uniform, estimating a normal tail probability, and importance sampling. Includes a short in-class assignment using a Cauchy proposal |
| `Random Variable Generation` | SVD in R — the Moore–Penrose generalized inverse computed by hand from `svd()` and checked against `MASS::ginv()`, solving a singular system in the least-squares sense, and deriving the OLS estimator in matrix form |

---

## Data

Two datasets are committed:

- `SVM in R/Titanic.csv`
- `RNN and LSTM/AAPL.csv` — daily OHLCV, roughly a year ending March 2022

The rest read from absolute local paths under `/Users/thomashuang/Downloads/` —
`Titanic.csv`, `banknote.csv`, and `data_factorize_notScaled.csv`. Those notebooks won't
knit on another machine until the paths are changed. The `Clustering`, `PCA`, and
`NN Regression` notebooks use datasets built into R (`USArrests`, `iris`, and
`MASS::Boston`) and run anywhere.

---

## Requirements

R and RStudio, plus:

- **Trees and ensembles:** `rpart`, `rpart.plot`, `rattle`, `randomForest`, `caTools`
- **General modeling:** `caret`, `tidyverse`, `dplyr`, `MASS`
- **SVM:** `e1071`, `kernlab`
- **Neural networks:** `neuralnet`, `keras`, `tensorflow`, `reticulate`
- **Unsupervised:** `cluster`, `factoextra`, `ggbiplot`, `ggplot2`
- **Regularization and diagnostics:** `glmnet`, `corrplot`, `car`
- **Time series:** `tidyquant`, `zoo`, `magrittr`
- **Other:** `leaps`, `lmtest`, `knitr`

The RNN and LSTM notebook needs a working Python and TensorFlow backend reachable
through `reticulate`.

---

## Running the notebooks

Open the `.Rproj` in the folder you want, then knit the `.Rmd` in RStudio. Seeds are set
inside each notebook, so results reproduce once the data paths resolve.
