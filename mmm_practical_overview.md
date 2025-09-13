# Abstract

Marketing Mix Models (MMMs) are statistical models used to estimate marketing effectiveness and guide budget planning. In this paper, we walk through the evolution of MMMs, providing example code in PyMC for practitioners to quickly get up to speed on recent MMM developments. Then, we examine current issues with MMMs, demonstrate those issues in current open source MMMs using simulations, and test out some different solutions. 

# Introduction

Give an overview of each section using a few sentences for each.

* History of MMM: MMM has come from combining econometrics, time-series analyses, and marketing theory. 
* Background - Progression from simple MMMs to Advanced: from linear models to bayesian to hierarchical bayesian MMMs to state of the art new advances.
* Current Issues: causality. 
* What this paper researches: demonstration of issues when causal dag is ignored / misunderstood. 

The rest of this paper is organized as follows. In the [Background](#background) section, we discuss the history, progression of MMMs, and current issues. In the [Methodology](#methodology) section, we demonstrate some of those key problems using simulated data. In the [Results](#results) section, we test out some of the proposed solutions from state of the art research. In the [Conclusion](#conclusion) section, we discuss future developments. 

# Background

## History

Give an overview of marketing science history in relation to MMMs. Research started in 1960s, became more widely known later. Bayesian MMM started to gain traction as computation and MCMC improved. 

References:
* [The History of Marketing Science](https://www.worldscientific.com/doi/epdf/10.1142/9789811272233_0005) - textbook that goes in depth on history - main source
* [Handbooks in Operations Research and Management Science - Marketing Mix Models Literature Review](https://www.sciencedirect.com/science/article/pii/S0927050705800386) - looks at all historical literature
* [BTVC at Uber](https://arxiv.org/html/2106.03322v4) - gives a brief summary of history
* [Hierarchical Marketing Mix Models with Sign Constraints](https://pmc.ncbi.nlm.nih.gov/articles/PMC9041956/) - also has two paragraphs about the history.

## 1. Bayesian Linear Regression

Regular linear regression finds coefficients, while Bayesian linear regression gives you a probability distribution for each coefficient, incorporating prior beliefs. 

Math formula:

$$ E(y_i|\beta, X) = \beta_1 x_{i1} + ... + \beta_k x_{ik} $$

$$ y_i \sim N(X_i^T \beta, \sigma^2) $$

Code Example:
* Show this in PyMC

Difference from Linear Regression
* Set prior on Coefficients: $ \beta $
* Set prior on Variance: $ \sigma^2 $
* Set prior on Intercept: $ \beta_0 $. 

Strengths:
* Core bayesian strengths such as using posterior distributions and talking about 95% probability instead of 95% confidence intervals.
* Include prior knowledge to help stabilize estimates and prevent overfitting (especially with small samples or multicollinearity).

Weaknesses:
* Prior sensitivity - need to find a good balance for the prior distributions - can't be too strong but can't be too weak in accurately estimating the data.
* Computational cost - when conjugate priors aren't used, have to use MCMC which is much more computationally intensive compared to regular regression which uses closed-form OLS solution.
* Scaling - bayesian and frequentist regression often agree (priors wash out) when you have large datasets.

References:
* [Bayesian Data Analysis](https://sites.stat.columbia.edu/gelman/book/BDA3.pdf)

## 2. MMM

MMM just adds some marketing specific adjustments to Bayesian Linear regression models.

Math Formula:

$$ y_t = \sum_{i=1}^m \beta_i s_i(c_i(x_{t-l+1,i},...,x_{t,i}; \alpha_i); k_i, \lambda_i) + \sum_{j=1}^n \gamma_j z_{t,j} + \epsilon_t $$

Code Example:
* Show this in PyMC

Adjustments:
* Adstock - ads can have a long lasting but decaying effect.
* Saturation - audience targeted by ads is finite. You can saturate your audience with ads and have diminishing returns. 
* Priors - assumption that ads won't negatively hurt sales. This assumption can be integrated in to the beta coefficient priors. 

References: 
* [Hierarchical Marketing Mix Models with Sign Constraints](https://pmc.ncbi.nlm.nih.gov/articles/PMC9041956/) - explains adstock, saturation, and priors briefly.
* [Bayesian Methods for Media Mix Modeling with Carryover and Shape Effects](https://research.google.com/pubs/archive/46001.pdf) - this is the seminal paper about adstock and saturation. 

## 3. Hierarchical Bayesian Linear Models

Hierarchical Bayesian Linear Models add a nested structure to the model. For example, if we are using a regular regression model to predict sales in the US using marketing spend as the predictor, there would be a single coefficient for marketing spend. With a hierarchical regression, we recognize that each state may have different marketing performance. Thus, we estimate a global marketing spend coefficient, representing the average marketing impact across all states, and local state-specific coefficients that capture deviations for each individual state.

Math formula:

$$ y_{ij} \sim N(X_{ij}^T \beta_j, \sigma^2) $$

where:
* $ i $ indexes observations within group $ j $.
* each group has its own regression coefficient $ \beta_j $.

Each local $ \beta_j $ comes from a global distribution:

$$ \beta_j \sim N(\mu, \tao^2 I) $$

where: 
* hyperparameters $ \mu $, $ \tao^2 $ get priors.

So, instead of a single $ \beta $, we now have a distribution of $ \beta_j $s that center around a population mean $ \mu $. 

Code Example:

How is this different than just adding state as a variable?:
* explain the concept of pooling and why this is good in our example

References:
* [Bayesian Data Analysis](https://sites.stat.columbia.edu/gelman/book/BDA3.pdf)

## 4. Hierarchical MMMs

Hierarchical MMMs extend regular MMMs by introducing a hierarchy. For example, a hierarchical MMM might include national-level parameters for adstock, saturation, and priors, while also estimating state-level parameters that capture local variations in adstock, saturation, and priors.

Math formula:

Code Example:

Strengths:

Weaknesses:

Resources:
* [Geo-level Bayesian Hierarchical MMM](https://research.google/pubs/geo-level-bayesian-hierarchical-media-mix-modeling/)

## 5. State of the Art

# Methodology

# Results

# Conclusion

