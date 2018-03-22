
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.888409.svg)](https://doi.org/10.5281/zenodo.888409) [![Build Status](https://travis-ci.org/mrecos/DistRegLMERR.svg?branch=master)](https://travis-ci.org/mrecos/DistRegLMERR)

![](https://github.com/mrecos/klrfome/blob/master/klrfome_hex.png?raw=true)

PRE-RELEASE
===========

klrfome - Kernel Logistic Regression with Focal Mean Embeddings
---------------------------------------------------------------

### SAA 2018 Abstract

A model of Distribution Regression using Kernel Logistic Regression (KLR) on Mean Feature Space Embeddings is developed to address two primary shortcomings of current Archaeological Predictive Modeling (APM) practice; 1) neglecting the richness of archaeological land forms by collapsing a site to a single point or observation; and 2) disregarding the implicit and explicit uncertainty of archaeological data, predictions, and model parameters. This research addresses the first hurdle by developing a KLR on embeddings approach to Distribution Regression. This method first samples a distribution of variable measurements from the spatial area of each site, then uses a kernel to project the distributions into a non-geographical feature space to calculate mean embeddings, finally Kernel Logistic Regression estimates similarity coefficients for inference and prediction. The primary benefits of this approach to APM are the consideration of archaeological land form richness and variation, explicitly modeling similarity between sites and the environment, and allowing for similarity metrics specific to archaeological research questions. The second hurdle is addressed by applying the MERR method within a Bayesian framework for probabilistic modeling. As such, the uncertainty of data and parameters can be explicitly modeled with priors resulting in a posterior predictive distribution useful for quantifying and visualizing risk.

-   Note: Changes from original abstract; 1) change of terms to Kernel Logistic Regression (KLR); and 2) deemphasized Bayesian approach as it is a work in progress. The repo contains working Stan code for probabilistic models, but they are not ready for prime-time.

![](https://github.com/mrecos/klrfome/blob/master/analysis/images/KLR_map.jpg?raw=true)

### Example

``` r
# install.packages("devtools")
devtools::install_github("mrecos/klrfome")
```

### Main Package Functions

This package contains the functions necessary to compute Kernel Linear Regression (KLR) on mean kernel embeddings, functions for preparing site and background data, and a function for simulate archaeological site location data.

#### Fitting and Predicting

-   `build_K` - Function takes list of training data, scalar value for `sigma` hyperparameter, and a distance method to compute a mean embedding similarity kernel. This kernel is a pair-wise (N x N) matrix of the mean similarity between the attributes describing each site location and background group.
-   `KLR` - Function takes the similarity kernel matrix `K`, a vector of presence/absence coded as `1` or `0`, and a scalar value for the `lambda` regularizing hyperparameter; optionally values for maximum iterations and threshold. This function performs Kernel Logistic Regression (KLR) via Iterative Re-weighted Least Squares (IRLS). The objective is to approximate a set of parameters that minimize the negative likelihood of the parameters given the data and response. This function returns a list of `pred`, the estimated response (probability of site-presence) for the training data, and `alphas`, the approximated parameters from the IRLS algorithm.
-   `KLR_predict` - Function takes a list of the training data, a list of the testing data, a vector of the approximated `alphas` parameters, a scalar value for the `sigma` kernel hyperparameter, and a distance method. This function predicts the probability of site presence for new observations based on the training data and `alphas` parameters. This is accomplished by building the `k*k` kernel matrix as the similarity between the training test data then computing the inverse logit of `k*k %*% alphas`. The output is the predicted probability of site presence for each training data example.

#### Data Formatting

-   `format_site_data` - Function takes a `data.frame` of presence/background observations. The column `presence` indicated site presence of background as `1` or `0` respectively. The `SITENO` column indicates either a site identifier or `background`. The remaining columns should be measurements of environmental or cultural variables for modeling similarity. It is expected that each site will have multiple rows indicating multiple measurements of environmental variables from within the site boundary or land form. The function centers and scales the predictor variables, sub-samples the data into training and test sets, and re-formats the data as both tabular format and list format for both training and test data sets. Importantly, the function makes sure that no single site is in both the training and test set. No test sites are used to train the model. Also returned is the mean and standard deviation of the data so that new data can be center and scaled to the same parameters.
-   `get_sim_data` - Function takes a mean and SD for two simulated predictor variables for each of sites and background. With archaeological site data being a protected data set in many settings (including this project), it cannot be freely shared. However, it is difficult to implement this model without properly format data. This function simulates archaeological site data and background environmental data for testing or tuning of this model. The concept of the function is to simulate two predictor variables for both sites and background. The inputs to the function (defaults provided) control how similar or different the site data is from the background. The model can be tested on simulated site data that is very similar to the background or very different; the model behaves accordingly. The output of the function is formatted in the same way as the `format_site_data` function.

#### Calculating Performance Metrics

-   `metrics` -

#### Plotting

-   `K_corrplot` -

### Citation

Please cite this compendium as:

> Harris, Matthew D., (2017). *klrfome - Kernel Logistic Regression with Focal Mean Embeddings*. Accessed 10 Sep 2017. Online at <https://doi.org/10.5281/zenodo.888409>

### Installation

You can install DistRegLMERR from github with:

``` r
# install.packages("devtools")
devtools::install_github("mrecos/klrfome")
```

### Licenses

**Text and figures:** [CC-BY-4.0](http://creativecommons.org/licenses/by/4.0/)

**Code:** See the [DESCRIPTION](DESCRIPTION) file

**Data:** [CC-0](http://creativecommons.org/publicdomain/zero/1.0/) attribution requested in reuse

### Contributions

We welcome contributions from everyone. Before you get started, please see our [contributor guidelines](CONTRIBUTING.md). Please note that this project is released with a [Contributor Code of Conduct](CONDUCT.md). By participating in this project you agree to abide by its terms.
