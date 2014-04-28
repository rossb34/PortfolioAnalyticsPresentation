---
title       : Complex Portfolio Optimization with PortfolioAnalytics
subtitle    : R/Finance 2014
author      : Ross Bennett
date        : May 16, 2014
framework   : io2012 # {io2012, html5slides, shower, dzslides, ...}
ext_widgets : {rCharts: libraries/nvd3}
widgets     : mathjax
mode        : selfcontained
---





## Overview
* Discuss Portfolio Optimization
* Introduce PortfolioAnalytics
* Demonstrate PortfolioAnalytics with Examples

<!---
Discuss Portfolio Optimization
- Some background and theory of portfolio theory
- challenges
Introduce PortfolioAnalytics
- What PortfolioAnalytics does and the problems it solves
Demonstrate PortfolioAnalytics with Examples
- Brief overview of the examples I will be giving
-->

---

## Modern Portfolio Theory
"Modern" Portfolio Theory (MPT) was introduced by Harry Markowitz in 1952.

In general, MPT states that an investor's objective is to maximize portfolio expected return for a given amount of risk.

General Objectives

* Maximize a measure of gain per unit measure of risk
* Minimize a measure of risk

How do we define risk? What about more complex objectives?

<!---
Several approaches follow the Markowitz approach using mean return as a measure of gain and standard deviation of returns as a measure of risk.
-->

---

## Portfolio Optimization Objectives
* Minimize Risk
    * Volatility
    * Tail Loss (VaR, ES)
    * Other Downside Risk Measure
* Maximize Risk Adjusted Return
    * Sharpe Ratio, Modified Sharpe Ratio
    * Several Others
* Risk Budgets
    * Equal Component Contribution to Risk (i.e. Risk Parity)
    * Limits on Component Contribution
* Maximize a Utility Function
    * Quadratic, CRRA, CARA, etc.

<!---
The challenge here is knowing what solver to use and the capabilities/limits of the chosen solver. Talk about pros/cons of closed-form solvers vs. global solvers and what objectives can be solved. 
-->

---

## PortfolioAnalytics Overview
PortfolioAnalytics is an R package designed to provide numerical solutions and visualizations for portfolio optimization problems with complex constraints and objectives.

* Support for multiple constraint and objective types
* An objective function can be any valid R function
* Modular constraints and objectives
* Support for user defined moment functions
* Visualizations
* Solver agnostic
* Support for parallel computing

<!---
The key points to make here are:
- Flexibility
  - The multiple types and modularity of constraints and objectives allows us to add, remove, combine, etc. multiple constraint and objective types very easily.
  - Define an objective as any valid R function
  - Define a function to compute the moments (sample, robust, shrinkage, factor model, GARCH model, etc.)
  - Estimation error is a significant concern with optimization. Having the ability to test different models with different parameters is critical.
- PortfolioAnalytics comes "pre-built" with several constraint types.
- Visualization helps to build intuition about the problem and understand the feasible space of portfolios
- Periodic rebalancing and analyzing out of sample performance will help refine objectives and constraints
-->

---

## Support Multiple Solvers
Linear and Quadratic Programming Solvers

* R Optimization Infrastructure (ROI)
    * GLPK (Rglpk)
    * Symphony (Rsymphony)
    * Quadprog (quadprog)

Global (stochastic or continuous solvers)

* Random Portfolios
* Differential Evolution (DEoptim)
* Particle Swarm Optimization (pso)
* Generalized Simulated Annealing (GenSA)

<!---
Brief explanation of each solver and what optimization problems (constraints and objectives) are supported
-->

---

## Random Portfolios
PortfolioAnalytics has three methods to generate random portfolios.

1. The **sample** method to generate random portfolios is based on an idea by Pat Burns.
2. The **simplex** method to generate random portfolios is based on a paper by W. T. Shaw.
3. The **grid** method to generate random portfolios is based on the `gridSearch` function in the NMOF package.

<!---
Random portfolios allow one to generate an arbitray number of portfolios based on given constraints. Will cover the edges as well as evenly cover the interior of the feasible space. 

The sample method to generate random portfolios is based on an idea by Pat Burns. This is the most flexible method, but also the slowest, and can generate portfolios to satisfy leverage, box, group, and position limit constraints.

The simplex method to generate random portfolios is based on a paper by W. T. Shaw. The simplex method is useful to generate random portfolios with the full investment constraint, where the sum of the weights is equal to 1, and min box constraints. Values for min_sum and max_sum of the leverage constraint will be ignored, the sum of weights will equal 1. All other constraints such as the box constraint max, group and position limit constraints will be handled by elimination. If the constraints are very restrictive, this may result in very few feasible portfolios remaining. Another key point to note is that the solution may not be along the vertexes depending on the objective. For example, a risk budget objective will likely place the portfolio somewhere on the interior.

The grid method to generate random portfolios is based on the gridSearch function in NMOF package. The grid search method only satisfies the min and max box constraints. The min_sum and max_sum leverage constraint will likely be violated and the weights in the random portfolios should be normalized. Normalization may cause the box constraints to be violated and will be penalized in constrained_objective.
-->

---

## Comparison of Random Portfolio Methods
![](figures/rp_plot.png)

<!---
This chart is a prime candidate for an interactive viz
-->

---

## Random Portfolios: Simplex Method
![](figures/fev_plot.png)

---

## Workflow
![](figures/workflow.png)

<!---
Describe each function:
- portfolio.spec
- add.constraint
- add.objective
- optimize.portfolio and optimize.portfolio.rebalancing
Just give a general description of the functions to analyze results
-->

---

## Workflow: Specify Portfolio

```r
args(portfolio.spec)
```

```
## function (assets = NULL, category_labels = NULL, weight_seq = NULL, 
##     message = FALSE) 
## NULL
```


---

## Workflow: Add Constraints

```r
args(add.constraint)
```

```
## function (portfolio, type, enabled = TRUE, message = FALSE, ..., 
##     indexnum = NULL) 
## NULL
```


Supported Constraint Types

* Sum of Weights
* Box
* Group
* Turnover
* Diversification
* Position Limit
* Return
* Factor Exposure

---

## Workflow: Add Objectives

```r
args(add.objective)
```

```
## function (portfolio, constraints = NULL, type, name, arguments = NULL, 
##     enabled = TRUE, ..., indexnum = NULL) 
## NULL
```


Supported Objective types

* Return
* Risk
* Risk Budget
* Weight Concentration

---

## Workflow: Run Optimization

```r
args(optimize.portfolio)
```

```
## function (R, portfolio = NULL, constraints = NULL, objectives = NULL, 
##     optimize_method = c("DEoptim", "random", "ROI", "pso", "GenSA"), 
##     search_size = 20000, trace = FALSE, ..., rp = NULL, momentFUN = "set.portfolio.moments", 
##     message = FALSE) 
## NULL
```

```r
args(optimize.portfolio.rebalancing)
```

```
## function (R, portfolio = NULL, constraints = NULL, objectives = NULL, 
##     optimize_method = c("DEoptim", "random", "ROI"), search_size = 20000, 
##     trace = FALSE, ..., rp = NULL, rebalance_on = NULL, training_period = NULL, 
##     trailing_periods = NULL) 
## NULL
```


Supported Optimization Methods

* ROI
* random
* DEoptim
* pso
* GenSA

---

## Workflow: Analyze Results

*** =left

* plot
* chart.Concentration
* chart.EfficientFrontier
* chart.RiskBudget
* chart.RiskReward
* chart.Weights

***=right

* extractObjectiveMeasures
* extractStats
* extractWeights

<!---
I'd like to make this a two column slide if possible. Might have to try slidify.
-->

---

## Example 1: Data Setup
Here we will look at portfolio optimization in the context of stocks.

* Selection of large cap, mid cap, and small cap stocks from CRSP data
* 15 Large Cap
* 15 Mid Cap
* 5 Small Cap


```r
equity.data <- cbind(largecap_weekly[,1:15], 
                     midcap_weekly[,1:15], 
                     smallcap_weekly[,1:5])
```


---

## Distribution of Monthly Returns
![](figures/equity_box.png)

---

## Minimum Variance Portfolio
Here we consider a portfolio of stocks. Our objective is to minimize portfolio variance subect to full investment and box constraints. We will use out of sample backtesting to compare the sample covariance matrix estimate and a Ledoit-Wolf shinkage estimate.

$$
\min_{w} w^{T} \Sigma w
$$

<!---
Demonstrate a custom moments function to compare a sample covariance matrix estimate and a Ledoit-Wolf shrinkage covariance matrix estimate. An alternative is a robust (MCD, MVE, etc.) estimate, DCC GARCH model, factor model, etc.
-->

---

## Specify Portfolio

```r
# Specify an initial portfolio
stocks <- colnames(equity.data)
portf.init <- portfolio.spec(stocks)

# Add full investment constraint such that the weights sum to 1
portf.minvar <- add.constraint(portf.init, type = "full_investment")

# Add box constraint such that no asset can have a weight of greater than
# 45% or less than 1%
portf.minvar <- add.constraint(portf.minvar, type = "box", min = 0.01, max = 0.45)

# Add objective to minimize portfolio variance
portf.minvar <- add.objective(portf.minvar, type = "risk", name = "var")
```


<!---
Talk a little about adding constraints and objectives
-->

---

## Ledoit-Wolf Shrinkage Estimate
The default function for `momentFUN` is `set.portfolio.moments`. We need to write our own function to estimate the covariance matrix.

```r
# Function to estimate covariance matrix via Ledoit-Wolf shrinkage
lw.sigma <- function(R, ...) {
    out <- list()
    out$sigma <- lwShrink(R)$cov
    return(out)
}
```


---

## Run Optimization

```r
# Backtest using sample covariance matrix estimate
opt.minVarSample <- optimize.portfolio.rebalancing(equity.data, portf.minvar, 
                                                   optimize_method="ROI", 
                                                   rebalance_on="quarters", 
                                                   training_period=400, 
                                                   trailing_periods=250)

# Backtest using Ledoit-Wolf shrinkage covariance matrix estimate
opt.minVarLW <- optimize.portfolio.rebalancing(equity.data, portf.minvar, 
                                               optimize_method="ROI", 
                                               momentFUN=lw.sigma,
                                               rebalance_on="quarters", 
                                               training_period=400, 
                                               trailing_periods=250)
```


<!---
Explain each of the rebalancing parameters
-->

---

## Chart Weights Through Time

```r
chart.Weights(opt.minVarSample, main = "minVarSample Weights", legend.loc = NULL)
chart.Weights(opt.minVarLW, main = "minVarLW Weights", legend.loc = NULL)
```

![](figures/weights_minVarSample.png)
![](figures/weights_minVarLW.png)

---

## Chart Weights Through Time

<div id = 'chart49e738a3258' class = 'rChart nvd3'></div>
<script type='text/javascript'>
 $(document).ready(function(){
      drawchart49e738a3258()
    });
    function drawchart49e738a3258(){  
      var opts = {
 "dom": "chart49e738a3258",
"width":    800,
"height":    400,
"x": "date",
"y": "weight",
"group": "stock",
"type": "stackedAreaChart",
"id": "chart49e738a3258" 
},
        data = [
 {
 "date":          12689,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "ORCL",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "MSFT",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "HON",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "EMC",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "DELL",
"weight": 0.01503241146447 
},
{
 "date":          13963,
"stock": "DELL",
"weight": 0.01449005070478 
},
{
 "date":          14054,
"stock": "DELL",
"weight": 0.02279248371229 
},
{
 "date":          14152,
"stock": "DELL",
"weight": 0.03498017199176 
},
{
 "date":          14243,
"stock": "DELL",
"weight": 0.01295608578269 
},
{
 "date":          14334,
"stock": "DELL",
"weight": 0.02162381645153 
},
{
 "date":          14425,
"stock": "DELL",
"weight": 0.01501908006339 
},
{
 "date":          14516,
"stock": "DELL",
"weight": 0.0147925215602 
},
{
 "date":          14607,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "DELL",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "KO",
"weight": 0.05677116459124 
},
{
 "date":          12780,
"stock": "KO",
"weight": 0.08497738473103 
},
{
 "date":          12871,
"stock": "KO",
"weight": 0.1203868549117 
},
{
 "date":          12962,
"stock": "KO",
"weight": 0.1096676789066 
},
{
 "date":          13053,
"stock": "KO",
"weight": 0.1071281364332 
},
{
 "date":          13144,
"stock": "KO",
"weight": 0.1304080363009 
},
{
 "date":          13235,
"stock": "KO",
"weight": 0.1219294306757 
},
{
 "date":          13326,
"stock": "KO",
"weight": 0.143320818191 
},
{
 "date":          13417,
"stock": "KO",
"weight": 0.1308038639855 
},
{
 "date":          13508,
"stock": "KO",
"weight": 0.1241687739741 
},
{
 "date":          13599,
"stock": "KO",
"weight": 0.1287702651563 
},
{
 "date":          13690,
"stock": "KO",
"weight": 0.0755273682945 
},
{
 "date":          13781,
"stock": "KO",
"weight": 0.07323915155307 
},
{
 "date":          13872,
"stock": "KO",
"weight": 0.1259525752134 
},
{
 "date":          13963,
"stock": "KO",
"weight": 0.1225258083044 
},
{
 "date":          14054,
"stock": "KO",
"weight": 0.0740363751382 
},
{
 "date":          14152,
"stock": "KO",
"weight": 0.03631727094922 
},
{
 "date":          14243,
"stock": "KO",
"weight": 0.09918656895077 
},
{
 "date":          14334,
"stock": "KO",
"weight": 0.07184875555677 
},
{
 "date":          14425,
"stock": "KO",
"weight": 0.1248734414283 
},
{
 "date":          14516,
"stock": "KO",
"weight": 0.1369094116794 
},
{
 "date":          14607,
"stock": "KO",
"weight": 0.113905963648 
},
{
 "date":          14698,
"stock": "KO",
"weight": 0.1260289329595 
},
{
 "date":          14789,
"stock": "KO",
"weight": 0.131599265834 
},
{
 "date":          14880,
"stock": "KO",
"weight": 0.1275762760037 
},
{
 "date":          14971,
"stock": "KO",
"weight": 0.1312812309523 
},
{
 "date":          12689,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "DD",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "XOM",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "XOM",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "XOM",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "XOM",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "XOM",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "XOM",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "XOM",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "XOM",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "XOM",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "XOM",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "XOM",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "XOM",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "XOM",
"weight": 0.01472321176072 
},
{
 "date":          13872,
"stock": "XOM",
"weight": 0.02309536379927 
},
{
 "date":          13963,
"stock": "XOM",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "XOM",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "XOM",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "XOM",
"weight": 0.09439347564634 
},
{
 "date":          14334,
"stock": "XOM",
"weight": 0.1051989064631 
},
{
 "date":          14425,
"stock": "XOM",
"weight": 0.1040610379877 
},
{
 "date":          14516,
"stock": "XOM",
"weight": 0.06824241705926 
},
{
 "date":          14607,
"stock": "XOM",
"weight": 0.07061257142104 
},
{
 "date":          14698,
"stock": "XOM",
"weight": 0.06925398193511 
},
{
 "date":          14789,
"stock": "XOM",
"weight": 0.05617208341663 
},
{
 "date":          14880,
"stock": "XOM",
"weight": 0.05446482758594 
},
{
 "date":          14971,
"stock": "XOM",
"weight": 0.05225462074595 
},
{
 "date":          12689,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "GE",
"weight": 0.04323824455527 
},
{
 "date":          14054,
"stock": "GE",
"weight": 0.02163016514387 
},
{
 "date":          14152,
"stock": "GE",
"weight": 0.01033276091845 
},
{
 "date":          14243,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "GE",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "IBM",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "PEP",
"weight": 0.09796038199808 
},
{
 "date":          12780,
"stock": "PEP",
"weight": 0.0848751296314 
},
{
 "date":          12871,
"stock": "PEP",
"weight": 0.104946406016 
},
{
 "date":          12962,
"stock": "PEP",
"weight": 0.09518680050938 
},
{
 "date":          13053,
"stock": "PEP",
"weight": 0.09311278038033 
},
{
 "date":          13144,
"stock": "PEP",
"weight": 0.07740914580197 
},
{
 "date":          13235,
"stock": "PEP",
"weight": 0.07336968899725 
},
{
 "date":          13326,
"stock": "PEP",
"weight": 0.07855879710886 
},
{
 "date":          13417,
"stock": "PEP",
"weight": 0.05015547723109 
},
{
 "date":          13508,
"stock": "PEP",
"weight": 0.06143496069471 
},
{
 "date":          13599,
"stock": "PEP",
"weight": 0.06418189907325 
},
{
 "date":          13690,
"stock": "PEP",
"weight": 0.2084546698433 
},
{
 "date":          13781,
"stock": "PEP",
"weight": 0.2484276723653 
},
{
 "date":          13872,
"stock": "PEP",
"weight": 0.2980036242918 
},
{
 "date":          13963,
"stock": "PEP",
"weight": 0.2879374219641 
},
{
 "date":          14054,
"stock": "PEP",
"weight": 0.3144747444691 
},
{
 "date":          14152,
"stock": "PEP",
"weight": 0.3576597306006 
},
{
 "date":          14243,
"stock": "PEP",
"weight": 0.2382822165754 
},
{
 "date":          14334,
"stock": "PEP",
"weight": 0.2989317712145 
},
{
 "date":          14425,
"stock": "PEP",
"weight":  0.23929485612 
},
{
 "date":          14516,
"stock": "PEP",
"weight": 0.2342328252326 
},
{
 "date":          14607,
"stock": "PEP",
"weight": 0.2459572333964 
},
{
 "date":          14698,
"stock": "PEP",
"weight": 0.2307437662173 
},
{
 "date":          14789,
"stock": "PEP",
"weight": 0.2228552556883 
},
{
 "date":          14880,
"stock": "PEP",
"weight": 0.2254756787808 
},
{
 "date":          14971,
"stock": "PEP",
"weight": 0.2327590931896 
},
{
 "date":          12689,
"stock": "MO",
"weight": 0.04814097074433 
},
{
 "date":          12780,
"stock": "MO",
"weight": 0.03001143792041 
},
{
 "date":          12871,
"stock": "MO",
"weight": 0.03201212532064 
},
{
 "date":          12962,
"stock": "MO",
"weight": 0.04289355470324 
},
{
 "date":          13053,
"stock": "MO",
"weight": 0.05437268089988 
},
{
 "date":          13144,
"stock": "MO",
"weight": 0.05576708085393 
},
{
 "date":          13235,
"stock": "MO",
"weight": 0.06368448484706 
},
{
 "date":          13326,
"stock": "MO",
"weight": 0.06568504571278 
},
{
 "date":          13417,
"stock": "MO",
"weight": 0.06198774253984 
},
{
 "date":          13508,
"stock": "MO",
"weight": 0.06045399031485 
},
{
 "date":          13599,
"stock": "MO",
"weight": 0.05177742891272 
},
{
 "date":          13690,
"stock": "MO",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "MO",
"weight": 0.02646842994626 
},
{
 "date":          13872,
"stock": "MO",
"weight": 0.0247874728887 
},
{
 "date":          13963,
"stock": "MO",
"weight": 0.04648986929569 
},
{
 "date":          14054,
"stock": "MO",
"weight": 0.03736217877452 
},
{
 "date":          14152,
"stock": "MO",
"weight": 0.02065477471119 
},
{
 "date":          14243,
"stock": "MO",
"weight": 0.118512037456 
},
{
 "date":          14334,
"stock": "MO",
"weight": 0.1917459628396 
},
{
 "date":          14425,
"stock": "MO",
"weight": 0.2022657354236 
},
{
 "date":          14516,
"stock": "MO",
"weight": 0.235392434728 
},
{
 "date":          14607,
"stock": "MO",
"weight": 0.2534299526118 
},
{
 "date":          14698,
"stock": "MO",
"weight": 0.2559541651161 
},
{
 "date":          14789,
"stock": "MO",
"weight": 0.2663984414556 
},
{
 "date":          14880,
"stock": "MO",
"weight": 0.2654341535942 
},
{
 "date":          14971,
"stock": "MO",
"weight": 0.2631446751809 
},
{
 "date":          12689,
"stock": "COP",
"weight": 0.083424407532 
},
{
 "date":          12780,
"stock": "COP",
"weight": 0.03540558833105 
},
{
 "date":          12871,
"stock": "COP",
"weight": 0.1035921243592 
},
{
 "date":          12962,
"stock": "COP",
"weight": 0.1044961398382 
},
{
 "date":          13053,
"stock": "COP",
"weight": 0.07771976038599 
},
{
 "date":          13144,
"stock": "COP",
"weight": 0.06206758411869 
},
{
 "date":          13235,
"stock": "COP",
"weight": 0.07373434069398 
},
{
 "date":          13326,
"stock": "COP",
"weight": 0.06386890630108 
},
{
 "date":          13417,
"stock": "COP",
"weight": 0.06593678520582 
},
{
 "date":          13508,
"stock": "COP",
"weight": 0.06168853546862 
},
{
 "date":          13599,
"stock": "COP",
"weight": 0.06887904679064 
},
{
 "date":          13690,
"stock": "COP",
"weight": 0.05463342463008 
},
{
 "date":          13781,
"stock": "COP",
"weight": 0.0268290681177 
},
{
 "date":          13872,
"stock": "COP",
"weight": 0.02835803263486 
},
{
 "date":          13963,
"stock": "COP",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "COP",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "COP",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "COP",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "COP",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "COP",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "COP",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "COP",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "COP",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "COP",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "COP",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "COP",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "AMGN",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "AMGN",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "AMGN",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "AMGN",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "AMGN",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "AMGN",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "AMGN",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "AMGN",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "AMGN",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "AMGN",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "AMGN",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "AMGN",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "AMGN",
"weight": 0.03494781195121 
},
{
 "date":          13872,
"stock": "AMGN",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "AMGN",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "AMGN",
"weight": 0.02958501363921 
},
{
 "date":          14152,
"stock": "AMGN",
"weight": 0.03145655686124 
},
{
 "date":          14243,
"stock": "AMGN",
"weight": 0.02070593264746 
},
{
 "date":          14334,
"stock": "AMGN",
"weight": 0.02065078747452 
},
{
 "date":          14425,
"stock": "AMGN",
"weight": 0.02448584897708 
},
{
 "date":          14516,
"stock": "AMGN",
"weight": 0.02043038974062 
},
{
 "date":          14607,
"stock": "AMGN",
"weight": 0.01609427892282 
},
{
 "date":          14698,
"stock": "AMGN",
"weight": 0.018019153772 
},
{
 "date":          14789,
"stock": "AMGN",
"weight": 0.02297495360543 
},
{
 "date":          14880,
"stock": "AMGN",
"weight": 0.02208160215528 
},
{
 "date":          14971,
"stock": "AMGN",
"weight": 0.02056037993116 
},
{
 "date":          12689,
"stock": "CVX",
"weight": 0.08410619399406 
},
{
 "date":          12780,
"stock": "CVX",
"weight": 0.1229695322009 
},
{
 "date":          12871,
"stock": "CVX",
"weight": 0.01126358051852 
},
{
 "date":          12962,
"stock": "CVX",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "CVX",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "CVX",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "CVX",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "CVX",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "CVX",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "CVX",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "CVX",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "CVX",
"weight": 0.02102965626705 
},
{
 "date":          13781,
"stock": "CVX",
"weight": 0.07238278871078 
},
{
 "date":          13872,
"stock": "CVX",
"weight": 0.05861317034639 
},
{
 "date":          13963,
"stock": "CVX",
"weight": 0.06097080394255 
},
{
 "date":          14054,
"stock": "CVX",
"weight": 0.06169062435752 
},
{
 "date":          14152,
"stock": "CVX",
"weight": 0.07475986444108 
},
{
 "date":          14243,
"stock": "CVX",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "CVX",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "CVX",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "CVX",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "CVX",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "CVX",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "CVX",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "CVX",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "CVX",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "TROW",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "TECD",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "FO",
"weight": 0.04190480679653 
},
{
 "date":          12780,
"stock": "FO",
"weight": 0.05136331818314 
},
{
 "date":          12871,
"stock": "FO",
"weight": 0.08871427381241 
},
{
 "date":          12962,
"stock": "FO",
"weight": 0.09690282901717 
},
{
 "date":          13053,
"stock": "FO",
"weight": 0.1253087126105 
},
{
 "date":          13144,
"stock": "FO",
"weight": 0.1050514108237 
},
{
 "date":          13235,
"stock": "FO",
"weight": 0.1053788883738 
},
{
 "date":          13326,
"stock": "FO",
"weight": 0.08897219103557 
},
{
 "date":          13417,
"stock": "FO",
"weight": 0.09693357869416 
},
{
 "date":          13508,
"stock": "FO",
"weight": 0.101102777532 
},
{
 "date":          13599,
"stock": "FO",
"weight": 0.09351727426068 
},
{
 "date":          13690,
"stock": "FO",
"weight": 0.07405624237916 
},
{
 "date":          13781,
"stock": "FO",
"weight": 0.01132373461694 
},
{
 "date":          13872,
"stock": "FO",
"weight": 0.0162339498082 
},
{
 "date":          13963,
"stock": "FO",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "FO",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "FO",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "FO",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "FO",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "FO",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "FO",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "FO",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "FO",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "FO",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "FO",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "FO",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "TCB",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "FISV",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "CDNS",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "XRAY",
"weight": 0.09975566555437 
},
{
 "date":          12780,
"stock": "XRAY",
"weight": 0.07700405466497 
},
{
 "date":          12871,
"stock": "XRAY",
"weight": 0.08055400263437 
},
{
 "date":          12962,
"stock": "XRAY",
"weight": 0.07624095602137 
},
{
 "date":          13053,
"stock": "XRAY",
"weight": 0.06870210251847 
},
{
 "date":          13144,
"stock": "XRAY",
"weight": 0.06527192109021 
},
{
 "date":          13235,
"stock": "XRAY",
"weight": 0.06327210323596 
},
{
 "date":          13326,
"stock": "XRAY",
"weight": 0.06741290214316 
},
{
 "date":          13417,
"stock": "XRAY",
"weight": 0.1023108000064 
},
{
 "date":          13508,
"stock": "XRAY",
"weight": 0.08999917360498 
},
{
 "date":          13599,
"stock": "XRAY",
"weight": 0.08882781886791 
},
{
 "date":          13690,
"stock": "XRAY",
"weight": 0.0681593237495 
},
{
 "date":          13781,
"stock": "XRAY",
"weight": 0.0422987868314 
},
{
 "date":          13872,
"stock": "XRAY",
"weight": 0.02739613259405 
},
{
 "date":          13963,
"stock": "XRAY",
"weight": 0.03178546265803 
},
{
 "date":          14054,
"stock": "XRAY",
"weight": 0.03129177624332 
},
{
 "date":          14152,
"stock": "XRAY",
"weight": 0.02590357664168 
},
{
 "date":          14243,
"stock": "XRAY",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "XRAY",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "XRAY",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "XRAY",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "XRAY",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "XRAY",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "XRAY",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "XRAY",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "XRAY",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "FAST",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "DTE",
"weight": 0.1816938290508 
},
{
 "date":          12780,
"stock": "DTE",
"weight": 0.2111002517203 
},
{
 "date":          12871,
"stock": "DTE",
"weight": 0.1846319043165 
},
{
 "date":          12962,
"stock": "DTE",
"weight": 0.1893508080557 
},
{
 "date":          13053,
"stock": "DTE",
"weight": 0.1895652549755 
},
{
 "date":          13144,
"stock": "DTE",
"weight": 0.2240248210106 
},
{
 "date":          13235,
"stock": "DTE",
"weight": 0.2152617374576 
},
{
 "date":          13326,
"stock": "DTE",
"weight": 0.1980460584994 
},
{
 "date":          13417,
"stock": "DTE",
"weight": 0.1798938989997 
},
{
 "date":          13508,
"stock": "DTE",
"weight": 0.1800654556638 
},
{
 "date":          13599,
"stock": "DTE",
"weight": 0.1607124266192 
},
{
 "date":          13690,
"stock": "DTE",
"weight": 0.1865517955673 
},
{
 "date":          13781,
"stock": "DTE",
"weight": 0.1166799423159 
},
{
 "date":          13872,
"stock": "DTE",
"weight": 0.102193802987 
},
{
 "date":          13963,
"stock": "DTE",
"weight": 0.1225623385751 
},
{
 "date":          14054,
"stock": "DTE",
"weight": 0.1378727557877 
},
{
 "date":          14152,
"stock": "DTE",
"weight": 0.1285601346566 
},
{
 "date":          14243,
"stock": "DTE",
"weight": 0.1224114426103 
},
{
 "date":          14334,
"stock": "DTE",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "DTE",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "DTE",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "DTE",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "DTE",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "DTE",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "DTE",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "DTE",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "ETN",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "NVLS",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "GR",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "ITT",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "VMC",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "ASBC",
"weight": 0.04119365352739 
},
{
 "date":          13781,
"stock": "ASBC",
"weight": 0.09267940183076 
},
{
 "date":          13872,
"stock": "ASBC",
"weight": 0.0403334639719 
},
{
 "date":          13963,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "ASBC",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "PLXS",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "BWINB",
"weight": 0.02518602233558 
},
{
 "date":          12780,
"stock": "BWINB",
"weight": 0.04107409735676 
},
{
 "date":          12871,
"stock": "BWINB",
"weight": 0.01389872811072 
},
{
 "date":          12962,
"stock": "BWINB",
"weight": 0.01526123294828 
},
{
 "date":          13053,
"stock": "BWINB",
"weight": 0.0140905717961 
},
{
 "date":          13144,
"stock": "BWINB",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "BWINB",
"weight": 0.01336932571873 
},
{
 "date":          13326,
"stock": "BWINB",
"weight": 0.02413528100816 
},
{
 "date":          13417,
"stock": "BWINB",
"weight": 0.04197785333751 
},
{
 "date":          13508,
"stock": "BWINB",
"weight": 0.05108633274701 
},
{
 "date":          13599,
"stock": "BWINB",
"weight": 0.07333384031933 
},
{
 "date":          13690,
"stock": "BWINB",
"weight": 0.01039386574167 
},
{
 "date":          13781,
"stock": "BWINB",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "BWINB",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "BWINB",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "BWINB",
"weight": 0.01926388273421 
},
{
 "date":          14152,
"stock": "BWINB",
"weight": 0.02937515822817 
},
{
 "date":          14243,
"stock": "BWINB",
"weight": 0.02355224033102 
},
{
 "date":          14334,
"stock": "BWINB",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "BWINB",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "BWINB",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "BWINB",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "BWINB",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "BWINB",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "BWINB",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "BWINB",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "HGIC",
"weight": 0.01496746188003 
},
{
 "date":          14971,
"stock": "HGIC",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          12780,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          12871,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "WTS",
"weight":           0.01 
},
{
 "date":          12689,
"stock": "HTLD",
"weight": 0.03105655740307 
},
{
 "date":          12780,
"stock": "HTLD",
"weight": 0.01121920526002 
},
{
 "date":          12871,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          12962,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          13053,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          13144,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          13235,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          13326,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          13417,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          13508,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          13599,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          13690,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          13781,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          13872,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          13963,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          14054,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          14152,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          14243,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          14334,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          14425,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          14516,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          14607,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          14698,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          14789,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          14880,
"stock": "HTLD",
"weight":           0.01 
},
{
 "date":          14971,
"stock": "HTLD",
"weight":           0.01 
} 
]
  
      if(!(opts.type==="pieChart" || opts.type==="sparklinePlus" || opts.type==="bulletChart")) {
        var data = d3.nest()
          .key(function(d){
            //return opts.group === undefined ? 'main' : d[opts.group]
            //instead of main would think a better default is opts.x
            return opts.group === undefined ? opts.y : d[opts.group];
          })
          .entries(data);
      }
      
      if (opts.disabled != undefined){
        data.map(function(d, i){
          d.disabled = opts.disabled[i]
        })
      }
      
      nv.addGraph(function() {
        var chart = nv.models[opts.type]()
          .width(opts.width)
          .height(opts.height)
          
        if (opts.type != "bulletChart"){
          chart
            .x(function(d) { return d[opts.x] })
            .y(function(d) { return d[opts.y] })
        }
          
         
        
          
        chart.xAxis
  .tickFormat(function(d){
    return d3.time.format('%b %Y')(new Date(d * 24 * 60 * 60 * 1000))
    })

        
        
        chart.yAxis
  .tickFormat(function(d){
    return d3.format('0.2%')(d)
    })
      
       d3.select("#" + opts.id)
        .append('svg')
        .datum(data)
        .transition().duration(500)
        .call(chart);

       nv.utils.windowResize(chart.update);
       return chart;
      });
    };
</script>


---

## Returns
Compute the portfolio rebalancing returns and chart the performance.

```r
ret.minVarSample <- summary(opt.minVarSample)$portfolio_returns
ret.minVarRobust <- summary(opt.minVarLW)$portfolio_returns
ret.minVar <- cbind(ret.minVarSample, ret.minVarRobust)
colnames(ret.minVar) <- c("Sample", "LW")
charts.PerformanceSummary(ret.minVar)
```

![](figures/ret_minVar.png)

---

## Example 2: Market Neutral Portfolio
Here we consider a portfolio of stocks. Our objective is to maximize portfolio return with a target of 0.0015 and minimize portfolio StdDev with a target of 0.02 subject to dollar neutral, beta, box, and position limit constraints. We will use the same data considered in Example 1.

<!---
This involves combining several constraints. This is an MIQPQC problem that can't be solved by quadprog so we will use random portfolios.
-->

---

## Specify Portfolio: Constraints

```r
portf.init <- portfolio.spec(stocks)

# Add constraint such that the portfolio weights sum to 0*
portf.dn <- add.constraint(portf.init, type="weight_sum", 
                                  min_sum=-0.01, max_sum=0.01)

# Add box constraint such that no asset can have a weight of greater than
# 20% or less than -20%
portf.dn <- add.constraint(portf.dn, type="box", min=-0.2, max=0.2)

# Add constraint such that we have at most 20 positions
portf.dn <- add.constraint(portf.dn, type="position_limit", max_pos=20)

# Compute the betas of each stock
betas <- t(CAPM.beta(equity.data, market, Rf))

# Add constraint such that the portfolio beta is between -0.25 and 0.25
portf.dn <- add.constraint(portf.dn, type="factor_exposure", B=betas, 
                           lower=-0.25, upper=0.25)
```


<!---
explain the constraints
-->

---

## Specify Portfolio: Objectives

```r
# Add objective to maximize portfolio return with a target of 0.0015
portf.dn.StdDev <- add.objective(portf.dn, type="return", name="mean",
                                 target=0.0015)

# Add objective to minimize portfolio StdDev with a target of 0.02
portf.dn.StdDev <- add.objective(portf.dn.StdDev, type="risk", name="StdDev",
                                 target=0.02)
```


<!---
explain the objectives, specifically the target
-->

---

## Run Optimization

```r
# Generate random portfolios
rp <- random_portfolios(portf.dn, 10000, "sample", eliminate=TRUE)

# Run the optimization
opt.dn <- optimize.portfolio(equity.data, portf.dn.StdDev, 
                               optimize_method="random", rp=rp,
                               trace=TRUE)
```


<!---
generate a set of random portfolios and then pass directly to optimize.portfolio. Could just specify optimize_method = "random" and will automatically generate for you.
-->

---

## Plot Results

```r
plot(opt.dn, main="Dollar Neutral Portfolio", risk.col="StdDev", neighbors=10)
```

![](figures/opt_dn.png)

---

## Example 3: Data Setup
Here we will look at portfolio optimization in the context of portfolio of hedge funds.

* EDHEC-Risk Alternative Indexes monthly returns from 1/31/1997 to 1/31/2014

Relative Value | Directional
-------------- | -----------
Convertible Arbitrage (CA) | CTA Global (CTAG)
Equity Market Neutral (EMN) | Emerging Markets (EM)
Fixed Income Arbitrage (FIA) | Global Macro (GM)


```r
R <- edhec[,c("Convertible.Arbitrage", "Equity.Market.Neutral", 
              "Fixed.Income.Arbitrage", 
              "CTA.Global", "Emerging.Markets", "Global.Macro")]
# Abreviate column names for convenience and plotting
colnames(R) <- c("CA", "EMN", "FIA", "CTAG", "EM", "GM")
```


---

## Monthly Returns
![](figures/relative_barvar.png)
![](figures/directional_barvar.png)

---

## Distribution of Monthly Returns
![](figures/edhec_box.png)

---

## Minimum Expected Shortfall
Consider an allocation to hedge funds using the EDHEC-Risk Alternative Index as a proxy. This will be an extended example starting with an objective to minimize modified expected shortfall, then add risk budget percent contribution limit, and finally add equal risk contribution limit.

* Minimize Expected Shortfall
* Minimize Expected Shortfall with Risk Budget Limit
* Minimize Expected Shortfall with Equal Risk Contribution

Add risk budget objective to minimize concentration of percentage component contribution to risk. Concentration is defined as the Herfindahl Hirschman Index (HHI).

$$ \sum_{i=1}^n x_i^2 $$

<!---
comments
-->

---

## Specify Initial Portfolio

```r
# Specify an initial portfolio
funds <- colnames(R)
portf.init <- portfolio.spec(funds)

# Add constraint such that the weights sum to 1*
portf.init <- add.constraint(portf.init, type="weight_sum", 
                             min_sum=0.99, max_sum=1.01)

# Add box constraint such that no asset can have a weight of greater than
# 40% or less than 5%
portf.init <- add.constraint(portf.init, type="box", 
                             min=0.05, max=0.4)

# Add return objective with multiplier=0 such that the portfolio mean
# return is calculated, but does not impact optimization
portf.init <- add.objective(portf.init, type="return", 
                            name="mean", multiplier=0)
```


<!---
basic comments about setting up an initial portfolio
-->

---

## Add Objectives

```r
# Add objective to minimize expected shortfall
portf.minES <- add.objective(portf.init, type="risk", name="ES")

# Add objective to set upper bound on percentage component contribution
portf.minES.RB <- add.objective(portf.minES, type="risk_budget", 
                                name="ES", max_prisk=0.3)
# Relax box constraints
portf.minES.RB$constraints[[2]]$max <- rep(1,ncol(R))

# Add objective to minimize concentration of modified ES
# component contribution
portf.minES.EqRB <- add.objective(portf.minES, type="risk_budget", 
                                  name="ES", min_concentration=TRUE)
# Relax box constraints
portf.minES.EqRB <- add.constraint(portf.minES.EqRB, type="box", 
                                   min=0.05, max=1, indexnum=2)
```

<!---
Key points here are that we are creating 3 new portfolios by reusing the initial portfolio and we are relaxing the box constraints because we are no longer concerned with controlling weight concentration. We have limits on risk contribution.
-->

---

## Run Optimization

```r
# Combine the 3 portfolios
portf <- combine.portfolios(list(minES=portf.minES, 
                                 minES.RB=portf.minES.RB, 
                                 minES.EqRB=portf.minES.EqRB))

# Run the optimization
opt.minES <- optimize.portfolio(R, portf, optimize_method="DEoptim", 
                                search_size=5000, trace=TRUE, traceDE=0)
```


<!---
explain how portf is a list of portfolios and passed to optimize.portfolio
-->

---

## Plot in Risk-Return Space
![](figures/opt_minES.png)

---

## Chart Risk Budgets

```r
chart.RiskBudget(opt.minES[[2]], main="Risk Budget Limit", 
                 risk.type="percentage", neighbors=10)

chart.RiskBudget(opt.minES[[3]], main="Equal ES Component Contribution", 
                 risk.type="percentage", neighbors=10)
```

![](figures/rb_minES.png)
![](figures/eqrb_minES.png)

---

## Set Rebalancing Parameters and Run Backtest

```r
# Set rebalancing frequency
rebal.freq <- "quarters"

# Training Period
training <- 120

# Trailing Period
trailing <- 72

bt.opt.minES <- optimize.portfolio.rebalancing(R, portf,
                                               optimize_method="DEoptim", 
                                               rebalance_on=rebal.freq, 
                                               training_period=training, 
                                               trailing_periods=trailing,
                                               search_size=5000,
                                               traceDE=0)
```


---

## Min ES Risk Contributions and Weights Through Time
![](figures/risk_minES.png)
![](figures/weights_minES.png)

---

## Min ES Risk Budget Limit Risk Contributions and Weights Through Time
![](figures/risk_minESRB.png)
![](figures/weights_minESRB.png)

---

## Min ES Equal Component Contribution Risk Contributions and Weights Through Time
![](figures/risk_minESEqRB.png)
![](figures/weights_minESEqRB.png)

---

## Compute Returns and Chart Performance

```r
ret.bt.opt <- do.call(cbind, lapply(bt.opt.minES, 
                                    function(x) summary(x)$portfolio_returns))
colnames(ret.bt.opt) <- c("min ES", "min ES RB", "min ES Eq RB")
charts.PerformanceSummary(ret.bt.opt)
```

![](figures/ret_minES.png)

---

## Example 4: Maximize CRRA
Consider an allocation to hedge funds using the EDHEC-Risk Alternative Index as a proxy. Our objective to maximize the fourth order expansion of the Constant Relative Risk Aversion (CRRA) expected utility function as in the Boudt paper and Martinelli paper. We use the same data as Example 3.

$$
EU_{\lambda}(w) = - \frac{\lambda}{2} m_{(2)}(w) + 
\frac{\lambda (\lambda + 1)}{6} m_{(3)}(w) -
\frac{\lambda (\lambda + 1) (\lambda + 2)}{24} m_{(4)}(w)
$$

<!---
Demonstrate a custom moment function and a custom objective function.
-->

---

## Define a function to compute CRRA

```r
CRRA <- function(R, weights, lambda, sigma, m3, m4){
  weights <- matrix(weights, ncol=1)
  M2.w <- t(weights) %*% sigma %*% weights
  M3.w <- t(weights) %*% m3 %*% (weights %x% weights)
  M4.w <- t(weights) %*% m4 %*% (weights %x% weights %x% weights)
  term1 <- (1 / 2) * lambda * M2.w
  term2 <- (1 / 6) * lambda * (lambda + 1) * M3.w
  term3 <- (1 / 24) * lambda * (lambda + 1) * (lambda + 2) * M4.w
  out <- -term1 + term2 - term3
  out
}
```


<!---
The function arguments should have 'R' as the name of the returns and 'weights' as the name of the weights. 'R' and 'weights' are automatically matched, any other function arguments can be passed in through arguments in add.objective.
-->

---

## Specify Portfolio

```r
# Specify portfolio
portf.crra <- portfolio.spec(funds)

# Add constraint such that the weights sum to 1
portf.crra <- add.constraint(portf.crra, type="weight_sum", 
                             min_sum=0.99, max_sum=1.01)

# Add box constraint such that no asset can have a weight of greater than
# 40% or less than 5% 
portf.crra <- add.constraint(portf.crra, type="box", 
                             min=0.05, max=0.4)

# Add objective to maximize CRRA
portf.crra <- add.objective(portf.crra, type="return", 
                            name="CRRA", arguments=list(lambda=10))

# I just want these for plotting
# Set multiplier=0 so that it is calculated, but does not affect the optimization
portf.crra <- add.objective(portf.crra, type="return", name="mean", multiplier=0)
portf.crra <- add.objective(portf.crra, type="risk", name="ES", multiplier=0)
portf.crra <- add.objective(portf.crra, type="risk", name="StdDev", multiplier=0)
```


<!---
Focus on how CRRA is added as an objective
-->

---

## Run Optimization

```r
opt.crra <- optimize.portfolio(R, portf.crra, optimize_method="DEoptim", 
                                 search_size=5000, trace=TRUE, traceDE=0,
                                 momentFUN="crra.moments")
```





```r
opt.crra
```

```
## ***********************************
## PortfolioAnalytics Optimization
## ***********************************
## 
## Call:
## optimize.portfolio(R = R, portfolio = portf.crra, optimize_method = "DEoptim", 
##     search_size = 5000, trace = TRUE, traceDE = 0, momentFUN = "crra.moments")
## 
## Optimal Weights:
##     CA    EMN    FIA   CTAG     EM     GM 
## 0.0540 0.3960 0.3121 0.1284 0.0500 0.0500 
## 
## Objective Measures:
##       CRRA 
## -0.0004953 
## 
## 
##     mean 
## 0.005311 
## 
## 
##      ES 
## 0.02715 
## 
## 
##   StdDev 
## 0.009703
```



<!---
remember to specify a momentFUN to match the arguments in CRRA
-->

---

## Chart Results

```r
chart.RiskReward(opt.crra, risk.col = "ES")
chart.RiskReward(opt.crra, risk.col = "StdDev")
```


![](figures/crra_RR_ES.png)
![](figures/crra_RR_StdDev.png)

---

## Run Backtest and Compute Returns

```r
bt.opt.crra <- optimize.portfolio.rebalancing(R, portf.crra, 
                                              optimize_method="DEoptim",
                                              search_size=5000, trace=TRUE,
                                              traceDE=0,
                                              momentFUN="crra.moments",
                                              rebalance_on=rebal.freq, 
                                              training_period=training, 
                                              trailing_periods=trailing)

ret.crra <- summary(bt.opt.crra)$portfolio_returns
colnames(ret.crra) <- "CRRA"
```


<!---
Run optimization and extract the portfolio rebalancing returns from the summary method
-->

---

## Chart Performance

```r
charts.PerformanceSummary(cbind(ret.bt.opt, ret.crra), 
                          main="Optimization Performance")
```

![](figures/ret_crra.png)

---

## Conclusion
TODO

* Overview of what was covered
* Additional information and plans for PortfolioAnalytics

## Acknowledgements
Many thanks to

* Google: funding for Google Summer of Code (GSoC)
* GSoC Mentors: Brian Peterson, Peter Carl, Doug Martin, and Guy Yollin
* R/Finance Committee

<!---
- One of the best things about GSoC is the opportunity to work and interact with the mentors.
- Thank the GSoC mentors for offering help and guidance during the GSoC project and after as I continued to work on the PortfolioAnalytics package.
- R/Finance Committee for the conference and the opportunity to talk about PortfolioAnalytics.
- Google for funding the Google Summer of Code for PortfolioAnalytics and many other proposals for R
-->

---

## References and Useful Links

* [ROI](http://cran.r-project.org/web/packages/ROI/index.html)
* [DEoptim](http://cran.r-project.org/web/packages/DEoptim/index.html)
* [pso](http://cran.r-project.org/web/packages/pso/index.html)
* [GenSA](http://cran.r-project.org/web/packages/GenSA/index.html)
* [PerformanceAnalytics](http://cran.r-project.org/web/packages/PerformanceAnalytics/index.html)
* Pat Burns Random Portfolios
* W.T. Shaw Random Portfolios
* Martinelli paper
* Boudt paper
* [PortfolioAnalytics on R-Forge](https://r-forge.r-project.org/projects/returnanalytics/)
* [Shiny App](http://spark.rstudio.com/rossbennett3/PortfolioOptimization/)
