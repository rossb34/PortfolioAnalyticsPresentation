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

## Comparison of Random Portfolio Methods (Interactive!)

<div id = 'chart6897dbca1bc' class = 'rChart nvd3'></div>
<script type='text/javascript'>
 $(document).ready(function(){
      drawchart6897dbca1bc()
    });
    function drawchart6897dbca1bc(){  
      var opts = {
 "dom": "chart6897dbca1bc",
"width":    800,
"height":    400,
"x": "sd",
"y": "mean",
"group": "name",
"type": "scatterChart",
"id": "chart6897dbca1bc" 
},
        data = [
 {
 "name": "sample",
"mean": 0.005906097560976,
"sd": 0.01367140149733 
},
{
 "name": "sample",
"mean": 0.004952042926829,
"sd": 0.0121391923217 
},
{
 "name": "sample",
"mean": 0.005785585365854,
"sd": 0.009725331272808 
},
{
 "name": "sample",
"mean": 0.006578966829268,
"sd": 0.02241036603673 
},
{
 "name": "sample",
"mean": 0.005378606829268,
"sd": 0.01874483648959 
},
{
 "name": "sample",
"mean": 0.005778972682927,
"sd": 0.0108782295748 
},
{
 "name": "sample",
"mean": 0.006313540487805,
"sd": 0.01386759424652 
},
{
 "name": "sample",
"mean": 0.006953228292683,
"sd": 0.02952693708114 
},
{
 "name": "sample",
"mean": 0.006692132682927,
"sd": 0.02509257280399 
},
{
 "name": "sample",
"mean": 0.006520742439024,
"sd": 0.01816259800605 
},
{
 "name": "sample",
"mean": 0.006254571707317,
"sd": 0.01821548844088 
},
{
 "name": "sample",
"mean": 0.005272463414634,
"sd": 0.01116896426611 
},
{
 "name": "sample",
"mean": 0.00523147902439,
"sd": 0.01242878457001 
},
{
 "name": "sample",
"mean": 0.00682099804878,
"sd": 0.02450973068907 
},
{
 "name": "sample",
"mean": 0.00719788097561,
"sd": 0.03282755767797 
},
{
 "name": "sample",
"mean": 0.005298214634146,
"sd": 0.00847339786735 
},
{
 "name": "sample",
"mean": 0.006326096585366,
"sd": 0.01700692323435 
},
{
 "name": "sample",
"mean": 0.006277894634146,
"sd": 0.01467886428909 
},
{
 "name": "sample",
"mean": 0.00649556195122,
"sd": 0.02259715615913 
},
{
 "name": "sample",
"mean": 0.005589290731707,
"sd": 0.01418352633798 
},
{
 "name": "sample",
"mean": 0.005852112195122,
"sd": 0.01568459020402 
},
{
 "name": "sample",
"mean": 0.00501123804878,
"sd": 0.02006313727933 
},
{
 "name": "sample",
"mean": 0.005294270243902,
"sd": 0.011855910746 
},
{
 "name": "sample",
"mean": 0.005096590243902,
"sd": 0.01182773487389 
},
{
 "name": "sample",
"mean": 0.005613467317073,
"sd": 0.009556624634599 
},
{
 "name": "sample",
"mean": 0.006240882926829,
"sd": 0.01619294372859 
},
{
 "name": "sample",
"mean": 0.00587400195122,
"sd": 0.01890219963315 
},
{
 "name": "sample",
"mean": 0.00550708097561,
"sd": 0.008880437757906 
},
{
 "name": "sample",
"mean": 0.006267088780488,
"sd": 0.0146050313139 
},
{
 "name": "sample",
"mean": 0.006438124878049,
"sd": 0.01392439178671 
},
{
 "name": "sample",
"mean": 0.006682496585366,
"sd": 0.02028880513781 
},
{
 "name": "sample",
"mean": 0.00715808195122,
"sd": 0.0329085650396 
},
{
 "name": "sample",
"mean": 0.005579124878049,
"sd": 0.009647461518361 
},
{
 "name": "sample",
"mean": 0.006428412682927,
"sd": 0.01662041245521 
},
{
 "name": "sample",
"mean": 0.005591991219512,
"sd": 0.01365390314423 
},
{
 "name": "sample",
"mean": 0.006404642926829,
"sd": 0.01670066566959 
},
{
 "name": "sample",
"mean": 0.00537836195122,
"sd": 0.00854633450853 
},
{
 "name": "sample",
"mean": 0.005330766829268,
"sd": 0.01969854998764 
},
{
 "name": "sample",
"mean": 0.005970374634146,
"sd": 0.01201137296758 
},
{
 "name": "sample",
"mean": 0.006062707317073,
"sd": 0.01605463172072 
},
{
 "name": "sample",
"mean": 0.006107077073171,
"sd": 0.01328381734298 
},
{
 "name": "sample",
"mean": 0.006072703414634,
"sd": 0.01145415296009 
},
{
 "name": "sample",
"mean": 0.006261042926829,
"sd": 0.01538901642408 
},
{
 "name": "sample",
"mean": 0.005945571707317,
"sd": 0.01978484999119 
},
{
 "name": "sample",
"mean": 0.005487685853659,
"sd": 0.008752497185826 
},
{
 "name": "sample",
"mean": 0.005295255609756,
"sd": 0.01340297603915 
},
{
 "name": "sample",
"mean": 0.006165783414634,
"sd": 0.01624856419115 
},
{
 "name": "sample",
"mean": 0.005132846829268,
"sd": 0.01187471432643 
},
{
 "name": "sample",
"mean": 0.006186380487805,
"sd": 0.0172932322603 
},
{
 "name": "sample",
"mean": 0.006986191219512,
"sd": 0.03172016072677 
},
{
 "name": "sample",
"mean": 0.005419686829268,
"sd": 0.01152088658821 
},
{
 "name": "sample",
"mean": 0.006589589268293,
"sd": 0.0193693858443 
},
{
 "name": "sample",
"mean":     0.00642072,
"sd": 0.01478500679512 
},
{
 "name": "sample",
"mean": 0.006415765853659,
"sd": 0.01452722853369 
},
{
 "name": "sample",
"mean": 0.005307952195122,
"sd": 0.009024637276827 
},
{
 "name": "sample",
"mean": 0.006228396097561,
"sd": 0.01523926899766 
},
{
 "name": "sample",
"mean":     0.00616484,
"sd": 0.01690549647419 
},
{
 "name": "sample",
"mean": 0.006223008780488,
"sd": 0.02080779623729 
},
{
 "name": "sample",
"mean": 0.005441673170732,
"sd": 0.009952061780075 
},
{
 "name": "sample",
"mean": 0.006517383414634,
"sd": 0.02035913918417 
},
{
 "name": "sample",
"mean": 0.005073134634146,
"sd": 0.01999744514611 
},
{
 "name": "sample",
"mean": 0.005140216585366,
"sd": 0.0213402810645 
},
{
 "name": "sample",
"mean": 0.005244447804878,
"sd": 0.01227105771448 
},
{
 "name": "sample",
"mean": 0.006426748292683,
"sd": 0.01406459673092 
},
{
 "name": "sample",
"mean": 0.005225495609756,
"sd": 0.01974078852296 
},
{
 "name": "sample",
"mean": 0.00539227902439,
"sd": 0.00921873000533 
},
{
 "name": "sample",
"mean": 0.006557929756098,
"sd": 0.02307172919444 
},
{
 "name": "sample",
"mean": 0.006261420487805,
"sd": 0.01519406801918 
},
{
 "name": "sample",
"mean": 0.005096822439024,
"sd": 0.01124774640011 
},
{
 "name": "sample",
"mean": 0.006257098536585,
"sd": 0.01425292230855 
},
{
 "name": "sample",
"mean": 0.006605866341463,
"sd": 0.0238590346698 
},
{
 "name": "sample",
"mean": 0.006219757073171,
"sd": 0.01373873215263 
},
{
 "name": "sample",
"mean": 0.005140763902439,
"sd": 0.01541822161487 
},
{
 "name": "sample",
"mean": 0.005122498536585,
"sd": 0.0113540592731 
},
{
 "name": "sample",
"mean": 0.005598173658537,
"sd": 0.01154257309676 
},
{
 "name": "sample",
"mean": 0.006908660487805,
"sd": 0.02434781245441 
},
{
 "name": "sample",
"mean": 0.005262980487805,
"sd": 0.01512676741072 
},
{
 "name": "sample",
"mean": 0.005158912195122,
"sd": 0.01718220343051 
},
{
 "name": "sample",
"mean": 0.005158353170732,
"sd": 0.02133804359055 
},
{
 "name": "sample",
"mean": 0.006176025365854,
"sd": 0.01370745921149 
},
{
 "name": "sample",
"mean": 0.004993025365854,
"sd": 0.02284774061572 
},
{
 "name": "sample",
"mean": 0.005555501463415,
"sd": 0.009650557977155 
},
{
 "name": "sample",
"mean": 0.006433924878049,
"sd": 0.01506062963877 
},
{
 "name": "sample",
"mean": 0.005666737560976,
"sd": 0.01897385669451 
},
{
 "name": "sample",
"mean": 0.00525244195122,
"sd": 0.01307735126727 
},
{
 "name": "sample",
"mean": 0.005571384390244,
"sd": 0.00933496618254 
},
{
 "name": "sample",
"mean": 0.006064618536585,
"sd": 0.01665170717505 
},
{
 "name": "sample",
"mean": 0.006380899512195,
"sd": 0.01910049260498 
},
{
 "name": "sample",
"mean": 0.00634651804878,
"sd": 0.01460175970017 
},
{
 "name": "sample",
"mean": 0.005351251707317,
"sd": 0.01610961554617 
},
{
 "name": "sample",
"mean": 0.005024460487805,
"sd": 0.01497503268214 
},
{
 "name": "sample",
"mean": 0.005112809756098,
"sd": 0.0129688536448 
},
{
 "name": "sample",
"mean": 0.006133552195122,
"sd": 0.01421750049867 
},
{
 "name": "sample",
"mean": 0.005368965853659,
"sd": 0.008449596639741 
},
{
 "name": "sample",
"mean": 0.007086456585366,
"sd": 0.02850714439537 
},
{
 "name": "sample",
"mean": 0.006118105365854,
"sd": 0.01407691340509 
},
{
 "name": "sample",
"mean": 0.005327177560976,
"sd": 0.01899578711262 
},
{
 "name": "sample",
"mean": 0.005299991219512,
"sd": 0.008532499445851 
},
{
 "name": "sample",
"mean": 0.005688491707317,
"sd": 0.01002687678108 
},
{
 "name": "sample",
"mean": 0.006435292682927,
"sd": 0.01543509656521 
},
{
 "name": "sample",
"mean": 0.005695431219512,
"sd": 0.01629635298799 
},
{
 "name": "sample",
"mean": 0.005441018536585,
"sd": 0.01017505415652 
},
{
 "name": "sample",
"mean": 0.006788466341463,
"sd": 0.02803499650311 
},
{
 "name": "sample",
"mean": 0.005066487804878,
"sd": 0.01179720891679 
},
{
 "name": "sample",
"mean": 0.005591384390244,
"sd": 0.01130783188914 
},
{
 "name": "sample",
"mean": 0.00585268097561,
"sd": 0.01248099706495 
},
{
 "name": "sample",
"mean": 0.006475779512195,
"sd": 0.01576589183816 
},
{
 "name": "sample",
"mean": 0.006332535609756,
"sd": 0.01447445627181 
},
{
 "name": "sample",
"mean": 0.006556430243902,
"sd": 0.02076569760524 
},
{
 "name": "sample",
"mean": 0.006376943414634,
"sd": 0.01782665397004 
},
{
 "name": "sample",
"mean": 0.006660082926829,
"sd": 0.02590241519863 
},
{
 "name": "sample",
"mean": 0.005580988292683,
"sd": 0.0107287733162 
},
{
 "name": "sample",
"mean": 0.006770455609756,
"sd": 0.02231576239923 
},
{
 "name": "sample",
"mean": 0.006165202926829,
"sd": 0.01668868597024 
},
{
 "name": "sample",
"mean": 0.00586720097561,
"sd": 0.01458013823754 
},
{
 "name": "sample",
"mean": 0.005491557073171,
"sd": 0.01093565186933 
},
{
 "name": "sample",
"mean":     0.00634528,
"sd": 0.01383903261513 
},
{
 "name": "sample",
"mean": 0.006156979512195,
"sd": 0.0161103336296 
},
{
 "name": "sample",
"mean": 0.006372769756098,
"sd": 0.01510654561926 
},
{
 "name": "sample",
"mean": 0.005361253658537,
"sd": 0.01101141641962 
},
{
 "name": "sample",
"mean": 0.005377897560976,
"sd": 0.01235715890795 
},
{
 "name": "sample",
"mean": 0.006191939512195,
"sd": 0.01481929744705 
},
{
 "name": "sample",
"mean": 0.004993047804878,
"sd": 0.02355604421609 
},
{
 "name": "sample",
"mean": 0.005196995121951,
"sd": 0.01183588536628 
},
{
 "name": "sample",
"mean": 0.005545983414634,
"sd": 0.01538342923326 
},
{
 "name": "sample",
"mean": 0.004901352195122,
"sd": 0.01255075044999 
},
{
 "name": "sample",
"mean": 0.006137658536585,
"sd": 0.01519907641358 
},
{
 "name": "sample",
"mean": 0.005508513170732,
"sd": 0.01033558909414 
},
{
 "name": "sample",
"mean": 0.004973807804878,
"sd": 0.01317899080356 
},
{
 "name": "sample",
"mean": 0.005244522926829,
"sd": 0.01315061349371 
},
{
 "name": "sample",
"mean": 0.006201982439024,
"sd": 0.01332649661573 
},
{
 "name": "sample",
"mean": 0.005371237073171,
"sd": 0.00855357460899 
},
{
 "name": "sample",
"mean": 0.006043046829268,
"sd": 0.0158679385308 
},
{
 "name": "sample",
"mean": 0.005302644878049,
"sd": 0.01894315239964 
},
{
 "name": "sample",
"mean": 0.005425053658537,
"sd": 0.009302958438659 
},
{
 "name": "sample",
"mean": 0.004949824390244,
"sd": 0.01271356445313 
},
{
 "name": "sample",
"mean": 0.006275096585366,
"sd": 0.02025408986749 
},
{
 "name": "sample",
"mean": 0.005429310243902,
"sd": 0.01295314982191 
},
{
 "name": "sample",
"mean": 0.00497004195122,
"sd": 0.0125630624619 
},
{
 "name": "sample",
"mean": 0.007042784390244,
"sd": 0.03125017476034 
},
{
 "name": "sample",
"mean": 0.006169437073171,
"sd": 0.01302951489583 
},
{
 "name": "sample",
"mean": 0.005389388292683,
"sd": 0.01397494974039 
},
{
 "name": "sample",
"mean": 0.00727864195122,
"sd": 0.03321179574286 
},
{
 "name": "sample",
"mean": 0.00611656097561,
"sd": 0.01484325678675 
},
{
 "name": "sample",
"mean": 0.005797724878049,
"sd": 0.01286397044631 
},
{
 "name": "sample",
"mean": 0.005758055609756,
"sd": 0.01470498030339 
},
{
 "name": "sample",
"mean": 0.006298474146341,
"sd": 0.01533297665758 
},
{
 "name": "sample",
"mean": 0.006197984390244,
"sd": 0.01434118059293 
},
{
 "name": "sample",
"mean": 0.006141669268293,
"sd": 0.0188185374386 
},
{
 "name": "sample",
"mean": 0.005196900487805,
"sd": 0.01015499833378 
},
{
 "name": "sample",
"mean": 0.005920012682927,
"sd": 0.01332520697208 
},
{
 "name": "sample",
"mean": 0.006303129756098,
"sd": 0.01277429318122 
},
{
 "name": "sample",
"mean": 0.005808274146341,
"sd": 0.01365087762348 
},
{
 "name": "sample",
"mean": 0.006267340487805,
"sd": 0.0178617098897 
},
{
 "name": "sample",
"mean": 0.00515711804878,
"sd": 0.02202271654315 
},
{
 "name": "sample",
"mean": 0.006335819512195,
"sd": 0.01562703129184 
},
{
 "name": "sample",
"mean": 0.00527576195122,
"sd": 0.01319905758338 
},
{
 "name": "sample",
"mean": 0.005590707317073,
"sd": 0.009652969425945 
},
{
 "name": "sample",
"mean": 0.005295783414634,
"sd": 0.01118425209187 
},
{
 "name": "sample",
"mean": 0.005159647804878,
"sd": 0.01226239641593 
},
{
 "name": "sample",
"mean": 0.00510788097561,
"sd": 0.01210589044107 
},
{
 "name": "sample",
"mean": 0.005443666341463,
"sd": 0.008928560796212 
},
{
 "name": "sample",
"mean": 0.006455894634146,
"sd": 0.01781270468698 
},
{
 "name": "sample",
"mean": 0.006986134634146,
"sd": 0.02989639541776 
},
{
 "name": "sample",
"mean": 0.00494691902439,
"sd": 0.0228053544843 
},
{
 "name": "sample",
"mean": 0.006231291707317,
"sd": 0.01385990440438 
},
{
 "name": "sample",
"mean": 0.005343153170732,
"sd": 0.009049712669112 
},
{
 "name": "sample",
"mean": 0.005892562926829,
"sd": 0.02003160600717 
},
{
 "name": "sample",
"mean": 0.006298053658537,
"sd": 0.01548506857832 
},
{
 "name": "sample",
"mean":     0.00624364,
"sd": 0.01582856033592 
},
{
 "name": "sample",
"mean": 0.005760350243902,
"sd": 0.01302313710139 
},
{
 "name": "sample",
"mean": 0.006170650731707,
"sd": 0.01325105062974 
},
{
 "name": "sample",
"mean": 0.006104124878049,
"sd": 0.01545377267784 
},
{
 "name": "sample",
"mean": 0.005187276097561,
"sd": 0.01667152586512 
},
{
 "name": "sample",
"mean": 0.005975353170732,
"sd": 0.01343384272315 
},
{
 "name": "sample",
"mean": 0.007122978536585,
"sd": 0.03228701649444 
},
{
 "name": "sample",
"mean": 0.006907692682927,
"sd": 0.0259482985751 
},
{
 "name": "sample",
"mean": 0.005083416585366,
"sd": 0.01083365878219 
},
{
 "name": "sample",
"mean": 0.006145097560976,
"sd": 0.01459711947381 
},
{
 "name": "sample",
"mean": 0.006142577560976,
"sd": 0.02145186543704 
},
{
 "name": "sample",
"mean": 0.00615384097561,
"sd": 0.02043988268667 
},
{
 "name": "sample",
"mean": 0.004989384390244,
"sd": 0.01088077913766 
},
{
 "name": "sample",
"mean": 0.005898942439024,
"sd": 0.0141185404846 
},
{
 "name": "sample",
"mean": 0.005621846829268,
"sd": 0.01363258913274 
},
{
 "name": "sample",
"mean": 0.005939451707317,
"sd": 0.01500204509814 
},
{
 "name": "sample",
"mean": 0.006335144390244,
"sd": 0.0145179778715 
},
{
 "name": "sample",
"mean": 0.005595245853659,
"sd": 0.01017706971637 
},
{
 "name": "sample",
"mean": 0.006387505365854,
"sd": 0.01616459927645 
},
{
 "name": "sample",
"mean": 0.006457424390244,
"sd": 0.02209761160986 
},
{
 "name": "sample",
"mean": 0.005499366829268,
"sd": 0.01534830234226 
},
{
 "name": "sample",
"mean": 0.006485588292683,
"sd": 0.01846607077684 
},
{
 "name": "sample",
"mean": 0.006038004878049,
"sd": 0.01393826600134 
},
{
 "name": "sample",
"mean": 0.00683355902439,
"sd": 0.0214741762414 
},
{
 "name": "sample",
"mean": 0.005064352195122,
"sd": 0.0220500755569 
},
{
 "name": "sample",
"mean": 0.005663051707317,
"sd": 0.01151312978057 
},
{
 "name": "sample",
"mean": 0.005731329756098,
"sd": 0.01101839631818 
},
{
 "name": "sample",
"mean": 0.00596356097561,
"sd": 0.01388961207488 
},
{
 "name": "sample",
"mean": 0.006308782439024,
"sd": 0.02227614938297 
},
{
 "name": "sample",
"mean": 0.005824829268293,
"sd": 0.01052146907939 
},
{
 "name": "sample",
"mean":     0.00642476,
"sd": 0.01527945002902 
},
{
 "name": "sample",
"mean": 0.005915660487805,
"sd": 0.01252360128511 
},
{
 "name": "sample",
"mean": 0.006950180487805,
"sd": 0.02587212843755 
},
{
 "name": "sample",
"mean": 0.005736452682927,
"sd": 0.01258579937062 
},
{
 "name": "sample",
"mean": 0.005666642926829,
"sd": 0.01001615165699 
},
{
 "name": "sample",
"mean": 0.005465150243902,
"sd": 0.009251986027264 
},
{
 "name": "sample",
"mean": 0.005355425365854,
"sd": 0.01312173180224 
},
{
 "name": "sample",
"mean": 0.00506431804878,
"sd": 0.01112149416797 
},
{
 "name": "sample",
"mean": 0.005314023414634,
"sd": 0.009783710756542 
},
{
 "name": "sample",
"mean": 0.006553884878049,
"sd": 0.02291395262367 
},
{
 "name": "sample",
"mean": 0.005222083902439,
"sd": 0.01637194329096 
},
{
 "name": "sample",
"mean": 0.006056928780488,
"sd": 0.01127860365461 
},
{
 "name": "sample",
"mean": 0.007118829268293,
"sd": 0.03027344628783 
},
{
 "name": "sample",
"mean": 0.006154102439024,
"sd": 0.0146254696166 
},
{
 "name": "sample",
"mean": 0.00495227902439,
"sd": 0.02353052970443 
},
{
 "name": "sample",
"mean": 0.006046466341463,
"sd": 0.01375105241448 
},
{
 "name": "sample",
"mean": 0.005761791219512,
"sd": 0.01606083225632 
},
{
 "name": "sample",
"mean": 0.006239716097561,
"sd": 0.01313746697133 
},
{
 "name": "sample",
"mean":     0.00614892,
"sd": 0.01595910889056 
},
{
 "name": "sample",
"mean": 0.006040533658537,
"sd": 0.01176741526597 
},
{
 "name": "sample",
"mean": 0.006497567804878,
"sd": 0.01515633020692 
},
{
 "name": "sample",
"mean": 0.005334575609756,
"sd": 0.008378871632418 
},
{
 "name": "sample",
"mean":      0.0049122,
"sd": 0.01130290929901 
},
{
 "name": "sample",
"mean": 0.006277577560976,
"sd": 0.01417653071488 
},
{
 "name": "sample",
"mean": 0.005081746341463,
"sd": 0.01168936491833 
},
{
 "name": "sample",
"mean": 0.005502492682927,
"sd": 0.01383966198117 
},
{
 "name": "sample",
"mean": 0.005924070243902,
"sd": 0.01784841960027 
},
{
 "name": "sample",
"mean": 0.005835792195122,
"sd": 0.01858264549036 
},
{
 "name": "sample",
"mean": 0.005385209756098,
"sd": 0.008611550514047 
},
{
 "name": "sample",
"mean": 0.006140389268293,
"sd": 0.01680063285163 
},
{
 "name": "sample",
"mean":     0.00594832,
"sd": 0.01219765999667 
},
{
 "name": "sample",
"mean": 0.006089763902439,
"sd": 0.0172472343144 
},
{
 "name": "sample",
"mean": 0.005260089756098,
"sd": 0.008703471733792 
},
{
 "name": "sample",
"mean": 0.006551153170732,
"sd": 0.02503851035108 
},
{
 "name": "sample",
"mean": 0.006594334634146,
"sd": 0.02297309584311 
},
{
 "name": "sample",
"mean": 0.007145197073171,
"sd": 0.03303693431184 
},
{
 "name": "sample",
"mean": 0.005252255609756,
"sd": 0.0113144723155 
},
{
 "name": "sample",
"mean": 0.006155544390244,
"sd": 0.01226983837566 
},
{
 "name": "sample",
"mean": 0.006409024390244,
"sd": 0.01566797627095 
},
{
 "name": "sample",
"mean": 0.006649094634146,
"sd": 0.01960738439221 
},
{
 "name": "sample",
"mean": 0.005331270243902,
"sd": 0.01079074334759 
},
{
 "name": "sample",
"mean": 0.007186245853659,
"sd": 0.03259129669038 
},
{
 "name": "sample",
"mean": 0.005085326829268,
"sd": 0.01280570532825 
},
{
 "name": "sample",
"mean": 0.00598771804878,
"sd": 0.0129647683984 
},
{
 "name": "sample",
"mean": 0.006779297560976,
"sd": 0.0253230901299 
},
{
 "name": "sample",
"mean": 0.004979133658537,
"sd": 0.01141664025213 
},
{
 "name": "sample",
"mean": 0.006175562926829,
"sd": 0.01985851959287 
},
{
 "name": "sample",
"mean": 0.005318451707317,
"sd": 0.008435142829355 
},
{
 "name": "sample",
"mean": 0.005557090731707,
"sd": 0.009908255860415 
},
{
 "name": "sample",
"mean": 0.005228396097561,
"sd": 0.008326839517758 
},
{
 "name": "sample",
"mean": 0.005904915121951,
"sd": 0.01576176692445 
},
{
 "name": "sample",
"mean": 0.006512343414634,
"sd": 0.01573530832633 
},
{
 "name": "sample",
"mean": 0.005319335609756,
"sd": 0.01102486763018 
},
{
 "name": "sample",
"mean": 0.006360738536585,
"sd": 0.02234194879783 
},
{
 "name": "sample",
"mean": 0.005855197073171,
"sd": 0.0159929383222 
},
{
 "name": "sample",
"mean": 0.006425103414634,
"sd": 0.01749410775627 
},
{
 "name": "sample",
"mean": 0.005468021463415,
"sd": 0.01162766756293 
},
{
 "name": "sample",
"mean": 0.005834483902439,
"sd": 0.01304585011843 
},
{
 "name": "sample",
"mean": 0.006058261463415,
"sd": 0.01762533185818 
},
{
 "name": "sample",
"mean": 0.006364297560976,
"sd": 0.02139740417141 
},
{
 "name": "sample",
"mean": 0.005537416585366,
"sd": 0.01360429096227 
},
{
 "name": "sample",
"mean": 0.006453355121951,
"sd": 0.01690761648865 
},
{
 "name": "sample",
"mean": 0.005428213658537,
"sd": 0.009179236858867 
},
{
 "name": "sample",
"mean": 0.005545564878049,
"sd": 0.01697207335769 
},
{
 "name": "sample",
"mean": 0.005952389268293,
"sd": 0.01096692113911 
},
{
 "name": "sample",
"mean": 0.004870305365854,
"sd": 0.01157989605329 
},
{
 "name": "sample",
"mean": 0.006058821463415,
"sd": 0.01605435024633 
},
{
 "name": "sample",
"mean": 0.006076831219512,
"sd": 0.01892378158302 
},
{
 "name": "sample",
"mean": 0.005918849756098,
"sd": 0.01156275622931 
},
{
 "name": "sample",
"mean": 0.00509160097561,
"sd": 0.01135159463922 
},
{
 "name": "sample",
"mean": 0.00641892097561,
"sd": 0.0166252500981 
},
{
 "name": "sample",
"mean": 0.005705500487805,
"sd": 0.01387513139877 
},
{
 "name": "sample",
"mean": 0.007176499512195,
"sd": 0.03134752637736 
},
{
 "name": "sample",
"mean": 0.005861436097561,
"sd": 0.01491129731662 
},
{
 "name": "sample",
"mean": 0.005693726829268,
"sd": 0.0145580257038 
},
{
 "name": "sample",
"mean": 0.005354249756098,
"sd": 0.009595512395276 
},
{
 "name": "sample",
"mean": 0.005264738536585,
"sd": 0.02020752303293 
},
{
 "name": "sample",
"mean": 0.004968476097561,
"sd": 0.01235167801688 
},
{
 "name": "sample",
"mean": 0.005297148292683,
"sd": 0.01236111173257 
},
{
 "name": "sample",
"mean": 0.006493724878049,
"sd": 0.01800606955455 
},
{
 "name": "sample",
"mean": 0.005120700487805,
"sd": 0.01131909250711 
},
{
 "name": "sample",
"mean": 0.006498542439024,
"sd": 0.02414384321494 
},
{
 "name": "sample",
"mean": 0.00597616097561,
"sd": 0.0110821697055 
},
{
 "name": "sample",
"mean": 0.006330128780488,
"sd": 0.01512244895267 
},
{
 "name": "sample",
"mean": 0.005885700487805,
"sd": 0.01469551275611 
},
{
 "name": "sample",
"mean": 0.00493816097561,
"sd": 0.01482326164573 
},
{
 "name": "sample",
"mean": 0.005271964878049,
"sd": 0.01413824157646 
},
{
 "name": "sample",
"mean": 0.005142470243902,
"sd": 0.02069447242837 
},
{
 "name": "sample",
"mean": 0.005929886829268,
"sd": 0.01830504777135 
},
{
 "name": "sample",
"mean": 0.005847255609756,
"sd": 0.01749332874252 
},
{
 "name": "sample",
"mean": 0.005779927804878,
"sd": 0.0133269866376 
},
{
 "name": "sample",
"mean": 0.006000973658537,
"sd": 0.01310389230383 
},
{
 "name": "sample",
"mean": 0.005134567804878,
"sd": 0.01003390001869 
},
{
 "name": "sample",
"mean": 0.005956425365854,
"sd": 0.01403258361441 
},
{
 "name": "sample",
"mean": 0.005595749268293,
"sd": 0.01309734954062 
},
{
 "name": "sample",
"mean": 0.005666229268293,
"sd": 0.01172612287127 
},
{
 "name": "sample",
"mean": 0.006240062439024,
"sd": 0.01888483784605 
},
{
 "name": "sample",
"mean": 0.006474873170732,
"sd": 0.01729368040892 
},
{
 "name": "sample",
"mean": 0.00516491804878,
"sd": 0.01571463463966 
},
{
 "name": "sample",
"mean": 0.005249258536585,
"sd": 0.01297878651972 
},
{
 "name": "sample",
"mean": 0.005012862439024,
"sd": 0.01213300505169 
},
{
 "name": "sample",
"mean": 0.006104682926829,
"sd": 0.01608399984879 
},
{
 "name": "sample",
"mean": 0.004971875121951,
"sd": 0.01258526593915 
},
{
 "name": "sample",
"mean": 0.006248192195122,
"sd": 0.01503529186667 
},
{
 "name": "sample",
"mean": 0.006357606829268,
"sd": 0.01384959443825 
},
{
 "name": "sample",
"mean": 0.005751274146341,
"sd": 0.01348746404783 
},
{
 "name": "sample",
"mean": 0.006435337560976,
"sd": 0.01581395353517 
},
{
 "name": "sample",
"mean": 0.005934315121951,
"sd": 0.01176992786342 
},
{
 "name": "sample",
"mean": 0.005564420487805,
"sd": 0.008929830672382 
},
{
 "name": "sample",
"mean": 0.005495048780488,
"sd": 0.01150334512479 
},
{
 "name": "sample",
"mean": 0.006362009756098,
"sd": 0.01931678311543 
},
{
 "name": "sample",
"mean": 0.007218843902439,
"sd": 0.03413735713084 
},
{
 "name": "sample",
"mean": 0.005211530731707,
"sd": 0.01778090337305 
},
{
 "name": "sample",
"mean": 0.005458175609756,
"sd": 0.0089863568823 
},
{
 "name": "sample",
"mean": 0.006219370731707,
"sd": 0.01533555007676 
},
{
 "name": "sample",
"mean": 0.006118709268293,
"sd": 0.01277113415955 
},
{
 "name": "sample",
"mean": 0.005878061463415,
"sd": 0.01471313124853 
},
{
 "name": "sample",
"mean": 0.005780952195122,
"sd": 0.01421939127073 
},
{
 "name": "sample",
"mean": 0.005964108292683,
"sd": 0.01637125083519 
},
{
 "name": "sample",
"mean": 0.00626628097561,
"sd": 0.01563298133015 
},
{
 "name": "sample",
"mean": 0.005826647804878,
"sd": 0.01349272231677 
},
{
 "name": "sample",
"mean": 0.005290740487805,
"sd": 0.01194421618457 
},
{
 "name": "sample",
"mean": 0.005627214634146,
"sd": 0.01694438571314 
},
{
 "name": "sample",
"mean": 0.005889896585366,
"sd": 0.01657661080396 
},
{
 "name": "sample",
"mean": 0.006476786341463,
"sd": 0.01593898552661 
},
{
 "name": "sample",
"mean": 0.00541635804878,
"sd": 0.01376293714855 
},
{
 "name": "sample",
"mean": 0.005772464390244,
"sd": 0.01016790169271 
},
{
 "name": "sample",
"mean": 0.006238769756098,
"sd": 0.01505770534604 
},
{
 "name": "sample",
"mean": 0.005023107317073,
"sd": 0.01266853480705 
},
{
 "name": "sample",
"mean": 0.006378756097561,
"sd": 0.01786327945729 
},
{
 "name": "sample",
"mean": 0.006197644878049,
"sd": 0.0155393942755 
},
{
 "name": "sample",
"mean": 0.00553876097561,
"sd": 0.01386064138252 
},
{
 "name": "sample",
"mean": 0.005828128780488,
"sd": 0.01450929555632 
},
{
 "name": "sample",
"mean": 0.006279469268293,
"sd": 0.01515093218896 
},
{
 "name": "sample",
"mean": 0.006658406829268,
"sd": 0.02361840494645 
},
{
 "name": "sample",
"mean": 0.005466552195122,
"sd": 0.008685460486955 
},
{
 "name": "sample",
"mean": 0.005698690731707,
"sd": 0.009414796073482 
},
{
 "name": "sample",
"mean": 0.005777597073171,
"sd": 0.013597445587 
},
{
 "name": "sample",
"mean": 0.006507444878049,
"sd": 0.01540949450953 
},
{
 "name": "sample",
"mean": 0.006405996097561,
"sd": 0.01723877791625 
},
{
 "name": "sample",
"mean": 0.005875802926829,
"sd": 0.01873170123737 
},
{
 "name": "sample",
"mean": 0.006523131707317,
"sd": 0.01453701478374 
},
{
 "name": "sample",
"mean": 0.006323503414634,
"sd": 0.01416005930905 
},
{
 "name": "sample",
"mean": 0.006486177560976,
"sd": 0.01422453447366 
},
{
 "name": "sample",
"mean": 0.005378993170732,
"sd": 0.01700125899506 
},
{
 "name": "sample",
"mean": 0.005847030243902,
"sd": 0.01232192510278 
},
{
 "name": "sample",
"mean": 0.006214372682927,
"sd": 0.01795053805074 
},
{
 "name": "sample",
"mean": 0.00593388097561,
"sd": 0.01156122609945 
},
{
 "name": "sample",
"mean": 0.00697071902439,
"sd": 0.02834658872994 
},
{
 "name": "sample",
"mean": 0.005113690731707,
"sd": 0.01579116533442 
},
{
 "name": "sample",
"mean": 0.005324029268293,
"sd": 0.01200328792392 
},
{
 "name": "sample",
"mean": 0.006383413658537,
"sd": 0.02043935666685 
},
{
 "name": "sample",
"mean": 0.005614028292683,
"sd": 0.01426030195308 
},
{
 "name": "sample",
"mean": 0.005994643902439,
"sd": 0.01479397118758 
},
{
 "name": "sample",
"mean": 0.005591877073171,
"sd": 0.01308958109188 
},
{
 "name": "sample",
"mean": 0.005931362926829,
"sd": 0.0140662407535 
},
{
 "name": "sample",
"mean": 0.005015724878049,
"sd": 0.01246153445967 
},
{
 "name": "sample",
"mean": 0.004983377560976,
"sd": 0.0123363397075 
},
{
 "name": "sample",
"mean": 0.007103618536585,
"sd": 0.03253453532611 
},
{
 "name": "sample",
"mean": 0.006194686829268,
"sd": 0.01404671668185 
},
{
 "name": "sample",
"mean": 0.006094386341463,
"sd": 0.01442171004764 
},
{
 "name": "sample",
"mean": 0.006483590243902,
"sd": 0.01623505389462 
},
{
 "name": "sample",
"mean": 0.006204057560976,
"sd": 0.01756115690263 
},
{
 "name": "sample",
"mean": 0.00552751804878,
"sd": 0.009376504097671 
},
{
 "name": "sample",
"mean":     0.00663472,
"sd": 0.01747332079931 
},
{
 "name": "sample",
"mean": 0.006513182439024,
"sd": 0.01547205551071 
},
{
 "name": "sample",
"mean": 0.006817122926829,
"sd": 0.02571018032135 
},
{
 "name": "sample",
"mean": 0.005900373658537,
"sd": 0.01396100554749 
},
{
 "name": "sample",
"mean": 0.006098688780488,
"sd": 0.01352020209552 
},
{
 "name": "sample",
"mean": 0.006314392195122,
"sd": 0.01783654845109 
},
{
 "name": "sample",
"mean": 0.005693412682927,
"sd": 0.01539306424711 
},
{
 "name": "sample",
"mean": 0.005479673170732,
"sd": 0.01197076122971 
},
{
 "name": "sample",
"mean": 0.006422075121951,
"sd": 0.01952842283133 
},
{
 "name": "sample",
"mean": 0.005490692682927,
"sd": 0.01170464281906 
},
{
 "name": "sample",
"mean": 0.005322424390244,
"sd": 0.01679748071442 
},
{
 "name": "sample",
"mean": 0.005532724878049,
"sd": 0.01395198460479 
},
{
 "name": "sample",
"mean": 0.005561392195122,
"sd": 0.01271008942708 
},
{
 "name": "sample",
"mean": 0.00549551902439,
"sd": 0.008886892802229 
},
{
 "name": "sample",
"mean": 0.005661126829268,
"sd": 0.01202709532445 
},
{
 "name": "sample",
"mean": 0.006543867317073,
"sd": 0.01892749710573 
},
{
 "name": "sample",
"mean": 0.006931155121951,
"sd": 0.02642830796902 
},
{
 "name": "sample",
"mean": 0.00591092097561,
"sd": 0.01605472397985 
},
{
 "name": "sample",
"mean": 0.005682379512195,
"sd": 0.01619028046661 
},
{
 "name": "sample",
"mean": 0.005350447804878,
"sd": 0.01088686705159 
},
{
 "name": "sample",
"mean": 0.005410635121951,
"sd": 0.008611062750422 
},
{
 "name": "sample",
"mean": 0.006181935609756,
"sd": 0.01345370381554 
},
{
 "name": "sample",
"mean": 0.00614124195122,
"sd": 0.01373028819465 
},
{
 "name": "sample",
"mean": 0.006390122926829,
"sd": 0.01379704836127 
},
{
 "name": "sample",
"mean": 0.006748736585366,
"sd": 0.02258350362034 
},
{
 "name": "sample",
"mean": 0.006009050731707,
"sd": 0.01317878212208 
},
{
 "name": "sample",
"mean": 0.006504034146341,
"sd": 0.02348638689741 
},
{
 "name": "sample",
"mean": 0.006978690731707,
"sd": 0.03044184278824 
},
{
 "name": "sample",
"mean": 0.005036126829268,
"sd": 0.02220786764153 
},
{
 "name": "sample",
"mean": 0.005359266341463,
"sd": 0.008611999301766 
},
{
 "name": "sample",
"mean": 0.006076870243902,
"sd": 0.01440712054725 
},
{
 "name": "sample",
"mean": 0.006111114146341,
"sd": 0.0142991916184 
},
{
 "name": "sample",
"mean": 0.00510003804878,
"sd": 0.01379925522683 
},
{
 "name": "sample",
"mean": 0.006425854634146,
"sd": 0.01741328687531 
},
{
 "name": "sample",
"mean": 0.006422870243902,
"sd": 0.01530477847067 
},
{
 "name": "sample",
"mean": 0.005160338536585,
"sd": 0.01181259073375 
},
{
 "name": "sample",
"mean": 0.006410164878049,
"sd": 0.01728050439764 
},
{
 "name": "sample",
"mean": 0.006206498536585,
"sd": 0.01554474305716 
},
{
 "name": "sample",
"mean": 0.007052313170732,
"sd": 0.03170626275993 
},
{
 "name": "sample",
"mean": 0.006437895609756,
"sd": 0.01583563529302 
},
{
 "name": "sample",
"mean": 0.005637503414634,
"sd": 0.01130519372899 
},
{
 "name": "sample",
"mean": 0.006434305365854,
"sd": 0.02245798174825 
},
{
 "name": "sample",
"mean": 0.006103190243902,
"sd": 0.01415703983576 
},
{
 "name": "sample",
"mean": 0.005480594146341,
"sd": 0.01291279670535 
},
{
 "name": "sample",
"mean":     0.00647172,
"sd": 0.01572602088171 
},
{
 "name": "sample",
"mean": 0.006566096585366,
"sd": 0.02309192763684 
},
{
 "name": "sample",
"mean": 0.005138700487805,
"sd": 0.0123439459979 
},
{
 "name": "sample",
"mean": 0.006015256585366,
"sd": 0.01440499621389 
},
{
 "name": "sample",
"mean": 0.006962413658537,
"sd": 0.02651526089965 
},
{
 "name": "sample",
"mean": 0.005194295609756,
"sd": 0.01836001506708 
},
{
 "name": "sample",
"mean": 0.005607068292683,
"sd": 0.0104701453468 
},
{
 "name": "sample",
"mean": 0.006436092682927,
"sd": 0.01793395235219 
},
{
 "name": "sample",
"mean": 0.006320396097561,
"sd": 0.01344648875984 
},
{
 "name": "sample",
"mean": 0.005979624390244,
"sd": 0.01692928688739 
},
{
 "name": "sample",
"mean": 0.005311909268293,
"sd": 0.008674993264808 
},
{
 "name": "sample",
"mean": 0.005930876097561,
"sd": 0.0128648471969 
},
{
 "name": "sample",
"mean": 0.005489373658537,
"sd": 0.01501018129883 
},
{
 "name": "sample",
"mean": 0.006125616585366,
"sd": 0.01262280985673 
},
{
 "name": "sample",
"mean": 0.006376818536585,
"sd": 0.01544604013461 
},
{
 "name": "sample",
"mean": 0.005913462439024,
"sd": 0.01398173671996 
},
{
 "name": "sample",
"mean": 0.005189268292683,
"sd": 0.009234028204483 
},
{
 "name": "sample",
"mean": 0.006198100487805,
"sd": 0.01594564857284 
},
{
 "name": "sample",
"mean": 0.006435464390244,
"sd": 0.01505236724447 
},
{
 "name": "sample",
"mean": 0.005701009756098,
"sd": 0.01008380967291 
},
{
 "name": "sample",
"mean":     0.00534012,
"sd": 0.01013513121419 
},
{
 "name": "sample",
"mean": 0.005469553170732,
"sd": 0.009420557744851 
},
{
 "name": "sample",
"mean": 0.006532856585366,
"sd": 0.01588848422988 
},
{
 "name": "sample",
"mean": 0.005750330731707,
"sd": 0.01218310765413 
},
{
 "name": "sample",
"mean": 0.005839485853659,
"sd": 0.01401569215513 
},
{
 "name": "sample",
"mean": 0.006141349268293,
"sd": 0.01517255324151 
},
{
 "name": "sample",
"mean": 0.005047325853659,
"sd": 0.02142835304971 
},
{
 "name": "sample",
"mean": 0.00608295804878,
"sd": 0.01362079908714 
},
{
 "name": "sample",
"mean": 0.005123244878049,
"sd": 0.01814149275175 
},
{
 "name": "sample",
"mean": 0.006431811707317,
"sd": 0.01464093174235 
},
{
 "name": "sample",
"mean":      0.0054808,
"sd": 0.009797175926746 
},
{
 "name": "sample",
"mean":     0.00531148,
"sd": 0.008523757370941 
},
{
 "name": "sample",
"mean": 0.005908064390244,
"sd": 0.01167434352504 
},
{
 "name": "sample",
"mean": 0.005439764878049,
"sd": 0.01421157130616 
},
{
 "name": "sample",
"mean": 0.00584004195122,
"sd": 0.01243212938688 
},
{
 "name": "sample",
"mean": 0.00664119902439,
"sd": 0.02234647337059 
},
{
 "name": "sample",
"mean": 0.005588772682927,
"sd": 0.009110969126366 
},
{
 "name": "sample",
"mean": 0.005191604878049,
"sd": 0.01629238753135 
},
{
 "name": "sample",
"mean": 0.00608331902439,
"sd": 0.01741437338876 
},
{
 "name": "sample",
"mean": 0.006199508292683,
"sd": 0.01306308926831 
},
{
 "name": "sample",
"mean": 0.005098314146341,
"sd": 0.0151326544812 
},
{
 "name": "sample",
"mean": 0.006380939512195,
"sd": 0.01716018214172 
},
{
 "name": "sample",
"mean": 0.006138821463415,
"sd": 0.01484135198608 
},
{
 "name": "sample",
"mean": 0.006447834146341,
"sd": 0.01530402694282 
},
{
 "name": "sample",
"mean": 0.00612624097561,
"sd": 0.01381182376248 
},
{
 "name": "sample",
"mean": 0.006400633170732,
"sd": 0.01999097256136 
},
{
 "name": "sample",
"mean": 0.006798902439024,
"sd": 0.02342375545888 
},
{
 "name": "sample",
"mean": 0.00615699902439,
"sd": 0.01279805612497 
},
{
 "name": "sample",
"mean": 0.005139435121951,
"sd": 0.01284186742989 
},
{
 "name": "sample",
"mean": 0.005064410731707,
"sd": 0.02123446541421 
},
{
 "name": "sample",
"mean": 0.00498588097561,
"sd": 0.01124592537743 
},
{
 "name": "sample",
"mean": 0.005635125853659,
"sd": 0.01007866262111 
},
{
 "name": "sample",
"mean": 0.006463803902439,
"sd": 0.02168065467005 
},
{
 "name": "sample",
"mean": 0.005679794146341,
"sd": 0.01010850398838 
},
{
 "name": "sample",
"mean": 0.006496006829268,
"sd": 0.01588387303221 
},
{
 "name": "sample",
"mean": 0.006295703414634,
"sd": 0.01666781399376 
},
{
 "name": "sample",
"mean": 0.005153915121951,
"sd": 0.0120827967162 
},
{
 "name": "sample",
"mean": 0.005024500487805,
"sd": 0.01324746431822 
},
{
 "name": "sample",
"mean": 0.006531251707317,
"sd": 0.02324141466483 
},
{
 "name": "sample",
"mean": 0.00565195804878,
"sd": 0.01493364430572 
},
{
 "name": "sample",
"mean": 0.004921814634146,
"sd": 0.01190498305972 
},
{
 "name": "sample",
"mean": 0.006216340487805,
"sd": 0.0220883447827 
},
{
 "name": "sample",
"mean": 0.006446548292683,
"sd": 0.01474008849128 
},
{
 "name": "sample",
"mean": 0.006418913170732,
"sd": 0.0157215191407 
},
{
 "name": "sample",
"mean": 0.005139850731707,
"sd": 0.02181386547102 
},
{
 "name": "sample",
"mean": 0.005594836097561,
"sd": 0.01532124868369 
},
{
 "name": "sample",
"mean": 0.005484814634146,
"sd": 0.008780074862023 
},
{
 "name": "sample",
"mean": 0.006293914146341,
"sd": 0.01729933879944 
},
{
 "name": "sample",
"mean": 0.005279004878049,
"sd": 0.01056992041535 
},
{
 "name": "sample",
"mean": 0.005357356097561,
"sd": 0.01439898363177 
},
{
 "name": "sample",
"mean": 0.00609636195122,
"sd": 0.01546119110588 
},
{
 "name": "sample",
"mean": 0.006573415609756,
"sd": 0.0199074917085 
},
{
 "name": "sample",
"mean": 0.005246540487805,
"sd": 0.01882589384246 
},
{
 "name": "sample",
"mean": 0.005960384390244,
"sd": 0.01244213593867 
},
{
 "name": "sample",
"mean": 0.007266917073171,
"sd": 0.03398497708582 
},
{
 "name": "sample",
"mean": 0.006448349268293,
"sd": 0.01400167749558 
},
{
 "name": "sample",
"mean": 0.00552791804878,
"sd": 0.009140111214876 
},
{
 "name": "sample",
"mean": 0.00664083902439,
"sd": 0.01838187148326 
},
{
 "name": "sample",
"mean": 0.005475939512195,
"sd": 0.01135997963545 
},
{
 "name": "sample",
"mean":      0.0058644,
"sd": 0.0135242088857 
},
{
 "name": "sample",
"mean": 0.006914219512195,
"sd": 0.02942447353824 
},
{
 "name": "sample",
"mean": 0.005963629268293,
"sd": 0.01632187098784 
},
{
 "name": "sample",
"mean": 0.005493388292683,
"sd": 0.01175555321624 
},
{
 "name": "sample",
"mean": 0.006540655609756,
"sd": 0.01829239079237 
},
{
 "name": "sample",
"mean": 0.005546950243902,
"sd": 0.01592562029617 
},
{
 "name": "sample",
"mean": 0.005406018536585,
"sd": 0.01471825262574 
},
{
 "name": "sample",
"mean": 0.00589404195122,
"sd": 0.01631454262505 
},
{
 "name": "sample",
"mean": 0.005349947317073,
"sd": 0.01436588724118 
},
{
 "name": "sample",
"mean": 0.006013027317073,
"sd": 0.01498707780146 
},
{
 "name": "sample",
"mean": 0.006181365853659,
"sd": 0.02019768805702 
},
{
 "name": "sample",
"mean": 0.005329604878049,
"sd": 0.01053753920655 
},
{
 "name": "sample",
"mean": 0.006210864390244,
"sd": 0.01496128367792 
},
{
 "name": "sample",
"mean": 0.005796209756098,
"sd": 0.01266302024929 
},
{
 "name": "sample",
"mean": 0.005153537560976,
"sd": 0.02091228211608 
},
{
 "name": "sample",
"mean": 0.006601005853659,
"sd": 0.02415869140417 
},
{
 "name": "sample",
"mean": 0.007041517073171,
"sd": 0.03146390328798 
},
{
 "name": "sample",
"mean": 0.006005339512195,
"sd": 0.01249111945651 
},
{
 "name": "sample",
"mean": 0.006562683902439,
"sd": 0.01793430437297 
},
{
 "name": "sample",
"mean": 0.005708859512195,
"sd": 0.01263919445619 
},
{
 "name": "sample",
"mean": 0.005380782439024,
"sd": 0.008627663988503 
},
{
 "name": "sample",
"mean": 0.005721055609756,
"sd": 0.01099706928717 
},
{
 "name": "sample",
"mean": 0.005696504390244,
"sd": 0.01967998505236 
},
{
 "name": "sample",
"mean": 0.006117419512195,
"sd": 0.01654004552469 
},
{
 "name": "sample",
"mean": 0.005088458536585,
"sd": 0.01098043944589 
},
{
 "name": "sample",
"mean": 0.006263605853659,
"sd": 0.01933795071088 
},
{
 "name": "sample",
"mean": 0.00521568097561,
"sd": 0.009631393580812 
},
{
 "name": "sample",
"mean": 0.006772466341463,
"sd": 0.02588017157323 
},
{
 "name": "sample",
"mean": 0.006042848780488,
"sd": 0.01492600960718 
},
{
 "name": "sample",
"mean": 0.005088592195122,
"sd": 0.01999215126953 
},
{
 "name": "sample",
"mean": 0.005305185365854,
"sd": 0.01376667534713 
},
{
 "name": "sample",
"mean": 0.006431949268293,
"sd": 0.0158955645559 
},
{
 "name": "sample",
"mean": 0.006128854634146,
"sd": 0.01412186385659 
},
{
 "name": "sample",
"mean": 0.006285732682927,
"sd": 0.02017561503594 
},
{
 "name": "sample",
"mean": 0.005968589268293,
"sd": 0.01410132301798 
},
{
 "name": "sample",
"mean": 0.00492312195122,
"sd": 0.02309655310516 
},
{
 "name": "sample",
"mean": 0.006234636097561,
"sd": 0.01533754308639 
},
{
 "name": "sample",
"mean": 0.006376291707317,
"sd": 0.01496599578145 
},
{
 "name": "sample",
"mean": 0.00549491902439,
"sd": 0.01281791281897 
},
{
 "name": "sample",
"mean": 0.00535903804878,
"sd": 0.01044267561946 
},
{
 "name": "sample",
"mean": 0.005396188292683,
"sd": 0.009494649269377 
},
{
 "name": "sample",
"mean": 0.00577831902439,
"sd": 0.01516458373498 
},
{
 "name": "sample",
"mean": 0.005290790243902,
"sd": 0.01334757420958 
},
{
 "name": "sample",
"mean": 0.006573583414634,
"sd": 0.02157889518822 
},
{
 "name": "sample",
"mean": 0.005208604878049,
"sd": 0.01231250621579 
},
{
 "name": "sample",
"mean": 0.004968674146341,
"sd": 0.02345361589933 
},
{
 "name": "sample",
"mean": 0.005410017560976,
"sd": 0.01905756056984 
},
{
 "name": "sample",
"mean": 0.006064608780488,
"sd": 0.01184360043145 
},
{
 "name": "sample",
"mean": 0.006007230243902,
"sd": 0.01660740124416 
},
{
 "name": "sample",
"mean": 0.005278962926829,
"sd": 0.01621581999737 
},
{
 "name": "sample",
"mean": 0.005374464390244,
"sd": 0.01113727003739 
},
{
 "name": "sample",
"mean": 0.005916083902439,
"sd": 0.01154489076914 
},
{
 "name": "sample",
"mean": 0.005315124878049,
"sd": 0.008326284351873 
},
{
 "name": "sample",
"mean": 0.004941653658537,
"sd": 0.01209817569176 
},
{
 "name": "sample",
"mean": 0.005332630243902,
"sd": 0.01883323158016 
},
{
 "name": "sample",
"mean": 0.006439229268293,
"sd": 0.01502109064173 
},
{
 "name": "sample",
"mean": 0.005806495609756,
"sd": 0.01353433468408 
},
{
 "name": "sample",
"mean": 0.005410569756098,
"sd": 0.01512832937488 
},
{
 "name": "sample",
"mean": 0.006272690731707,
"sd": 0.01572236841494 
},
{
 "name": "sample",
"mean": 0.006155298536585,
"sd": 0.01759661301664 
},
{
 "name": "sample",
"mean": 0.006137395121951,
"sd": 0.01212855236336 
},
{
 "name": "sample",
"mean": 0.005069135609756,
"sd": 0.02095442658859 
},
{
 "name": "sample",
"mean": 0.006059403902439,
"sd": 0.01450709194118 
},
{
 "name": "sample",
"mean": 0.004898616585366,
"sd": 0.02316916632265 
},
{
 "name": "sample",
"mean": 0.006008010731707,
"sd": 0.01345160989516 
},
{
 "name": "sample",
"mean": 0.006011796097561,
"sd": 0.01346916043907 
},
{
 "name": "sample",
"mean": 0.006839588292683,
"sd": 0.0258527438074 
},
{
 "name": "sample",
"mean": 0.005552542439024,
"sd": 0.01446029409936 
},
{
 "name": "sample",
"mean": 0.006542106341463,
"sd": 0.01558531425553 
},
{
 "name": "sample",
"mean": 0.004932202926829,
"sd": 0.01150925394557 
},
{
 "name": "sample",
"mean": 0.005259930731707,
"sd": 0.00855044354245 
},
{
 "name": "sample",
"mean": 0.00626936195122,
"sd": 0.01624949535384 
},
{
 "name": "sample",
"mean": 0.005218371707317,
"sd": 0.0201667116586 
},
{
 "name": "sample",
"mean": 0.004922717073171,
"sd": 0.01156380419095 
},
{
 "name": "sample",
"mean": 0.007298668292683,
"sd": 0.03430121962741 
},
{
 "name": "sample",
"mean": 0.006528306341463,
"sd": 0.02188050179583 
},
{
 "name": "sample",
"mean": 0.005162485853659,
"sd": 0.0178524116492 
},
{
 "name": "sample",
"mean": 0.005512317073171,
"sd": 0.00926047455386 
},
{
 "name": "sample",
"mean": 0.005515704390244,
"sd": 0.009456447401134 
},
{
 "name": "sample",
"mean": 0.006977110243902,
"sd": 0.02845291209923 
},
{
 "name": "sample",
"mean": 0.006395507317073,
"sd": 0.0155937009554 
},
{
 "name": "sample",
"mean": 0.006714347317073,
"sd": 0.02109707586426 
},
{
 "name": "sample",
"mean": 0.005173006829268,
"sd": 0.02119817006104 
},
{
 "name": "sample",
"mean": 0.006498061463415,
"sd": 0.01777872629626 
},
{
 "name": "sample",
"mean": 0.005349981463415,
"sd": 0.008824905124961 
},
{
 "name": "sample",
"mean": 0.006347205853659,
"sd": 0.01397478517497 
},
{
 "name": "sample",
"mean": 0.006449330731707,
"sd": 0.02004940049294 
},
{
 "name": "sample",
"mean": 0.007277994146341,
"sd": 0.03461366453688 
},
{
 "name": "sample",
"mean": 0.006500204878049,
"sd": 0.01580505411715 
},
{
 "name": "sample",
"mean": 0.005466810731707,
"sd": 0.008693394424667 
},
{
 "name": "sample",
"mean":      0.0058584,
"sd": 0.0170706261825 
},
{
 "name": "sample",
"mean": 0.005569192195122,
"sd": 0.009513667220823 
},
{
 "name": "sample",
"mean": 0.005380972682927,
"sd": 0.01249870180404 
},
{
 "name": "sample",
"mean": 0.005304530731707,
"sd": 0.009025259259265 
},
{
 "name": "sample",
"mean": 0.006043461463415,
"sd": 0.0121921472932 
},
{
 "name": "sample",
"mean": 0.006362363902439,
"sd": 0.01586238183736 
},
{
 "name": "sample",
"mean": 0.005323112195122,
"sd": 0.009119860873815 
},
{
 "name": "sample",
"mean": 0.005677904390244,
"sd": 0.01195204662867 
},
{
 "name": "sample",
"mean": 0.006963857560976,
"sd": 0.02701652761851 
},
{
 "name": "sample",
"mean": 0.005562783414634,
"sd": 0.01028352038211 
},
{
 "name": "sample",
"mean": 0.005425029268293,
"sd": 0.01233772445309 
},
{
 "name": "sample",
"mean":     0.00680824,
"sd": 0.02215632543495 
},
{
 "name": "sample",
"mean": 0.006194830243902,
"sd": 0.01586117100634 
},
{
 "name": "sample",
"mean": 0.005534851707317,
"sd": 0.01434515150767 
},
{
 "name": "sample",
"mean": 0.007091048780488,
"sd": 0.03178762866138 
},
{
 "name": "sample",
"mean": 0.005606338536585,
"sd": 0.01512112014486 
},
{
 "name": "sample",
"mean": 0.006350915121951,
"sd": 0.01552260781047 
},
{
 "name": "sample",
"mean": 0.005083740487805,
"sd": 0.0197128947627 
},
{
 "name": "sample",
"mean": 0.006316213658537,
"sd": 0.02080615084783 
},
{
 "name": "sample",
"mean": 0.005090124878049,
"sd": 0.01124021201382 
},
{
 "name": "sample",
"mean": 0.006585145365854,
"sd": 0.0222870914139 
},
{
 "name": "sample",
"mean": 0.005056734634146,
"sd": 0.02069657686789 
},
{
 "name": "sample",
"mean": 0.005450854634146,
"sd": 0.00898691336648 
},
{
 "name": "sample",
"mean": 0.005580225365854,
"sd": 0.009416405254814 
},
{
 "name": "sample",
"mean": 0.006550314146341,
"sd": 0.017626694652 
},
{
 "name": "sample",
"mean": 0.006549117073171,
"sd": 0.01606600062485 
},
{
 "name": "sample",
"mean": 0.006600885853659,
"sd": 0.0236897096639 
},
{
 "name": "sample",
"mean": 0.006454691707317,
"sd": 0.01531868581249 
},
{
 "name": "sample",
"mean": 0.004955770731707,
"sd": 0.01083172143473 
},
{
 "name": "sample",
"mean": 0.006449739512195,
"sd": 0.01943852639235 
},
{
 "name": "sample",
"mean": 0.006050242926829,
"sd": 0.01544188664168 
},
{
 "name": "sample",
"mean": 0.006184529756098,
"sd": 0.01517804872854 
},
{
 "name": "sample",
"mean": 0.00654228195122,
"sd": 0.02027508121217 
},
{
 "name": "sample",
"mean": 0.006297112195122,
"sd": 0.01392772720254 
},
{
 "name": "sample",
"mean": 0.005588887804878,
"sd": 0.009371659743354 
},
{
 "name": "sample",
"mean": 0.006024872195122,
"sd": 0.01376249758127 
},
{
 "name": "sample",
"mean": 0.00524220195122,
"sd": 0.009962931642313 
},
{
 "name": "sample",
"mean": 0.006412414634146,
"sd": 0.01366558350157 
},
{
 "name": "sample",
"mean": 0.005713209756098,
"sd": 0.01045364422887 
},
{
 "name": "sample",
"mean": 0.006080713170732,
"sd": 0.01297741132448 
},
{
 "name": "sample",
"mean": 0.006622182439024,
"sd": 0.01704563548204 
},
{
 "name": "sample",
"mean": 0.006312985365854,
"sd": 0.01585752296407 
},
{
 "name": "sample",
"mean": 0.005528144390244,
"sd": 0.01263271905427 
},
{
 "name": "sample",
"mean": 0.005820582439024,
"sd": 0.01606980834217 
},
{
 "name": "sample",
"mean": 0.005351111219512,
"sd": 0.008502180814732 
},
{
 "name": "sample",
"mean": 0.006336427317073,
"sd": 0.01536034986013 
},
{
 "name": "sample",
"mean": 0.005820663414634,
"sd": 0.01042634314916 
},
{
 "name": "sample",
"mean": 0.00631119902439,
"sd": 0.01408023794118 
},
{
 "name": "sample",
"mean": 0.006873423414634,
"sd": 0.02469265154033 
},
{
 "name": "sample",
"mean": 0.006756204878049,
"sd": 0.0215098886444 
},
{
 "name": "sample",
"mean": 0.005286995121951,
"sd": 0.01400472860668 
},
{
 "name": "sample",
"mean": 0.006007886829268,
"sd": 0.01931675316492 
},
{
 "name": "sample",
"mean": 0.006870328780488,
"sd": 0.0244111758289 
},
{
 "name": "sample",
"mean": 0.006468148292683,
"sd": 0.01548832259109 
},
{
 "name": "sample",
"mean": 0.005277169756098,
"sd": 0.01605273468607 
},
{
 "name": "sample",
"mean": 0.00511103902439,
"sd": 0.01905612718785 
},
{
 "name": "sample",
"mean": 0.005255533658537,
"sd": 0.01648005339702 
},
{
 "name": "sample",
"mean": 0.005330255609756,
"sd": 0.01574495758908 
},
{
 "name": "sample",
"mean": 0.005637414634146,
"sd": 0.01011630672427 
},
{
 "name": "sample",
"mean": 0.005799148292683,
"sd": 0.01112525085674 
},
{
 "name": "sample",
"mean": 0.006521588292683,
"sd": 0.01563856480563 
},
{
 "name": "sample",
"mean": 0.006514648780488,
"sd": 0.02327753213219 
},
{
 "name": "sample",
"mean": 0.005517502439024,
"sd": 0.008839922068164 
},
{
 "name": "sample",
"mean": 0.005673498536585,
"sd": 0.01042298928056 
},
{
 "name": "sample",
"mean": 0.005200556097561,
"sd": 0.01207662355237 
},
{
 "name": "sample",
"mean": 0.006961097560976,
"sd": 0.02869148080734 
},
{
 "name": "sample",
"mean": 0.006150455609756,
"sd": 0.01820468552761 
},
{
 "name": "sample",
"mean": 0.006325584390244,
"sd": 0.01529662342739 
},
{
 "name": "sample",
"mean": 0.006895791219512,
"sd": 0.02806137814817 
},
{
 "name": "sample",
"mean": 0.00525596097561,
"sd": 0.01329790237276 
},
{
 "name": "sample",
"mean": 0.006684419512195,
"sd": 0.01876295079768 
},
{
 "name": "sample",
"mean": 0.006859420487805,
"sd": 0.02326250248102 
},
{
 "name": "sample",
"mean": 0.00720752195122,
"sd": 0.03283250576602 
},
{
 "name": "sample",
"mean": 0.006634573658537,
"sd": 0.01854049491155 
},
{
 "name": "sample",
"mean": 0.00599911804878,
"sd": 0.01393257373412 
},
{
 "name": "sample",
"mean": 0.005835724878049,
"sd": 0.01926413484038 
},
{
 "name": "sample",
"mean": 0.005346887804878,
"sd": 0.008526556811636 
},
{
 "name": "sample",
"mean": 0.00707032195122,
"sd": 0.02882169308916 
},
{
 "name": "sample",
"mean": 0.006780097560976,
"sd": 0.02541927250741 
},
{
 "name": "sample",
"mean": 0.006549371707317,
"sd": 0.01787269821307 
},
{
 "name": "sample",
"mean": 0.006218301463415,
"sd": 0.01364655458145 
},
{
 "name": "sample",
"mean": 0.005738944390244,
"sd": 0.01363280968594 
},
{
 "name": "sample",
"mean": 0.004942073170732,
"sd": 0.01147916557454 
},
{
 "name": "sample",
"mean": 0.004948850731707,
"sd": 0.01171830827497 
},
{
 "name": "sample",
"mean": 0.006492396097561,
"sd": 0.01689185587091 
},
{
 "name": "sample",
"mean": 0.006217644878049,
"sd": 0.01594126472796 
},
{
 "name": "sample",
"mean": 0.005425093658537,
"sd": 0.01675326229919 
},
{
 "name": "sample",
"mean": 0.007166379512195,
"sd": 0.02977243938337 
},
{
 "name": "sample",
"mean": 0.005996413658537,
"sd": 0.01291471590631 
},
{
 "name": "sample",
"mean": 0.005480020487805,
"sd": 0.008878069436305 
},
{
 "name": "sample",
"mean": 0.005529570731707,
"sd": 0.01449323555971 
},
{
 "name": "sample",
"mean": 0.005402341463415,
"sd": 0.01340574851968 
},
{
 "name": "sample",
"mean": 0.005469446829268,
"sd": 0.008849605213846 
},
{
 "name": "sample",
"mean": 0.005465450731707,
"sd": 0.009511276145082 
},
{
 "name": "sample",
"mean": 0.005307082926829,
"sd": 0.01899606674275 
},
{
 "name": "sample",
"mean": 0.007083197073171,
"sd": 0.03292960049003 
},
{
 "name": "sample",
"mean": 0.006361876097561,
"sd": 0.01484929489374 
},
{
 "name": "sample",
"mean": 0.005038503414634,
"sd": 0.02270352753832 
},
{
 "name": "sample",
"mean": 0.005350065365854,
"sd": 0.008506419693075 
},
{
 "name": "sample",
"mean": 0.00618583902439,
"sd": 0.01477450721606 
},
{
 "name": "sample",
"mean": 0.005763311219512,
"sd": 0.009910972788136 
},
{
 "name": "sample",
"mean": 0.005736687804878,
"sd": 0.01401231924124 
},
{
 "name": "sample",
"mean": 0.005267412682927,
"sd": 0.0192177463837 
},
{
 "name": "sample",
"mean": 0.005479328780488,
"sd": 0.009227588305717 
},
{
 "name": "sample",
"mean": 0.007075450731707,
"sd": 0.03126502429852 
},
{
 "name": "sample",
"mean": 0.005191931707317,
"sd": 0.01343787448179 
},
{
 "name": "sample",
"mean": 0.005049583414634,
"sd": 0.02272926438452 
},
{
 "name": "sample",
"mean":     0.00508688,
"sd": 0.0225794715149 
},
{
 "name": "sample",
"mean": 0.005867830243902,
"sd": 0.01458839716813 
},
{
 "name": "sample",
"mean": 0.005104083902439,
"sd": 0.01152698893999 
},
{
 "name": "sample",
"mean": 0.005773033170732,
"sd": 0.01302269904659 
},
{
 "name": "sample",
"mean": 0.00699120195122,
"sd": 0.0285446341983 
},
{
 "name": "sample",
"mean": 0.005430070243902,
"sd": 0.01471769038484 
},
{
 "name": "sample",
"mean": 0.005842142439024,
"sd": 0.01214632274352 
},
{
 "name": "sample",
"mean": 0.005752267317073,
"sd": 0.01442649117773 
},
{
 "name": "sample",
"mean": 0.005410138536585,
"sd": 0.01086380817586 
},
{
 "name": "sample",
"mean": 0.005529729756098,
"sd": 0.008896472160122 
},
{
 "name": "sample",
"mean": 0.006112194146341,
"sd": 0.01437753358645 
},
{
 "name": "sample",
"mean": 0.005450954146341,
"sd": 0.01313172671377 
},
{
 "name": "sample",
"mean": 0.00644863804878,
"sd": 0.01516263638737 
},
{
 "name": "sample",
"mean": 0.006091214634146,
"sd": 0.02049998363566 
},
{
 "name": "sample",
"mean": 0.005344381463415,
"sd": 0.01079399769205 
},
{
 "name": "sample",
"mean": 0.005357968780488,
"sd": 0.01870552312297 
},
{
 "name": "sample",
"mean": 0.00669155804878,
"sd": 0.01837213242211 
},
{
 "name": "sample",
"mean": 0.005826332682927,
"sd": 0.01391806461137 
},
{
 "name": "sample",
"mean": 0.005038829268293,
"sd": 0.01081988704156 
},
{
 "name": "sample",
"mean": 0.005803451707317,
"sd": 0.01659919980673 
},
{
 "name": "sample",
"mean": 0.006390778536585,
"sd": 0.02170946897256 
},
{
 "name": "sample",
"mean": 0.00611411804878,
"sd": 0.01237949863062 
},
{
 "name": "sample",
"mean": 0.006411306341463,
"sd": 0.02465689476191 
},
{
 "name": "sample",
"mean": 0.005788180487805,
"sd": 0.01204672049876 
},
{
 "name": "sample",
"mean": 0.006406818536585,
"sd": 0.01499644889513 
},
{
 "name": "sample",
"mean": 0.005315652682927,
"sd": 0.01176079362815 
},
{
 "name": "sample",
"mean": 0.006022348292683,
"sd": 0.01249527055483 
},
{
 "name": "sample",
"mean": 0.006993111219512,
"sd": 0.02966511373286 
},
{
 "name": "sample",
"mean": 0.00567664195122,
"sd": 0.01009072133095 
},
{
 "name": "sample",
"mean": 0.00618351804878,
"sd": 0.01358958442171 
},
{
 "name": "sample",
"mean": 0.005578192195122,
"sd": 0.01430398942715 
},
{
 "name": "sample",
"mean": 0.005052567804878,
"sd": 0.01300825940369 
},
{
 "name": "sample",
"mean": 0.005915623414634,
"sd": 0.01160950328517 
},
{
 "name": "sample",
"mean": 0.006250502439024,
"sd": 0.01330061703962 
},
{
 "name": "sample",
"mean": 0.006260187317073,
"sd": 0.01416436317968 
},
{
 "name": "sample",
"mean": 0.006249179512195,
"sd": 0.01637517263226 
},
{
 "name": "sample",
"mean": 0.005419310243902,
"sd": 0.01151176060558 
},
{
 "name": "sample",
"mean": 0.005751923902439,
"sd": 0.01705918546103 
},
{
 "name": "sample",
"mean": 0.005462006829268,
"sd": 0.01487811949731 
},
{
 "name": "sample",
"mean": 0.005491215609756,
"sd": 0.008903247542316 
},
{
 "name": "sample",
"mean": 0.006338245853659,
"sd": 0.02044088985317 
},
{
 "name": "sample",
"mean":      0.0062616,
"sd": 0.01419623055725 
},
{
 "name": "sample",
"mean": 0.006173545365854,
"sd": 0.01376134725432 
},
{
 "name": "sample",
"mean": 0.006040369756098,
"sd": 0.01365859498562 
},
{
 "name": "sample",
"mean": 0.006113772682927,
"sd": 0.01974396218258 
},
{
 "name": "sample",
"mean": 0.006005170731707,
"sd": 0.01984211397704 
},
{
 "name": "sample",
"mean": 0.005590982439024,
"sd": 0.01441849517561 
},
{
 "name": "sample",
"mean": 0.006439553170732,
"sd": 0.01759544829438 
},
{
 "name": "sample",
"mean": 0.006253613658537,
"sd": 0.01406948556585 
},
{
 "name": "sample",
"mean": 0.005143269268293,
"sd": 0.01879599718039 
},
{
 "name": "sample",
"mean": 0.00642867804878,
"sd": 0.01401966886045 
},
{
 "name": "sample",
"mean": 0.006770211707317,
"sd": 0.02731057946498 
},
{
 "name": "sample",
"mean": 0.006421804878049,
"sd": 0.01487690061374 
},
{
 "name": "sample",
"mean": 0.005690140487805,
"sd": 0.01445862891346 
},
{
 "name": "sample",
"mean": 0.005605170731707,
"sd": 0.01011872205635 
},
{
 "name": "sample",
"mean": 0.006039908292683,
"sd": 0.01155743947649 
},
{
 "name": "sample",
"mean": 0.005290053658537,
"sd": 0.01209991312186 
},
{
 "name": "sample",
"mean": 0.006233339512195,
"sd": 0.02282975365298 
},
{
 "name": "sample",
"mean": 0.005160813658537,
"sd": 0.01179377523438 
},
{
 "name": "sample",
"mean": 0.006336796097561,
"sd": 0.01342227532259 
},
{
 "name": "sample",
"mean": 0.005150983414634,
"sd": 0.009462685254255 
},
{
 "name": "sample",
"mean": 0.005064808780488,
"sd": 0.02168913441966 
},
{
 "name": "sample",
"mean": 0.00582224097561,
"sd": 0.01587981319943 
},
{
 "name": "sample",
"mean": 0.005745780487805,
"sd": 0.01369821613171 
},
{
 "name": "sample",
"mean": 0.006405542439024,
"sd": 0.01433341769716 
},
{
 "name": "sample",
"mean": 0.00512192195122,
"sd": 0.01538095756384 
},
{
 "name": "sample",
"mean": 0.006083162926829,
"sd": 0.01244768666999 
},
{
 "name": "sample",
"mean": 0.005483726829268,
"sd": 0.01278030498896 
},
{
 "name": "sample",
"mean": 0.004933728780488,
"sd": 0.01206163350051 
},
{
 "name": "sample",
"mean": 0.005656122926829,
"sd": 0.01064253997153 
},
{
 "name": "sample",
"mean": 0.005387164878049,
"sd": 0.01029190205895 
},
{
 "name": "sample",
"mean": 0.005689377560976,
"sd": 0.01895464454098 
},
{
 "name": "sample",
"mean": 0.005201002926829,
"sd": 0.01751887473456 
},
{
 "name": "sample",
"mean": 0.006651047804878,
"sd": 0.02407115153517 
},
{
 "name": "sample",
"mean": 0.004977685853659,
"sd": 0.01294746651212 
},
{
 "name": "sample",
"mean": 0.006966772682927,
"sd": 0.03054299127933 
},
{
 "name": "sample",
"mean": 0.006418206829268,
"sd": 0.01730434822239 
},
{
 "name": "sample",
"mean": 0.004983580487805,
"sd": 0.01220449807965 
},
{
 "name": "sample",
"mean": 0.006171682926829,
"sd": 0.01486336820535 
},
{
 "name": "sample",
"mean": 0.00637628097561,
"sd": 0.01609761628002 
},
{
 "name": "sample",
"mean": 0.00496752097561,
"sd": 0.01213830515739 
},
{
 "name": "sample",
"mean": 0.00559368195122,
"sd": 0.01065902966034 
},
{
 "name": "sample",
"mean": 0.006675030243902,
"sd": 0.02383541878717 
},
{
 "name": "sample",
"mean": 0.005579937560976,
"sd": 0.01387089591812 
},
{
 "name": "sample",
"mean": 0.006426698536585,
"sd": 0.01384335739303 
},
{
 "name": "sample",
"mean": 0.005026494634146,
"sd": 0.01306963901814 
},
{
 "name": "sample",
"mean": 0.00617291804878,
"sd": 0.02080923118518 
},
{
 "name": "sample",
"mean": 0.005977094634146,
"sd": 0.01582388392644 
},
{
 "name": "sample",
"mean": 0.005688334634146,
"sd": 0.01131528097672 
},
{
 "name": "sample",
"mean": 0.005030650731707,
"sd": 0.01214297294987 
},
{
 "name": "sample",
"mean": 0.005350304390244,
"sd": 0.01872851689841 
},
{
 "name": "sample",
"mean":     0.00526932,
"sd": 0.008465491699874 
},
{
 "name": "sample",
"mean": 0.005793511219512,
"sd": 0.01305991287512 
},
{
 "name": "sample",
"mean": 0.006350467317073,
"sd": 0.01506622085754 
},
{
 "name": "sample",
"mean": 0.005645273170732,
"sd": 0.009417975899431 
},
{
 "name": "sample",
"mean": 0.005330699512195,
"sd": 0.02114725033483 
},
{
 "name": "sample",
"mean": 0.006378656585366,
"sd": 0.01575682752508 
},
{
 "name": "sample",
"mean": 0.006479591219512,
"sd": 0.02356441283733 
},
{
 "name": "sample",
"mean": 0.005376863414634,
"sd": 0.02066971175381 
},
{
 "name": "sample",
"mean": 0.005718471219512,
"sd": 0.01512963686587 
},
{
 "name": "sample",
"mean": 0.006156832195122,
"sd": 0.01421575086145 
},
{
 "name": "sample",
"mean": 0.006425885853659,
"sd": 0.02216153719408 
},
{
 "name": "sample",
"mean":     0.00569308,
"sd": 0.01078030980203 
},
{
 "name": "sample",
"mean": 0.005283835121951,
"sd": 0.01895415014525 
},
{
 "name": "sample",
"mean": 0.004982089756098,
"sd": 0.0148896170594 
},
{
 "name": "sample",
"mean": 0.005989311219512,
"sd": 0.01497905978695 
},
{
 "name": "sample",
"mean":     0.00609216,
"sd": 0.01617392166573 
},
{
 "name": "sample",
"mean": 0.006392629268293,
"sd": 0.01428083259097 
},
{
 "name": "sample",
"mean": 0.005650268292683,
"sd": 0.0145494979679 
},
{
 "name": "sample",
"mean": 0.006190395121951,
"sd": 0.01450437948856 
},
{
 "name": "sample",
"mean": 0.006034883902439,
"sd": 0.01483721080692 
},
{
 "name": "sample",
"mean": 0.004968581463415,
"sd": 0.02130961756652 
},
{
 "name": "sample",
"mean": 0.005250327804878,
"sd": 0.009493959401489 
},
{
 "name": "sample",
"mean": 0.005150104390244,
"sd": 0.01895215463034 
},
{
 "name": "sample",
"mean": 0.005684864390244,
"sd": 0.01270802050918 
},
{
 "name": "sample",
"mean": 0.005370667317073,
"sd": 0.01806508655578 
},
{
 "name": "sample",
"mean": 0.00627839902439,
"sd": 0.01419057084558 
},
{
 "name": "sample",
"mean": 0.00540512195122,
"sd": 0.01692611914308 
},
{
 "name": "sample",
"mean": 0.006555003902439,
"sd": 0.01763601549367 
},
{
 "name": "sample",
"mean": 0.005833546341463,
"sd": 0.01154596276785 
},
{
 "name": "sample",
"mean": 0.007120628292683,
"sd": 0.03219713941051 
},
{
 "name": "sample",
"mean": 0.006167446829268,
"sd": 0.0141077230154 
},
{
 "name": "sample",
"mean": 0.007244328780488,
"sd": 0.03299956551235 
},
{
 "name": "sample",
"mean": 0.005972578536585,
"sd": 0.01562251140921 
},
{
 "name": "sample",
"mean": 0.005813246829268,
"sd": 0.01346639704615 
},
{
 "name": "sample",
"mean":     0.00589372,
"sd": 0.01345070389696 
},
{
 "name": "sample",
"mean": 0.005540703414634,
"sd": 0.01048381383339 
},
{
 "name": "sample",
"mean": 0.006504749268293,
"sd": 0.01778840557678 
},
{
 "name": "sample",
"mean": 0.00529867902439,
"sd": 0.01139991173635 
},
{
 "name": "sample",
"mean": 0.007015985365854,
"sd": 0.0308540946194 
},
{
 "name": "sample",
"mean": 0.006132220487805,
"sd": 0.01552110355431 
},
{
 "name": "sample",
"mean": 0.005319769756098,
"sd": 0.01129325602072 
},
{
 "name": "sample",
"mean": 0.005176845853659,
"sd": 0.01677865453698 
},
{
 "name": "sample",
"mean": 0.005089986341463,
"sd": 0.01243418532487 
},
{
 "name": "sample",
"mean": 0.006701474146341,
"sd": 0.02844559512037 
},
{
 "name": "sample",
"mean": 0.005718983414634,
"sd": 0.01860521136985 
},
{
 "name": "sample",
"mean": 0.00603495902439,
"sd": 0.01272033878885 
},
{
 "name": "sample",
"mean": 0.006144070243902,
"sd": 0.01516846561642 
},
{
 "name": "sample",
"mean": 0.006285809756098,
"sd": 0.01504637205346 
},
{
 "name": "sample",
"mean": 0.005117592195122,
"sd": 0.01527165320818 
},
{
 "name": "sample",
"mean": 0.006208274146341,
"sd": 0.01335771586622 
},
{
 "name": "sample",
"mean": 0.006808849756098,
"sd": 0.02150714941968 
},
{
 "name": "sample",
"mean": 0.005975850731707,
"sd": 0.01708460734043 
},
{
 "name": "sample",
"mean": 0.005881849756098,
"sd": 0.01401113811328 
},
{
 "name": "sample",
"mean": 0.005794392195122,
"sd": 0.01137984487073 
},
{
 "name": "sample",
"mean": 0.005401710243902,
"sd": 0.01476978339772 
},
{
 "name": "sample",
"mean": 0.005484510243902,
"sd": 0.01161975567932 
},
{
 "name": "sample",
"mean": 0.005496580487805,
"sd": 0.009535356206596 
},
{
 "name": "sample",
"mean": 0.005022807804878,
"sd": 0.01281985318249 
},
{
 "name": "sample",
"mean": 0.005320611707317,
"sd": 0.0177141644317 
},
{
 "name": "sample",
"mean": 0.005573370731707,
"sd": 0.008983640370157 
},
{
 "name": "sample",
"mean": 0.005817985365854,
"sd": 0.01955069874274 
},
{
 "name": "sample",
"mean": 0.004978524878049,
"sd": 0.01213506012769 
},
{
 "name": "sample",
"mean":      0.0052598,
"sd": 0.01761677400341 
},
{
 "name": "sample",
"mean": 0.005071344390244,
"sd": 0.01252373387402 
},
{
 "name": "sample",
"mean": 0.005857053658537,
"sd": 0.01281198733122 
},
{
 "name": "sample",
"mean": 0.005900823414634,
"sd": 0.01209899747781 
},
{
 "name": "sample",
"mean": 0.005037651707317,
"sd": 0.02209049351397 
},
{
 "name": "sample",
"mean": 0.006104135609756,
"sd": 0.01717267525813 
},
{
 "name": "sample",
"mean": 0.00519559902439,
"sd": 0.0123607023884 
},
{
 "name": "sample",
"mean": 0.004871477073171,
"sd": 0.01166984800424 
},
{
 "name": "sample",
"mean": 0.006101651707317,
"sd": 0.01711222799494 
},
{
 "name": "sample",
"mean": 0.006235414634146,
"sd": 0.01367052531544 
},
{
 "name": "sample",
"mean": 0.005173831219512,
"sd": 0.02020613725526 
},
{
 "name": "sample",
"mean": 0.005842286829268,
"sd": 0.01956868512296 
},
{
 "name": "sample",
"mean": 0.005556564878049,
"sd": 0.01158458221991 
},
{
 "name": "sample",
"mean": 0.005403807804878,
"sd": 0.01570442185716 
},
{
 "name": "sample",
"mean": 0.006180168780488,
"sd": 0.01532433093347 
},
{
 "name": "sample",
"mean": 0.005390934634146,
"sd": 0.008607404621411 
},
{
 "name": "sample",
"mean": 0.005042150243902,
"sd": 0.01633208525768 
},
{
 "name": "sample",
"mean": 0.006631456585366,
"sd": 0.02229702825785 
},
{
 "name": "sample",
"mean": 0.00659739804878,
"sd": 0.01781625187527 
},
{
 "name": "sample",
"mean": 0.004963051707317,
"sd": 0.01711356142008 
},
{
 "name": "sample",
"mean": 0.005236451707317,
"sd": 0.01282879307153 
},
{
 "name": "sample",
"mean": 0.006136994146341,
"sd": 0.02154134340422 
},
{
 "name": "sample",
"mean": 0.005503366829268,
"sd": 0.01205890837651 
},
{
 "name": "sample",
"mean": 0.006393964878049,
"sd": 0.01749279689756 
},
{
 "name": "sample",
"mean": 0.006175124878049,
"sd": 0.01560720945269 
},
{
 "name": "sample",
"mean": 0.005952448780488,
"sd": 0.01127035409361 
},
{
 "name": "sample",
"mean": 0.006401495609756,
"sd": 0.01926852191071 
},
{
 "name": "sample",
"mean": 0.006205043902439,
"sd": 0.0200153929495 
},
{
 "name": "sample",
"mean": 0.00655351804878,
"sd": 0.02128000194348 
},
{
 "name": "sample",
"mean": 0.005267653658537,
"sd": 0.0132444524856 
},
{
 "name": "sample",
"mean":      0.0073046,
"sd": 0.03492883883327 
},
{
 "name": "sample",
"mean": 0.005427791219512,
"sd": 0.00910101659248 
},
{
 "name": "sample",
"mean": 0.005856329756098,
"sd": 0.01006680420784 
},
{
 "name": "sample",
"mean": 0.005903363902439,
"sd": 0.01860923521794 
},
{
 "name": "sample",
"mean": 0.005992270243902,
"sd": 0.01414602425624 
},
{
 "name": "sample",
"mean": 0.005236277073171,
"sd": 0.009788907451169 
},
{
 "name": "sample",
"mean": 0.005566347317073,
"sd": 0.008975918292699 
},
{
 "name": "sample",
"mean": 0.005815630243902,
"sd": 0.01818347601459 
},
{
 "name": "sample",
"mean": 0.005309373658537,
"sd": 0.02068354780716 
},
{
 "name": "sample",
"mean": 0.006631802926829,
"sd": 0.02477342328092 
},
{
 "name": "sample",
"mean": 0.005094016585366,
"sd": 0.02039300065796 
},
{
 "name": "sample",
"mean": 0.005036221463415,
"sd": 0.01052172037843 
},
{
 "name": "sample",
"mean": 0.006061826341463,
"sd": 0.01503910080147 
},
{
 "name": "sample",
"mean": 0.00601736195122,
"sd": 0.0147217864252 
},
{
 "name": "sample",
"mean": 0.006051997073171,
"sd": 0.01386096496391 
},
{
 "name": "sample",
"mean": 0.007236566829268,
"sd": 0.03489280569172 
},
{
 "name": "sample",
"mean": 0.005886379512195,
"sd": 0.01553392117095 
},
{
 "name": "sample",
"mean": 0.005250992195122,
"sd": 0.021582158573 
},
{
 "name": "sample",
"mean": 0.005387212682927,
"sd": 0.01885969095264 
},
{
 "name": "sample",
"mean": 0.005497635121951,
"sd": 0.01018424460321 
},
{
 "name": "sample",
"mean": 0.006182007804878,
"sd": 0.01338517654242 
},
{
 "name": "sample",
"mean": 0.005320968780488,
"sd": 0.009464081255318 
},
{
 "name": "sample",
"mean": 0.00626468195122,
"sd": 0.01982035467252 
},
{
 "name": "sample",
"mean": 0.006774502439024,
"sd": 0.02586236756434 
},
{
 "name": "sample",
"mean": 0.005098709268293,
"sd": 0.01388701848118 
},
{
 "name": "sample",
"mean": 0.005874354146341,
"sd": 0.01054697101692 
},
{
 "name": "sample",
"mean": 0.005354254634146,
"sd": 0.008849414173706 
},
{
 "name": "sample",
"mean": 0.007039482926829,
"sd": 0.02884190790264 
},
{
 "name": "sample",
"mean": 0.005764978536585,
"sd": 0.01324313722422 
},
{
 "name": "sample",
"mean": 0.006329530731707,
"sd": 0.01671565081782 
},
{
 "name": "sample",
"mean": 0.006392979512195,
"sd": 0.01647844759835 
},
{
 "name": "sample",
"mean": 0.006563303414634,
"sd": 0.02144886259944 
},
{
 "name": "sample",
"mean": 0.005351100487805,
"sd": 0.008757753442424 
},
{
 "name": "sample",
"mean": 0.006229938536585,
"sd": 0.01485326192981 
},
{
 "name": "sample",
"mean": 0.005252174634146,
"sd": 0.01327074220513 
},
{
 "name": "sample",
"mean": 0.005705615609756,
"sd": 0.01133114550619 
},
{
 "name": "sample",
"mean": 0.006533589268293,
"sd": 0.01838572344748 
},
{
 "name": "sample",
"mean": 0.006019739512195,
"sd": 0.01359927417699 
},
{
 "name": "sample",
"mean": 0.006146812682927,
"sd": 0.01305071537775 
},
{
 "name": "sample",
"mean": 0.00582488097561,
"sd": 0.01197329384557 
},
{
 "name": "sample",
"mean": 0.005354945365854,
"sd": 0.01679871478698 
},
{
 "name": "sample",
"mean": 0.005030603902439,
"sd": 0.01947444022349 
},
{
 "name": "sample",
"mean": 0.005987994146341,
"sd": 0.01602678331508 
},
{
 "name": "sample",
"mean": 0.005786089756098,
"sd": 0.01056872381179 
},
{
 "name": "sample",
"mean": 0.005116597073171,
"sd": 0.01323915478502 
},
{
 "name": "sample",
"mean": 0.005981455609756,
"sd": 0.01322292983769 
},
{
 "name": "sample",
"mean": 0.006606625365854,
"sd": 0.02391901266538 
},
{
 "name": "sample",
"mean": 0.005640488780488,
"sd": 0.01326626872886 
},
{
 "name": "sample",
"mean": 0.006336904390244,
"sd": 0.0147928873554 
},
{
 "name": "sample",
"mean": 0.006500042926829,
"sd": 0.01470831810593 
},
{
 "name": "sample",
"mean": 0.006822948292683,
"sd": 0.02546781815421 
},
{
 "name": "sample",
"mean": 0.005747817560976,
"sd": 0.01299241899586 
},
{
 "name": "sample",
"mean": 0.005911735609756,
"sd": 0.01260075333993 
},
{
 "name": "sample",
"mean": 0.006761583414634,
"sd": 0.0215729589015 
},
{
 "name": "sample",
"mean": 0.005997447804878,
"sd": 0.01687244350043 
},
{
 "name": "sample",
"mean": 0.00643980195122,
"sd": 0.01567082869736 
},
{
 "name": "sample",
"mean": 0.005928045853659,
"sd": 0.01535980312374 
},
{
 "name": "sample",
"mean": 0.006660106341463,
"sd": 0.02251993989414 
},
{
 "name": "sample",
"mean": 0.006784149268293,
"sd": 0.02438654348295 
},
{
 "name": "sample",
"mean": 0.006061831219512,
"sd": 0.01471243200399 
},
{
 "name": "sample",
"mean": 0.006481141463415,
"sd": 0.02085882590174 
},
{
 "name": "sample",
"mean":     0.00645224,
"sd": 0.01886964446032 
},
{
 "name": "sample",
"mean": 0.00636359804878,
"sd": 0.01959833840602 
},
{
 "name": "sample",
"mean": 0.005940271219512,
"sd": 0.01446715803122 
},
{
 "name": "sample",
"mean": 0.005492468292683,
"sd": 0.01129694093037 
},
{
 "name": "sample",
"mean": 0.007237488780488,
"sd": 0.03362961228478 
},
{
 "name": "sample",
"mean": 0.005198244878049,
"sd": 0.01163783363463 
},
{
 "name": "sample",
"mean": 0.005557865365854,
"sd": 0.01551830751401 
},
{
 "name": "sample",
"mean": 0.006341965853659,
"sd": 0.01498653954668 
},
{
 "name": "sample",
"mean": 0.006297242926829,
"sd": 0.02068284303773 
},
{
 "name": "sample",
"mean": 0.005508842926829,
"sd": 0.01051821256459 
},
{
 "name": "sample",
"mean": 0.006242743414634,
"sd": 0.01717552145555 
},
{
 "name": "sample",
"mean": 0.005862827317073,
"sd": 0.01165423104286 
},
{
 "name": "sample",
"mean": 0.00579831804878,
"sd": 0.0166570688018 
},
{
 "name": "sample",
"mean": 0.006049387317073,
"sd": 0.01591248413557 
},
{
 "name": "sample",
"mean": 0.007140873170732,
"sd": 0.03189977383754 
},
{
 "name": "sample",
"mean": 0.006280703414634,
"sd": 0.0226844762769 
},
{
 "name": "sample",
"mean": 0.005224030243902,
"sd": 0.01293119279281 
},
{
 "name": "sample",
"mean": 0.005533991219512,
"sd": 0.02052057527556 
},
{
 "name": "sample",
"mean": 0.005397646829268,
"sd": 0.01287666966246 
},
{
 "name": "sample",
"mean": 0.005459673170732,
"sd": 0.01947603284767 
},
{
 "name": "sample",
"mean": 0.005072661463415,
"sd": 0.01822212287804 
},
{
 "name": "sample",
"mean": 0.005517704390244,
"sd": 0.01000839539157 
},
{
 "name": "sample",
"mean": 0.007113870243902,
"sd": 0.03042410355557 
},
{
 "name": "sample",
"mean": 0.006078368780488,
"sd": 0.01639235812242 
},
{
 "name": "sample",
"mean": 0.005960729756098,
"sd": 0.01340996387927 
},
{
 "name": "sample",
"mean": 0.005258096585366,
"sd": 0.01102744037454 
},
{
 "name": "sample",
"mean": 0.005175666341463,
"sd": 0.01166012270135 
},
{
 "name": "sample",
"mean": 0.006104913170732,
"sd": 0.01423767300212 
},
{
 "name": "sample",
"mean": 0.005208705365854,
"sd": 0.01334266012764 
},
{
 "name": "sample",
"mean":     0.00708308,
"sd": 0.03244984590958 
},
{
 "name": "sample",
"mean": 0.006267810731707,
"sd": 0.0144096000688 
},
{
 "name": "sample",
"mean": 0.006806613658537,
"sd": 0.02130446489965 
},
{
 "name": "sample",
"mean": 0.005479140487805,
"sd": 0.01394977389371 
},
{
 "name": "sample",
"mean": 0.006185812682927,
"sd": 0.01596065587812 
},
{
 "name": "sample",
"mean": 0.006145384390244,
"sd": 0.01436470130816 
},
{
 "name": "sample",
"mean": 0.006115097560976,
"sd": 0.01500320206774 
},
{
 "name": "sample",
"mean": 0.006398170731707,
"sd": 0.01789667381025 
},
{
 "name": "sample",
"mean": 0.006337732682927,
"sd": 0.01530029324462 
},
{
 "name": "sample",
"mean": 0.005884696585366,
"sd": 0.01215183383015 
},
{
 "name": "sample",
"mean": 0.005386883902439,
"sd": 0.01190303536473 
},
{
 "name": "sample",
"mean": 0.005050492682927,
"sd": 0.01116009683413 
},
{
 "name": "sample",
"mean": 0.005146460487805,
"sd": 0.01753137733686 
},
{
 "name": "sample",
"mean": 0.006977763902439,
"sd": 0.02980575167019 
},
{
 "name": "sample",
"mean": 0.007222233170732,
"sd": 0.03216769030418 
},
{
 "name": "sample",
"mean": 0.006460865365854,
"sd": 0.01724005163697 
},
{
 "name": "sample",
"mean": 0.007063567804878,
"sd": 0.03124976740329 
},
{
 "name": "sample",
"mean": 0.005138791219512,
"sd": 0.01800749380515 
},
{
 "name": "sample",
"mean": 0.005698072195122,
"sd": 0.01300561371405 
},
{
 "name": "sample",
"mean": 0.005330773658537,
"sd": 0.008808161215594 
},
{
 "name": "sample",
"mean": 0.005175242926829,
"sd": 0.01659610098889 
},
{
 "name": "sample",
"mean": 0.007226808780488,
"sd": 0.03394357535813 
},
{
 "name": "sample",
"mean": 0.006547036097561,
"sd": 0.01839957324124 
},
{
 "name": "sample",
"mean": 0.005100725853659,
"sd": 0.02234299759209 
},
{
 "name": "sample",
"mean": 0.007234475121951,
"sd": 0.03284042696977 
},
{
 "name": "sample",
"mean": 0.005558493658537,
"sd": 0.009395735321933 
},
{
 "name": "sample",
"mean": 0.006499168780488,
"sd": 0.02477850867521 
},
{
 "name": "sample",
"mean":     0.00726124,
"sd": 0.03471465212919 
},
{
 "name": "sample",
"mean": 0.005161510243902,
"sd": 0.01148678224068 
},
{
 "name": "sample",
"mean": 0.005974586341463,
"sd": 0.01978780439546 
},
{
 "name": "sample",
"mean": 0.004937220487805,
"sd": 0.01256098715764 
},
{
 "name": "sample",
"mean": 0.005207172682927,
"sd": 0.01855718431049 
},
{
 "name": "sample",
"mean": 0.006551315121951,
"sd": 0.01790520800843 
},
{
 "name": "sample",
"mean": 0.005660819512195,
"sd": 0.00984417653155 
},
{
 "name": "sample",
"mean": 0.005431575609756,
"sd": 0.01761000285631 
},
{
 "name": "sample",
"mean":     0.00613512,
"sd": 0.01377569784686 
},
{
 "name": "sample",
"mean": 0.007219863414634,
"sd": 0.03266413042484 
},
{
 "name": "sample",
"mean": 0.006053130731707,
"sd": 0.01921290531719 
},
{
 "name": "sample",
"mean": 0.006081474146341,
"sd": 0.01290059588923 
},
{
 "name": "sample",
"mean": 0.006502684878049,
"sd": 0.01661295619016 
},
{
 "name": "sample",
"mean": 0.005451221463415,
"sd": 0.01712156280345 
},
{
 "name": "sample",
"mean": 0.006198789268293,
"sd": 0.01505628072063 
},
{
 "name": "sample",
"mean": 0.006463612682927,
"sd": 0.01658485214191 
},
{
 "name": "sample",
"mean": 0.006236217560976,
"sd": 0.01459847922569 
},
{
 "name": "sample",
"mean": 0.005102456585366,
"sd": 0.02159986727819 
},
{
 "name": "sample",
"mean":      0.0054522,
"sd": 0.009058079909077 
},
{
 "name": "sample",
"mean": 0.005747745365854,
"sd": 0.01281873108563 
},
{
 "name": "sample",
"mean": 0.006373164878049,
"sd": 0.01449428218256 
},
{
 "name": "sample",
"mean": 0.005016594146341,
"sd": 0.0126757185095 
},
{
 "name": "sample",
"mean": 0.005748419512195,
"sd": 0.01158453164717 
},
{
 "name": "sample",
"mean": 0.006500844878049,
"sd": 0.01820286095134 
},
{
 "name": "sample",
"mean": 0.006529185365854,
"sd": 0.02111384371872 
},
{
 "name": "sample",
"mean": 0.006442093658537,
"sd": 0.0169413049487 
},
{
 "name": "sample",
"mean": 0.004964269268293,
"sd": 0.02289872980593 
},
{
 "name": "sample",
"mean": 0.006487506341463,
"sd": 0.01729535738585 
},
{
 "name": "sample",
"mean": 0.006095054634146,
"sd": 0.01341353090469 
},
{
 "name": "sample",
"mean": 0.00653212195122,
"sd": 0.01554444907559 
},
{
 "name": "sample",
"mean":     0.00597252,
"sd": 0.01549147757029 
},
{
 "name": "sample",
"mean": 0.005194244878049,
"sd": 0.01242315383433 
},
{
 "name": "sample",
"mean": 0.00629044097561,
"sd": 0.01814888904815 
},
{
 "name": "sample",
"mean": 0.005032237073171,
"sd": 0.0225172302609 
},
{
 "name": "sample",
"mean": 0.005068381463415,
"sd": 0.01490681514006 
},
{
 "name": "sample",
"mean": 0.00598267804878,
"sd": 0.01423755854184 
},
{
 "name": "sample",
"mean": 0.006027873170732,
"sd": 0.01232953518933 
},
{
 "name": "sample",
"mean": 0.005527091707317,
"sd": 0.01239074396327 
},
{
 "name": "sample",
"mean": 0.005834485853659,
"sd": 0.01573242101523 
},
{
 "name": "sample",
"mean": 0.006075637073171,
"sd": 0.01398060766307 
},
{
 "name": "sample",
"mean": 0.005252035121951,
"sd": 0.02088634020206 
},
{
 "name": "sample",
"mean": 0.005890666341463,
"sd": 0.01404576745972 
},
{
 "name": "sample",
"mean": 0.006277486829268,
"sd": 0.01579841148968 
},
{
 "name": "sample",
"mean": 0.006174236097561,
"sd": 0.01401541070117 
},
{
 "name": "sample",
"mean": 0.005712383414634,
"sd": 0.01169183043986 
},
{
 "name": "sample",
"mean": 0.005611469268293,
"sd": 0.01655349756522 
},
{
 "name": "sample",
"mean": 0.006151660487805,
"sd": 0.01367161798963 
},
{
 "name": "sample",
"mean": 0.004976995121951,
"sd": 0.01271098831486 
},
{
 "name": "sample",
"mean": 0.005709020487805,
"sd": 0.01427735394032 
},
{
 "name": "sample",
"mean": 0.006044696585366,
"sd": 0.01400519893752 
},
{
 "name": "sample",
"mean": 0.004895345365854,
"sd": 0.0114580215228 
},
{
 "name": "sample",
"mean": 0.005356715121951,
"sd": 0.01664844611361 
},
{
 "name": "sample",
"mean": 0.005923990243902,
"sd": 0.0190645345144 
},
{
 "name": "sample",
"mean": 0.005014909268293,
"sd": 0.02334106262973 
},
{
 "name": "sample",
"mean": 0.006100316097561,
"sd": 0.01624712963981 
},
{
 "name": "sample",
"mean": 0.006963235121951,
"sd": 0.0292893092996 
},
{
 "name": "sample",
"mean": 0.006383196097561,
"sd": 0.01736433927999 
},
{
 "name": "sample",
"mean": 0.005018884878049,
"sd": 0.01551923972841 
},
{
 "name": "sample",
"mean": 0.006377907317073,
"sd": 0.01719651860465 
},
{
 "name": "sample",
"mean": 0.004957436097561,
"sd": 0.01398008776491 
},
{
 "name": "sample",
"mean": 0.006067683902439,
"sd": 0.01957454321931 
},
{
 "name": "sample",
"mean": 0.006498396097561,
"sd": 0.01894081764395 
},
{
 "name": "sample",
"mean": 0.006632982439024,
"sd": 0.02636129437662 
},
{
 "name": "sample",
"mean": 0.006319976585366,
"sd": 0.02185816896701 
},
{
 "name": "sample",
"mean": 0.006997044878049,
"sd": 0.02703176299098 
},
{
 "name": "sample",
"mean": 0.00586632097561,
"sd": 0.02002773941713 
},
{
 "name": "sample",
"mean": 0.004970568780488,
"sd": 0.01975584108717 
},
{
 "name": "sample",
"mean": 0.004939125853659,
"sd": 0.01164077564973 
},
{
 "name": "sample",
"mean": 0.006122704390244,
"sd": 0.01227593018694 
},
{
 "name": "sample",
"mean":     0.00600792,
"sd": 0.01876458118129 
},
{
 "name": "sample",
"mean": 0.00561539902439,
"sd": 0.0116274750318 
},
{
 "name": "sample",
"mean": 0.006381317073171,
"sd": 0.01591236596745 
},
{
 "name": "sample",
"mean": 0.005391156097561,
"sd": 0.0124069566946 
},
{
 "name": "sample",
"mean": 0.005269973658537,
"sd": 0.01198914676994 
},
{
 "name": "sample",
"mean": 0.006182531707317,
"sd": 0.014949894141 
},
{
 "name": "sample",
"mean": 0.00640715804878,
"sd": 0.01770842801521 
},
{
 "name": "sample",
"mean": 0.005912929756098,
"sd": 0.01313772340217 
},
{
 "name": "sample",
"mean": 0.006133349268293,
"sd": 0.01252003111356 
},
{
 "name": "sample",
"mean": 0.006407267317073,
"sd": 0.01466564800753 
},
{
 "name": "sample",
"mean": 0.005274703414634,
"sd": 0.008873693799766 
},
{
 "name": "sample",
"mean": 0.005640511219512,
"sd": 0.01023926929296 
},
{
 "name": "sample",
"mean": 0.006469791219512,
"sd": 0.0140162189827 
},
{
 "name": "sample",
"mean": 0.005416219512195,
"sd": 0.01928820336057 
},
{
 "name": "sample",
"mean": 0.006097543414634,
"sd": 0.01742138917618 
},
{
 "name": "sample",
"mean": 0.005305769756098,
"sd": 0.01938577927693 
},
{
 "name": "sample",
"mean": 0.005645762926829,
"sd": 0.01926864496152 
},
{
 "name": "sample",
"mean": 0.005185308292683,
"sd": 0.01733764865189 
},
{
 "name": "sample",
"mean": 0.005908362926829,
"sd": 0.01345435167435 
},
{
 "name": "sample",
"mean": 0.005007463414634,
"sd": 0.02185299852218 
},
{
 "name": "sample",
"mean": 0.00652751902439,
"sd": 0.0172562884314 
},
{
 "name": "sample",
"mean": 0.005006491707317,
"sd": 0.01194339921209 
},
{
 "name": "sample",
"mean": 0.006853766829268,
"sd": 0.02710403280022 
},
{
 "name": "sample",
"mean": 0.006168349268293,
"sd": 0.01561886176379 
},
{
 "name": "sample",
"mean": 0.006160813658537,
"sd": 0.01349747162426 
},
{
 "name": "sample",
"mean": 0.006024425365854,
"sd": 0.01714458434496 
},
{
 "name": "sample",
"mean": 0.00588723902439,
"sd": 0.01068810854752 
},
{
 "name": "sample",
"mean": 0.006341893658537,
"sd": 0.01485946380192 
},
{
 "name": "sample",
"mean": 0.006990704390244,
"sd": 0.02848454590185 
},
{
 "name": "sample",
"mean": 0.006518233170732,
"sd": 0.01830840102542 
},
{
 "name": "sample",
"mean":     0.00491504,
"sd": 0.01168640168192 
},
{
 "name": "sample",
"mean": 0.005559665365854,
"sd": 0.01173746416852 
},
{
 "name": "sample",
"mean": 0.006260229268293,
"sd": 0.01924009602617 
},
{
 "name": "sample",
"mean": 0.00644723804878,
"sd": 0.01543237973839 
},
{
 "name": "sample",
"mean": 0.005381139512195,
"sd": 0.009288201843697 
},
{
 "name": "sample",
"mean": 0.006209907317073,
"sd": 0.01454815964675 
},
{
 "name": "sample",
"mean": 0.005897701463415,
"sd": 0.01444730601696 
},
{
 "name": "sample",
"mean": 0.006205463414634,
"sd": 0.01545505533834 
},
{
 "name": "sample",
"mean": 0.006147272195122,
"sd": 0.0149815566594 
},
{
 "name": "sample",
"mean": 0.005681077073171,
"sd": 0.01268616146666 
},
{
 "name": "sample",
"mean": 0.005854347317073,
"sd": 0.01595243484565 
},
{
 "name": "sample",
"mean": 0.006478385365854,
"sd": 0.01790399915459 
},
{
 "name": "sample",
"mean": 0.006049004878049,
"sd": 0.01325732427144 
},
{
 "name": "sample",
"mean": 0.006498664390244,
"sd": 0.02624921361322 
},
{
 "name": "sample",
"mean": 0.005500612682927,
"sd": 0.01047158833704 
},
{
 "name": "sample",
"mean": 0.005817738536585,
"sd": 0.01341592301675 
},
{
 "name": "sample",
"mean": 0.005711380487805,
"sd": 0.01416719965427 
},
{
 "name": "sample",
"mean": 0.005006347317073,
"sd": 0.02017366047231 
},
{
 "name": "sample",
"mean": 0.005732331707317,
"sd": 0.01410073981234 
},
{
 "name": "sample",
"mean": 0.006496595121951,
"sd": 0.01431392910008 
},
{
 "name": "sample",
"mean": 0.005520049756098,
"sd": 0.009169911700564 
},
{
 "name": "sample",
"mean": 0.006076823414634,
"sd": 0.01170431445481 
},
{
 "name": "sample",
"mean": 0.005275128780488,
"sd": 0.009714758929416 
},
{
 "name": "sample",
"mean": 0.006016101463415,
"sd": 0.01284105051212 
},
{
 "name": "sample",
"mean": 0.00630947804878,
"sd": 0.01650856157591 
},
{
 "name": "sample",
"mean": 0.00562176195122,
"sd": 0.01013909753007 
},
{
 "name": "sample",
"mean": 0.00647867902439,
"sd": 0.0157154645921 
},
{
 "name": "sample",
"mean": 0.005318512195122,
"sd": 0.01390353747826 
},
{
 "name": "sample",
"mean": 0.006485615609756,
"sd": 0.01755808669659 
},
{
 "name": "sample",
"mean": 0.005686511219512,
"sd": 0.009473694629751 
},
{
 "name": "sample",
"mean": 0.005882854634146,
"sd": 0.01841897534295 
},
{
 "name": "sample",
"mean": 0.006020507317073,
"sd": 0.01416684813132 
},
{
 "name": "sample",
"mean": 0.005617463414634,
"sd": 0.01424294337487 
},
{
 "name": "sample",
"mean": 0.005852685853659,
"sd": 0.01307341193379 
},
{
 "name": "sample",
"mean": 0.006536876097561,
"sd": 0.01807876074768 
},
{
 "name": "sample",
"mean": 0.006287710243902,
"sd": 0.01569996161639 
},
{
 "name": "sample",
"mean": 0.005087338536585,
"sd": 0.009596551559372 
},
{
 "name": "sample",
"mean": 0.00512855902439,
"sd": 0.01136989887045 
},
{
 "name": "sample",
"mean": 0.006513101463415,
"sd": 0.01471960359873 
},
{
 "name": "sample",
"mean": 0.004886210731707,
"sd": 0.01163702022004 
},
{
 "name": "sample",
"mean": 0.006473956097561,
"sd": 0.0156484199754 
},
{
 "name": "sample",
"mean": 0.005404049756098,
"sd": 0.01865753051427 
},
{
 "name": "sample",
"mean": 0.006411203902439,
"sd": 0.01702146710675 
},
{
 "name": "sample",
"mean": 0.005192787317073,
"sd": 0.01386403622611 
},
{
 "name": "sample",
"mean": 0.005013072195122,
"sd": 0.01509215708797 
},
{
 "name": "sample",
"mean": 0.005876029268293,
"sd": 0.01332821303798 
},
{
 "name": "sample",
"mean": 0.005620562926829,
"sd": 0.0145592007652 
},
{
 "name": "sample",
"mean": 0.005771317073171,
"sd": 0.01795640337186 
},
{
 "name": "sample",
"mean": 0.006568247804878,
"sd": 0.02400243696583 
},
{
 "name": "sample",
"mean": 0.006399575609756,
"sd": 0.01529403548299 
},
{
 "name": "sample",
"mean": 0.006914775609756,
"sd": 0.02808142465642 
},
{
 "name": "sample",
"mean": 0.005084847804878,
"sd": 0.01163871537722 
},
{
 "name": "sample",
"mean": 0.006014018536585,
"sd": 0.01355505942361 
},
{
 "name": "sample",
"mean": 0.005435907317073,
"sd": 0.01168796429474 
},
{
 "name": "sample",
"mean": 0.006432197073171,
"sd": 0.01750805152612 
},
{
 "name": "sample",
"mean": 0.006663470243902,
"sd": 0.02683859529095 
},
{
 "name": "sample",
"mean": 0.006774707317073,
"sd": 0.02884176549434 
},
{
 "name": "sample",
"mean": 0.005056704390244,
"sd": 0.01078086062357 
},
{
 "name": "sample",
"mean": 0.005156790243902,
"sd": 0.01004637190422 
},
{
 "name": "sample",
"mean": 0.006058823414634,
"sd":  0.01417854255 
},
{
 "name": "sample",
"mean": 0.005585530731707,
"sd": 0.01750833637489 
},
{
 "name": "sample",
"mean": 0.005598786341463,
"sd": 0.01451975343065 
},
{
 "name": "sample",
"mean":     0.00586908,
"sd": 0.01476584448271 
},
{
 "name": "sample",
"mean": 0.005575812682927,
"sd": 0.01550709592686 
},
{
 "name": "sample",
"mean": 0.006147649756098,
"sd": 0.01607010327891 
},
{
 "name": "sample",
"mean": 0.005245065365854,
"sd": 0.0123522943938 
},
{
 "name": "sample",
"mean": 0.006637510243902,
"sd": 0.0180266044806 
},
{
 "name": "sample",
"mean": 0.006000181463415,
"sd": 0.01275480442467 
},
{
 "name": "sample",
"mean": 0.007082513170732,
"sd": 0.02981647693556 
},
{
 "name": "sample",
"mean": 0.006521059512195,
"sd": 0.01937867497709 
},
{
 "name": "sample",
"mean":       0.006378,
"sd": 0.01487327228651 
},
{
 "name": "sample",
"mean": 0.005831378536585,
"sd": 0.01858567878965 
},
{
 "name": "sample",
"mean": 0.005610674146341,
"sd": 0.01753607783273 
},
{
 "name": "sample",
"mean": 0.005505522926829,
"sd": 0.01053368423372 
},
{
 "name": "sample",
"mean": 0.006455616585366,
"sd": 0.02447263546787 
},
{
 "name": "sample",
"mean": 0.005783857560976,
"sd": 0.0136734283226 
},
{
 "name": "sample",
"mean": 0.006361820487805,
"sd": 0.01708495467172 
},
{
 "name": "sample",
"mean":     0.00595396,
"sd": 0.0117366216238 
},
{
 "name": "sample",
"mean": 0.006486116097561,
"sd": 0.0175961481194 
},
{
 "name": "sample",
"mean": 0.005388996097561,
"sd": 0.008587588119911 
},
{
 "name": "sample",
"mean": 0.006186092682927,
"sd": 0.01602949798396 
},
{
 "name": "sample",
"mean": 0.006240025365854,
"sd": 0.01376059406367 
},
{
 "name": "sample",
"mean": 0.006025863414634,
"sd": 0.01577112763217 
},
{
 "name": "sample",
"mean": 0.00665879804878,
"sd": 0.02041896745778 
},
{
 "name": "sample",
"mean": 0.005241058536585,
"sd": 0.01349848660806 
},
{
 "name": "sample",
"mean": 0.005753500487805,
"sd": 0.01933549885979 
},
{
 "name": "sample",
"mean": 0.006488665365854,
"sd": 0.02437298023283 
},
{
 "name": "sample",
"mean":     0.00532232,
"sd": 0.01763566257297 
},
{
 "name": "sample",
"mean": 0.005713283902439,
"sd": 0.01252998422824 
},
{
 "name": "sample",
"mean": 0.005211898536585,
"sd": 0.01890015370768 
},
{
 "name": "sample",
"mean": 0.006238748292683,
"sd": 0.01510181230413 
},
{
 "name": "sample",
"mean": 0.005777019512195,
"sd": 0.01007284643843 
},
{
 "name": "sample",
"mean": 0.005540193170732,
"sd": 0.012816219013 
},
{
 "name": "sample",
"mean": 0.005415195121951,
"sd": 0.01940904907146 
},
{
 "name": "sample",
"mean": 0.006419055609756,
"sd": 0.02425488970671 
},
{
 "name": "sample",
"mean": 0.007071794146341,
"sd": 0.03012386580566 
},
{
 "name": "sample",
"mean": 0.005166433170732,
"sd": 0.01016731146778 
},
{
 "name": "sample",
"mean": 0.006558550243902,
"sd": 0.02448803242969 
},
{
 "name": "sample",
"mean": 0.006935357073171,
"sd": 0.02525978540311 
},
{
 "name": "sample",
"mean": 0.006810928780488,
"sd": 0.02231193334029 
},
{
 "name": "sample",
"mean": 0.005149676097561,
"sd": 0.01148952312364 
},
{
 "name": "sample",
"mean": 0.006321453658537,
"sd": 0.01970676797766 
},
{
 "name": "sample",
"mean": 0.005685365853659,
"sd": 0.01441737609218 
},
{
 "name": "sample",
"mean": 0.005949294634146,
"sd": 0.01738781581557 
},
{
 "name": "sample",
"mean": 0.006967003902439,
"sd": 0.02877456702823 
},
{
 "name": "sample",
"mean": 0.006529783414634,
"sd": 0.01430016873705 
},
{
 "name": "sample",
"mean": 0.005666906341463,
"sd": 0.01033552987018 
},
{
 "name": "sample",
"mean": 0.005629789268293,
"sd": 0.009070553911534 
},
{
 "name": "sample",
"mean": 0.005760945365854,
"sd": 0.01419034850115 
},
{
 "name": "sample",
"mean": 0.006580754146341,
"sd": 0.01832956745135 
},
{
 "name": "sample",
"mean": 0.006059499512195,
"sd": 0.02112976396206 
},
{
 "name": "sample",
"mean": 0.00554016195122,
"sd": 0.01453298661291 
},
{
 "name": "sample",
"mean": 0.005377221463415,
"sd": 0.01766385138284 
},
{
 "name": "sample",
"mean": 0.006152410731707,
"sd": 0.01332712957302 
},
{
 "name": "sample",
"mean": 0.005579350243902,
"sd": 0.02020505361792 
},
{
 "name": "sample",
"mean": 0.005136109268293,
"sd": 0.009485126445815 
},
{
 "name": "sample",
"mean":     0.00578484,
"sd": 0.01037334103606 
},
{
 "name": "sample",
"mean": 0.005867566829268,
"sd": 0.01387089782251 
},
{
 "name": "sample",
"mean": 0.006724698536585,
"sd": 0.02690385142122 
},
{
 "name": "sample",
"mean": 0.005516163902439,
"sd": 0.01622365310455 
},
{
 "name": "sample",
"mean": 0.005300573658537,
"sd": 0.01049340395742 
},
{
 "name": "sample",
"mean": 0.005514425365854,
"sd": 0.01352511550151 
},
{
 "name": "sample",
"mean": 0.006341218536585,
"sd": 0.01614824935703 
},
{
 "name": "sample",
"mean": 0.005649187317073,
"sd": 0.01221436193406 
},
{
 "name": "sample",
"mean": 0.006402540487805,
"sd": 0.01437026796937 
},
{
 "name": "sample",
"mean": 0.005349668292683,
"sd": 0.010370394198 
},
{
 "name": "sample",
"mean": 0.005904728780488,
"sd": 0.01413015566371 
},
{
 "name": "sample",
"mean": 0.005653613658537,
"sd": 0.01096702086517 
},
{
 "name": "sample",
"mean": 0.006424231219512,
"sd": 0.02373336778889 
},
{
 "name": "sample",
"mean": 0.005938376585366,
"sd": 0.01120111747355 
},
{
 "name": "sample",
"mean": 0.005157647804878,
"sd": 0.01261664981441 
},
{
 "name": "sample",
"mean":     0.00536852,
"sd": 0.01863157163367 
},
{
 "name": "sample",
"mean": 0.00519532097561,
"sd": 0.009273653890658 
},
{
 "name": "sample",
"mean": 0.005693507317073,
"sd": 0.01163882625552 
},
{
 "name": "sample",
"mean": 0.005584195121951,
"sd": 0.01060756179197 
},
{
 "name": "sample",
"mean": 0.006428628292683,
"sd": 0.01477741742604 
},
{
 "name": "sample",
"mean": 0.00615932097561,
"sd": 0.01448240130982 
},
{
 "name": "sample",
"mean": 0.005489709268293,
"sd": 0.009481664104696 
},
{
 "name": "sample",
"mean": 0.005483098536585,
"sd": 0.01730007851851 
},
{
 "name": "sample",
"mean": 0.005195539512195,
"sd": 0.0115639217724 
},
{
 "name": "sample",
"mean": 0.006062650731707,
"sd": 0.01419089415309 
},
{
 "name": "sample",
"mean": 0.005822476097561,
"sd": 0.01301257381165 
},
{
 "name": "sample",
"mean": 0.005233587317073,
"sd": 0.01208171290081 
},
{
 "name": "sample",
"mean": 0.005584098536585,
"sd": 0.009629176268388 
},
{
 "name": "sample",
"mean": 0.006258222439024,
"sd": 0.01373832750193 
},
{
 "name": "sample",
"mean": 0.005306385365854,
"sd": 0.01854217750992 
},
{
 "name": "sample",
"mean": 0.005821295609756,
"sd": 0.01556692375085 
},
{
 "name": "sample",
"mean": 0.005362377560976,
"sd": 0.01165590924292 
},
{
 "name": "sample",
"mean": 0.006960510243902,
"sd": 0.03051939167617 
},
{
 "name": "sample",
"mean": 0.00686932097561,
"sd": 0.02376010331903 
},
{
 "name": "sample",
"mean": 0.00669215902439,
"sd": 0.02331405681592 
},
{
 "name": "sample",
"mean": 0.005144749268293,
"sd": 0.01025745963582 
},
{
 "name": "sample",
"mean": 0.007157002926829,
"sd": 0.03180206871888 
},
{
 "name": "sample",
"mean": 0.006708933658537,
"sd": 0.01982484994927 
},
{
 "name": "sample",
"mean": 0.006484369756098,
"sd": 0.01894109435265 
},
{
 "name": "sample",
"mean": 0.005594797073171,
"sd": 0.01010829981941 
},
{
 "name": "sample",
"mean": 0.005866773658537,
"sd": 0.01357410402308 
},
{
 "name": "sample",
"mean": 0.005196151219512,
"sd": 0.01135077192182 
},
{
 "name": "sample",
"mean": 0.006611819512195,
"sd": 0.01894783280384 
},
{
 "name": "sample",
"mean":       0.006366,
"sd": 0.0229934491128 
},
{
 "name": "sample",
"mean": 0.006073657560976,
"sd": 0.0166737160184 
},
{
 "name": "sample",
"mean": 0.005698861463415,
"sd": 0.01518580456641 
},
{
 "name": "sample",
"mean": 0.00507903804878,
"sd": 0.02016298032078 
},
{
 "name": "sample",
"mean": 0.00520908195122,
"sd": 0.01371227934428 
},
{
 "name": "sample",
"mean": 0.005915422439024,
"sd": 0.0141611446292 
},
{
 "name": "sample",
"mean": 0.006209717073171,
"sd": 0.01638813023905 
},
{
 "name": "sample",
"mean": 0.005761688780488,
"sd": 0.01111044489915 
},
{
 "name": "sample",
"mean": 0.006217054634146,
"sd": 0.01461578003005 
},
{
 "name": "sample",
"mean": 0.005305730731707,
"sd": 0.01155696603638 
},
{
 "name": "sample",
"mean": 0.005755380487805,
"sd": 0.01114036786222 
},
{
 "name": "sample",
"mean": 0.006150705365854,
"sd": 0.01323580606379 
},
{
 "name": "sample",
"mean": 0.005680395121951,
"sd": 0.01448148824336 
},
{
 "name": "sample",
"mean": 0.005907644878049,
"sd": 0.01515281848972 
},
{
 "name": "sample",
"mean": 0.00538592195122,
"sd": 0.01550255471706 
},
{
 "name": "sample",
"mean": 0.005053990243902,
"sd": 0.01166121620832 
},
{
 "name": "sample",
"mean": 0.006401743414634,
"sd": 0.01487790810565 
},
{
 "name": "sample",
"mean": 0.005295845853659,
"sd": 0.01480892106739 
},
{
 "name": "sample",
"mean": 0.00622668097561,
"sd": 0.01506572875427 
},
{
 "name": "sample",
"mean": 0.00504907902439,
"sd": 0.01212436193923 
},
{
 "name": "sample",
"mean": 0.00521812195122,
"sd": 0.0140313027674 
},
{
 "name": "sample",
"mean": 0.006307366829268,
"sd": 0.01653188855396 
},
{
 "name": "sample",
"mean": 0.006138152195122,
"sd": 0.01313065856266 
},
{
 "name": "sample",
"mean": 0.005060895609756,
"sd": 0.01440352855276 
},
{
 "name": "sample",
"mean": 0.005641180487805,
"sd": 0.01040504271509 
},
{
 "name": "sample",
"mean": 0.00523355902439,
"sd": 0.01155386710354 
},
{
 "name": "sample",
"mean": 0.007244982439024,
"sd": 0.03459730231517 
},
{
 "name": "sample",
"mean": 0.005430556097561,
"sd": 0.01199252471382 
},
{
 "name": "sample",
"mean": 0.00585719804878,
"sd": 0.01231178430035 
},
{
 "name": "sample",
"mean": 0.00643879804878,
"sd": 0.01580353317318 
},
{
 "name": "sample",
"mean": 0.006170830243902,
"sd": 0.01378962798506 
},
{
 "name": "sample",
"mean":      0.0056828,
"sd": 0.01409337978588 
},
{
 "name": "sample",
"mean": 0.006027047804878,
"sd": 0.0149351049742 
},
{
 "name": "sample",
"mean": 0.006507504390244,
"sd": 0.01523767183736 
},
{
 "name": "sample",
"mean": 0.005657127804878,
"sd": 0.009304625453571 
},
{
 "name": "sample",
"mean": 0.006489908292683,
"sd": 0.01612525206992 
},
{
 "name": "sample",
"mean": 0.006232984390244,
"sd": 0.01656668156607 
},
{
 "name": "sample",
"mean": 0.006556108292683,
"sd": 0.02017555892555 
},
{
 "name": "sample",
"mean": 0.005993156097561,
"sd": 0.01169175280349 
},
{
 "name": "sample",
"mean": 0.004971539512195,
"sd": 0.02053444444082 
},
{
 "name": "sample",
"mean": 0.005905699512195,
"sd": 0.01975504639605 
},
{
 "name": "sample",
"mean": 0.005757153170732,
"sd": 0.01337381361136 
},
{
 "name": "sample",
"mean": 0.005227263414634,
"sd": 0.008505518278204 
},
{
 "name": "sample",
"mean": 0.005378347317073,
"sd": 0.01292958837699 
},
{
 "name": "sample",
"mean": 0.005990043902439,
"sd": 0.01365412744883 
},
{
 "name": "sample",
"mean": 0.006457939512195,
"sd": 0.01748875928652 
},
{
 "name": "sample",
"mean": 0.005552057560976,
"sd": 0.0103897578392 
},
{
 "name": "sample",
"mean": 0.005418506341463,
"sd": 0.01888271471548 
},
{
 "name": "sample",
"mean": 0.007138356097561,
"sd": 0.03232107811606 
},
{
 "name": "sample",
"mean": 0.006603629268293,
"sd": 0.01733196543512 
},
{
 "name": "sample",
"mean":     0.00650884,
"sd": 0.01711286850049 
},
{
 "name": "sample",
"mean": 0.004903093658537,
"sd": 0.01665140173189 
},
{
 "name": "sample",
"mean": 0.00548860195122,
"sd": 0.00903756975322 
},
{
 "name": "sample",
"mean": 0.007279811707317,
"sd": 0.03397593011968 
},
{
 "name": "sample",
"mean": 0.00511912195122,
"sd": 0.02185717731883 
},
{
 "name": "sample",
"mean": 0.005699282926829,
"sd": 0.01158525853926 
},
{
 "name": "sample",
"mean": 0.005044808780488,
"sd": 0.01267052323034 
},
{
 "name": "sample",
"mean": 0.006833388292683,
"sd": 0.02844773114271 
},
{
 "name": "sample",
"mean": 0.005587844878049,
"sd": 0.01187885489728 
},
{
 "name": "sample",
"mean": 0.005495730731707,
"sd": 0.01389302285696 
},
{
 "name": "sample",
"mean": 0.005825895609756,
"sd": 0.0128639368505 
},
{
 "name": "sample",
"mean": 0.006577250731707,
"sd": 0.01878840721712 
},
{
 "name": "sample",
"mean": 0.006154042926829,
"sd": 0.01813920046341 
},
{
 "name": "sample",
"mean": 0.00609840097561,
"sd": 0.0164646245191 
},
{
 "name": "sample",
"mean": 0.005950227317073,
"sd": 0.01139950036637 
},
{
 "name": "sample",
"mean": 0.005663539512195,
"sd": 0.01090495884229 
},
{
 "name": "sample",
"mean": 0.005773627317073,
"sd": 0.01715915362767 
},
{
 "name": "sample",
"mean": 0.006673564878049,
"sd": 0.02186884605902 
},
{
 "name": "sample",
"mean": 0.006266914146341,
"sd": 0.02215042637264 
},
{
 "name": "sample",
"mean": 0.004958937560976,
"sd": 0.02309665548626 
},
{
 "name": "sample",
"mean": 0.006050570731707,
"sd": 0.01430970208445 
},
{
 "name": "sample",
"mean": 0.00639056195122,
"sd": 0.01750384749499 
},
{
 "name": "sample",
"mean": 0.005343935609756,
"sd": 0.01410060027182 
},
{
 "name": "sample",
"mean": 0.006081330731707,
"sd": 0.0148896535353 
},
{
 "name": "sample",
"mean": 0.005527331707317,
"sd": 0.01244233175124 
},
{
 "name": "sample",
"mean": 0.00521556195122,
"sd": 0.01330540171528 
},
{
 "name": "sample",
"mean": 0.00555020097561,
"sd": 0.01290722775055 
},
{
 "name": "sample",
"mean": 0.005143033170732,
"sd": 0.01152368981843 
},
{
 "name": "sample",
"mean": 0.005212324878049,
"sd": 0.0177018935113 
},
{
 "name": "sample",
"mean": 0.005156794146341,
"sd": 0.01669520285886 
},
{
 "name": "sample",
"mean": 0.005486824390244,
"sd": 0.01451459955509 
},
{
 "name": "sample",
"mean": 0.004968908292683,
"sd": 0.01181732165768 
},
{
 "name": "sample",
"mean": 0.005017229268293,
"sd": 0.02268557016434 
},
{
 "name": "sample",
"mean": 0.00643168195122,
"sd": 0.01496924464488 
},
{
 "name": "sample",
"mean": 0.005721266341463,
"sd": 0.01440744766072 
},
{
 "name": "sample",
"mean": 0.005761906341463,
"sd": 0.01575084315687 
},
{
 "name": "sample",
"mean": 0.006772701463415,
"sd": 0.02568525803846 
},
{
 "name": "sample",
"mean": 0.005740993170732,
"sd": 0.01231920981339 
},
{
 "name": "sample",
"mean": 0.006118245853659,
"sd": 0.01539777334646 
},
{
 "name": "sample",
"mean": 0.006423522926829,
"sd": 0.01496590402132 
},
{
 "name": "sample",
"mean": 0.007152267317073,
"sd": 0.03189359831429 
},
{
 "name": "sample",
"mean": 0.006263437073171,
"sd": 0.01834961511759 
},
{
 "name": "sample",
"mean": 0.005514088780488,
"sd": 0.01061061567529 
},
{
 "name": "sample",
"mean": 0.006399073170732,
"sd": 0.01524812332827 
},
{
 "name": "sample",
"mean": 0.006903635121951,
"sd": 0.02958506591253 
},
{
 "name": "sample",
"mean":     0.00617844,
"sd": 0.01344462595035 
},
{
 "name": "sample",
"mean": 0.005065805853659,
"sd": 0.01197884171531 
},
{
 "name": "sample",
"mean": 0.00673435804878,
"sd": 0.02207256274035 
},
{
 "name": "sample",
"mean": 0.006264447804878,
"sd": 0.01549736894237 
},
{
 "name": "sample",
"mean": 0.006444819512195,
"sd": 0.01808221769555 
},
{
 "name": "sample",
"mean": 0.005744602926829,
"sd": 0.01253552971787 
},
{
 "name": "sample",
"mean": 0.005571102439024,
"sd": 0.01176263214365 
},
{
 "name": "sample",
"mean": 0.006590065365854,
"sd": 0.02162495525238 
},
{
 "name": "sample",
"mean": 0.005371837073171,
"sd": 0.01953319199805 
},
{
 "name": "sample",
"mean": 0.006033995121951,
"sd": 0.01307555691975 
},
{
 "name": "sample",
"mean": 0.005270611707317,
"sd": 0.008925138059165 
},
{
 "name": "sample",
"mean": 0.005481526829268,
"sd": 0.01504021805667 
},
{
 "name": "sample",
"mean": 0.005590448780488,
"sd": 0.01483684306353 
},
{
 "name": "sample",
"mean": 0.007063289756098,
"sd": 0.02987819621202 
},
{
 "name": "sample",
"mean": 0.006306004878049,
"sd": 0.01713087591027 
},
{
 "name": "sample",
"mean": 0.006124145365854,
"sd": 0.01651310537201 
},
{
 "name": "sample",
"mean": 0.006072167804878,
"sd": 0.01348299680469 
},
{
 "name": "sample",
"mean": 0.006093556097561,
"sd": 0.01240001746132 
},
{
 "name": "sample",
"mean": 0.006268835121951,
"sd": 0.01434012045896 
},
{
 "name": "sample",
"mean": 0.005739492682927,
"sd": 0.01344397335414 
},
{
 "name": "sample",
"mean": 0.006476973658537,
"sd": 0.01488220204332 
},
{
 "name": "sample",
"mean": 0.00522119902439,
"sd": 0.008812779450043 
},
{
 "name": "sample",
"mean": 0.006433969756098,
"sd": 0.01558465594567 
},
{
 "name": "sample",
"mean": 0.005481452682927,
"sd": 0.01288157472655 
},
{
 "name": "sample",
"mean": 0.005271608780488,
"sd": 0.009936864495291 
},
{
 "name": "sample",
"mean": 0.006712770731707,
"sd": 0.0272961849879 
},
{
 "name": "sample",
"mean": 0.006488045853659,
"sd": 0.01740037241122 
},
{
 "name": "sample",
"mean": 0.005370202926829,
"sd": 0.0125312293351 
},
{
 "name": "sample",
"mean": 0.006003088780488,
"sd": 0.01430451502517 
},
{
 "name": "sample",
"mean": 0.006136163902439,
"sd": 0.01319243210187 
},
{
 "name": "sample",
"mean": 0.00735596195122,
"sd": 0.03549885110424 
},
{
 "name": "sample",
"mean": 0.005170043902439,
"sd": 0.01099276682217 
},
{
 "name": "sample",
"mean": 0.006535123902439,
"sd": 0.01652741849208 
},
{
 "name": "sample",
"mean": 0.005904205853659,
"sd": 0.01299330315332 
},
{
 "name": "sample",
"mean": 0.006743685853659,
"sd": 0.01991912879395 
},
{
 "name": "sample",
"mean": 0.005955340487805,
"sd": 0.01392536797849 
},
{
 "name": "sample",
"mean": 0.006489383414634,
"sd": 0.01518898550619 
},
{
 "name": "sample",
"mean": 0.00543567902439,
"sd": 0.01358788888467 
},
{
 "name": "sample",
"mean": 0.007173229268293,
"sd": 0.03112333368139 
},
{
 "name": "sample",
"mean": 0.006431903414634,
"sd": 0.01544711692574 
},
{
 "name": "sample",
"mean": 0.006696316097561,
"sd": 0.0272252441142 
},
{
 "name": "sample",
"mean": 0.005618627317073,
"sd": 0.01120928195411 
},
{
 "name": "sample",
"mean": 0.006100634146341,
"sd": 0.01630579266992 
},
{
 "name": "sample",
"mean": 0.005666451707317,
"sd": 0.01329231835574 
},
{
 "name": "sample",
"mean": 0.006372388292683,
"sd": 0.01317274970488 
},
{
 "name": "sample",
"mean": 0.005100066341463,
"sd": 0.02049942166732 
},
{
 "name": "sample",
"mean": 0.006814547317073,
"sd": 0.02768107867611 
},
{
 "name": "sample",
"mean": 0.005319323902439,
"sd": 0.008697613127618 
},
{
 "name": "sample",
"mean":      0.0053046,
"sd": 0.01049260192989 
},
{
 "name": "sample",
"mean": 0.005532970731707,
"sd": 0.01226506119157 
},
{
 "name": "sample",
"mean": 0.005918708292683,
"sd": 0.01381449909013 
},
{
 "name": "sample",
"mean": 0.006956427317073,
"sd": 0.03034595493471 
},
{
 "name": "sample",
"mean": 0.006289615609756,
"sd": 0.01807666711567 
},
{
 "name": "sample",
"mean": 0.006104566829268,
"sd": 0.01210658797735 
},
{
 "name": "sample",
"mean": 0.006452762926829,
"sd": 0.01767797321328 
},
{
 "name": "sample",
"mean":     0.00627928,
"sd": 0.01524764794556 
},
{
 "name": "sample",
"mean": 0.004942628292683,
"sd": 0.01200164569221 
},
{
 "name": "sample",
"mean": 0.006453059512195,
"sd": 0.01744047371302 
},
{
 "name": "sample",
"mean": 0.005717323902439,
"sd": 0.01249046808018 
},
{
 "name": "sample",
"mean": 0.005366272195122,
"sd": 0.0189712817354 
},
{
 "name": "sample",
"mean": 0.005258925853659,
"sd": 0.01257659084861 
},
{
 "name": "sample",
"mean": 0.005262608780488,
"sd": 0.008659949116689 
},
{
 "name": "sample",
"mean": 0.006432063414634,
"sd": 0.01769821129889 
},
{
 "name": "sample",
"mean": 0.005307626341463,
"sd": 0.01814093823316 
},
{
 "name": "sample",
"mean": 0.005560065365854,
"sd": 0.01140088159131 
},
{
 "name": "sample",
"mean": 0.005260390243902,
"sd": 0.01295965300199 
},
{
 "name": "sample",
"mean": 0.006339490731707,
"sd": 0.01541107226583 
},
{
 "name": "sample",
"mean": 0.00595703804878,
"sd": 0.01283318714589 
},
{
 "name": "sample",
"mean": 0.006277235121951,
"sd": 0.01598934223065 
},
{
 "name": "sample",
"mean": 0.005887805853659,
"sd": 0.01925488098157 
},
{
 "name": "sample",
"mean": 0.00563104097561,
"sd": 0.01429936593006 
},
{
 "name": "sample",
"mean": 0.005505927804878,
"sd": 0.01813445244458 
},
{
 "name": "sample",
"mean": 0.005330030243902,
"sd": 0.01308671766935 
},
{
 "name": "sample",
"mean": 0.00630391902439,
"sd": 0.01480923387903 
},
{
 "name": "sample",
"mean": 0.005206416585366,
"sd": 0.01757752392402 
},
{
 "name": "sample",
"mean": 0.005832937560976,
"sd": 0.00998249715945 
},
{
 "name": "sample",
"mean": 0.006052529756098,
"sd": 0.01249622287332 
},
{
 "name": "sample",
"mean": 0.005655507317073,
"sd": 0.01082434871456 
},
{
 "name": "sample",
"mean": 0.004937010731707,
"sd": 0.01086866141373 
},
{
 "name": "sample",
"mean": 0.005036728780488,
"sd": 0.01190547466388 
},
{
 "name": "sample",
"mean": 0.005634933658537,
"sd": 0.009880350382624 
},
{
 "name": "sample",
"mean": 0.00640368097561,
"sd": 0.01472735525689 
},
{
 "name": "sample",
"mean": 0.005377185365854,
"sd": 0.01130229235299 
},
{
 "name": "sample",
"mean": 0.005306854634146,
"sd": 0.008440685054657 
},
{
 "name": "sample",
"mean": 0.006355462439024,
"sd": 0.01586998854229 
},
{
 "name": "sample",
"mean": 0.006047931707317,
"sd": 0.01559026308884 
},
{
 "name": "sample",
"mean": 0.006923074146341,
"sd": 0.02789092755592 
},
{
 "name": "sample",
"mean": 0.005005007804878,
"sd": 0.01299381269522 
},
{
 "name": "sample",
"mean": 0.005034811707317,
"sd": 0.01098328900928 
},
{
 "name": "sample",
"mean": 0.005439587317073,
"sd": 0.01419262707559 
},
{
 "name": "sample",
"mean": 0.006438084878049,
"sd": 0.01498791136088 
},
{
 "name": "sample",
"mean": 0.006310302439024,
"sd": 0.01731612431461 
},
{
 "name": "sample",
"mean": 0.00651368097561,
"sd": 0.01576152535157 
},
{
 "name": "sample",
"mean": 0.005139382439024,
"sd": 0.01439179778071 
},
{
 "name": "sample",
"mean": 0.005289845853659,
"sd": 0.01847004798249 
},
{
 "name": "sample",
"mean": 0.006464013658537,
"sd": 0.01512474550126 
},
{
 "name": "sample",
"mean": 0.00586007902439,
"sd": 0.01668246216484 
},
{
 "name": "sample",
"mean": 0.006668776585366,
"sd": 0.025727911322 
},
{
 "name": "sample",
"mean": 0.005369649756098,
"sd": 0.0166699676074 
},
{
 "name": "sample",
"mean": 0.007106925853659,
"sd": 0.03216419017742 
},
{
 "name": "sample",
"mean": 0.005738252682927,
"sd": 0.009799733325941 
},
{
 "name": "sample",
"mean": 0.006196942439024,
"sd": 0.01543472902879 
},
{
 "name": "sample",
"mean": 0.005959261463415,
"sd": 0.01126493621405 
},
{
 "name": "sample",
"mean": 0.006307554146341,
"sd": 0.01428347765866 
},
{
 "name": "sample",
"mean": 0.006770669268293,
"sd": 0.02628354509646 
},
{
 "name": "sample",
"mean": 0.006187179512195,
"sd": 0.01529285535623 
},
{
 "name": "sample",
"mean": 0.006007305365854,
"sd": 0.0144629356969 
},
{
 "name": "sample",
"mean": 0.005058049756098,
"sd": 0.01473765839055 
},
{
 "name": "sample",
"mean": 0.006535994146341,
"sd": 0.01562688046844 
},
{
 "name": "sample",
"mean": 0.006751549268293,
"sd": 0.02376365711921 
},
{
 "name": "sample",
"mean": 0.006457964878049,
"sd": 0.01614838766053 
},
{
 "name": "sample",
"mean": 0.005581836097561,
"sd": 0.01046022684038 
},
{
 "name": "sample",
"mean": 0.004992050731707,
"sd": 0.02202783831554 
},
{
 "name": "sample",
"mean": 0.006159395121951,
"sd": 0.01351049787689 
},
{
 "name": "sample",
"mean": 0.005661017560976,
"sd": 0.01613908306262 
},
{
 "name": "sample",
"mean": 0.005216809756098,
"sd": 0.01738192828855 
},
{
 "name": "sample",
"mean": 0.005834349268293,
"sd": 0.01139802010586 
},
{
 "name": "sample",
"mean": 0.006003386341463,
"sd": 0.01670243226819 
},
{
 "name": "sample",
"mean": 0.005736229268293,
"sd": 0.01493766488997 
},
{
 "name": "sample",
"mean": 0.006633685853659,
"sd": 0.02217443131418 
},
{
 "name": "sample",
"mean": 0.005960393170732,
"sd": 0.01249193538872 
},
{
 "name": "sample",
"mean": 0.005945936585366,
"sd": 0.01742250599077 
},
{
 "name": "sample",
"mean": 0.004934391219512,
"sd": 0.01284578620657 
},
{
 "name": "sample",
"mean": 0.007156457560976,
"sd": 0.03289282321814 
},
{
 "name": "sample",
"mean": 0.006175582439024,
"sd": 0.01385771429192 
},
{
 "name": "sample",
"mean": 0.005856738536585,
"sd": 0.01585357989633 
},
{
 "name": "sample",
"mean": 0.005476071219512,
"sd": 0.01329435950384 
},
{
 "name": "sample",
"mean": 0.007252005853659,
"sd": 0.03228211305418 
},
{
 "name": "sample",
"mean": 0.00561340195122,
"sd": 0.01116143495322 
},
{
 "name": "sample",
"mean": 0.005141138536585,
"sd": 0.01084131331956 
},
{
 "name": "sample",
"mean": 0.00540432097561,
"sd": 0.01421701538132 
},
{
 "name": "sample",
"mean": 0.005972243902439,
"sd": 0.01489200729528 
},
{
 "name": "sample",
"mean":     0.00497876,
"sd": 0.02031643488689 
},
{
 "name": "sample",
"mean": 0.006239027317073,
"sd": 0.01471048934447 
},
{
 "name": "sample",
"mean": 0.005134826341463,
"sd": 0.02054166118461 
},
{
 "name": "sample",
"mean": 0.005981468292683,
"sd": 0.01229501106247 
},
{
 "name": "sample",
"mean": 0.005140821463415,
"sd": 0.009612969233664 
},
{
 "name": "sample",
"mean": 0.006148596097561,
"sd": 0.01521897635166 
},
{
 "name": "sample",
"mean": 0.006013011707317,
"sd": 0.01476013054473 
},
{
 "name": "sample",
"mean": 0.005748912195122,
"sd": 0.01149023016425 
},
{
 "name": "sample",
"mean": 0.00655895902439,
"sd": 0.01542246175044 
},
{
 "name": "sample",
"mean":       0.005201,
"sd": 0.02017058396386 
},
{
 "name": "sample",
"mean": 0.006455295609756,
"sd": 0.01536284235131 
},
{
 "name": "sample",
"mean": 0.005126670243902,
"sd": 0.0130757489968 
},
{
 "name": "sample",
"mean": 0.00532944195122,
"sd": 0.01899769134036 
},
{
 "name": "sample",
"mean": 0.006131277073171,
"sd": 0.02031002408622 
},
{
 "name": "sample",
"mean": 0.006587793170732,
"sd": 0.01586845088854 
},
{
 "name": "sample",
"mean": 0.006595906341463,
"sd": 0.01963441392357 
},
{
 "name": "sample",
"mean": 0.005060056585366,
"sd": 0.01586065509765 
},
{
 "name": "sample",
"mean": 0.006984975609756,
"sd": 0.0297069997529 
},
{
 "name": "sample",
"mean": 0.006535930731707,
"sd": 0.01437095058119 
},
{
 "name": "sample",
"mean": 0.005973876097561,
"sd": 0.01224132935789 
},
{
 "name": "sample",
"mean": 0.006117242926829,
"sd": 0.01579694745155 
},
{
 "name": "sample",
"mean": 0.006437259512195,
"sd": 0.01613603716856 
},
{
 "name": "sample",
"mean": 0.005993810731707,
"sd": 0.01144239618694 
},
{
 "name": "sample",
"mean": 0.005647591219512,
"sd": 0.01775537910518 
},
{
 "name": "sample",
"mean": 0.005213380487805,
"sd": 0.01368072815266 
},
{
 "name": "sample",
"mean": 0.005801223414634,
"sd": 0.01247476069929 
},
{
 "name": "sample",
"mean": 0.006248754146341,
"sd": 0.02230330948201 
},
{
 "name": "sample",
"mean": 0.005331672195122,
"sd": 0.01368621605132 
},
{
 "name": "sample",
"mean": 0.005515249756098,
"sd": 0.00896267783315 
},
{
 "name": "sample",
"mean": 0.006400331707317,
"sd": 0.01989206272512 
},
{
 "name": "sample",
"mean": 0.006535951219512,
"sd": 0.01556737490389 
},
{
 "name": "sample",
"mean": 0.00584668195122,
"sd": 0.01861499370569 
},
{
 "name": "sample",
"mean": 0.006331405853659,
"sd": 0.01546306470282 
},
{
 "name": "sample",
"mean": 0.006460729756098,
"sd": 0.0229794809948 
},
{
 "name": "sample",
"mean": 0.00547864097561,
"sd": 0.01589612464247 
},
{
 "name": "sample",
"mean": 0.005776977560976,
"sd": 0.009975981430027 
},
{
 "name": "sample",
"mean": 0.00690032195122,
"sd": 0.02781229756609 
},
{
 "name": "sample",
"mean": 0.006327322926829,
"sd": 0.01594487063628 
},
{
 "name": "sample",
"mean": 0.005385653658537,
"sd": 0.009485944317409 
},
{
 "name": "sample",
"mean": 0.006071583414634,
"sd": 0.01686312533704 
},
{
 "name": "sample",
"mean": 0.006339755121951,
"sd": 0.01444790396363 
},
{
 "name": "sample",
"mean": 0.006557389268293,
"sd": 0.0242136236602 
},
{
 "name": "sample",
"mean": 0.005664208780488,
"sd": 0.01010269380513 
},
{
 "name": "sample",
"mean": 0.006478148292683,
"sd": 0.0188494154494 
},
{
 "name": "sample",
"mean": 0.005940889756098,
"sd": 0.01712239331633 
},
{
 "name": "sample",
"mean": 0.006436225365854,
"sd": 0.01579153970017 
},
{
 "name": "sample",
"mean":     0.00542536,
"sd": 0.009026306716403 
},
{
 "name": "sample",
"mean": 0.005209059512195,
"sd": 0.01056504487732 
},
{
 "name": "sample",
"mean": 0.005607849756098,
"sd": 0.01174056693304 
},
{
 "name": "sample",
"mean": 0.006827032195122,
"sd": 0.02889837146004 
},
{
 "name": "sample",
"mean": 0.006013836097561,
"sd": 0.0120959157329 
},
{
 "name": "sample",
"mean": 0.006291547317073,
"sd": 0.01605777299423 
},
{
 "name": "sample",
"mean": 0.005443251707317,
"sd": 0.009360031859835 
},
{
 "name": "sample",
"mean": 0.006940943414634,
"sd": 0.02945411168654 
},
{
 "name": "sample",
"mean": 0.00533127902439,
"sd": 0.01336119450895 
},
{
 "name": "sample",
"mean": 0.006192006829268,
"sd": 0.01284331508833 
},
{
 "name": "sample",
"mean": 0.006228034146341,
"sd": 0.01552765625394 
},
{
 "name": "sample",
"mean": 0.005520307317073,
"sd": 0.01657501618065 
},
{
 "name": "sample",
"mean": 0.005554830243902,
"sd": 0.009023442389386 
},
{
 "name": "sample",
"mean": 0.006907111219512,
"sd": 0.02859508196938 
},
{
 "name": "sample",
"mean": 0.005928548292683,
"sd": 0.01262783600667 
},
{
 "name": "sample",
"mean": 0.005570087804878,
"sd": 0.01557949417776 
},
{
 "name": "sample",
"mean": 0.005250309268293,
"sd": 0.02084006656534 
},
{
 "name": "sample",
"mean":      0.0052726,
"sd": 0.01017570915378 
},
{
 "name": "sample",
"mean": 0.006112814634146,
"sd": 0.01368441583822 
},
{
 "name": "sample",
"mean": 0.005471624390244,
"sd": 0.02048459553108 
},
{
 "name": "sample",
"mean": 0.005087737560976,
"sd": 0.009112690406478 
},
{
 "name": "sample",
"mean": 0.006582792195122,
"sd": 0.02583189280712 
},
{
 "name": "sample",
"mean": 0.006351992195122,
"sd": 0.01496491047784 
},
{
 "name": "sample",
"mean": 0.005345396097561,
"sd": 0.01947699799721 
},
{
 "name": "sample",
"mean": 0.005058741463415,
"sd":   0.0209359453 
},
{
 "name": "sample",
"mean": 0.005596762926829,
"sd": 0.01807938799096 
},
{
 "name": "sample",
"mean": 0.006158451707317,
"sd": 0.01290886632483 
},
{
 "name": "sample",
"mean": 0.006650405853659,
"sd": 0.01652895731147 
},
{
 "name": "sample",
"mean": 0.005548780487805,
"sd": 0.01313292752913 
},
{
 "name": "sample",
"mean": 0.005806767804878,
"sd": 0.0119037063329 
},
{
 "name": "sample",
"mean": 0.006277259512195,
"sd": 0.0144544784007 
},
{
 "name": "sample",
"mean": 0.005581009756098,
"sd": 0.0117388168625 
},
{
 "name": "sample",
"mean": 0.005068265365854,
"sd": 0.01985628465867 
},
{
 "name": "sample",
"mean": 0.00581856097561,
"sd": 0.01632888109055 
},
{
 "name": "sample",
"mean": 0.005661868292683,
"sd": 0.01495418409585 
},
{
 "name": "sample",
"mean": 0.006542305365854,
"sd": 0.01637308479946 
},
{
 "name": "sample",
"mean": 0.007091746341463,
"sd": 0.03236475347376 
},
{
 "name": "sample",
"mean": 0.00542284195122,
"sd": 0.008640976755155 
},
{
 "name": "sample",
"mean": 0.005515785365854,
"sd": 0.008962850779811 
},
{
 "name": "sample",
"mean": 0.005799584390244,
"sd": 0.01406052541138 
},
{
 "name": "sample",
"mean": 0.006117953170732,
"sd": 0.01516521331093 
},
{
 "name": "sample",
"mean": 0.005384260487805,
"sd": 0.008510938852744 
},
{
 "name": "sample",
"mean": 0.007144737560976,
"sd": 0.03034869704256 
},
{
 "name": "sample",
"mean": 0.006482463414634,
"sd": 0.01459346213501 
},
{
 "name": "sample",
"mean": 0.005643816585366,
"sd": 0.009671030493195 
},
{
 "name": "sample",
"mean": 0.005348496585366,
"sd": 0.01242541526109 
},
{
 "name": "sample",
"mean": 0.005222954146341,
"sd": 0.01296773694868 
},
{
 "name": "sample",
"mean": 0.006762527804878,
"sd": 0.0220510760946 
},
{
 "name": "sample",
"mean": 0.006489312195122,
"sd": 0.01817246835681 
},
{
 "name": "sample",
"mean": 0.005381096585366,
"sd": 0.01165262608027 
},
{
 "name": "sample",
"mean":      0.0064716,
"sd": 0.01788022888235 
},
{
 "name": "sample",
"mean": 0.006505357073171,
"sd": 0.01537202940128 
},
{
 "name": "sample",
"mean": 0.005848371707317,
"sd": 0.0120530925414 
},
{
 "name": "sample",
"mean": 0.00567112097561,
"sd": 0.010657351478 
},
{
 "name": "sample",
"mean": 0.004976009756098,
"sd": 0.01267715626355 
},
{
 "name": "sample",
"mean": 0.00529647804878,
"sd": 0.008285312883803 
},
{
 "name": "sample",
"mean": 0.00655016195122,
"sd": 0.01631457176884 
},
{
 "name": "sample",
"mean": 0.005696071219512,
"sd": 0.01287337147348 
},
{
 "name": "sample",
"mean": 0.005398949268293,
"sd": 0.008668331756195 
},
{
 "name": "sample",
"mean": 0.007180251707317,
"sd": 0.0331437953979 
},
{
 "name": "sample",
"mean": 0.006109834146341,
"sd": 0.01385431939527 
},
{
 "name": "sample",
"mean": 0.006136415609756,
"sd": 0.01355221460712 
},
{
 "name": "sample",
"mean": 0.004922627317073,
"sd": 0.01258591422926 
},
{
 "name": "sample",
"mean": 0.00513724195122,
"sd": 0.0218655428889 
},
{
 "name": "sample",
"mean": 0.005455767804878,
"sd": 0.01471520653776 
},
{
 "name": "sample",
"mean": 0.005625849756098,
"sd": 0.01037516831777 
},
{
 "name": "sample",
"mean": 0.006485703414634,
"sd": 0.02610953130163 
},
{
 "name": "sample",
"mean": 0.006238922926829,
"sd": 0.0198893239669 
},
{
 "name": "sample",
"mean": 0.00562308195122,
"sd": 0.01376629443861 
},
{
 "name": "sample",
"mean": 0.005610103414634,
"sd": 0.01366018831996 
},
{
 "name": "sample",
"mean": 0.005743802926829,
"sd": 0.01254025162603 
},
{
 "name": "sample",
"mean": 0.005719821463415,
"sd": 0.01183079957977 
},
{
 "name": "sample",
"mean": 0.006430912195122,
"sd": 0.01963637545595 
},
{
 "name": "sample",
"mean": 0.007127916097561,
"sd": 0.03206702521431 
},
{
 "name": "sample",
"mean": 0.005268143414634,
"sd": 0.01886239757365 
},
{
 "name": "sample",
"mean": 0.006494087804878,
"sd": 0.01589207356205 
},
{
 "name": "sample",
"mean": 0.006753099512195,
"sd": 0.02764045924667 
},
{
 "name": "sample",
"mean": 0.00507184097561,
"sd": 0.01259851431627 
},
{
 "name": "sample",
"mean": 0.005566727804878,
"sd": 0.01409250545603 
},
{
 "name": "sample",
"mean": 0.005347666341463,
"sd": 0.01353089535221 
},
{
 "name": "sample",
"mean": 0.005585164878049,
"sd": 0.0106179083941 
},
{
 "name": "sample",
"mean": 0.007002975609756,
"sd": 0.02683282409941 
},
{
 "name": "sample",
"mean": 0.00609948097561,
"sd": 0.01599512974301 
},
{
 "name": "sample",
"mean": 0.005285157073171,
"sd": 0.01796398454401 
},
{
 "name": "sample",
"mean": 0.006361162926829,
"sd": 0.02065834660016 
},
{
 "name": "sample",
"mean": 0.006234451707317,
"sd": 0.01999623330211 
},
{
 "name": "sample",
"mean": 0.004903286829268,
"sd": 0.01552116540126 
},
{
 "name": "sample",
"mean": 0.00574383902439,
"sd": 0.01496558555236 
},
{
 "name": "sample",
"mean": 0.006254547317073,
"sd": 0.01413449784952 
},
{
 "name": "sample",
"mean": 0.006327253658537,
"sd": 0.01508744780869 
},
{
 "name": "sample",
"mean": 0.006099665365854,
"sd": 0.011736886407 
},
{
 "name": "sample",
"mean": 0.006353228292683,
"sd": 0.01667535066558 
},
{
 "name": "sample",
"mean": 0.006390805853659,
"sd": 0.01621684138045 
},
{
 "name": "sample",
"mean": 0.00526731902439,
"sd": 0.0130864502485 
},
{
 "name": "sample",
"mean": 0.00633952097561,
"sd": 0.01707501349177 
},
{
 "name": "sample",
"mean": 0.005062191219512,
"sd": 0.0205501919416 
},
{
 "name": "sample",
"mean": 0.005392385365854,
"sd": 0.008533334665218 
},
{
 "name": "sample",
"mean": 0.006474145365854,
"sd": 0.0179419988493 
},
{
 "name": "sample",
"mean": 0.006454305365854,
"sd": 0.02435335039286 
},
{
 "name": "sample",
"mean": 0.006040723902439,
"sd": 0.01273281245834 
},
{
 "name": "sample",
"mean": 0.006475298536585,
"sd": 0.01629505810661 
},
{
 "name": "sample",
"mean": 0.005503727804878,
"sd": 0.01926655816693 
},
{
 "name": "sample",
"mean": 0.006046723902439,
"sd": 0.01526457164697 
},
{
 "name": "sample",
"mean": 0.007200506341463,
"sd": 0.03327315380673 
},
{
 "name": "sample",
"mean": 0.007186152195122,
"sd": 0.03384444697822 
},
{
 "name": "sample",
"mean": 0.006574191219512,
"sd": 0.01998898002875 
},
{
 "name": "sample",
"mean": 0.005125454634146,
"sd": 0.01234902351518 
},
{
 "name": "sample",
"mean": 0.005453210731707,
"sd": 0.008828336584293 
},
{
 "name": "sample",
"mean": 0.006514089756098,
"sd": 0.02350824215437 
},
{
 "name": "sample",
"mean": 0.006001513170732,
"sd": 0.01294699183245 
},
{
 "name": "sample",
"mean": 0.006129608780488,
"sd": 0.01428824278771 
},
{
 "name": "sample",
"mean": 0.006684905365854,
"sd": 0.02562976391879 
},
{
 "name": "sample",
"mean": 0.005068594146341,
"sd": 0.01126727214955 
},
{
 "name": "sample",
"mean": 0.006409058536585,
"sd": 0.01667919336883 
},
{
 "name": "sample",
"mean": 0.006801345365854,
"sd": 0.02868362481885 
},
{
 "name": "sample",
"mean": 0.006021571707317,
"sd": 0.01300520349357 
},
{
 "name": "sample",
"mean": 0.006512885853659,
"sd": 0.01545614755201 
},
{
 "name": "sample",
"mean": 0.00644528195122,
"sd": 0.02235162303577 
},
{
 "name": "sample",
"mean": 0.005919485853659,
"sd": 0.01559643772293 
},
{
 "name": "sample",
"mean": 0.006195246829268,
"sd": 0.01558888017666 
},
{
 "name": "sample",
"mean": 0.006724754146341,
"sd": 0.02582434069427 
},
{
 "name": "sample",
"mean": 0.006892180487805,
"sd": 0.02595552245105 
},
{
 "name": "sample",
"mean": 0.005660494634146,
"sd": 0.01416408828564 
},
{
 "name": "sample",
"mean": 0.007080072195122,
"sd": 0.03178057344782 
},
{
 "name": "sample",
"mean": 0.005307948292683,
"sd": 0.0136083393257 
},
{
 "name": "sample",
"mean": 0.005425845853659,
"sd": 0.01922820448004 
},
{
 "name": "sample",
"mean": 0.006031167804878,
"sd": 0.01278711649261 
},
{
 "name": "sample",
"mean": 0.006134727804878,
"sd": 0.01654522070334 
},
{
 "name": "sample",
"mean": 0.005348537560976,
"sd": 0.01591013869872 
},
{
 "name": "sample",
"mean": 0.005377054634146,
"sd": 0.01852297338683 
},
{
 "name": "sample",
"mean": 0.005173108292683,
"sd": 0.01321368058704 
},
{
 "name": "sample",
"mean": 0.006145221463415,
"sd": 0.01451029348974 
},
{
 "name": "sample",
"mean": 0.005620562926829,
"sd": 0.01404545733195 
},
{
 "name": "sample",
"mean":     0.00728768,
"sd": 0.0340421599636 
},
{
 "name": "sample",
"mean": 0.006353083902439,
"sd": 0.01501646363518 
},
{
 "name": "sample",
"mean": 0.005524477073171,
"sd": 0.009851748556102 
},
{
 "name": "sample",
"mean": 0.006080045853659,
"sd": 0.01384857279908 
},
{
 "name": "sample",
"mean": 0.006000780487805,
"sd": 0.01503426625988 
},
{
 "name": "sample",
"mean":     0.00605152,
"sd": 0.01999040981367 
},
{
 "name": "sample",
"mean": 0.006180860487805,
"sd": 0.01457649456843 
},
{
 "name": "sample",
"mean":     0.00617616,
"sd": 0.01374659029044 
},
{
 "name": "sample",
"mean": 0.005444837073171,
"sd": 0.008794767512951 
},
{
 "name": "sample",
"mean": 0.007058156097561,
"sd": 0.03122800764687 
},
{
 "name": "sample",
"mean": 0.004911854634146,
"sd": 0.01109528090325 
},
{
 "name": "sample",
"mean": 0.006467769756098,
"sd": 0.01471573960787 
},
{
 "name": "sample",
"mean": 0.005354445853659,
"sd": 0.0123504014603 
},
{
 "name": "sample",
"mean": 0.006170451707317,
"sd": 0.01408354243931 
},
{
 "name": "sample",
"mean": 0.005213364878049,
"sd": 0.01215654765871 
},
{
 "name": "sample",
"mean": 0.007058893658537,
"sd": 0.02840634927529 
},
{
 "name": "sample",
"mean": 0.005926853658537,
"sd": 0.01408367132659 
},
{
 "name": "sample",
"mean": 0.005232288780488,
"sd": 0.02099006525517 
},
{
 "name": "sample",
"mean": 0.007193794146341,
"sd": 0.0334539463996 
},
{
 "name": "sample",
"mean": 0.005601654634146,
"sd": 0.009910857253149 
},
{
 "name": "sample",
"mean":      0.0054004,
"sd": 0.01225504606881 
},
{
 "name": "sample",
"mean": 0.005275882926829,
"sd": 0.01440157550726 
},
{
 "name": "sample",
"mean": 0.006524506341463,
"sd": 0.01723109546777 
},
{
 "name": "sample",
"mean": 0.006365307317073,
"sd": 0.01508164084636 
},
{
 "name": "sample",
"mean": 0.005273115121951,
"sd": 0.01102245982498 
},
{
 "name": "sample",
"mean": 0.005072130731707,
"sd": 0.0157305643249 
},
{
 "name": "sample",
"mean": 0.006225554146341,
"sd": 0.01501183581458 
},
{
 "name": "sample",
"mean": 0.006537188292683,
"sd": 0.01471592907693 
},
{
 "name": "sample",
"mean": 0.005737914146341,
"sd": 0.00965518373796 
},
{
 "name": "sample",
"mean": 0.006398934634146,
"sd": 0.0237503105844 
},
{
 "name": "sample",
"mean": 0.005462231219512,
"sd": 0.009423240055903 
},
{
 "name": "sample",
"mean": 0.006789190243902,
"sd": 0.01992047509233 
},
{
 "name": "sample",
"mean": 0.006628904390244,
"sd": 0.01876755567498 
},
{
 "name": "sample",
"mean": 0.005855773658537,
"sd": 0.01503914803893 
},
{
 "name": "sample",
"mean": 0.004967175609756,
"sd": 0.02227820584229 
},
{
 "name": "sample",
"mean": 0.005575739512195,
"sd": 0.01288191300258 
},
{
 "name": "sample",
"mean": 0.00723803902439,
"sd": 0.03197945042874 
},
{
 "name": "sample",
"mean": 0.00633419902439,
"sd": 0.01435219426015 
},
{
 "name": "sample",
"mean": 0.006272888780488,
"sd": 0.01488245035319 
},
{
 "name": "sample",
"mean": 0.005142849756098,
"sd": 0.01724579924637 
},
{
 "name": "sample",
"mean": 0.005565754146341,
"sd": 0.01095348274424 
},
{
 "name": "sample",
"mean": 0.005323187317073,
"sd": 0.01182364283653 
},
{
 "name": "sample",
"mean": 0.00615479902439,
"sd": 0.01419458176442 
},
{
 "name": "sample",
"mean": 0.005800113170732,
"sd": 0.01547615127406 
},
{
 "name": "sample",
"mean": 0.00634723902439,
"sd": 0.01660177389596 
},
{
 "name": "sample",
"mean": 0.005169069268293,
"sd": 0.01380427668722 
},
{
 "name": "sample",
"mean": 0.005440773658537,
"sd": 0.008835288848674 
},
{
 "name": "sample",
"mean": 0.005218704390244,
"sd": 0.02169239562598 
},
{
 "name": "sample",
"mean": 0.006926217560976,
"sd": 0.02547698548196 
},
{
 "name": "sample",
"mean": 0.006387656585366,
"sd": 0.01711998940347 
},
{
 "name": "sample",
"mean":     0.00518472,
"sd": 0.01315102135311 
},
{
 "name": "sample",
"mean": 0.005349755121951,
"sd": 0.008531875765641 
},
{
 "name": "sample",
"mean": 0.005119417560976,
"sd": 0.01981223578978 
},
{
 "name": "sample",
"mean": 0.006139508292683,
"sd": 0.01409677852147 
},
{
 "name": "sample",
"mean": 0.006842450731707,
"sd": 0.02533101249295 
},
{
 "name": "sample",
"mean": 0.006892815609756,
"sd": 0.02448877889251 
},
{
 "name": "sample",
"mean": 0.006398588292683,
"sd": 0.01576823101282 
},
{
 "name": "sample",
"mean": 0.006212708292683,
"sd": 0.01346813553294 
},
{
 "name": "sample",
"mean": 0.005013712195122,
"sd": 0.01214798894215 
},
{
 "name": "sample",
"mean": 0.005731889756098,
"sd": 0.01383392054592 
},
{
 "name": "sample",
"mean":      0.0057774,
"sd": 0.01336570270733 
},
{
 "name": "sample",
"mean": 0.004991653658537,
"sd": 0.02332599685104 
},
{
 "name": "sample",
"mean": 0.005340788292683,
"sd": 0.01260359466758 
},
{
 "name": "sample",
"mean": 0.005690268292683,
"sd": 0.01606623488921 
},
{
 "name": "sample",
"mean": 0.00568811902439,
"sd": 0.01077214801244 
},
{
 "name": "sample",
"mean": 0.006244786341463,
"sd": 0.02145909261256 
},
{
 "name": "sample",
"mean": 0.006007412682927,
"sd": 0.01890980791514 
},
{
 "name": "sample",
"mean": 0.005888492682927,
"sd": 0.01359654313667 
},
{
 "name": "sample",
"mean": 0.006599803902439,
"sd": 0.01933014765766 
},
{
 "name": "sample",
"mean": 0.006664029268293,
"sd": 0.01788184218966 
},
{
 "name": "sample",
"mean": 0.006551628292683,
"sd": 0.01621901497349 
},
{
 "name": "sample",
"mean": 0.005805445853659,
"sd": 0.01292231624606 
},
{
 "name": "sample",
"mean": 0.006450688780488,
"sd": 0.01707320061785 
},
{
 "name": "sample",
"mean": 0.005324003902439,
"sd": 0.01283882820384 
},
{
 "name": "sample",
"mean": 0.005148949268293,
"sd": 0.0213762747441 
},
{
 "name": "sample",
"mean": 0.005365830243902,
"sd": 0.008583565563426 
},
{
 "name": "sample",
"mean": 0.005619723902439,
"sd": 0.01034426190064 
},
{
 "name": "sample",
"mean": 0.005751091707317,
"sd": 0.01287614766746 
},
{
 "name": "sample",
"mean": 0.005047128780488,
"sd": 0.02306349574832 
},
{
 "name": "sample",
"mean": 0.005874163902439,
"sd": 0.01423898533245 
},
{
 "name": "sample",
"mean": 0.00581932097561,
"sd": 0.01298382044308 
},
{
 "name": "sample",
"mean": 0.005956653658537,
"sd": 0.01541845634425 
},
{
 "name": "sample",
"mean": 0.005203666341463,
"sd": 0.01390766040282 
},
{
 "name": "sample",
"mean": 0.006558816585366,
"sd": 0.01821055008012 
},
{
 "name": "sample",
"mean": 0.006311476097561,
"sd": 0.01651960494298 
},
{
 "name": "sample",
"mean": 0.005539070243902,
"sd": 0.01107858845228 
},
{
 "name": "sample",
"mean": 0.005303794146341,
"sd": 0.01216444049481 
},
{
 "name": "sample",
"mean": 0.005285381463415,
"sd": 0.01125429948093 
},
{
 "name": "sample",
"mean": 0.00528724097561,
"sd": 0.02059328445915 
},
{
 "name": "sample",
"mean": 0.005589621463415,
"sd": 0.01019608315255 
},
{
 "name": "sample",
"mean": 0.006742349268293,
"sd": 0.02270844798391 
},
{
 "name": "sample",
"mean": 0.005384650731707,
"sd": 0.008752912581836 
},
{
 "name": "sample",
"mean": 0.005922532682927,
"sd": 0.01624198564811 
},
{
 "name": "sample",
"mean": 0.006276136585366,
"sd": 0.0152284324824 
},
{
 "name": "sample",
"mean": 0.005149797073171,
"sd": 0.009580227613111 
},
{
 "name": "sample",
"mean": 0.005181082926829,
"sd": 0.008630907704425 
},
{
 "name": "sample",
"mean": 0.005324370731707,
"sd": 0.01263342001519 
},
{
 "name": "sample",
"mean": 0.005753983414634,
"sd": 0.0170604875015 
},
{
 "name": "sample",
"mean": 0.006281475121951,
"sd": 0.01614290556763 
},
{
 "name": "sample",
"mean": 0.005942555121951,
"sd": 0.01527908558264 
},
{
 "name": "sample",
"mean": 0.005374815609756,
"sd": 0.008494143580054 
},
{
 "name": "sample",
"mean": 0.006212100487805,
"sd": 0.01788036823797 
},
{
 "name": "sample",
"mean": 0.005100636097561,
"sd": 0.01283074903733 
},
{
 "name": "sample",
"mean": 0.006521335609756,
"sd": 0.01629680828887 
},
{
 "name": "sample",
"mean": 0.00646783902439,
"sd": 0.01764120745411 
},
{
 "name": "sample",
"mean": 0.005692157073171,
"sd": 0.01913928492405 
},
{
 "name": "sample",
"mean": 0.00544632097561,
"sd": 0.01708710113116 
},
{
 "name": "sample",
"mean": 0.006147996097561,
"sd": 0.01853873992896 
},
{
 "name": "sample",
"mean": 0.006161830243902,
"sd": 0.01557441208414 
},
{
 "name": "sample",
"mean": 0.005263452682927,
"sd": 0.01129570392421 
},
{
 "name": "sample",
"mean": 0.005387770731707,
"sd": 0.009268691303201 
},
{
 "name": "sample",
"mean": 0.006669596097561,
"sd": 0.02004097258401 
},
{
 "name": "sample",
"mean": 0.005475396097561,
"sd": 0.00913350480561 
},
{
 "name": "sample",
"mean": 0.006803053658537,
"sd": 0.02238111819938 
},
{
 "name": "sample",
"mean": 0.00497188097561,
"sd": 0.01137792651346 
},
{
 "name": "sample",
"mean": 0.00626940195122,
"sd": 0.01612745405764 
},
{
 "name": "sample",
"mean": 0.006200649756098,
"sd": 0.01723534684053 
},
{
 "name": "sample",
"mean": 0.005122056585366,
"sd": 0.01076094101845 
},
{
 "name": "sample",
"mean": 0.006199722926829,
"sd": 0.0220139671075 
},
{
 "name": "sample",
"mean": 0.006046234146341,
"sd": 0.01340233512547 
},
{
 "name": "sample",
"mean": 0.006085909268293,
"sd": 0.01537523468921 
},
{
 "name": "sample",
"mean": 0.006391543414634,
"sd": 0.02050176078034 
},
{
 "name": "sample",
"mean": 0.006522127804878,
"sd": 0.01464259161526 
},
{
 "name": "sample",
"mean": 0.005642222439024,
"sd": 0.009895663252335 
},
{
 "name": "sample",
"mean": 0.005690584390244,
"sd": 0.01587966123082 
},
{
 "name": "sample",
"mean": 0.00608628097561,
"sd": 0.0135127447417 
},
{
 "name": "sample",
"mean": 0.005785329756098,
"sd": 0.0119826543786 
},
{
 "name": "sample",
"mean": 0.007069299512195,
"sd": 0.03226986739741 
},
{
 "name": "sample",
"mean": 0.005545685853659,
"sd": 0.01236635362311 
},
{
 "name": "sample",
"mean": 0.006214644878049,
"sd": 0.01890752711176 
},
{
 "name": "sample",
"mean": 0.005829804878049,
"sd": 0.01373150798093 
},
{
 "name": "sample",
"mean": 0.00511607902439,
"sd": 0.01136338253358 
},
{
 "name": "sample",
"mean": 0.005657503414634,
"sd": 0.0107300587048 
},
{
 "name": "sample",
"mean": 0.005385592195122,
"sd": 0.008599048033537 
},
{
 "name": "sample",
"mean": 0.006313086829268,
"sd": 0.01453123679763 
},
{
 "name": "sample",
"mean": 0.006912169756098,
"sd": 0.02684709728825 
},
{
 "name": "sample",
"mean": 0.005447576585366,
"sd": 0.01195813474309 
},
{
 "name": "sample",
"mean":     0.00510916,
"sd": 0.009477791031368 
},
{
 "name": "sample",
"mean": 0.005428131707317,
"sd": 0.008625696404896 
},
{
 "name": "sample",
"mean": 0.006579986341463,
"sd": 0.02462776726745 
},
{
 "name": "sample",
"mean": 0.00544364097561,
"sd": 0.009149864575033 
},
{
 "name": "sample",
"mean":      0.0059434,
"sd": 0.01178312880783 
},
{
 "name": "sample",
"mean": 0.006058315121951,
"sd": 0.01287555521852 
},
{
 "name": "sample",
"mean": 0.006202781463415,
"sd": 0.01398387974939 
},
{
 "name": "sample",
"mean": 0.006103716097561,
"sd": 0.01556761627295 
},
{
 "name": "sample",
"mean": 0.006157619512195,
"sd": 0.01872800149113 
},
{
 "name": "sample",
"mean": 0.007179837073171,
"sd": 0.0335609165963 
},
{
 "name": "sample",
"mean": 0.005017906341463,
"sd": 0.01189241561269 
},
{
 "name": "sample",
"mean": 0.006445182439024,
"sd": 0.01751473105691 
},
{
 "name": "sample",
"mean": 0.006354738536585,
"sd": 0.01430530949029 
},
{
 "name": "sample",
"mean": 0.005351242926829,
"sd": 0.01322363243909 
},
{
 "name": "sample",
"mean": 0.00520103804878,
"sd": 0.0081816155844 
},
{
 "name": "sample",
"mean": 0.005941010731707,
"sd": 0.01145125242458 
},
{
 "name": "sample",
"mean": 0.006563485853659,
"sd": 0.02691034009682 
},
{
 "name": "sample",
"mean": 0.005776185365854,
"sd": 0.01186699915364 
},
{
 "name": "sample",
"mean": 0.006030085853659,
"sd": 0.01328652424565 
},
{
 "name": "sample",
"mean": 0.006401971707317,
"sd": 0.0196796555048 
},
{
 "name": "sample",
"mean": 0.005003074146341,
"sd": 0.02284059094496 
},
{
 "name": "sample",
"mean": 0.00638519902439,
"sd": 0.01692043689914 
},
{
 "name": "sample",
"mean": 0.006776082926829,
"sd": 0.02863904050175 
},
{
 "name": "sample",
"mean": 0.006007584390244,
"sd": 0.01347852925604 
},
{
 "name": "sample",
"mean": 0.005765132682927,
"sd": 0.01490566592182 
},
{
 "name": "sample",
"mean": 0.006175231219512,
"sd": 0.01329977750222 
},
{
 "name": "sample",
"mean": 0.006371874146341,
"sd": 0.01456714616452 
},
{
 "name": "sample",
"mean": 0.005730507317073,
"sd": 0.01707076206268 
},
{
 "name": "sample",
"mean":     0.00662524,
"sd": 0.01906758059527 
},
{
 "name": "sample",
"mean": 0.005133673170732,
"sd": 0.01307409803421 
},
{
 "name": "sample",
"mean": 0.006156357073171,
"sd": 0.01475811213239 
},
{
 "name": "sample",
"mean": 0.006554260487805,
"sd": 0.01930138823777 
},
{
 "name": "sample",
"mean": 0.005502034146341,
"sd": 0.01633165709562 
},
{
 "name": "sample",
"mean": 0.006251866341463,
"sd": 0.01342632540552 
},
{
 "name": "sample",
"mean": 0.005984653658537,
"sd": 0.01434393622448 
},
{
 "name": "sample",
"mean": 0.00552795902439,
"sd": 0.009847380923954 
},
{
 "name": "sample",
"mean": 0.006138256585366,
"sd": 0.01430085661903 
},
{
 "name": "sample",
"mean": 0.005976672195122,
"sd": 0.01283468952637 
},
{
 "name": "sample",
"mean": 0.005595099512195,
"sd": 0.01213141320306 
},
{
 "name": "sample",
"mean": 0.006523511219512,
"sd": 0.01412933961467 
},
{
 "name": "sample",
"mean": 0.00705687804878,
"sd": 0.0301612120424 
},
{
 "name": "sample",
"mean": 0.005283523902439,
"sd": 0.01945912868918 
},
{
 "name": "sample",
"mean": 0.005899503414634,
"sd": 0.01371154811253 
},
{
 "name": "sample",
"mean": 0.005965696585366,
"sd": 0.01311205501836 
},
{
 "name": "sample",
"mean": 0.005792181463415,
"sd": 0.01191928034998 
},
{
 "name": "sample",
"mean": 0.005729524878049,
"sd": 0.01031398896346 
},
{
 "name": "sample",
"mean": 0.006216940487805,
"sd": 0.01531135964352 
},
{
 "name": "sample",
"mean":     0.00631688,
"sd": 0.02079969132791 
},
{
 "name": "sample",
"mean": 0.006085728780488,
"sd": 0.01223910945927 
},
{
 "name": "sample",
"mean": 0.005622852682927,
"sd": 0.009474312787907 
},
{
 "name": "sample",
"mean": 0.005309842926829,
"sd": 0.01068477439738 
},
{
 "name": "sample",
"mean": 0.005505215609756,
"sd": 0.009288708388217 
},
{
 "name": "sample",
"mean": 0.004969676097561,
"sd": 0.01144866023243 
},
{
 "name": "sample",
"mean": 0.005138663414634,
"sd": 0.01827804735032 
},
{
 "name": "sample",
"mean": 0.006467307317073,
"sd": 0.01662459410528 
},
{
 "name": "sample",
"mean": 0.006051538536585,
"sd": 0.01502712632497 
},
{
 "name": "sample",
"mean": 0.005047977560976,
"sd": 0.02179980968769 
},
{
 "name": "sample",
"mean": 0.005473455609756,
"sd": 0.01183940928085 
},
{
 "name": "sample",
"mean": 0.00629687804878,
"sd": 0.01361291437922 
},
{
 "name": "sample",
"mean": 0.005146233170732,
"sd": 0.01869578428188 
},
{
 "name": "sample",
"mean": 0.007196662439024,
"sd": 0.03071082354951 
},
{
 "name": "sample",
"mean": 0.005895371707317,
"sd": 0.0123716930755 
},
{
 "name": "sample",
"mean": 0.004926708292683,
"sd": 0.01289002594961 
},
{
 "name": "sample",
"mean": 0.005402580487805,
"sd": 0.01495306265644 
},
{
 "name": "sample",
"mean": 0.007068011707317,
"sd": 0.02935170788382 
},
{
 "name": "sample",
"mean": 0.005619842926829,
"sd": 0.01741569776202 
},
{
 "name": "sample",
"mean": 0.006855949268293,
"sd": 0.02420284367319 
},
{
 "name": "sample",
"mean": 0.005830116097561,
"sd": 0.0127410954838 
},
{
 "name": "sample",
"mean": 0.006161202926829,
"sd": 0.01595931290327 
},
{
 "name": "sample",
"mean": 0.006006046829268,
"sd": 0.01440029516367 
},
{
 "name": "sample",
"mean": 0.006629002926829,
"sd": 0.02460474534595 
},
{
 "name": "sample",
"mean": 0.005945182439024,
"sd": 0.01614560107559 
},
{
 "name": "sample",
"mean": 0.005598088780488,
"sd": 0.009026190376774 
},
{
 "name": "sample",
"mean": 0.00628364097561,
"sd": 0.0143724051799 
},
{
 "name": "sample",
"mean": 0.006495343414634,
"sd": 0.02344371510459 
},
{
 "name": "sample",
"mean": 0.006034863414634,
"sd": 0.01630644418846 
},
{
 "name": "sample",
"mean": 0.006737632195122,
"sd": 0.02801621326173 
},
{
 "name": "sample",
"mean": 0.00661651902439,
"sd": 0.01713823821265 
},
{
 "name": "sample",
"mean": 0.005218183414634,
"sd": 0.01160416011829 
},
{
 "name": "sample",
"mean": 0.005368819512195,
"sd": 0.0115807207118 
},
{
 "name": "sample",
"mean": 0.006109617560976,
"sd": 0.0165150687958 
},
{
 "name": "sample",
"mean": 0.006241002926829,
"sd": 0.01763870744225 
},
{
 "name": "sample",
"mean": 0.006280491707317,
"sd": 0.01605470556452 
},
{
 "name": "sample",
"mean": 0.006078928780488,
"sd": 0.01548036212779 
},
{
 "name": "sample",
"mean":     0.00517336,
"sd": 0.01387836582019 
},
{
 "name": "sample",
"mean": 0.005871155121951,
"sd": 0.0150010885806 
},
{
 "name": "sample",
"mean": 0.006153664390244,
"sd": 0.016390975428 
},
{
 "name": "sample",
"mean": 0.006245032195122,
"sd": 0.01425161099121 
},
{
 "name": "sample",
"mean": 0.006486575609756,
"sd": 0.02253527057773 
},
{
 "name": "sample",
"mean": 0.006127697560976,
"sd": 0.01299664514303 
},
{
 "name": "sample",
"mean": 0.005744705365854,
"sd": 0.01458587055502 
},
{
 "name": "sample",
"mean": 0.005431096585366,
"sd": 0.009513835037039 
},
{
 "name": "sample",
"mean": 0.007329110243902,
"sd": 0.03507318354426 
},
{
 "name": "sample",
"mean": 0.006992473170732,
"sd": 0.03113987299123 
},
{
 "name": "sample",
"mean": 0.006187927804878,
"sd": 0.01537908331207 
},
{
 "name": "sample",
"mean": 0.006361735609756,
"sd": 0.01710709149857 
},
{
 "name": "sample",
"mean": 0.006475324878049,
"sd": 0.02308159346042 
},
{
 "name": "sample",
"mean": 0.005136618536585,
"sd": 0.02132790482494 
},
{
 "name": "sample",
"mean": 0.005084857560976,
"sd": 0.0107481977831 
},
{
 "name": "sample",
"mean": 0.005347602926829,
"sd": 0.01244048645666 
},
{
 "name": "sample",
"mean": 0.006110566829268,
"sd": 0.01451120517888 
},
{
 "name": "sample",
"mean": 0.005933124878049,
"sd": 0.01301056949846 
},
{
 "name": "sample",
"mean": 0.005055101463415,
"sd": 0.01662537841448 
},
{
 "name": "sample",
"mean": 0.00551508195122,
"sd": 0.01709214846811 
},
{
 "name": "sample",
"mean": 0.006791578536585,
"sd": 0.02379904071724 
},
{
 "name": "sample",
"mean": 0.005495170731707,
"sd": 0.01200495588317 
},
{
 "name": "sample",
"mean": 0.006456432195122,
"sd": 0.01733837217427 
},
{
 "name": "sample",
"mean":     0.00566296,
"sd": 0.0128554472539 
},
{
 "name": "sample",
"mean": 0.006184083902439,
"sd": 0.01305700719635 
},
{
 "name": "sample",
"mean": 0.006227769756098,
"sd": 0.01583955139823 
},
{
 "name": "sample",
"mean": 0.005264956097561,
"sd": 0.008519333739842 
},
{
 "name": "sample",
"mean": 0.005097112195122,
"sd": 0.0186020095542 
},
{
 "name": "sample",
"mean": 0.005380312195122,
"sd": 0.01271847504056 
},
{
 "name": "sample",
"mean": 0.004938716097561,
"sd": 0.0114692076639 
},
{
 "name": "sample",
"mean": 0.006819633170732,
"sd": 0.02914970684528 
},
{
 "name": "sample",
"mean": 0.005874709268293,
"sd": 0.01365177256283 
},
{
 "name": "sample",
"mean": 0.005970848780488,
"sd": 0.0116742187439 
},
{
 "name": "sample",
"mean": 0.005634844878049,
"sd": 0.01440888613143 
},
{
 "name": "sample",
"mean": 0.005800077073171,
"sd": 0.01224099681928 
},
{
 "name": "sample",
"mean": 0.005545450731707,
"sd": 0.01836058455722 
},
{
 "name": "sample",
"mean": 0.006423006829268,
"sd": 0.01480656778922 
},
{
 "name": "sample",
"mean": 0.00632884195122,
"sd": 0.02042633421499 
},
{
 "name": "sample",
"mean": 0.005599633170732,
"sd": 0.009125415661039 
},
{
 "name": "sample",
"mean": 0.006315933658537,
"sd": 0.01925781852906 
},
{
 "name": "sample",
"mean": 0.005509633170732,
"sd": 0.01206875481551 
},
{
 "name": "sample",
"mean": 0.006390284878049,
"sd": 0.02198441956963 
},
{
 "name": "sample",
"mean": 0.00540604097561,
"sd": 0.01784136473087 
},
{
 "name": "sample",
"mean": 0.004903932682927,
"sd": 0.01189152745127 
},
{
 "name": "sample",
"mean": 0.005064688780488,
"sd": 0.01452651796836 
},
{
 "name": "sample",
"mean": 0.006760037073171,
"sd": 0.0263794947018 
},
{
 "name": "sample",
"mean": 0.005172338536585,
"sd": 0.02112802620795 
},
{
 "name": "sample",
"mean": 0.006380122926829,
"sd": 0.01784439126334 
},
{
 "name": "sample",
"mean": 0.004970362926829,
"sd": 0.01203664658449 
},
{
 "name": "sample",
"mean": 0.005948510243902,
"sd": 0.01279272152235 
},
{
 "name": "sample",
"mean": 0.005061085853659,
"sd": 0.02174651137898 
},
{
 "name": "sample",
"mean": 0.006139231219512,
"sd": 0.01733927660132 
},
{
 "name": "sample",
"mean": 0.006450832195122,
"sd": 0.01684535377528 
},
{
 "name": "sample",
"mean": 0.006529026341463,
"sd": 0.02375946949284 
},
{
 "name": "sample",
"mean": 0.006249206829268,
"sd": 0.01434783470817 
},
{
 "name": "sample",
"mean": 0.005314525853659,
"sd": 0.01250357400431 
},
{
 "name": "sample",
"mean": 0.005319555121951,
"sd": 0.01626857043738 
},
{
 "name": "sample",
"mean": 0.005621806829268,
"sd": 0.0122057981235 
},
{
 "name": "sample",
"mean": 0.006352580487805,
"sd": 0.02203501030917 
},
{
 "name": "sample",
"mean": 0.005238903414634,
"sd": 0.01293904490624 
},
{
 "name": "sample",
"mean": 0.005060554146341,
"sd": 0.009462898001226 
},
{
 "name": "sample",
"mean": 0.00645063902439,
"sd": 0.01423218966516 
},
{
 "name": "sample",
"mean": 0.005355594146341,
"sd": 0.01485353545899 
},
{
 "name": "sample",
"mean": 0.006151433170732,
"sd": 0.01435768069137 
},
{
 "name": "sample",
"mean": 0.005513777560976,
"sd": 0.009030119762049 
},
{
 "name": "sample",
"mean": 0.006855304390244,
"sd": 0.02862669942999 
},
{
 "name": "sample",
"mean": 0.006980963902439,
"sd": 0.02986195808306 
},
{
 "name": "sample",
"mean": 0.00549480195122,
"sd": 0.009097782647372 
},
{
 "name": "sample",
"mean": 0.007118106341463,
"sd": 0.02996775342356 
},
{
 "name": "sample",
"mean": 0.007091237073171,
"sd": 0.0294978269796 
},
{
 "name": "sample",
"mean": 0.007010592195122,
"sd": 0.02913343009786 
},
{
 "name": "sample",
"mean": 0.006392336585366,
"sd": 0.01643760204635 
},
{
 "name": "sample",
"mean": 0.005780257560976,
"sd": 0.01349873991235 
},
{
 "name": "sample",
"mean": 0.006330139512195,
"sd": 0.01605380577181 
},
{
 "name": "sample",
"mean": 0.005274309268293,
"sd": 0.02121640097626 
},
{
 "name": "sample",
"mean": 0.005589135609756,
"sd": 0.01193645801379 
},
{
 "name": "sample",
"mean": 0.005915188292683,
"sd": 0.01304751819117 
},
{
 "name": "sample",
"mean": 0.006129571707317,
"sd": 0.01539367895506 
},
{
 "name": "sample",
"mean": 0.006459814634146,
"sd": 0.01567826941355 
},
{
 "name": "sample",
"mean": 0.005551712195122,
"sd": 0.01176679751493 
},
{
 "name": "sample",
"mean": 0.006507189268293,
"sd": 0.01556099988425 
},
{
 "name": "sample",
"mean": 0.005821454634146,
"sd": 0.01311547938502 
},
{
 "name": "sample",
"mean": 0.007312525853659,
"sd": 0.03425884660881 
},
{
 "name": "sample",
"mean": 0.005133818536585,
"sd": 0.01223825894484 
},
{
 "name": "sample",
"mean": 0.006174966829268,
"sd": 0.01712771063133 
},
{
 "name": "sample",
"mean": 0.005847824390244,
"sd": 0.01207348081697 
},
{
 "name": "sample",
"mean": 0.006831963902439,
"sd": 0.02355326558027 
},
{
 "name": "sample",
"mean": 0.006373293658537,
"sd": 0.02196117864389 
},
{
 "name": "sample",
"mean": 0.005779336585366,
"sd": 0.01032547044523 
},
{
 "name": "sample",
"mean": 0.006708620487805,
"sd": 0.0234487116777 
},
{
 "name": "sample",
"mean": 0.005241508292683,
"sd": 0.008215769885671 
},
{
 "name": "sample",
"mean": 0.006764342439024,
"sd": 0.02855522387612 
},
{
 "name": "sample",
"mean": 0.005466522926829,
"sd": 0.008843378910874 
},
{
 "name": "sample",
"mean": 0.005929408780488,
"sd": 0.01397686812264 
},
{
 "name": "sample",
"mean": 0.005595695609756,
"sd": 0.0176411869813 
},
{
 "name": "sample",
"mean": 0.005061344390244,
"sd": 0.01214275649926 
},
{
 "name": "sample",
"mean": 0.006654426341463,
"sd": 0.02591255376229 
},
{
 "name": "sample",
"mean": 0.006599730731707,
"sd": 0.01682371055489 
},
{
 "name": "sample",
"mean": 0.005142552195122,
"sd": 0.01949184030199 
},
{
 "name": "sample",
"mean": 0.005664513170732,
"sd": 0.01590256775917 
},
{
 "name": "sample",
"mean":       0.005873,
"sd": 0.01074425971765 
},
{
 "name": "sample",
"mean": 0.005548533658537,
"sd": 0.01269310755042 
},
{
 "name": "sample",
"mean": 0.005992215609756,
"sd": 0.01273948676227 
},
{
 "name": "sample",
"mean": 0.005169947317073,
"sd": 0.02067425158765 
},
{
 "name": "sample",
"mean": 0.00527011902439,
"sd": 0.00992559199277 
},
{
 "name": "sample",
"mean":     0.00612744,
"sd": 0.01612322255223 
},
{
 "name": "sample",
"mean": 0.006357849756098,
"sd": 0.01371768639185 
},
{
 "name": "sample",
"mean": 0.006225547317073,
"sd": 0.02237378441241 
},
{
 "name": "sample",
"mean": 0.004923957073171,
"sd": 0.01156402398985 
},
{
 "name": "sample",
"mean": 0.005288914146341,
"sd": 0.018538493734 
},
{
 "name": "sample",
"mean": 0.005565215609756,
"sd": 0.01527962483562 
},
{
 "name": "sample",
"mean": 0.006071551219512,
"sd": 0.01520252547892 
},
{
 "name": "sample",
"mean": 0.006243575609756,
"sd": 0.01587531131999 
},
{
 "name": "sample",
"mean": 0.005393233170732,
"sd": 0.009258770954988 
},
{
 "name": "sample",
"mean": 0.006340022439024,
"sd": 0.01361467083614 
},
{
 "name": "sample",
"mean": 0.005194447804878,
"sd": 0.01321329737974 
},
{
 "name": "sample",
"mean": 0.006089126829268,
"sd": 0.01287924152644 
},
{
 "name": "sample",
"mean": 0.005070286829268,
"sd": 0.01668800790881 
},
{
 "name": "sample",
"mean": 0.006085846829268,
"sd": 0.01610439276445 
},
{
 "name": "sample",
"mean": 0.005312865365854,
"sd": 0.01064776225465 
},
{
 "name": "sample",
"mean": 0.005615811707317,
"sd": 0.009507956123589 
},
{
 "name": "sample",
"mean": 0.006350117073171,
"sd": 0.01641233168626 
},
{
 "name": "sample",
"mean": 0.004975167804878,
"sd": 0.02213127352846 
},
{
 "name": "sample",
"mean": 0.006283352195122,
"sd": 0.01647241799272 
},
{
 "name": "sample",
"mean": 0.005703741463415,
"sd": 0.01521305386638 
},
{
 "name": "sample",
"mean": 0.005817088780488,
"sd": 0.01231363981909 
},
{
 "name": "sample",
"mean": 0.006437365853659,
"sd": 0.01746741187081 
},
{
 "name": "sample",
"mean": 0.00565088195122,
"sd": 0.01313203911094 
},
{
 "name": "sample",
"mean": 0.004924123902439,
"sd": 0.01265121786041 
},
{
 "name": "sample",
"mean": 0.004907286829268,
"sd": 0.01688398299653 
},
{
 "name": "sample",
"mean": 0.00728551902439,
"sd": 0.03472183693895 
},
{
 "name": "simplex",
"mean": 0.006114802958597,
"sd": 0.01607204388833 
},
{
 "name": "simplex",
"mean": 0.006485789031401,
"sd": 0.02108826492369 
},
{
 "name": "simplex",
"mean": 0.005514830122287,
"sd": 0.0149558580549 
},
{
 "name": "simplex",
"mean": 0.005845506318079,
"sd": 0.01196833678539 
},
{
 "name": "simplex",
"mean": 0.005246088636513,
"sd": 0.009398483368582 
},
{
 "name": "simplex",
"mean": 0.006589963174424,
"sd": 0.02320739797228 
},
{
 "name": "simplex",
"mean": 0.006418850114773,
"sd": 0.01925595295976 
},
{
 "name": "simplex",
"mean": 0.006151288544515,
"sd": 0.01367455532542 
},
{
 "name": "simplex",
"mean": 0.005855775852259,
"sd": 0.01354203748709 
},
{
 "name": "simplex",
"mean": 0.005745233867658,
"sd": 0.0135279131359 
},
{
 "name": "simplex",
"mean": 0.005802091546863,
"sd": 0.01452753926015 
},
{
 "name": "simplex",
"mean": 0.005952878868473,
"sd": 0.01163179795148 
},
{
 "name": "simplex",
"mean": 0.005878588717844,
"sd": 0.01191907953394 
},
{
 "name": "simplex",
"mean": 0.005664901720977,
"sd": 0.0119995376585 
},
{
 "name": "simplex",
"mean": 0.005578653094036,
"sd": 0.011862648522 
},
{
 "name": "simplex",
"mean": 0.006513742123257,
"sd": 0.01960887935823 
},
{
 "name": "simplex",
"mean": 0.006275934720553,
"sd": 0.01922865665399 
},
{
 "name": "simplex",
"mean": 0.006521248732464,
"sd": 0.01860742859475 
},
{
 "name": "simplex",
"mean": 0.006060354239652,
"sd": 0.01513436710472 
},
{
 "name": "simplex",
"mean": 0.006072128368771,
"sd": 0.0174087411165 
},
{
 "name": "simplex",
"mean": 0.005759817843193,
"sd": 0.01127697362261 
},
{
 "name": "simplex",
"mean": 0.005098417793313,
"sd": 0.01095157781339 
},
{
 "name": "simplex",
"mean": 0.005810733607483,
"sd": 0.01266529763902 
},
{
 "name": "simplex",
"mean": 0.005899623580416,
"sd": 0.01423270583771 
},
{
 "name": "simplex",
"mean": 0.006557334722091,
"sd": 0.01808927222597 
},
{
 "name": "simplex",
"mean": 0.00564583374981,
"sd": 0.01155101823848 
},
{
 "name": "simplex",
"mean": 0.005275149655627,
"sd": 0.01507414536653 
},
{
 "name": "simplex",
"mean": 0.005711776494208,
"sd": 0.01416443374883 
},
{
 "name": "simplex",
"mean": 0.005927311483249,
"sd": 0.01256627021573 
},
{
 "name": "simplex",
"mean": 0.00612036632142,
"sd": 0.01538445287564 
},
{
 "name": "simplex",
"mean": 0.006176166320839,
"sd": 0.01302277139211 
},
{
 "name": "simplex",
"mean": 0.006304418699382,
"sd": 0.0187949563613 
},
{
 "name": "simplex",
"mean": 0.005823651596857,
"sd": 0.01580611860961 
},
{
 "name": "simplex",
"mean": 0.005333694100169,
"sd": 0.01252163453534 
},
{
 "name": "simplex",
"mean": 0.005883800395474,
"sd": 0.01162360753538 
},
{
 "name": "simplex",
"mean": 0.005974423649411,
"sd": 0.01384247428327 
},
{
 "name": "simplex",
"mean": 0.005577714166503,
"sd": 0.01173319917304 
},
{
 "name": "simplex",
"mean": 0.005591625745535,
"sd": 0.01216507971225 
},
{
 "name": "simplex",
"mean": 0.005530479891429,
"sd": 0.00957224812511 
},
{
 "name": "simplex",
"mean": 0.005256407930358,
"sd": 0.01219591403019 
},
{
 "name": "simplex",
"mean": 0.005225194145538,
"sd": 0.0115311465073 
},
{
 "name": "simplex",
"mean": 0.005914239143064,
"sd": 0.01485839139701 
},
{
 "name": "simplex",
"mean": 0.005722882635529,
"sd": 0.0129545701704 
},
{
 "name": "simplex",
"mean": 0.00585470367498,
"sd": 0.01266690897951 
},
{
 "name": "simplex",
"mean": 0.006027700537711,
"sd": 0.01428565991608 
},
{
 "name": "simplex",
"mean": 0.005945961777761,
"sd": 0.01458952214169 
},
{
 "name": "simplex",
"mean": 0.005826721688174,
"sd": 0.0142639107714 
},
{
 "name": "simplex",
"mean": 0.006473299399354,
"sd": 0.02141006815499 
},
{
 "name": "simplex",
"mean": 0.005807191147365,
"sd": 0.01486700385351 
},
{
 "name": "simplex",
"mean": 0.005748118177299,
"sd": 0.01266390538702 
},
{
 "name": "simplex",
"mean": 0.005571642643611,
"sd": 0.01109517101199 
},
{
 "name": "simplex",
"mean": 0.006303622737422,
"sd": 0.01632095076003 
},
{
 "name": "simplex",
"mean": 0.005594903224887,
"sd": 0.01398219447137 
},
{
 "name": "simplex",
"mean": 0.006009788497288,
"sd": 0.01425126794273 
},
{
 "name": "simplex",
"mean": 0.005236509960337,
"sd": 0.01244929919334 
},
{
 "name": "simplex",
"mean": 0.00557660069044,
"sd": 0.01191671784934 
},
{
 "name": "simplex",
"mean": 0.005223741342793,
"sd": 0.01228126265596 
},
{
 "name": "simplex",
"mean": 0.005414757015477,
"sd": 0.01182622702081 
},
{
 "name": "simplex",
"mean": 0.005972389714056,
"sd": 0.01344695747321 
},
{
 "name": "simplex",
"mean": 0.006046734265809,
"sd": 0.01641011708388 
},
{
 "name": "simplex",
"mean": 0.006731907102437,
"sd": 0.02259400745253 
},
{
 "name": "simplex",
"mean": 0.005330755269305,
"sd": 0.01102411284091 
},
{
 "name": "simplex",
"mean": 0.005518277047436,
"sd": 0.01647993413954 
},
{
 "name": "simplex",
"mean": 0.005511241507097,
"sd": 0.01156316682444 
},
{
 "name": "simplex",
"mean": 0.005383539182834,
"sd": 0.009717896447272 
},
{
 "name": "simplex",
"mean": 0.006570389119163,
"sd": 0.01876893404028 
},
{
 "name": "simplex",
"mean": 0.006722423596129,
"sd": 0.02361037669073 
},
{
 "name": "simplex",
"mean": 0.005696136264913,
"sd": 0.01287460686036 
},
{
 "name": "simplex",
"mean": 0.005928133578006,
"sd": 0.01328148789956 
},
{
 "name": "simplex",
"mean": 0.005188959894762,
"sd": 0.01847698375936 
},
{
 "name": "simplex",
"mean": 0.00583634922661,
"sd": 0.01277303169503 
},
{
 "name": "simplex",
"mean": 0.00588103090491,
"sd": 0.01566301222918 
},
{
 "name": "simplex",
"mean": 0.006337181094879,
"sd": 0.01560220000325 
},
{
 "name": "simplex",
"mean": 0.006032993715688,
"sd": 0.01257111448831 
},
{
 "name": "simplex",
"mean": 0.005522936427379,
"sd": 0.01218071208132 
},
{
 "name": "simplex",
"mean": 0.005466601084806,
"sd": 0.01060190606523 
},
{
 "name": "simplex",
"mean": 0.006097497203643,
"sd": 0.01302729307706 
},
{
 "name": "simplex",
"mean": 0.006304232958128,
"sd": 0.01568099525139 
},
{
 "name": "simplex",
"mean": 0.005212204539661,
"sd": 0.01168649821563 
},
{
 "name": "simplex",
"mean": 0.005927827714474,
"sd": 0.01479218718148 
},
{
 "name": "simplex",
"mean": 0.006530027528723,
"sd": 0.01852500372866 
},
{
 "name": "simplex",
"mean": 0.005627784010547,
"sd": 0.01084234288799 
},
{
 "name": "simplex",
"mean": 0.006160747526218,
"sd": 0.01497139424201 
},
{
 "name": "simplex",
"mean": 0.005921312930158,
"sd": 0.01561022956961 
},
{
 "name": "simplex",
"mean": 0.006282319647859,
"sd": 0.01611629808249 
},
{
 "name": "simplex",
"mean": 0.005458293437438,
"sd": 0.01323325662963 
},
{
 "name": "simplex",
"mean": 0.005687408834579,
"sd": 0.01594939555041 
},
{
 "name": "simplex",
"mean": 0.00595800265097,
"sd": 0.01322229089603 
},
{
 "name": "simplex",
"mean": 0.006070907386838,
"sd": 0.01739992657159 
},
{
 "name": "simplex",
"mean": 0.006086093180861,
"sd": 0.01233365179644 
},
{
 "name": "simplex",
"mean": 0.005954506432856,
"sd": 0.01254995312158 
},
{
 "name": "simplex",
"mean": 0.005987806179743,
"sd": 0.01480905771628 
},
{
 "name": "simplex",
"mean": 0.005665670650576,
"sd": 0.01263422764664 
},
{
 "name": "simplex",
"mean": 0.005737682160774,
"sd": 0.01465389552261 
},
{
 "name": "simplex",
"mean": 0.00558659696147,
"sd": 0.01162380966854 
},
{
 "name": "simplex",
"mean": 0.005713167685624,
"sd": 0.01108768875023 
},
{
 "name": "simplex",
"mean": 0.00612039482179,
"sd": 0.01512123300398 
},
{
 "name": "simplex",
"mean": 0.006628977376289,
"sd": 0.02520826212309 
},
{
 "name": "simplex",
"mean": 0.005874400867217,
"sd": 0.01532894072742 
},
{
 "name": "simplex",
"mean": 0.005717347347286,
"sd": 0.01018960555328 
},
{
 "name": "simplex",
"mean": 0.005583240916149,
"sd": 0.01189513057337 
},
{
 "name": "simplex",
"mean": 0.006123928614141,
"sd": 0.01918122018221 
},
{
 "name": "simplex",
"mean": 0.005301918012413,
"sd": 0.01295589522645 
},
{
 "name": "simplex",
"mean": 0.006324961947568,
"sd": 0.01518022015922 
},
{
 "name": "simplex",
"mean": 0.005710754525274,
"sd": 0.01082494391284 
},
{
 "name": "simplex",
"mean": 0.005768674698781,
"sd": 0.01022101034334 
},
{
 "name": "simplex",
"mean": 0.005649466944878,
"sd": 0.01205977456047 
},
{
 "name": "simplex",
"mean": 0.005726340178932,
"sd": 0.01080902713048 
},
{
 "name": "simplex",
"mean": 0.005902792352101,
"sd": 0.01219062647622 
},
{
 "name": "simplex",
"mean": 0.005769653179879,
"sd": 0.0135728163942 
},
{
 "name": "simplex",
"mean": 0.005779332042041,
"sd": 0.01178636577989 
},
{
 "name": "simplex",
"mean": 0.005541828197851,
"sd": 0.01377247852442 
},
{
 "name": "simplex",
"mean": 0.005843262997996,
"sd": 0.01158081401949 
},
{
 "name": "simplex",
"mean": 0.005338956980436,
"sd": 0.01159039051072 
},
{
 "name": "simplex",
"mean": 0.00572085118213,
"sd": 0.01003738844357 
},
{
 "name": "simplex",
"mean": 0.00656385523931,
"sd": 0.02054910855342 
},
{
 "name": "simplex",
"mean": 0.006137084981529,
"sd": 0.01443732718715 
},
{
 "name": "simplex",
"mean": 0.006186496255848,
"sd": 0.01608017279577 
},
{
 "name": "simplex",
"mean": 0.005939477243409,
"sd": 0.0118179564205 
},
{
 "name": "simplex",
"mean": 0.006482942334665,
"sd": 0.02035334140838 
},
{
 "name": "simplex",
"mean": 0.006443079842931,
"sd": 0.02051701738019 
},
{
 "name": "simplex",
"mean": 0.005672939118859,
"sd": 0.01257193082993 
},
{
 "name": "simplex",
"mean": 0.005696514158888,
"sd": 0.01331814694712 
},
{
 "name": "simplex",
"mean": 0.005814125888852,
"sd": 0.01219346801793 
},
{
 "name": "simplex",
"mean": 0.005381671303093,
"sd": 0.01235407065822 
},
{
 "name": "simplex",
"mean": 0.005380051482107,
"sd": 0.01368869645142 
},
{
 "name": "simplex",
"mean": 0.005407816801003,
"sd": 0.01152410124191 
},
{
 "name": "simplex",
"mean": 0.006295815991482,
"sd": 0.0161636588878 
},
{
 "name": "simplex",
"mean": 0.005781401898743,
"sd": 0.0126298881536 
},
{
 "name": "simplex",
"mean": 0.005146530630961,
"sd": 0.0120701863607 
},
{
 "name": "simplex",
"mean": 0.006151526607828,
"sd": 0.01785228857616 
},
{
 "name": "simplex",
"mean": 0.005632032353398,
"sd": 0.0130005534748 
},
{
 "name": "simplex",
"mean": 0.005768328722073,
"sd": 0.01371464370154 
},
{
 "name": "simplex",
"mean": 0.00558741450619,
"sd": 0.01397632155087 
},
{
 "name": "simplex",
"mean": 0.005839347021532,
"sd": 0.01231892452587 
},
{
 "name": "simplex",
"mean": 0.005354964827111,
"sd": 0.01340307290632 
},
{
 "name": "simplex",
"mean": 0.006390046296712,
"sd": 0.0199620291859 
},
{
 "name": "simplex",
"mean": 0.005529564384244,
"sd": 0.009559492504289 
},
{
 "name": "simplex",
"mean": 0.005513628906357,
"sd": 0.01636627544787 
},
{
 "name": "simplex",
"mean": 0.005478363562743,
"sd": 0.01018902353472 
},
{
 "name": "simplex",
"mean": 0.00607122511297,
"sd": 0.01808604653079 
},
{
 "name": "simplex",
"mean": 0.005442283722266,
"sd": 0.011660049614 
},
{
 "name": "simplex",
"mean": 0.005533298230979,
"sd": 0.01786554957753 
},
{
 "name": "simplex",
"mean": 0.005923686039443,
"sd": 0.01224242658386 
},
{
 "name": "simplex",
"mean": 0.005569052792255,
"sd": 0.01153413297435 
},
{
 "name": "simplex",
"mean": 0.005819193757208,
"sd": 0.01336950008969 
},
{
 "name": "simplex",
"mean": 0.005161388540552,
"sd": 0.01048446049862 
},
{
 "name": "simplex",
"mean": 0.00640984791757,
"sd": 0.01837728348656 
},
{
 "name": "simplex",
"mean": 0.005712517299587,
"sd": 0.01129688208402 
},
{
 "name": "simplex",
"mean": 0.005797462147182,
"sd": 0.01218064783928 
},
{
 "name": "simplex",
"mean": 0.006075021696908,
"sd": 0.01298528777966 
},
{
 "name": "simplex",
"mean": 0.005790252767016,
"sd": 0.01306087672957 
},
{
 "name": "simplex",
"mean": 0.00539699123614,
"sd": 0.01315594621189 
},
{
 "name": "simplex",
"mean": 0.005525089578508,
"sd": 0.0144952674656 
},
{
 "name": "simplex",
"mean": 0.005789369676959,
"sd": 0.01101536586788 
},
{
 "name": "simplex",
"mean": 0.006350814187392,
"sd": 0.01711678331421 
},
{
 "name": "simplex",
"mean": 0.006097209085605,
"sd": 0.01341589299694 
},
{
 "name": "simplex",
"mean": 0.006116334089236,
"sd": 0.01465744804686 
},
{
 "name": "simplex",
"mean": 0.006373735742334,
"sd": 0.01687350607861 
},
{
 "name": "simplex",
"mean": 0.005509091583678,
"sd": 0.0114213578093 
},
{
 "name": "simplex",
"mean": 0.006002173341021,
"sd": 0.01557862071901 
},
{
 "name": "simplex",
"mean": 0.005676741081791,
"sd": 0.01359802030186 
},
{
 "name": "simplex",
"mean": 0.006133384971872,
"sd": 0.01885340973044 
},
{
 "name": "simplex",
"mean": 0.006016543994895,
"sd": 0.01398012942035 
},
{
 "name": "simplex",
"mean": 0.005884525758269,
"sd": 0.01524911632274 
},
{
 "name": "simplex",
"mean": 0.005488330206332,
"sd": 0.01021151705332 
},
{
 "name": "simplex",
"mean": 0.005312098723167,
"sd": 0.01000869732259 
},
{
 "name": "simplex",
"mean": 0.006112530638229,
"sd": 0.01482645010273 
},
{
 "name": "simplex",
"mean": 0.006269363221336,
"sd": 0.01616525947169 
},
{
 "name": "simplex",
"mean": 0.005826126603122,
"sd": 0.01591830814523 
},
{
 "name": "simplex",
"mean": 0.006110331727647,
"sd": 0.01366727138313 
},
{
 "name": "simplex",
"mean": 0.005590148268357,
"sd": 0.01363344176537 
},
{
 "name": "simplex",
"mean": 0.005432612701272,
"sd": 0.01016135505873 
},
{
 "name": "simplex",
"mean": 0.005556269161038,
"sd": 0.01465681051187 
},
{
 "name": "simplex",
"mean": 0.006176412556114,
"sd": 0.01786131229345 
},
{
 "name": "simplex",
"mean": 0.005606874917574,
"sd": 0.01289529147403 
},
{
 "name": "simplex",
"mean": 0.006007482540433,
"sd": 0.01413239135892 
},
{
 "name": "simplex",
"mean": 0.005597155913112,
"sd": 0.01304332311895 
},
{
 "name": "simplex",
"mean": 0.005880366464065,
"sd":  0.01288126806 
},
{
 "name": "simplex",
"mean": 0.005590085981059,
"sd": 0.01215817436867 
},
{
 "name": "simplex",
"mean": 0.005962761460095,
"sd": 0.01498825782702 
},
{
 "name": "simplex",
"mean": 0.005268163822261,
"sd": 0.01883651062657 
},
{
 "name": "simplex",
"mean": 0.005821129354028,
"sd": 0.01543206886725 
},
{
 "name": "simplex",
"mean": 0.005854449412379,
"sd": 0.01477060196973 
},
{
 "name": "simplex",
"mean": 0.005744457810229,
"sd": 0.01316876901845 
},
{
 "name": "simplex",
"mean": 0.006134519136181,
"sd": 0.01267362259552 
},
{
 "name": "simplex",
"mean": 0.006414913565432,
"sd": 0.02224017153154 
},
{
 "name": "simplex",
"mean": 0.006008593136972,
"sd": 0.01659175590887 
},
{
 "name": "simplex",
"mean": 0.00585739886825,
"sd": 0.01689413569482 
},
{
 "name": "simplex",
"mean": 0.005555767546702,
"sd": 0.01165153981043 
},
{
 "name": "simplex",
"mean": 0.00659181948566,
"sd": 0.02075580486609 
},
{
 "name": "simplex",
"mean": 0.006274011579733,
"sd": 0.02027352601803 
},
{
 "name": "simplex",
"mean": 0.006508512778346,
"sd": 0.02073739103775 
},
{
 "name": "simplex",
"mean": 0.006158576387137,
"sd": 0.01567264214268 
},
{
 "name": "simplex",
"mean": 0.006156197958533,
"sd": 0.01488831369527 
},
{
 "name": "simplex",
"mean": 0.005721487222749,
"sd": 0.01494835433605 
},
{
 "name": "simplex",
"mean": 0.005973591035997,
"sd": 0.01345997909625 
},
{
 "name": "simplex",
"mean": 0.005828850635249,
"sd": 0.01592193117826 
},
{
 "name": "simplex",
"mean": 0.006311208980379,
"sd": 0.01891493842612 
},
{
 "name": "simplex",
"mean": 0.005994998552311,
"sd": 0.01335611653081 
},
{
 "name": "simplex",
"mean": 0.006193698579164,
"sd": 0.01511462838769 
},
{
 "name": "simplex",
"mean": 0.005407256293706,
"sd": 0.01087098991334 
},
{
 "name": "simplex",
"mean": 0.006149129944225,
"sd": 0.01401078652208 
},
{
 "name": "simplex",
"mean": 0.005504234044825,
"sd": 0.01312207947607 
},
{
 "name": "simplex",
"mean": 0.005687841242291,
"sd": 0.0107257321204 
},
{
 "name": "simplex",
"mean": 0.006510021679434,
"sd": 0.01855543907215 
},
{
 "name": "simplex",
"mean": 0.005998780342064,
"sd": 0.01414545309889 
},
{
 "name": "simplex",
"mean": 0.005919123116799,
"sd": 0.01538608838889 
},
{
 "name": "simplex",
"mean": 0.005927624048567,
"sd": 0.01461073172507 
},
{
 "name": "simplex",
"mean": 0.005500048955878,
"sd": 0.01309453609895 
},
{
 "name": "simplex",
"mean": 0.006382553202855,
"sd": 0.0187850805714 
},
{
 "name": "simplex",
"mean": 0.006367861600786,
"sd": 0.01846801708993 
},
{
 "name": "simplex",
"mean": 0.005838990368931,
"sd": 0.01453437236728 
},
{
 "name": "simplex",
"mean": 0.005976573100319,
"sd": 0.01556310846207 
},
{
 "name": "simplex",
"mean": 0.005454055817243,
"sd": 0.0127572986316 
},
{
 "name": "simplex",
"mean": 0.006435765756463,
"sd": 0.01639439417151 
},
{
 "name": "simplex",
"mean": 0.006471463389523,
"sd": 0.01789031413227 
},
{
 "name": "simplex",
"mean": 0.006256501148987,
"sd": 0.01466628593101 
},
{
 "name": "simplex",
"mean": 0.00589275325333,
"sd": 0.01098643208872 
},
{
 "name": "simplex",
"mean": 0.006162362433064,
"sd": 0.01829754593477 
},
{
 "name": "simplex",
"mean": 0.005847641048067,
"sd": 0.01430968321944 
},
{
 "name": "simplex",
"mean": 0.006300190444065,
"sd": 0.01490863560943 
},
{
 "name": "simplex",
"mean": 0.00527832913791,
"sd": 0.01256678609405 
},
{
 "name": "simplex",
"mean": 0.006213717833225,
"sd": 0.01608973306097 
},
{
 "name": "simplex",
"mean": 0.006039684989114,
"sd": 0.01493020119952 
},
{
 "name": "simplex",
"mean": 0.006637909462202,
"sd": 0.0211907340174 
},
{
 "name": "simplex",
"mean": 0.006153547950493,
"sd": 0.01456786047032 
},
{
 "name": "simplex",
"mean": 0.005268434455122,
"sd": 0.01073708810019 
},
{
 "name": "simplex",
"mean": 0.005303259050918,
"sd": 0.01285463414012 
},
{
 "name": "simplex",
"mean": 0.006247208531599,
"sd": 0.01484657082673 
},
{
 "name": "simplex",
"mean": 0.005182342772715,
"sd": 0.009882061272507 
},
{
 "name": "simplex",
"mean": 0.00546459068559,
"sd": 0.009965227212131 
},
{
 "name": "simplex",
"mean": 0.005306047985852,
"sd": 0.0175456060009 
},
{
 "name": "simplex",
"mean": 0.005876511076427,
"sd": 0.01380300692438 
},
{
 "name": "simplex",
"mean": 0.006346589376683,
"sd": 0.01633561381965 
},
{
 "name": "simplex",
"mean": 0.006097519309205,
"sd": 0.01583621037421 
},
{
 "name": "simplex",
"mean": 0.006772109606107,
"sd": 0.02660089568033 
},
{
 "name": "simplex",
"mean": 0.00538345312648,
"sd": 0.01451659217617 
},
{
 "name": "simplex",
"mean": 0.006369027833259,
"sd": 0.0196489634393 
},
{
 "name": "simplex",
"mean": 0.005701128344392,
"sd": 0.01169543727627 
},
{
 "name": "simplex",
"mean": 0.005524926468117,
"sd": 0.01223911082384 
},
{
 "name": "simplex",
"mean": 0.006008095970334,
"sd": 0.0162821693825 
},
{
 "name": "simplex",
"mean": 0.005802499477646,
"sd": 0.01223109710444 
},
{
 "name": "simplex",
"mean": 0.005272867608686,
"sd": 0.01735352046988 
},
{
 "name": "simplex",
"mean": 0.005670781803741,
"sd": 0.01166799438193 
},
{
 "name": "simplex",
"mean": 0.006389395447869,
"sd": 0.01510693300975 
},
{
 "name": "simplex",
"mean": 0.005899121475662,
"sd": 0.01240739288692 
},
{
 "name": "simplex",
"mean": 0.005876128058569,
"sd": 0.0143917609248 
},
{
 "name": "simplex",
"mean": 0.005694192877653,
"sd": 0.0140871128948 
},
{
 "name": "simplex",
"mean": 0.005495941623727,
"sd": 0.01753084033053 
},
{
 "name": "simplex",
"mean": 0.005963520676257,
"sd": 0.01437640936903 
},
{
 "name": "simplex",
"mean": 0.00552978864588,
"sd": 0.01582131922085 
},
{
 "name": "simplex",
"mean": 0.00599326546925,
"sd": 0.01492332070758 
},
{
 "name": "simplex",
"mean": 0.005479504631027,
"sd": 0.0108444533071 
},
{
 "name": "simplex",
"mean": 0.00675319197604,
"sd": 0.02324736413272 
},
{
 "name": "simplex",
"mean": 0.005933169347232,
"sd": 0.01335293980372 
},
{
 "name": "simplex",
"mean": 0.005602527964325,
"sd": 0.01066778999211 
},
{
 "name": "simplex",
"mean": 0.005808981885307,
"sd": 0.01107691040699 
},
{
 "name": "simplex",
"mean": 0.006082161338932,
"sd": 0.01434809713598 
},
{
 "name": "simplex",
"mean": 0.006094177153701,
"sd": 0.01345888911133 
},
{
 "name": "simplex",
"mean": 0.005922099105932,
"sd": 0.01426571157627 
},
{
 "name": "simplex",
"mean": 0.005390250704051,
"sd": 0.01233656732922 
},
{
 "name": "simplex",
"mean": 0.006392693608631,
"sd": 0.01689904495208 
},
{
 "name": "simplex",
"mean": 0.006382659815332,
"sd": 0.01610654755864 
},
{
 "name": "simplex",
"mean": 0.005567020281526,
"sd": 0.009577850375433 
},
{
 "name": "simplex",
"mean": 0.006150074320913,
"sd": 0.01433858196473 
},
{
 "name": "simplex",
"mean": 0.005910776496496,
"sd": 0.01704049899929 
},
{
 "name": "simplex",
"mean": 0.006712034116787,
"sd": 0.0264722963599 
},
{
 "name": "simplex",
"mean": 0.006137843291408,
"sd": 0.01513860848399 
},
{
 "name": "simplex",
"mean": 0.006027171237127,
"sd": 0.01256798316807 
},
{
 "name": "simplex",
"mean": 0.005748772158307,
"sd": 0.01574450423759 
},
{
 "name": "simplex",
"mean": 0.005626379281481,
"sd": 0.01195019406394 
},
{
 "name": "simplex",
"mean": 0.005670671702867,
"sd": 0.01306915057674 
},
{
 "name": "simplex",
"mean": 0.006323720986466,
"sd": 0.0173667705062 
},
{
 "name": "simplex",
"mean": 0.005849548176668,
"sd": 0.01231763445429 
},
{
 "name": "simplex",
"mean": 0.006414983189957,
"sd": 0.01761788420669 
},
{
 "name": "simplex",
"mean": 0.00585369066842,
"sd": 0.01412986326272 
},
{
 "name": "simplex",
"mean": 0.005809437772664,
"sd": 0.01316718028597 
},
{
 "name": "simplex",
"mean": 0.005900918690025,
"sd": 0.0149961894089 
},
{
 "name": "simplex",
"mean": 0.005755399281872,
"sd": 0.01138733929605 
},
{
 "name": "simplex",
"mean": 0.005399989322926,
"sd": 0.01239739220498 
},
{
 "name": "simplex",
"mean": 0.005817811756587,
"sd": 0.0118521582681 
},
{
 "name": "simplex",
"mean": 0.006464545469185,
"sd": 0.0172787882117 
},
{
 "name": "simplex",
"mean": 0.006119786189816,
"sd": 0.01419303345051 
},
{
 "name": "simplex",
"mean": 0.006337092130565,
"sd": 0.02056039285836 
},
{
 "name": "simplex",
"mean": 0.005650922420783,
"sd": 0.01112060777754 
},
{
 "name": "simplex",
"mean": 0.005698248051302,
"sd": 0.01356017092424 
},
{
 "name": "simplex",
"mean": 0.005801030642046,
"sd": 0.01329327293103 
},
{
 "name": "simplex",
"mean": 0.005537099154042,
"sd": 0.01123860919742 
},
{
 "name": "simplex",
"mean": 0.00601542907564,
"sd": 0.01458757564631 
},
{
 "name": "simplex",
"mean": 0.006053283611444,
"sd": 0.01248507511833 
},
{
 "name": "simplex",
"mean": 0.006030503353112,
"sd": 0.01504000248423 
},
{
 "name": "simplex",
"mean": 0.005849646714331,
"sd": 0.01126598862606 
},
{
 "name": "simplex",
"mean": 0.005824488715917,
"sd": 0.01320273433962 
},
{
 "name": "simplex",
"mean": 0.005702790380443,
"sd": 0.01430926163697 
},
{
 "name": "simplex",
"mean": 0.006272131201964,
"sd": 0.01831030676796 
},
{
 "name": "simplex",
"mean": 0.005372424964505,
"sd": 0.01048560686499 
},
{
 "name": "simplex",
"mean": 0.005865591607295,
"sd": 0.01374478453087 
},
{
 "name": "simplex",
"mean": 0.006356575638912,
"sd": 0.0159497363573 
},
{
 "name": "simplex",
"mean": 0.005461471631069,
"sd": 0.01509518482952 
},
{
 "name": "simplex",
"mean": 0.00591233146612,
"sd": 0.01390091221612 
},
{
 "name": "simplex",
"mean": 0.005500193756627,
"sd": 0.01257864135289 
},
{
 "name": "simplex",
"mean": 0.005954188811655,
"sd": 0.01168171614307 
},
{
 "name": "simplex",
"mean": 0.005587633418312,
"sd": 0.01422998803384 
},
{
 "name": "simplex",
"mean": 0.006096097096327,
"sd": 0.01504224645095 
},
{
 "name": "simplex",
"mean": 0.00567317906982,
"sd": 0.01125826162549 
},
{
 "name": "simplex",
"mean": 0.005602621728902,
"sd": 0.01273328124981 
},
{
 "name": "simplex",
"mean": 0.00607800877198,
"sd": 0.01593338723416 
},
{
 "name": "simplex",
"mean": 0.005325139915586,
"sd": 0.01021879270771 
},
{
 "name": "simplex",
"mean": 0.006729876818641,
"sd": 0.02727464464253 
},
{
 "name": "simplex",
"mean": 0.005792534241904,
"sd": 0.01196762302525 
},
{
 "name": "simplex",
"mean": 0.005595327177968,
"sd": 0.01333559180387 
},
{
 "name": "simplex",
"mean": 0.006124732291395,
"sd": 0.01326762166459 
},
{
 "name": "simplex",
"mean": 0.006060136467468,
"sd": 0.01410655853149 
},
{
 "name": "simplex",
"mean": 0.006452397344564,
"sd": 0.01821600643891 
},
{
 "name": "simplex",
"mean": 0.005985277048356,
"sd": 0.01379222710488 
},
{
 "name": "simplex",
"mean": 0.006211925683167,
"sd": 0.0149236961017 
},
{
 "name": "simplex",
"mean": 0.005080409867388,
"sd": 0.01185804736584 
},
{
 "name": "simplex",
"mean": 0.005981110085555,
"sd": 0.01331978420234 
},
{
 "name": "simplex",
"mean": 0.005453666436001,
"sd": 0.01215483809461 
},
{
 "name": "simplex",
"mean": 0.005894312549894,
"sd": 0.01240543385257 
},
{
 "name": "simplex",
"mean": 0.00595089902673,
"sd": 0.01209940406681 
},
{
 "name": "simplex",
"mean": 0.006023815673635,
"sd": 0.01532285189439 
},
{
 "name": "simplex",
"mean": 0.00534289552584,
"sd": 0.01197067816137 
},
{
 "name": "simplex",
"mean": 0.006158691684179,
"sd": 0.01459849469136 
},
{
 "name": "simplex",
"mean": 0.005795115530215,
"sd": 0.0120916047662 
},
{
 "name": "simplex",
"mean": 0.006258529990261,
"sd": 0.01747696964999 
},
{
 "name": "simplex",
"mean": 0.005527590294956,
"sd": 0.01581945869753 
},
{
 "name": "simplex",
"mean": 0.00592628919737,
"sd": 0.01502716033536 
},
{
 "name": "simplex",
"mean": 0.005682036221044,
"sd": 0.01269727977691 
},
{
 "name": "simplex",
"mean": 0.006582606306121,
"sd": 0.02280782579464 
},
{
 "name": "simplex",
"mean": 0.006108200961283,
"sd": 0.01810510096143 
},
{
 "name": "simplex",
"mean": 0.005890934591546,
"sd": 0.0114729037839 
},
{
 "name": "simplex",
"mean": 0.005764755768708,
"sd": 0.01169873421791 
},
{
 "name": "simplex",
"mean": 0.006306419419172,
"sd": 0.01692469846677 
},
{
 "name": "simplex",
"mean": 0.006793362736538,
"sd": 0.02467929798793 
},
{
 "name": "simplex",
"mean": 0.005243659422345,
"sd": 0.01884953576251 
},
{
 "name": "simplex",
"mean": 0.005994009590904,
"sd": 0.01238012084805 
},
{
 "name": "simplex",
"mean": 0.005152957213032,
"sd": 0.008949495041561 
},
{
 "name": "simplex",
"mean": 0.007108315181262,
"sd": 0.03155888076636 
},
{
 "name": "simplex",
"mean": 0.006839582685766,
"sd": 0.02612489952155 
},
{
 "name": "simplex",
"mean": 0.006317988395909,
"sd": 0.01376104131788 
},
{
 "name": "simplex",
"mean": 0.005912027910154,
"sd": 0.01497072502204 
},
{
 "name": "simplex",
"mean": 0.005597702055014,
"sd": 0.01343104969286 
},
{
 "name": "simplex",
"mean": 0.005612303387651,
"sd": 0.01618647952292 
},
{
 "name": "simplex",
"mean": 0.005966330382266,
"sd": 0.01089933742622 
},
{
 "name": "simplex",
"mean": 0.005779531432214,
"sd": 0.01069746372908 
},
{
 "name": "simplex",
"mean": 0.005485760427212,
"sd": 0.009718713166551 
},
{
 "name": "simplex",
"mean": 0.0053091353312,
"sd": 0.01078117670853 
},
{
 "name": "simplex",
"mean": 0.006835080734072,
"sd": 0.02380281091162 
},
{
 "name": "simplex",
"mean": 0.006722249248497,
"sd": 0.02594816197488 
},
{
 "name": "simplex",
"mean": 0.006653837244268,
"sd": 0.01886716714608 
},
{
 "name": "simplex",
"mean": 0.006111375356113,
"sd": 0.01611862476895 
},
{
 "name": "simplex",
"mean": 0.006288769611226,
"sd": 0.02097656624576 
},
{
 "name": "simplex",
"mean": 0.005886163200739,
"sd": 0.01174317286651 
},
{
 "name": "simplex",
"mean": 0.004922466250158,
"sd": 0.01218456099892 
},
{
 "name": "simplex",
"mean": 0.005864895779519,
"sd": 0.01322291879712 
},
{
 "name": "simplex",
"mean": 0.005838129889806,
"sd": 0.01397968603061 
},
{
 "name": "simplex",
"mean": 0.00671170657557,
"sd": 0.01921406988766 
},
{
 "name": "simplex",
"mean": 0.005413805368025,
"sd": 0.009195610223228 
},
{
 "name": "simplex",
"mean": 0.005028166620949,
"sd": 0.02052831637243 
},
{
 "name": "simplex",
"mean": 0.005606521653928,
"sd": 0.01547826388498 
},
{
 "name": "simplex",
"mean": 0.005992417104174,
"sd": 0.0124117755402 
},
{
 "name": "simplex",
"mean": 0.006237994538878,
"sd": 0.01607455660162 
},
{
 "name": "simplex",
"mean": 0.006412209575748,
"sd": 0.01480128494033 
},
{
 "name": "simplex",
"mean": 0.006519110849004,
"sd": 0.02258716096655 
},
{
 "name": "simplex",
"mean": 0.005941157934553,
"sd": 0.01810886779411 
},
{
 "name": "simplex",
"mean": 0.005047713726589,
"sd": 0.01173973353648 
},
{
 "name": "simplex",
"mean": 0.006038861906599,
"sd": 0.01191397755691 
},
{
 "name": "simplex",
"mean": 0.006099888003356,
"sd": 0.01402697022442 
},
{
 "name": "simplex",
"mean": 0.005504121906928,
"sd": 0.0106313068629 
},
{
 "name": "simplex",
"mean": 0.005441142631711,
"sd": 0.01053993973883 
},
{
 "name": "simplex",
"mean": 0.005404065352991,
"sd": 0.008569646752029 
},
{
 "name": "simplex",
"mean": 0.005005768623066,
"sd": 0.0121219339048 
},
{
 "name": "simplex",
"mean": 0.004941076501564,
"sd":  0.01176641675 
},
{
 "name": "simplex",
"mean": 0.00580984263082,
"sd": 0.01554620113036 
},
{
 "name": "simplex",
"mean": 0.005754762087284,
"sd": 0.01393764041027 
},
{
 "name": "simplex",
"mean": 0.005655034991863,
"sd": 0.01049110246072 
},
{
 "name": "simplex",
"mean": 0.006116298643314,
"sd": 0.01455592736972 
},
{
 "name": "simplex",
"mean": 0.006022409572777,
"sd": 0.01471073784999 
},
{
 "name": "simplex",
"mean": 0.005746300493454,
"sd": 0.01476112809178 
},
{
 "name": "simplex",
"mean": 0.006898125000862,
"sd": 0.02783818014078 
},
{
 "name": "simplex",
"mean": 0.005412213636193,
"sd": 0.01356443683766 
},
{
 "name": "simplex",
"mean": 0.0055898128432,
"sd": 0.01095318957541 
},
{
 "name": "simplex",
"mean": 0.005511184569804,
"sd": 0.01141779658693 
},
{
 "name": "simplex",
"mean": 0.006435282993255,
"sd": 0.01627751127994 
},
{
 "name": "simplex",
"mean": 0.005279071862986,
"sd": 0.01764830757409 
},
{
 "name": "simplex",
"mean": 0.005856921940718,
"sd": 0.01297489683757 
},
{
 "name": "simplex",
"mean": 0.004987679695435,
"sd": 0.01389742894979 
},
{
 "name": "simplex",
"mean": 0.005394387459402,
"sd": 0.01254542592005 
},
{
 "name": "simplex",
"mean": 0.005006849584515,
"sd": 0.01233236919946 
},
{
 "name": "simplex",
"mean": 0.005340011317914,
"sd": 0.01158290293563 
},
{
 "name": "simplex",
"mean": 0.005708268834688,
"sd": 0.01098596854627 
},
{
 "name": "simplex",
"mean": 0.00627131738443,
"sd": 0.01839879197698 
},
{
 "name": "simplex",
"mean": 0.007017196456266,
"sd": 0.02745275174254 
},
{
 "name": "simplex",
"mean": 0.005064519122989,
"sd": 0.01057912250406 
},
{
 "name": "simplex",
"mean": 0.005175321315807,
"sd": 0.02112961686614 
},
{
 "name": "simplex",
"mean": 0.005222090445992,
"sd": 0.01082081350016 
},
{
 "name": "simplex",
"mean": 0.005212657644959,
"sd": 0.009517285057666 
},
{
 "name": "simplex",
"mean": 0.006717705525137,
"sd": 0.02096960025221 
},
{
 "name": "simplex",
"mean": 0.007106314637545,
"sd": 0.03063050130348 
},
{
 "name": "simplex",
"mean": 0.005433332431578,
"sd": 0.01264659381772 
},
{
 "name": "simplex",
"mean": 0.006102630970997,
"sd": 0.01448920576203 
},
{
 "name": "simplex",
"mean": 0.004948148439744,
"sd": 0.02330442124885 
},
{
 "name": "simplex",
"mean": 0.00573348330404,
"sd": 0.0112200677351 
},
{
 "name": "simplex",
"mean": 0.005857226386247,
"sd": 0.01552478357031 
},
{
 "name": "simplex",
"mean": 0.006472751988055,
"sd": 0.01771747730002 
},
{
 "name": "simplex",
"mean": 0.006192201754709,
"sd": 0.01396384381236 
},
{
 "name": "simplex",
"mean": 0.005430178425509,
"sd": 0.0116713540871 
},
{
 "name": "simplex",
"mean": 0.005351110386825,
"sd": 0.01031100682509 
},
{
 "name": "simplex",
"mean": 0.006210315642072,
"sd": 0.01307902827913 
},
{
 "name": "simplex",
"mean": 0.006462904732216,
"sd": 0.01748697762809 
},
{
 "name": "simplex",
"mean": 0.00503686696268,
"sd": 0.0124019624564 
},
{
 "name": "simplex",
"mean": 0.005801753395768,
"sd": 0.01481901075471 
},
{
 "name": "simplex",
"mean": 0.006709670475728,
"sd": 0.02066496300679 
},
{
 "name": "simplex",
"mean": 0.005636867454846,
"sd": 0.01111387261144 
},
{
 "name": "simplex",
"mean": 0.006358566302166,
"sd": 0.01616054819178 
},
{
 "name": "simplex",
"mean": 0.005921713614369,
"sd": 0.01692596371608 
},
{
 "name": "simplex",
"mean": 0.006493824569729,
"sd": 0.01791700909688 
},
{
 "name": "simplex",
"mean": 0.005309985009836,
"sd": 0.01676770205778 
},
{
 "name": "simplex",
"mean": 0.005501739336116,
"sd": 0.01817073474047 
},
{
 "name": "simplex",
"mean": 0.005857427973862,
"sd": 0.01191196349142 
},
{
 "name": "simplex",
"mean": 0.006163136975844,
"sd": 0.0189066520683 
},
{
 "name": "simplex",
"mean": 0.006311880734627,
"sd": 0.01404969252709 
},
{
 "name": "simplex",
"mean": 0.005889929405276,
"sd": 0.01153933415975 
},
{
 "name": "simplex",
"mean": 0.005971808002071,
"sd": 0.01502598253327 
},
{
 "name": "simplex",
"mean": 0.005622916602441,
"sd": 0.01235022976214 
},
{
 "name": "simplex",
"mean": 0.00553271858719,
"sd": 0.01733741859825 
},
{
 "name": "simplex",
"mean": 0.005370850942234,
"sd": 0.01053528207527 
},
{
 "name": "simplex",
"mean": 0.00579270107809,
"sd": 0.0111926205228 
},
{
 "name": "simplex",
"mean": 0.006405412164988,
"sd": 0.01762554108927 
},
{
 "name": "simplex",
"mean": 0.007096264638321,
"sd": 0.03231377328422 
},
{
 "name": "simplex",
"mean": 0.005883680088714,
"sd": 0.01694126019402 
},
{
 "name": "simplex",
"mean": 0.005675953272644,
"sd": 0.009523769336982 
},
{
 "name": "simplex",
"mean": 0.005273336836233,
"sd": 0.01122908704042 
},
{
 "name": "simplex",
"mean": 0.00634692529638,
"sd": 0.02261445700449 
},
{
 "name": "simplex",
"mean": 0.005065647745942,
"sd": 0.01700376969896 
},
{
 "name": "simplex",
"mean": 0.006467035317928,
"sd": 0.01540623193964 
},
{
 "name": "simplex",
"mean": 0.005790294775917,
"sd": 0.01073897004437 
},
{
 "name": "simplex",
"mean": 0.005553320522005,
"sd": 0.008971795705452 
},
{
 "name": "simplex",
"mean": 0.005695152891665,
"sd": 0.01246344309584 
},
{
 "name": "simplex",
"mean": 0.005503445333895,
"sd": 0.008990921017568 
},
{
 "name": "simplex",
"mean": 0.005867791550935,
"sd": 0.01130431024151 
},
{
 "name": "simplex",
"mean": 0.005611596432732,
"sd": 0.01309046884786 
},
{
 "name": "simplex",
"mean": 0.00598455130756,
"sd": 0.01354940678681 
},
{
 "name": "simplex",
"mean": 0.005289435115227,
"sd": 0.01649292217354 
},
{
 "name": "simplex",
"mean": 0.005685663560152,
"sd": 0.009792158350451 
},
{
 "name": "simplex",
"mean": 0.005083817161308,
"sd": 0.01201793000559 
},
{
 "name": "simplex",
"mean": 0.005546215520167,
"sd": 0.008901619793823 
},
{
 "name": "simplex",
"mean": 0.006855094413652,
"sd": 0.02435381400731 
},
{
 "name": "simplex",
"mean": 0.006336252909666,
"sd": 0.01631383670876 
},
{
 "name": "simplex",
"mean": 0.006335164223915,
"sd": 0.01751419481945 
},
{
 "name": "simplex",
"mean": 0.006079679138722,
"sd": 0.01193022304622 
},
{
 "name": "simplex",
"mean": 0.006837274189431,
"sd": 0.02430373134524 
},
{
 "name": "simplex",
"mean": 0.006937128797127,
"sd": 0.02857402375369 
},
{
 "name": "simplex",
"mean": 0.005427723916767,
"sd": 0.01103181846612 
},
{
 "name": "simplex",
"mean": 0.005391184512565,
"sd": 0.0123124570816 
},
{
 "name": "simplex",
"mean": 0.005967202639069,
"sd": 0.01259166791219 
},
{
 "name": "simplex",
"mean": 0.005143494535006,
"sd": 0.01467523953344 
},
{
 "name": "simplex",
"mean": 0.005066287058236,
"sd": 0.01545503162756 
},
{
 "name": "simplex",
"mean": 0.005072014126759,
"sd": 0.0115231527561 
},
{
 "name": "simplex",
"mean": 0.006393145349243,
"sd": 0.01665623973928 
},
{
 "name": "simplex",
"mean": 0.005816683893542,
"sd": 0.01256709776392 
},
{
 "name": "simplex",
"mean": 0.004928806784055,
"sd": 0.011557196129 
},
{
 "name": "simplex",
"mean": 0.006313581486198,
"sd": 0.02108477954441 
},
{
 "name": "simplex",
"mean": 0.005398160134197,
"sd": 0.01220728079025 
},
{
 "name": "simplex",
"mean": 0.005729784381696,
"sd": 0.01326312445619 
},
{
 "name": "simplex",
"mean": 0.005270170147466,
"sd": 0.01713758716657 
},
{
 "name": "simplex",
"mean": 0.005899712200668,
"sd": 0.0122896627281 
},
{
 "name": "simplex",
"mean": 0.005101832169884,
"sd": 0.01661841409092 
},
{
 "name": "simplex",
"mean": 0.006648493796035,
"sd": 0.02406967343562 
},
{
 "name": "simplex",
"mean": 0.00536574545166,
"sd": 0.008553535201415 
},
{
 "name": "simplex",
"mean": 0.005284372274728,
"sd": 0.01969671728176 
},
{
 "name": "simplex",
"mean": 0.005363772934129,
"sd": 0.008928557083246 
},
{
 "name": "simplex",
"mean": 0.006311714074572,
"sd": 0.02218959247642 
},
{
 "name": "simplex",
"mean": 0.005223186560443,
"sd": 0.0110431081065 
},
{
 "name": "simplex",
"mean": 0.005130143618478,
"sd": 0.02195007922072 
},
{
 "name": "simplex",
"mean": 0.006078789680682,
"sd": 0.01289550328274 
},
{
 "name": "simplex",
"mean": 0.005384701397362,
"sd": 0.009689976637264 
},
{
 "name": "simplex",
"mean": 0.005978093700696,
"sd": 0.01454086337884 
},
{
 "name": "simplex",
"mean": 0.004944307634142,
"sd": 0.01194335203172 
},
{
 "name": "simplex",
"mean": 0.006751434866672,
"sd": 0.02267793058987 
},
{
 "name": "simplex",
"mean": 0.005493176196586,
"sd": 0.009261341950008 
},
{
 "name": "simplex",
"mean": 0.00597907809584,
"sd": 0.01339789905678 
},
{
 "name": "simplex",
"mean": 0.005997289260832,
"sd": 0.01155429859918 
},
{
 "name": "simplex",
"mean": 0.005626968672543,
"sd": 0.01185422344012 
},
{
 "name": "simplex",
"mean": 0.005246291122661,
"sd": 0.01562818644776 
},
{
 "name": "simplex",
"mean": 0.005243124626301,
"sd": 0.01911026968147 
},
{
 "name": "simplex",
"mean": 0.005817394489267,
"sd": 0.01049659119624 
},
{
 "name": "simplex",
"mean": 0.006573338732009,
"sd": 0.01882537290752 
},
{
 "name": "simplex",
"mean": 0.006328132595686,
"sd": 0.0147321935675 
},
{
 "name": "simplex",
"mean": 0.006357418147044,
"sd": 0.01671781420938 
},
{
 "name": "simplex",
"mean": 0.006532291421726,
"sd": 0.01744473335967 
},
{
 "name": "simplex",
"mean": 0.005255654232196,
"sd": 0.009940214831343 
},
{
 "name": "simplex",
"mean": 0.006136800449811,
"sd": 0.01586712709574 
},
{
 "name": "simplex",
"mean": 0.005393469222237,
"sd": 0.01332570868076 
},
{
 "name": "simplex",
"mean": 0.006400867896901,
"sd": 0.02353092311033 
},
{
 "name": "simplex",
"mean": 0.006264646700332,
"sd": 0.01628596654623 
},
{
 "name": "simplex",
"mean": 0.006088168806016,
"sd": 0.01577363377432 
},
{
 "name": "simplex",
"mean": 0.005283274428365,
"sd": 0.008648001730754 
},
{
 "name": "simplex",
"mean": 0.005151775903138,
"sd": 0.00896541752667 
},
{
 "name": "simplex",
"mean": 0.006250619053981,
"sd": 0.01544653596154 
},
{
 "name": "simplex",
"mean": 0.006437314042355,
"sd": 0.01722720575621 
},
{
 "name": "simplex",
"mean": 0.005393020314236,
"sd": 0.01451366259429 
},
{
 "name": "simplex",
"mean": 0.006184125456073,
"sd": 0.01454905274852 
},
{
 "name": "simplex",
"mean": 0.005375644160188,
"sd": 0.01270832146465 
},
{
 "name": "simplex",
"mean": 0.005304893124387,
"sd": 0.008736209924443 
},
{
 "name": "simplex",
"mean": 0.005244586703309,
"sd": 0.01874772800365 
},
{
 "name": "simplex",
"mean": 0.00625874246795,
"sd": 0.02054344711658 
},
{
 "name": "simplex",
"mean": 0.005529621713441,
"sd": 0.01289515142768 
},
{
 "name": "simplex",
"mean": 0.006112490474932,
"sd": 0.01464539958354 
},
{
 "name": "simplex",
"mean": 0.005438138195775,
"sd": 0.01579000272174 
},
{
 "name": "simplex",
"mean": 0.00569549015304,
"sd": 0.01116546251861 
},
{
 "name": "simplex",
"mean": 0.005360364019278,
"sd": 0.01210779049847 
},
{
 "name": "simplex",
"mean": 0.005896723576943,
"sd": 0.01482252519396 
},
{
 "name": "simplex",
"mean": 0.005010171815444,
"sd": 0.0227268229229 
},
{
 "name": "simplex",
"mean": 0.005626505218394,
"sd": 0.01721192734747 
},
{
 "name": "simplex",
"mean": 0.005895325132344,
"sd": 0.01580880762328 
},
{
 "name": "simplex",
"mean": 0.005759236849346,
"sd": 0.01308733741822 
},
{
 "name": "simplex",
"mean": 0.006296448423227,
"sd": 0.01348318488436 
},
{
 "name": "simplex",
"mean": 0.006963148886538,
"sd": 0.03057013092205 
},
{
 "name": "simplex",
"mean": 0.006024516952737,
"sd": 0.0187183987738 
},
{
 "name": "simplex",
"mean": 0.005946146698024,
"sd": 0.01881753800647 
},
{
 "name": "simplex",
"mean": 0.005359368640417,
"sd": 0.0112238651824 
},
{
 "name": "simplex",
"mean": 0.006960018501487,
"sd": 0.02658496331547 
},
{
 "name": "simplex",
"mean": 0.006405948134304,
"sd": 0.02298673672362 
},
{
 "name": "simplex",
"mean": 0.006897322426603,
"sd": 0.02691560795613 
},
{
 "name": "simplex",
"mean": 0.006249917314526,
"sd": 0.01640519813566 
},
{
 "name": "simplex",
"mean": 0.006366130288589,
"sd": 0.0155148546158 
},
{
 "name": "simplex",
"mean": 0.005693186190238,
"sd": 0.01708568513673 
},
{
 "name": "simplex",
"mean": 0.00622112162202,
"sd": 0.01447042578613 
},
{
 "name": "simplex",
"mean": 0.00574868073104,
"sd": 0.01779187597836 
},
{
 "name": "simplex",
"mean": 0.006717772160559,
"sd": 0.02448716436268 
},
{
 "name": "simplex",
"mean": 0.005959696420487,
"sd": 0.01287255153726 
},
{
 "name": "simplex",
"mean": 0.006372605090204,
"sd": 0.0155590504528 
},
{
 "name": "simplex",
"mean": 0.005263451587991,
"sd": 0.01027764697381 
},
{
 "name": "simplex",
"mean": 0.006213796158146,
"sd": 0.01455036386115 
},
{
 "name": "simplex",
"mean": 0.00520360020422,
"sd": 0.01698695198512 
},
{
 "name": "simplex",
"mean": 0.005798829606706,
"sd": 0.01112931244979 
},
{
 "name": "simplex",
"mean": 0.006668554491614,
"sd": 0.01908441759758 
},
{
 "name": "simplex",
"mean": 0.006052427340487,
"sd": 0.01398262707804 
},
{
 "name": "simplex",
"mean": 0.005957593989743,
"sd": 0.01538456967974 
},
{
 "name": "simplex",
"mean": 0.005771083832135,
"sd": 0.01446268996378 
},
{
 "name": "simplex",
"mean": 0.00515765905803,
"sd": 0.01227282465468 
},
{
 "name": "simplex",
"mean": 0.006608386083308,
"sd": 0.02118812587936 
},
{
 "name": "simplex",
"mean": 0.006673332070028,
"sd": 0.022330081348 
},
{
 "name": "simplex",
"mean": 0.005524361723742,
"sd": 0.0134926983797 
},
{
 "name": "simplex",
"mean": 0.005698520973606,
"sd": 0.01657238384802 
},
{
 "name": "simplex",
"mean": 0.005067651028942,
"sd": 0.01262408239765 
},
{
 "name": "simplex",
"mean": 0.006537280735711,
"sd": 0.01647772933067 
},
{
 "name": "simplex",
"mean": 0.00661730225507,
"sd": 0.01913137748322 
},
{
 "name": "simplex",
"mean": 0.006382722921969,
"sd": 0.01455893087369 
},
{
 "name": "simplex",
"mean": 0.005880859597922,
"sd": 0.01062067082973 
},
{
 "name": "simplex",
"mean": 0.006558914219119,
"sd": 0.02436829910605 
},
{
 "name": "simplex",
"mean": 0.005845008917526,
"sd": 0.01443076774805 
},
{
 "name": "simplex",
"mean": 0.006423828067335,
"sd": 0.01484652519529 
},
{
 "name": "simplex",
"mean": 0.00503372280364,
"sd": 0.01550771632812 
},
{
 "name": "simplex",
"mean": 0.006415106705381,
"sd": 0.01748454883129 
},
{
 "name": "simplex",
"mean": 0.006135136872892,
"sd": 0.01547054658682 
},
{
 "name": "simplex",
"mean": 0.006903324997333,
"sd": 0.02463853872248 
},
{
 "name": "simplex",
"mean": 0.00628412966605,
"sd": 0.01475188799289 
},
{
 "name": "simplex",
"mean": 0.005046901469301,
"sd": 0.01156718075026 
},
{
 "name": "simplex",
"mean": 0.005053944945614,
"sd": 0.01508473008625 
},
{
 "name": "simplex",
"mean": 0.006418486814767,
"sd": 0.01501122785272 
},
{
 "name": "simplex",
"mean": 0.005037900856324,
"sd": 0.009571448743538 
},
{
 "name": "simplex",
"mean": 0.005332255918239,
"sd": 0.008550814916121 
},
{
 "name": "simplex",
"mean": 0.00497193442524,
"sd": 0.02306672949533 
},
{
 "name": "simplex",
"mean": 0.005905111997566,
"sd": 0.0150597160365 
},
{
 "name": "simplex",
"mean": 0.006511234386015,
"sd": 0.01774477897274 
},
{
 "name": "simplex",
"mean": 0.00632346130545,
"sd": 0.01857739670784 
},
{
 "name": "simplex",
"mean": 0.007207676427016,
"sd": 0.033795250679 
},
{
 "name": "simplex",
"mean": 0.005136376485257,
"sd": 0.01930255107613 
},
{
 "name": "simplex",
"mean": 0.006796580742984,
"sd": 0.02555657617595 
},
{
 "name": "simplex",
"mean": 0.005521478496611,
"sd": 0.01004755045874 
},
{
 "name": "simplex",
"mean": 0.005267297573344,
"sd": 0.01129117826623 
},
{
 "name": "simplex",
"mean": 0.006198009026605,
"sd": 0.01974213121521 
},
{
 "name": "simplex",
"mean": 0.005793877698797,
"sd": 0.01181313102206 
},
{
 "name": "simplex",
"mean": 0.00503037738368,
"sd": 0.02224929951867 
},
{
 "name": "simplex",
"mean": 0.005653865587901,
"sd": 0.01244516115121 
},
{
 "name": "simplex",
"mean": 0.00648254349018,
"sd": 0.01550453700628 
},
{
 "name": "simplex",
"mean": 0.005978019625835,
"sd": 0.01259134503194 
},
{
 "name": "simplex",
"mean": 0.005734079125084,
"sd": 0.01586179398225 
},
{
 "name": "simplex",
"mean": 0.005449143960288,
"sd": 0.01296000665255 
},
{
 "name": "simplex",
"mean": 0.005085220324709,
"sd": 0.02231238555302 
},
{
 "name": "simplex",
"mean": 0.00610139675318,
"sd": 0.01490775516565 
},
{
 "name": "simplex",
"mean": 0.005197901154317,
"sd": 0.02002937992996 
},
{
 "name": "simplex",
"mean": 0.006053584430883,
"sd": 0.01593441466046 
},
{
 "name": "simplex",
"mean": 0.005274813275026,
"sd": 0.00945277346966 
},
{
 "name": "simplex",
"mean": 0.007121015223796,
"sd": 0.03039717642074 
},
{
 "name": "simplex",
"mean": 0.005898952376424,
"sd": 0.01335008043834 
},
{
 "name": "simplex",
"mean": 0.005422527314069,
"sd": 0.008847995592669 
},
{
 "name": "simplex",
"mean": 0.005520051031395,
"sd": 0.009073451144917 
},
{
 "name": "simplex",
"mean": 0.005890494033846,
"sd": 0.01231325106574 
},
{
 "name": "simplex",
"mean": 0.006354482132124,
"sd": 0.01495000291319 
},
{
 "name": "simplex",
"mean": 0.005988418158989,
"sd": 0.01432913507279 
},
{
 "name": "simplex",
"mean": 0.005084711180728,
"sd": 0.01152866443654 
},
{
 "name": "simplex",
"mean": 0.006573456237179,
"sd": 0.0183647539964 
},
{
 "name": "simplex",
"mean": 0.006501754885147,
"sd": 0.01608771833471 
},
{
 "name": "simplex",
"mean": 0.005437464823381,
"sd": 0.008647616246007 
},
{
 "name": "simplex",
"mean": 0.006419209854087,
"sd": 0.01554815383236 
},
{
 "name": "simplex",
"mean": 0.006040750448894,
"sd": 0.01925353098228 
},
{
 "name": "simplex",
"mean": 0.007188311478045,
"sd": 0.03373026359453 
},
{
 "name": "simplex",
"mean": 0.006247013572133,
"sd": 0.01562010174054 
},
{
 "name": "simplex",
"mean": 0.00627803923478,
"sd": 0.01401128903985 
},
{
 "name": "simplex",
"mean": 0.00539346086948,
"sd": 0.01916821108331 
},
{
 "name": "simplex",
"mean": 0.005518072489191,
"sd": 0.01207492694141 
},
{
 "name": "simplex",
"mean": 0.005451811973249,
"sd": 0.013351086027 
},
{
 "name": "simplex",
"mean": 0.006536870477904,
"sd": 0.01854782985818 
},
{
 "name": "simplex",
"mean": 0.006172460053057,
"sd": 0.01401512176653 
},
{
 "name": "simplex",
"mean": 0.00650871659653,
"sd": 0.01826415602944 
},
{
 "name": "simplex",
"mean": 0.005739688694525,
"sd": 0.01265745722005 
},
{
 "name": "simplex",
"mean": 0.005715150931654,
"sd": 0.01227734977782 
},
{
 "name": "simplex",
"mean": 0.00581148000135,
"sd": 0.01528443452838 
},
{
 "name": "simplex",
"mean": 0.00588741368627,
"sd": 0.01194076233725 
},
{
 "name": "simplex",
"mean": 0.005145490183363,
"sd": 0.01517978627405 
},
{
 "name": "simplex",
"mean": 0.005679777517571,
"sd": 0.01010752704439 
},
{
 "name": "simplex",
"mean": 0.006502902745978,
"sd": 0.0181691977248 
},
{
 "name": "simplex",
"mean": 0.006382904751403,
"sd": 0.01533236770233 
},
{
 "name": "simplex",
"mean": 0.006673243013819,
"sd": 0.02596798025412 
},
{
 "name": "simplex",
"mean": 0.005512523743468,
"sd": 0.009721686524541 
},
{
 "name": "simplex",
"mean": 0.005537693596993,
"sd": 0.012734848919 
},
{
 "name": "simplex",
"mean": 0.005903843113232,
"sd": 0.01477225945604 
},
{
 "name": "simplex",
"mean": 0.005326844764263,
"sd": 0.009466882643846 
},
{
 "name": "simplex",
"mean": 0.006202461774219,
"sd": 0.01644630271962 
},
{
 "name": "simplex",
"mean": 0.006267636664448,
"sd": 0.0137539051822 
},
{
 "name": "simplex",
"mean": 0.006212309360208,
"sd": 0.01628009259209 
},
{
 "name": "simplex",
"mean": 0.005563869110616,
"sd": 0.009269205023969 
},
{
 "name": "simplex",
"mean": 0.00589701532929,
"sd": 0.01413328985151 
},
{
 "name": "simplex",
"mean": 0.00530791618046,
"sd": 0.01356557651046 
},
{
 "name": "simplex",
"mean": 0.006597534121706,
"sd": 0.02264807544715 
},
{
 "name": "simplex",
"mean": 0.005233591184123,
"sd": 0.009983490010513 
},
{
 "name": "simplex",
"mean": 0.005868360211217,
"sd": 0.0145500014393 
},
{
 "name": "simplex",
"mean": 0.006494454845967,
"sd": 0.01614920382323 
},
{
 "name": "simplex",
"mean": 0.005080055264743,
"sd": 0.02077727222372 
},
{
 "name": "simplex",
"mean": 0.005923469675161,
"sd": 0.01372362457668 
},
{
 "name": "simplex",
"mean": 0.005111034616087,
"sd": 0.01239622447676 
},
{
 "name": "simplex",
"mean": 0.005925834582069,
"sd": 0.01072874741876 
},
{
 "name": "simplex",
"mean": 0.005187686779603,
"sd": 0.01354409614346 
},
{
 "name": "simplex",
"mean": 0.006351556402158,
"sd": 0.0173064299779 
},
{
 "name": "simplex",
"mean": 0.005467542596856,
"sd": 0.009224377260136 
},
{
 "name": "simplex",
"mean": 0.00538117607812,
"sd": 0.01255816496585 
},
{
 "name": "simplex",
"mean": 0.006123896560915,
"sd": 0.01705125631433 
},
{
 "name": "simplex",
"mean": 0.005073908963236,
"sd": 0.01023776945271 
},
{
 "name": "simplex",
"mean": 0.007169990051095,
"sd": 0.03369759995361 
},
{
 "name": "simplex",
"mean": 0.005538547100349,
"sd": 0.009721385355872 
},
{
 "name": "simplex",
"mean": 0.005506695551412,
"sd": 0.01633627241653 
},
{
 "name": "simplex",
"mean": 0.006375291495338,
"sd": 0.01482582412105 
},
{
 "name": "simplex",
"mean": 0.006325905895317,
"sd": 0.01688215525753 
},
{
 "name": "simplex",
"mean": 0.006627070226248,
"sd": 0.01928624059247 
},
{
 "name": "simplex",
"mean": 0.006196320580081,
"sd": 0.01452195706627 
},
{
 "name": "simplex",
"mean": 0.006314586880552,
"sd": 0.01558402913686 
},
{
 "name": "simplex",
"mean": 0.004915521174983,
"sd": 0.0125287424038 
},
{
 "name": "simplex",
"mean": 0.006132112713644,
"sd": 0.01408426329201 
},
{
 "name": "simplex",
"mean": 0.005278567035069,
"sd": 0.01451500413094 
},
{
 "name": "simplex",
"mean": 0.00603372917598,
"sd": 0.01272078987945 
},
{
 "name": "simplex",
"mean": 0.005993412051518,
"sd": 0.0117542682817 
},
{
 "name": "simplex",
"mean": 0.005904697542197,
"sd": 0.01609620839873 
},
{
 "name": "simplex",
"mean": 0.005080283389134,
"sd": 0.01339293624797 
},
{
 "name": "simplex",
"mean": 0.006157177704424,
"sd": 0.01423810152727 
},
{
 "name": "simplex",
"mean": 0.005793248093691,
"sd": 0.01300048661726 
},
{
 "name": "simplex",
"mean": 0.006441121072584,
"sd": 0.01895731095618 
},
{
 "name": "simplex",
"mean": 0.005470570560722,
"sd": 0.01831809460218 
},
{
 "name": "simplex",
"mean": 0.005713175568913,
"sd": 0.01541454156489 
},
{
 "name": "simplex",
"mean": 0.005589985338159,
"sd": 0.01289964283763 
},
{
 "name": "simplex",
"mean": 0.007097884115851,
"sd": 0.03133884814125 
},
{
 "name": "simplex",
"mean": 0.00641545148369,
"sd": 0.02313623645079 
},
{
 "name": "simplex",
"mean": 0.005892580780838,
"sd": 0.01077551894287 
},
{
 "name": "simplex",
"mean": 0.005689323771245,
"sd": 0.01029391729136 
},
{
 "name": "simplex",
"mean": 0.006464252355417,
"sd": 0.01761824950593 
},
{
 "name": "simplex",
"mean": 0.007033756662742,
"sd": 0.02820049796484 
},
{
 "name": "simplex",
"mean": 0.005006399966434,
"sd": 0.02292938137083 
},
{
 "name": "simplex",
"mean": 0.006297105522949,
"sd": 0.01432415322591 
},
{
 "name": "simplex",
"mean": 0.005147963286766,
"sd": 0.008700583267663 
},
{
 "name": "simplex",
"mean": 0.007302396848605,
"sd": 0.03520787727041 
},
{
 "name": "simplex",
"mean": 0.007219196555525,
"sd": 0.03354564994925 
},
{
 "name": "simplex",
"mean": 0.006447547003693,
"sd": 0.01409011761039 
},
{
 "name": "simplex",
"mean": 0.006105801067119,
"sd": 0.01632361645049 
},
{
 "name": "simplex",
"mean": 0.005346205996423,
"sd": 0.01327862044869 
},
{
 "name": "simplex",
"mean": 0.005212699999173,
"sd": 0.02072000459916 
},
{
 "name": "simplex",
"mean": 0.00590433882069,
"sd": 0.01031061618253 
},
{
 "name": "simplex",
"mean": 0.005608175767676,
"sd": 0.009472300593102 
},
{
 "name": "simplex",
"mean": 0.005355147760677,
"sd": 0.008629261030346 
},
{
 "name": "simplex",
"mean": 0.005021423195243,
"sd": 0.01078345238372 
},
{
 "name": "simplex",
"mean": 0.007030520870483,
"sd": 0.02749776707461 
},
{
 "name": "simplex",
"mean": 0.007179715924364,
"sd": 0.03334052040768 
},
{
 "name": "simplex",
"mean": 0.006586107134503,
"sd": 0.01707620157234 
},
{
 "name": "simplex",
"mean": 0.006134848907935,
"sd": 0.0171179653956 
},
{
 "name": "simplex",
"mean": 0.006612395288243,
"sd": 0.0257354564246 
},
{
 "name": "simplex",
"mean": 0.006169934333735,
"sd": 0.01352762367243 
},
{
 "name": "simplex",
"mean": 0.004889899003555,
"sd": 0.01274356688604 
},
{
 "name": "simplex",
"mean": 0.006065636990647,
"sd": 0.01450045684915 
},
{
 "name": "simplex",
"mean": 0.005754763243408,
"sd": 0.01340983606967 
},
{
 "name": "simplex",
"mean": 0.006719695737968,
"sd": 0.0189397320448 
},
{
 "name": "simplex",
"mean": 0.005327240218751,
"sd": 0.008454082269443 
},
{
 "name": "simplex",
"mean": 0.00492690887403,
"sd": 0.02372742143132 
},
{
 "name": "simplex",
"mean": 0.005423058000103,
"sd": 0.01854259162215 
},
{
 "name": "simplex",
"mean": 0.006172166700031,
"sd": 0.0141408294242 
},
{
 "name": "simplex",
"mean": 0.006338304444011,
"sd": 0.01600960538559 
},
{
 "name": "simplex",
"mean": 0.006489220686089,
"sd": 0.01573840076646 
},
{
 "name": "simplex",
"mean": 0.006745612967138,
"sd": 0.02659601188096 
},
{
 "name": "simplex",
"mean": 0.006214086992261,
"sd": 0.02175529005735 
},
{
 "name": "simplex",
"mean": 0.004903240984964,
"sd": 0.01220212230671 
},
{
 "name": "simplex",
"mean": 0.006291290558159,
"sd": 0.01364301192088 
},
{
 "name": "simplex",
"mean": 0.006335460183566,
"sd": 0.0148668453419 
},
{
 "name": "simplex",
"mean": 0.005428593183071,
"sd": 0.009049334491126 
},
{
 "name": "simplex",
"mean": 0.005343795463012,
"sd": 0.008781319947762 
},
{
 "name": "simplex",
"mean": 0.005344589230689,
"sd": 0.008523349849227 
},
{
 "name": "simplex",
"mean": 0.004904940294234,
"sd": 0.01150570052331 
},
{
 "name": "simplex",
"mean": 0.004888944283082,
"sd": 0.01262280326513 
},
{
 "name": "simplex",
"mean": 0.005575995728681,
"sd": 0.01662973445266 
},
{
 "name": "simplex",
"mean": 0.005946516022063,
"sd": 0.01506794883778 
},
{
 "name": "simplex",
"mean": 0.005407175564451,
"sd": 0.008787979839047 
},
{
 "name": "simplex",
"mean": 0.006291460342087,
"sd": 0.01595798537657 
},
{
 "name": "simplex",
"mean": 0.006162803167856,
"sd": 0.01517307888508 
},
{
 "name": "simplex",
"mean": 0.005690202912429,
"sd": 0.01506523015116 
},
{
 "name": "simplex",
"mean": 0.007228624731997,
"sd": 0.03361963822187 
},
{
 "name": "simplex",
"mean": 0.00500172507356,
"sd": 0.01276514688717 
},
{
 "name": "simplex",
"mean": 0.005400530587452,
"sd": 0.008970393727844 
},
{
 "name": "simplex",
"mean": 0.005467818505258,
"sd": 0.01224810783647 
},
{
 "name": "simplex",
"mean": 0.006490048342838,
"sd": 0.01591048244535 
},
{
 "name": "simplex",
"mean": 0.004999381344113,
"sd": 0.02241281734929 
},
{
 "name": "simplex",
"mean": 0.005745662104116,
"sd": 0.01228807529243 
},
{
 "name": "simplex",
"mean": 0.004911835506819,
"sd": 0.01571491432672 
},
{
 "name": "simplex",
"mean": 0.005223933125859,
"sd": 0.01412716808404 
},
{
 "name": "simplex",
"mean": 0.00490917718029,
"sd": 0.01217183816741 
},
{
 "name": "simplex",
"mean": 0.00520980416329,
"sd": 0.01136049073434 
},
{
 "name": "simplex",
"mean": 0.005409988576724,
"sd": 0.008891680357513 
},
{
 "name": "simplex",
"mean": 0.006584837497962,
"sd": 0.0210233467593 
},
{
 "name": "simplex",
"mean": 0.007179369288204,
"sd": 0.03153297752373 
},
{
 "name": "simplex",
"mean": 0.004926899773734,
"sd": 0.01193683674792 
},
{
 "name": "simplex",
"mean": 0.00495616517916,
"sd": 0.02376269694494 
},
{
 "name": "simplex",
"mean": 0.004966674904472,
"sd": 0.01128040864592 
},
{
 "name": "simplex",
"mean": 0.005071101178238,
"sd": 0.009981378390562 
},
{
 "name": "simplex",
"mean": 0.006872674728944,
"sd": 0.02362429751698 
},
{
 "name": "simplex",
"mean": 0.007295354433711,
"sd": 0.0349810279605 
},
{
 "name": "simplex",
"mean": 0.005195445702928,
"sd": 0.01302012633789 
},
{
 "name": "simplex",
"mean": 0.006361302385337,
"sd": 0.01681769418515 
},
{
 "name": "simplex",
"mean": 0.00491703151978,
"sd": 0.02415274757914 
},
{
 "name": "simplex",
"mean": 0.005572472347103,
"sd": 0.009305853809953 
},
{
 "name": "simplex",
"mean": 0.005819988559555,
"sd": 0.01446089595516 
},
{
 "name": "simplex",
"mean": 0.006487292890584,
"sd": 0.01821168810589 
},
{
 "name": "simplex",
"mean": 0.006406718431629,
"sd": 0.01704753696986 
},
{
 "name": "simplex",
"mean": 0.005328292020796,
"sd": 0.01147940523587 
},
{
 "name": "simplex",
"mean": 0.005221579487477,
"sd": 0.01058092873736 
},
{
 "name": "simplex",
"mean": 0.006402483885868,
"sd": 0.0144214997874 
},
{
 "name": "simplex",
"mean": 0.006487902163257,
"sd": 0.01819121553777 
},
{
 "name": "simplex",
"mean": 0.004964916767811,
"sd": 0.0144502262427 
},
{
 "name": "simplex",
"mean": 0.00557633993142,
"sd": 0.01410138813229 
},
{
 "name": "simplex",
"mean": 0.006793367214123,
"sd": 0.02188560320745 
},
{
 "name": "simplex",
"mean": 0.005698988279007,
"sd": 0.01137513343912 
},
{
 "name": "simplex",
"mean": 0.006497018345743,
"sd": 0.01773231949382 
},
{
 "name": "simplex",
"mean": 0.005815539199544,
"sd": 0.0175705387009 
},
{
 "name": "simplex",
"mean": 0.006590173620969,
"sd": 0.01878529337844 
},
{
 "name": "simplex",
"mean": 0.005082455708487,
"sd": 0.02142995023634 
},
{
 "name": "simplex",
"mean": 0.005175666764282,
"sd": 0.02150469044489 
},
{
 "name": "simplex",
"mean": 0.005547236829435,
"sd": 0.009517279330433 
},
{
 "name": "simplex",
"mean": 0.006268626043794,
"sd": 0.02045419608759 
},
{
 "name": "simplex",
"mean": 0.006459637685764,
"sd": 0.01548332036694 
},
{
 "name": "simplex",
"mean": 0.005865152000848,
"sd": 0.01132670685251 
},
{
 "name": "simplex",
"mean": 0.005977469658878,
"sd": 0.01517051420842 
},
{
 "name": "simplex",
"mean": 0.005709584863478,
"sd": 0.01228632208005 
},
{
 "name": "simplex",
"mean": 0.005270407986557,
"sd": 0.02067589018094 
},
{
 "name": "simplex",
"mean": 0.005198942389595,
"sd": 0.009516414351498 
},
{
 "name": "simplex",
"mean": 0.006002806129273,
"sd": 0.0122300691741 
},
{
 "name": "simplex",
"mean": 0.006484548811333,
"sd": 0.01820346959869 
},
{
 "name": "simplex",
"mean": 0.007294807381293,
"sd": 0.0352264684528 
},
{
 "name": "simplex",
"mean": 0.005809193953337,
"sd": 0.01901961662195 
},
{
 "name": "simplex",
"mean": 0.005564921266903,
"sd": 0.00898685867351 
},
{
 "name": "simplex",
"mean": 0.004961038407628,
"sd": 0.0121861694862 
},
{
 "name": "simplex",
"mean": 0.006684200095023,
"sd": 0.02704120534014 
},
{
 "name": "simplex",
"mean": 0.004935776170763,
"sd": 0.02237167717077 
},
{
 "name": "simplex",
"mean": 0.006509229665442,
"sd": 0.0151152693817 
},
{
 "name": "simplex",
"mean": 0.005966736224836,
"sd": 0.01151947769852 
},
{
 "name": "simplex",
"mean": 0.005374420864879,
"sd": 0.008581463826417 
},
{
 "name": "simplex",
"mean": 0.005804809397276,
"sd": 0.01241767277765 
},
{
 "name": "simplex",
"mean": 0.005356149357517,
"sd": 0.008561575013712 
},
{
 "name": "simplex",
"mean": 0.005873907427198,
"sd": 0.01083040844005 
},
{
 "name": "simplex",
"mean": 0.005397069226296,
"sd": 0.01193544161015 
},
{
 "name": "simplex",
"mean": 0.006336662648267,
"sd": 0.01685680918879 
},
{
 "name": "simplex",
"mean": 0.005020464217076,
"sd": 0.02162040609866 
},
{
 "name": "simplex",
"mean": 0.005445597843451,
"sd": 0.008656435286722 
},
{
 "name": "simplex",
"mean": 0.004918627273097,
"sd": 0.01267047393396 
},
{
 "name": "simplex",
"mean": 0.005371795376279,
"sd": 0.008568266379015 
},
{
 "name": "simplex",
"mean": 0.007030993656627,
"sd": 0.02756257334095 
},
{
 "name": "simplex",
"mean": 0.006468476033098,
"sd": 0.01798695412665 
},
{
 "name": "simplex",
"mean": 0.006474025603104,
"sd": 0.01904799202927 
},
{
 "name": "simplex",
"mean": 0.00625784093148,
"sd": 0.01270924152618 
},
{
 "name": "simplex",
"mean": 0.006999568177005,
"sd": 0.02672904178654 
},
{
 "name": "simplex",
"mean": 0.007273323388387,
"sd": 0.03468092076219 
},
{
 "name": "simplex",
"mean": 0.005213693736986,
"sd": 0.00936672756444 
},
{
 "name": "simplex",
"mean": 0.005037259966995,
"sd": 0.01232754073264 
},
{
 "name": "simplex",
"mean": 0.006276449446583,
"sd": 0.01437106640785 
},
{
 "name": "simplex",
"mean": 0.004971035387785,
"sd": 0.01941966355957 
},
{
 "name": "simplex",
"mean": 0.004924364197541,
"sd": 0.01859441632864 
},
{
 "name": "simplex",
"mean": 0.00490759971813,
"sd": 0.01249406832689 
},
{
 "name": "simplex",
"mean": 0.006464936755271,
"sd": 0.0176851619088 
},
{
 "name": "simplex",
"mean": 0.005989474705822,
"sd": 0.0131425493733 
},
{
 "name": "simplex",
"mean": 0.004892330468577,
"sd": 0.01156026915167 
},
{
 "name": "simplex",
"mean": 0.006524329002045,
"sd": 0.02482273331982 
},
{
 "name": "simplex",
"mean": 0.005103083591814,
"sd": 0.01167929131186 
},
{
 "name": "simplex",
"mean": 0.005630555972062,
"sd": 0.01158098263791 
},
{
 "name": "simplex",
"mean": 0.004984962244723,
"sd": 0.02230738693029 
},
{
 "name": "simplex",
"mean": 0.006093001709777,
"sd": 0.01286347528904 
},
{
 "name": "simplex",
"mean": 0.004991202896309,
"sd": 0.02026921839356 
},
{
 "name": "simplex",
"mean": 0.006996079751147,
"sd": 0.02965971339521 
},
{
 "name": "simplex",
"mean": 0.005337491138382,
"sd": 0.008546429763032 
},
{
 "name": "simplex",
"mean": 0.005048065726354,
"sd": 0.02279353759282 
},
{
 "name": "simplex",
"mean": 0.005329424516185,
"sd": 0.008436959338898 
},
{
 "name": "simplex",
"mean": 0.006810122355599,
"sd": 0.02890899851188 
},
{
 "name": "simplex",
"mean": 0.005142657228144,
"sd": 0.01098188815316 
},
{
 "name": "simplex",
"mean": 0.004935720232382,
"sd": 0.0239848809645 
},
{
 "name": "simplex",
"mean": 0.006341148014189,
"sd": 0.01575446359438 
},
{
 "name": "simplex",
"mean": 0.005280617175597,
"sd": 0.008429881419461 
},
{
 "name": "simplex",
"mean": 0.006287427461215,
"sd": 0.01694107873249 
},
{
 "name": "simplex",
"mean": 0.004890733009562,
"sd": 0.0127304072527 
},
{
 "name": "simplex",
"mean": 0.007035951080458,
"sd": 0.02784102513779 
},
{
 "name": "simplex",
"mean": 0.005350743244289,
"sd": 0.008584754542992 
},
{
 "name": "simplex",
"mean": 0.006287638205191,
"sd": 0.01613658993384 
},
{
 "name": "simplex",
"mean": 0.005773953103013,
"sd": 0.009791936661626 
},
{
 "name": "simplex",
"mean": 0.005407731573215,
"sd": 0.009604554592215 
},
{
 "name": "simplex",
"mean": 0.00506340978283,
"sd": 0.0194632822024 
},
{
 "name": "simplex",
"mean": 0.004978187497078,
"sd": 0.02323772709019 
},
{
 "name": "simplex",
"mean": 0.005859021891561,
"sd": 0.01053961435902 
},
{
 "name": "simplex",
"mean": 0.006577434498358,
"sd": 0.01871931241298 
},
{
 "name": "simplex",
"mean": 0.006478187387325,
"sd": 0.01569869480184 
},
{
 "name": "simplex",
"mean": 0.006475929676445,
"sd": 0.01807806394683 
},
{
 "name": "simplex",
"mean": 0.006547134144963,
"sd": 0.01659171327281 
},
{
 "name": "simplex",
"mean": 0.005093155898927,
"sd": 0.009149707755498 
},
{
 "name": "simplex",
"mean": 0.00634638149461,
"sd": 0.01565586769839 
},
{
 "name": "simplex",
"mean": 0.005067157163974,
"sd": 0.01297860920808 
},
{
 "name": "simplex",
"mean": 0.006755143448207,
"sd": 0.0287418795945 
},
{
 "name": "simplex",
"mean": 0.006462754777307,
"sd": 0.01804909958686 
},
{
 "name": "simplex",
"mean": 0.00633305486723,
"sd": 0.01563877968021 
},
{
 "name": "simplex",
"mean": 0.005268220185807,
"sd": 0.008247783134069 
},
{
 "name": "simplex",
"mean": 0.005117265007035,
"sd": 0.008650190269541 
},
{
 "name": "simplex",
"mean": 0.00641623784903,
"sd": 0.01582785291024 
},
{
 "name": "simplex",
"mean": 0.00648992925357,
"sd": 0.01806744961337 
},
{
 "name": "simplex",
"mean": 0.004993433841196,
"sd": 0.01307697831969 
},
{
 "name": "simplex",
"mean": 0.006337811423299,
"sd": 0.01643752312428 
},
{
 "name": "simplex",
"mean": 0.005084527876627,
"sd": 0.01202409004539 
},
{
 "name": "simplex",
"mean": 0.005324831986759,
"sd": 0.008458254722738 
},
{
 "name": "simplex",
"mean": 0.00498927076462,
"sd": 0.0230814272385 
},
{
 "name": "simplex",
"mean": 0.006413077124843,
"sd": 0.02366141243339 
},
{
 "name": "simplex",
"mean": 0.005575556888342,
"sd": 0.01315472720183 
},
{
 "name": "simplex",
"mean": 0.006246232723438,
"sd": 0.01537894788306 
},
{
 "name": "simplex",
"mean": 0.005166176304008,
"sd": 0.02034338754924 
},
{
 "name": "simplex",
"mean": 0.005461277299563,
"sd": 0.00903121129416 
},
{
 "name": "simplex",
"mean": 0.005113205352427,
"sd": 0.01339960346982 
},
{
 "name": "simplex",
"mean": 0.005738836979012,
"sd": 0.01504973111609 
},
{
 "name": "simplex",
"mean": 0.004922720241717,
"sd": 0.02407552284719 
},
{
 "name": "simplex",
"mean": 0.005235850698321,
"sd": 0.02088129954883 
},
{
 "name": "simplex",
"mean": 0.006015596114665,
"sd": 0.01711736815233 
},
{
 "name": "simplex",
"mean": 0.005950434563741,
"sd": 0.01354109647654 
},
{
 "name": "simplex",
"mean": 0.006427183385106,
"sd": 0.01521375669652 
},
{
 "name": "simplex",
"mean": 0.007279040354538,
"sd": 0.03505505260615 
},
{
 "name": "simplex",
"mean": 0.005999511690778,
"sd": 0.02052795171768 
},
{
 "name": "simplex",
"mean": 0.006181500815001,
"sd": 0.02155308412018 
},
{
 "name": "simplex",
"mean": 0.005196967202355,
"sd": 0.01140496629008 
},
{
 "name": "simplex",
"mean": 0.007205383716135,
"sd": 0.03232827031905 
},
{
 "name": "simplex",
"mean": 0.006648270602183,
"sd": 0.02649927956381 
},
{
 "name": "simplex",
"mean": 0.007217175945483,
"sd": 0.03309863465742 
},
{
 "name": "simplex",
"mean": 0.006278527572562,
"sd": 0.0164788126232 
},
{
 "name": "simplex",
"mean": 0.006482494699979,
"sd": 0.01578992785247 
},
{
 "name": "simplex",
"mean": 0.005681493609887,
"sd": 0.01772679377112 
},
{
 "name": "simplex",
"mean": 0.00645465863484,
"sd": 0.01559735908103 
},
{
 "name": "simplex",
"mean": 0.005542936068781,
"sd": 0.01975054329848 
},
{
 "name": "simplex",
"mean": 0.007127592750879,
"sd": 0.03121637755535 
},
{
 "name": "simplex",
"mean": 0.005947681719725,
"sd": 0.01261479174467 
},
{
 "name": "simplex",
"mean": 0.006480389158645,
"sd": 0.01577713951745 
},
{
 "name": "simplex",
"mean": 0.005260192438899,
"sd": 0.00901998577642 
},
{
 "name": "simplex",
"mean": 0.00632688976173,
"sd": 0.01621746863561 
},
{
 "name": "simplex",
"mean": 0.00497007594518,
"sd": 0.02249407632876 
},
{
 "name": "simplex",
"mean": 0.0060421280178,
"sd": 0.01254177982877 
},
{
 "name": "simplex",
"mean": 0.006593266956848,
"sd": 0.01718334428922 
},
{
 "name": "simplex",
"mean": 0.006153296276866,
"sd": 0.01371035488979 
},
{
 "name": "simplex",
"mean": 0.005798582114041,
"sd": 0.01305355169362 
},
{
 "name": "simplex",
"mean": 0.005541183014977,
"sd": 0.01407398056196 
},
{
 "name": "simplex",
"mean": 0.004918772446837,
"sd": 0.01251043740335 
},
{
 "name": "simplex",
"mean": 0.006753660991147,
"sd": 0.02202619442484 
},
{
 "name": "simplex",
"mean": 0.00693551039003,
"sd": 0.02596176832559 
},
{
 "name": "simplex",
"mean": 0.005096125843656,
"sd": 0.01257699048809 
},
{
 "name": "simplex",
"mean": 0.005266138642529,
"sd": 0.02015439944209 
},
{
 "name": "simplex",
"mean": 0.004897244503023,
"sd": 0.01278155531258 
},
{
 "name": "simplex",
"mean": 0.006516041176025,
"sd": 0.01669791027369 
},
{
 "name": "simplex",
"mean": 0.006565427831549,
"sd": 0.0186329541062 
},
{
 "name": "simplex",
"mean": 0.006471239743016,
"sd": 0.01473707363592 
},
{
 "name": "simplex",
"mean": 0.005764542397684,
"sd": 0.01017671155891 
},
{
 "name": "simplex",
"mean": 0.007082427888435,
"sd": 0.03208500388242 
},
{
 "name": "simplex",
"mean": 0.006006147580415,
"sd": 0.01475204897829 
},
{
 "name": "simplex",
"mean": 0.006483822449933,
"sd": 0.01498376304021 
},
{
 "name": "simplex",
"mean": 0.00493512331013,
"sd": 0.02042760617377 
},
{
 "name": "simplex",
"mean": 0.006603607790515,
"sd": 0.01848010452893 
},
{
 "name": "simplex",
"mean": 0.006327756826596,
"sd": 0.01686396862053 
},
{
 "name": "simplex",
"mean": 0.007001437117559,
"sd": 0.02653591803306 
},
{
 "name": "simplex",
"mean": 0.006425662713502,
"sd": 0.01653086687076 
},
{
 "name": "simplex",
"mean": 0.004906545090376,
"sd": 0.01263556428062 
},
{
 "name": "simplex",
"mean": 0.004932466608796,
"sd": 0.01958867500905 
},
{
 "name": "simplex",
"mean": 0.006488339493058,
"sd": 0.01554038860807 
},
{
 "name": "simplex",
"mean": 0.00497271079121,
"sd": 0.01056279220428 
},
{
 "name": "simplex",
"mean": 0.005332658402146,
"sd": 0.00851624384588 
},
{
 "name": "simplex",
"mean": 0.004917181374861,
"sd": 0.02415648423186 
},
{
 "name": "simplex",
"mean": 0.005961317488598,
"sd": 0.0158357774555 
},
{
 "name": "simplex",
"mean": 0.006516220681015,
"sd": 0.0182291877433 
},
{
 "name": "simplex",
"mean": 0.006703601890216,
"sd": 0.02356823269765 
},
{
 "name": "simplex",
"mean": 0.00731131576578,
"sd": 0.03546236090157 
},
{
 "name": "simplex",
"mean": 0.004957723530978,
"sd": 0.02338104023261 
},
{
 "name": "simplex",
"mean": 0.007165490520785,
"sd": 0.03181423577396 
},
{
 "name": "simplex",
"mean": 0.005355009719605,
"sd": 0.008590988820801 
},
{
 "name": "simplex",
"mean": 0.005011902537889,
"sd": 0.01158339434221 
},
{
 "name": "simplex",
"mean": 0.006586268247932,
"sd": 0.02537200097184 
},
{
 "name": "simplex",
"mean": 0.005905903525556,
"sd": 0.01212536893847 
},
{
 "name": "simplex",
"mean": 0.004924804353869,
"sd": 0.0240605046051 
},
{
 "name": "simplex",
"mean": 0.00562324321751,
"sd": 0.013792863945 
},
{
 "name": "simplex",
"mean": 0.006491363620847,
"sd": 0.01659354637168 
},
{
 "name": "simplex",
"mean": 0.006207161018405,
"sd": 0.01491785185326 
},
{
 "name": "simplex",
"mean": 0.005485671168599,
"sd": 0.01846935008299 
},
{
 "name": "simplex",
"mean": 0.005141717898075,
"sd": 0.01199322463689 
},
{
 "name": "simplex",
"mean": 0.004926837658805,
"sd": 0.02407103812546 
},
{
 "name": "simplex",
"mean": 0.006313627624244,
"sd": 0.01632444528909 
},
{
 "name": "simplex",
"mean": 0.004973739266946,
"sd": 0.02328907947099 
},
{
 "name": "simplex",
"mean": 0.006039713271404,
"sd": 0.01608127281503 
},
{
 "name": "simplex",
"mean": 0.005241130656442,
"sd": 0.008396103704294 
},
{
 "name": "simplex",
"mean": 0.007288795971482,
"sd": 0.03474475390267 
},
{
 "name": "simplex",
"mean": 0.005913197148524,
"sd": 0.01422232041983 
},
{
 "name": "simplex",
"mean": 0.005342142339806,
"sd": 0.008520878000803 
},
{
 "name": "simplex",
"mean": 0.005350119351589,
"sd": 0.008565833669403 
},
{
 "name": "simplex",
"mean": 0.005551061828677,
"sd": 0.009525910925155 
},
{
 "name": "simplex",
"mean": 0.006478785024064,
"sd": 0.01571294271768 
},
{
 "name": "simplex",
"mean": 0.00622081353916,
"sd": 0.01563714240764 
},
{
 "name": "simplex",
"mean": 0.004910978490216,
"sd": 0.01232585516108 
},
{
 "name": "simplex",
"mean": 0.006669512051142,
"sd": 0.01924162246624 
},
{
 "name": "simplex",
"mean": 0.006496328406335,
"sd": 0.01583924246062 
},
{
 "name": "simplex",
"mean": 0.00534970025858,
"sd": 0.008552624940678 
},
{
 "name": "simplex",
"mean": 0.006490336475577,
"sd": 0.01579559102911 
},
{
 "name": "simplex",
"mean": 0.006220586131176,
"sd": 0.02175659378272 
},
{
 "name": "simplex",
"mean": 0.007310460989965,
"sd": 0.03545961201298 
},
{
 "name": "simplex",
"mean": 0.006400768026457,
"sd": 0.01574312105787 
},
{
 "name": "simplex",
"mean": 0.006459279341789,
"sd": 0.01549241530752 
},
{
 "name": "simplex",
"mean": 0.005012512254571,
"sd": 0.02319312121996 
},
{
 "name": "simplex",
"mean": 0.005429572720512,
"sd": 0.01278571713092 
},
{
 "name": "simplex",
"mean": 0.005241283694556,
"sd": 0.0141282725387 
},
{
 "name": "simplex",
"mean": 0.006532435426129,
"sd": 0.01839830137486 
},
{
 "name": "simplex",
"mean": 0.006452683281213,
"sd": 0.01557265752156 
},
{
 "name": "simplex",
"mean": 0.006490206432641,
"sd": 0.0182279657866 
},
{
 "name": "simplex",
"mean": 0.005511722641168,
"sd": 0.009881492826079 
},
{
 "name": "simplex",
"mean": 0.005544341133241,
"sd": 0.01028121646355 
},
{
 "name": "simplex",
"mean": 0.00561236790111,
"sd": 0.01564444449057 
},
{
 "name": "simplex",
"mean": 0.006178342862281,
"sd": 0.01373248217083 
},
{
 "name": "simplex",
"mean": 0.004972193946616,
"sd": 0.02086042690529 
},
{
 "name": "simplex",
"mean": 0.005486381053088,
"sd": 0.008802360112989 
},
{
 "name": "simplex",
"mean": 0.006487968164635,
"sd": 0.01822104449266 
},
{
 "name": "simplex",
"mean": 0.006487249354079,
"sd": 0.01577909954038 
},
{
 "name": "simplex",
"mean": 0.007074856210232,
"sd": 0.03210362386258 
},
{
 "name": "simplex",
"mean": 0.005409841532406,
"sd": 0.008643971955531 
},
{
 "name": "simplex",
"mean": 0.005324786762178,
"sd": 0.01103532236342 
},
{
 "name": "simplex",
"mean": 0.006149273087575,
"sd": 0.01556972329321 
},
{
 "name": "simplex",
"mean": 0.00523075232637,
"sd": 0.008335683579573 
},
{
 "name": "simplex",
"mean": 0.006416826170835,
"sd": 0.01784540327194 
},
{
 "name": "simplex",
"mean": 0.006443872305542,
"sd": 0.01533672543989 
},
{
 "name": "simplex",
"mean": 0.006471091529152,
"sd": 0.01740712798621 
},
{
 "name": "simplex",
"mean": 0.005372679835044,
"sd": 0.008590097067276 
},
{
 "name": "simplex",
"mean": 0.006160768423008,
"sd": 0.0153098918942 
},
{
 "name": "simplex",
"mean": 0.004952724791887,
"sd": 0.01289088315474 
},
{
 "name": "simplex",
"mean": 0.006974376132537,
"sd": 0.02842635074469 
},
{
 "name": "simplex",
"mean": 0.005151932321747,
"sd": 0.00945713739782 
},
{
 "name": "simplex",
"mean": 0.005967993979445,
"sd": 0.01566327701241 
},
{
 "name": "simplex",
"mean": 0.006510995026431,
"sd": 0.01594620025005 
},
{
 "name": "simplex",
"mean": 0.004923368458536,
"sd": 0.02399510282054 
},
{
 "name": "simplex",
"mean": 0.006105110348897,
"sd": 0.01476850086846 
},
{
 "name": "simplex",
"mean": 0.004903932398324,
"sd": 0.01273659496381 
},
{
 "name": "simplex",
"mean": 0.005814523447854,
"sd": 0.009861505548818 
},
{
 "name": "simplex",
"mean": 0.004926665360363,
"sd": 0.01288504637967 
},
{
 "name": "simplex",
"mean": 0.006476575776598,
"sd": 0.01815977337432 
},
{
 "name": "simplex",
"mean": 0.005346829046465,
"sd": 0.008570152646335 
},
{
 "name": "simplex",
"mean": 0.005172483522989,
"sd": 0.01220445858886 
},
{
 "name": "simplex",
"mean": 0.006253658402946,
"sd": 0.0191368912977 
},
{
 "name": "simplex",
"mean": 0.004965615268715,
"sd": 0.01128745538929 
},
{
 "name": "simplex",
"mean": 0.007306600255025,
"sd": 0.03541801288405 
},
{
 "name": "simplex",
"mean": 0.005356763480282,
"sd": 0.008645681010627 
},
{
 "name": "simplex",
"mean": 0.005323040704656,
"sd": 0.01991964676867 
},
{
 "name": "simplex",
"mean": 0.006484725780551,
"sd": 0.01572866132988 
},
{
 "name": "simplex",
"mean": 0.006473832220654,
"sd": 0.01813451433588 
},
{
 "name": "simplex",
"mean": 0.006652649366789,
"sd": 0.01842112267449 
},
{
 "name": "simplex",
"mean": 0.006442063141976,
"sd": 0.01556172583457 
},
{
 "name": "simplex",
"mean": 0.006444142384428,
"sd": 0.01758306024281 
},
{
 "name": "simplex",
"mean": 0.004888306995514,
"sd": 0.01277923866857 
},
{
 "name": "simplex",
"mean": 0.006384575423661,
"sd": 0.01672313708313 
},
{
 "name": "simplex",
"mean": 0.005069245206572,
"sd": 0.01944189797226 
},
{
 "name": "simplex",
"mean": 0.00622255015078,
"sd": 0.01348710781256 
},
{
 "name": "simplex",
"mean": 0.006114289504156,
"sd": 0.01245836730762 
},
{
 "name": "simplex",
"mean": 0.005640110188538,
"sd": 0.01793313053125 
},
{
 "name": "simplex",
"mean": 0.004955409043387,
"sd": 0.01712295844042 
},
{
 "name": "simplex",
"mean": 0.006135911134196,
"sd": 0.01402089600339 
},
{
 "name": "simplex",
"mean": 0.00586586964918,
"sd": 0.01460391751204 
},
{
 "name": "simplex",
"mean": 0.006563760300135,
"sd": 0.01908692161272 
},
{
 "name": "simplex",
"mean": 0.005309354499051,
"sd": 0.02043956946443 
},
{
 "name": "simplex",
"mean": 0.005507216362813,
"sd": 0.01651733406906 
},
{
 "name": "simplex",
"mean": 0.005601075400092,
"sd": 0.01362355028244 
},
{
 "name": "simplex",
"mean": 0.007302274104761,
"sd": 0.03522614477947 
},
{
 "name": "simplex",
"mean": 0.006858676325006,
"sd": 0.0297508930491 
},
{
 "name": "simplex",
"mean": 0.005850621638129,
"sd": 0.01027931946744 
},
{
 "name": "simplex",
"mean": 0.005544071117796,
"sd": 0.009123126365127 
},
{
 "name": "simplex",
"mean": 0.006493046055144,
"sd": 0.01816326230973 
},
{
 "name": "simplex",
"mean": 0.007193325818836,
"sd": 0.03195487545751 
},
{
 "name": "simplex",
"mean": 0.004922184589488,
"sd": 0.02410962813279 
},
{
 "name": "simplex",
"mean": 0.006474076127342,
"sd": 0.01565489679325 
},
{
 "name": "simplex",
"mean": 0.005180294078444,
"sd": 0.008452235702786 
},
{
 "name": "simplex",
"mean": 0.007315025178725,
"sd": 0.03551655951467 
},
{
 "name": "simplex",
"mean": 0.007312245228937,
"sd": 0.03546223613883 
},
{
 "name": "simplex",
"mean": 0.006487538061854,
"sd": 0.01428164411445 
},
{
 "name": "simplex",
"mean": 0.006345962542429,
"sd": 0.01750191549776 
},
{
 "name": "simplex",
"mean": 0.005078319798384,
"sd": 0.01300039926955 
},
{
 "name": "simplex",
"mean": 0.00494902456479,
"sd": 0.02383750821813 
},
{
 "name": "simplex",
"mean": 0.005744624291169,
"sd": 0.009611921626856 
},
{
 "name": "simplex",
"mean": 0.005433159667685,
"sd": 0.008707227277669 
},
{
 "name": "simplex",
"mean": 0.00533727770991,
"sd": 0.008549438780926 
},
{
 "name": "simplex",
"mean": 0.004902633598691,
"sd": 0.0123278748634 
},
{
 "name": "simplex",
"mean": 0.00715843367021,
"sd": 0.03093410475517 
},
{
 "name": "simplex",
"mean": 0.007309578703533,
"sd": 0.03543225444742 
},
{
 "name": "simplex",
"mean": 0.006506732152119,
"sd": 0.01595855366137 
},
{
 "name": "simplex",
"mean": 0.006071128130863,
"sd": 0.01663953707573 
},
{
 "name": "simplex",
"mean": 0.00699116131647,
"sd": 0.03097665359155 
},
{
 "name": "simplex",
"mean": 0.006430584272657,
"sd": 0.01535858454932 
},
{
 "name": "simplex",
"mean": 0.004887814634556,
"sd": 0.01278627322937 
},
{
 "name": "simplex",
"mean": 0.006338899121793,
"sd": 0.01547515032646 
},
{
 "name": "simplex",
"mean": 0.005733257432504,
"sd": 0.01311046273914 
},
{
 "name": "simplex",
"mean": 0.00665487507621,
"sd": 0.01777655215514 
},
{
 "name": "simplex",
"mean": 0.00533581393582,
"sd": 0.008539370503696 
},
{
 "name": "simplex",
"mean": 0.004916725782282,
"sd": 0.02416395590743 
},
{
 "name": "simplex",
"mean": 0.005177037405749,
"sd": 0.02160709135334 
},
{
 "name": "simplex",
"mean": 0.006394690399284,
"sd": 0.01705997156956 
},
{
 "name": "simplex",
"mean": 0.006431399945143,
"sd": 0.01569478384457 
},
{
 "name": "simplex",
"mean": 0.006492676358626,
"sd": 0.01580094509867 
},
{
 "name": "simplex",
"mean": 0.007040509939762,
"sd": 0.03124099998342 
},
{
 "name": "simplex",
"mean": 0.006593263029703,
"sd": 0.02613212679457 
},
{
 "name": "simplex",
"mean": 0.004887989932919,
"sd": 0.01274371244291 
},
{
 "name": "simplex",
"mean": 0.006457765476516,
"sd": 0.01541371969049 
},
{
 "name": "simplex",
"mean": 0.006478263727435,
"sd": 0.01569428810711 
},
{
 "name": "simplex",
"mean": 0.005356176108209,
"sd": 0.008497573629615 
},
{
 "name": "simplex",
"mean": 0.005332410835134,
"sd": 0.008499088836905 
},
{
 "name": "simplex",
"mean": 0.005337152460702,
"sd": 0.008548453703493 
},
{
 "name": "simplex",
"mean": 0.004891563883376,
"sd": 0.0116233655723 
},
{
 "name": "simplex",
"mean": 0.004887810563035,
"sd": 0.01278412955659 
},
{
 "name": "simplex",
"mean": 0.005227593052152,
"sd": 0.01973512639596 
},
{
 "name": "simplex",
"mean": 0.006243079002917,
"sd": 0.01552773302966 
},
{
 "name": "simplex",
"mean": 0.005339102859871,
"sd": 0.008553472851843 
},
{
 "name": "simplex",
"mean": 0.006453633908976,
"sd": 0.01781349653033 
},
{
 "name": "simplex",
"mean": 0.006372841252315,
"sd": 0.01694333693477 
},
{
 "name": "simplex",
"mean": 0.005666186534615,
"sd": 0.01519184272676 
},
{
 "name": "simplex",
"mean": 0.00731118234791,
"sd": 0.03541277322021 
},
{
 "name": "simplex",
"mean": 0.004891571861418,
"sd": 0.01278187816642 
},
{
 "name": "simplex",
"mean": 0.005339266546369,
"sd": 0.008555779892817 
},
{
 "name": "simplex",
"mean": 0.005335022895432,
"sd": 0.01281676648545 
},
{
 "name": "simplex",
"mean": 0.006492993605146,
"sd": 0.01580682787275 
},
{
 "name": "simplex",
"mean": 0.004920224275899,
"sd": 0.02408006019647 
},
{
 "name": "simplex",
"mean": 0.005743846680511,
"sd": 0.01226505492579 
},
{
 "name": "simplex",
"mean": 0.004908806781827,
"sd": 0.01798065360957 
},
{
 "name": "simplex",
"mean": 0.005083349456042,
"sd": 0.01672968638465 
},
{
 "name": "simplex",
"mean": 0.004895873657091,
"sd": 0.01150647231874 
},
{
 "name": "simplex",
"mean": 0.005007011230934,
"sd": 0.01217252553497 
},
{
 "name": "simplex",
"mean": 0.005339008740078,
"sd": 0.008557311223923 
},
{
 "name": "simplex",
"mean": 0.00682400889703,
"sd": 0.02295567545454 
},
{
 "name": "simplex",
"mean": 0.007284860744681,
"sd": 0.03461870249648 
},
{
 "name": "simplex",
"mean": 0.00489099086174,
"sd": 0.01271829426999 
},
{
 "name": "simplex",
"mean": 0.004917556886271,
"sd": 0.02416155425159 
},
{
 "name": "simplex",
"mean": 0.004891963208036,
"sd": 0.0125174306026 
},
{
 "name": "simplex",
"mean": 0.004982084567686,
"sd": 0.01098889273875 
},
{
 "name": "simplex",
"mean": 0.00695916675192,
"sd": 0.0254083379664 
},
{
 "name": "simplex",
"mean": 0.007314937761694,
"sd": 0.03551394224823 
},
{
 "name": "simplex",
"mean": 0.005142535191687,
"sd": 0.01294460361419 
},
{
 "name": "simplex",
"mean": 0.006476438908834,
"sd": 0.01809091246629 
},
{
 "name": "simplex",
"mean": 0.004916585613936,
"sd": 0.02417126947518 
},
{
 "name": "simplex",
"mean": 0.005412349351282,
"sd": 0.008612654522797 
},
{
 "name": "simplex",
"mean": 0.005690454941165,
"sd": 0.01207864092774 
},
{
 "name": "simplex",
"mean": 0.006487317123689,
"sd": 0.01821974892255 
},
{
 "name": "simplex",
"mean": 0.006482861727259,
"sd": 0.01816137071681 
},
{
 "name": "simplex",
"mean": 0.005122709312773,
"sd": 0.01201025160793 
},
{
 "name": "simplex",
"mean": 0.005034794332167,
"sd": 0.01128140480013 
},
{
 "name": "simplex",
"mean": 0.006487363229868,
"sd": 0.01562531036572 
},
{
 "name": "simplex",
"mean": 0.006487325954799,
"sd": 0.01821971461483 
},
{
 "name": "simplex",
"mean": 0.00492734680481,
"sd": 0.0180058921527 
},
{
 "name": "simplex",
"mean": 0.005273320470503,
"sd": 0.01289941696227 
},
{
 "name": "simplex",
"mean": 0.006750819933552,
"sd": 0.02110194333931 
},
{
 "name": "simplex",
"mean": 0.005776490223883,
"sd": 0.01137568363252 
},
{
 "name": "simplex",
"mean": 0.006491685736033,
"sd": 0.01821632089291 
},
{
 "name": "simplex",
"mean": 0.005540597063283,
"sd": 0.01631607052609 
},
{
 "name": "simplex",
"mean": 0.00653079050288,
"sd": 0.01839334921735 
},
{
 "name": "simplex",
"mean": 0.004937516166275,
"sd": 0.02384270824704 
},
{
 "name": "simplex",
"mean": 0.004950311695495,
"sd": 0.02383722244926 
},
{
 "name": "simplex",
"mean": 0.00535341758759,
"sd": 0.008604128154442 
},
{
 "name": "simplex",
"mean": 0.006374347342218,
"sd": 0.02225291141694 
},
{
 "name": "simplex",
"mean": 0.006491694011935,
"sd": 0.01579180922226 
},
{
 "name": "simplex",
"mean": 0.005865837000746,
"sd": 0.01142861386809 
},
{
 "name": "simplex",
"mean": 0.006097284426361,
"sd": 0.01608468972727 
},
{
 "name": "simplex",
"mean": 0.005864969179209,
"sd": 0.01265389781429 
},
{
 "name": "simplex",
"mean": 0.005032519516304,
"sd": 0.02303088636143 
},
{
 "name": "simplex",
"mean": 0.005194833101638,
"sd": 0.008551892984953 
},
{
 "name": "simplex",
"mean": 0.006320926054089,
"sd": 0.01445770697293 
},
{
 "name": "simplex",
"mean": 0.006487313135391,
"sd": 0.01821973089689 
},
{
 "name": "simplex",
"mean": 0.007314949282791,
"sd": 0.03551691370584 
},
{
 "name": "simplex",
"mean": 0.00554159491106,
"sd": 0.02032164243135 
},
{
 "name": "simplex",
"mean": 0.005409545941831,
"sd": 0.008612728638096 
},
{
 "name": "simplex",
"mean": 0.004889687358178,
"sd": 0.01276728401676 
},
{
 "name": "simplex",
"mean": 0.007060054815146,
"sd": 0.0319514294106 
},
{
 "name": "simplex",
"mean": 0.004916744181658,
"sd": 0.02408376591512 
},
{
 "name": "simplex",
"mean": 0.006493159354261,
"sd": 0.01524605526216 
},
{
 "name": "simplex",
"mean": 0.00615049653128,
"sd": 0.01276605889827 
},
{
 "name": "simplex",
"mean": 0.005338076323444,
"sd": 0.008549334623176 
},
{
 "name": "simplex",
"mean": 0.00599809817739,
"sd": 0.01265840090774 
},
{
 "name": "simplex",
"mean": 0.005337348617101,
"sd": 0.008548813132569 
},
{
 "name": "simplex",
"mean": 0.005879276533071,
"sd": 0.01068202728248 
},
{
 "name": "simplex",
"mean": 0.005261330942408,
"sd": 0.01029999527627 
},
{
 "name": "simplex",
"mean": 0.006479490937613,
"sd": 0.01815111655176 
},
{
 "name": "simplex",
"mean": 0.004921707642649,
"sd": 0.0240261277119 
},
{
 "name": "simplex",
"mean": 0.005343711808695,
"sd": 0.008551167694293 
},
{
 "name": "simplex",
"mean": 0.004888419976471,
"sd": 0.0127842124157 
},
{
 "name": "simplex",
"mean": 0.005337666279791,
"sd": 0.008549012692283 
},
{
 "name": "simplex",
"mean": 0.007159989027852,
"sd": 0.03096797597246 
},
{
 "name": "simplex",
"mean": 0.006487081965356,
"sd": 0.01821694267538 
},
{
 "name": "simplex",
"mean": 0.006642662871985,
"sd": 0.02113681350789 
},
{
 "name": "simplex",
"mean": 0.006399651086549,
"sd": 0.01368035475621 
},
{
 "name": "simplex",
"mean": 0.007097650803843,
"sd": 0.02923922253745 
},
{
 "name": "simplex",
"mean": 0.007314693113453,
"sd": 0.03550926093544 
},
{
 "name": "simplex",
"mean": 0.005159394277063,
"sd": 0.008680557648154 
},
{
 "name": "simplex",
"mean": 0.004900041256926,
"sd": 0.01274155149173 
},
{
 "name": "simplex",
"mean": 0.006465896515556,
"sd": 0.01562533387836 
},
{
 "name": "simplex",
"mean": 0.004918438970464,
"sd": 0.02330264900003 
},
{
 "name": "simplex",
"mean": 0.004914113755837,
"sd": 0.02204413228611 
},
{
 "name": "simplex",
"mean": 0.004888278336915,
"sd": 0.01277693631555 
},
{
 "name": "simplex",
"mean": 0.006486565348892,
"sd": 0.01819787663801 
},
{
 "name": "simplex",
"mean": 0.006284258866286,
"sd": 0.01458982769977 
},
{
 "name": "simplex",
"mean": 0.004888556383236,
"sd": 0.01247987802687 
},
{
 "name": "simplex",
"mean": 0.006858436967392,
"sd": 0.02977731276973 
},
{
 "name": "simplex",
"mean": 0.004923546808747,
"sd": 0.01232600484429 
},
{
 "name": "simplex",
"mean": 0.005441722108684,
"sd": 0.009277613841093 
},
{
 "name": "simplex",
"mean": 0.004918908736473,
"sd": 0.02408178262889 
},
{
 "name": "simplex",
"mean": 0.006340899383075,
"sd": 0.01366950034173 
},
{
 "name": "simplex",
"mean": 0.004934450799227,
"sd": 0.02321493257951 
},
{
 "name": "simplex",
"mean": 0.007261803094773,
"sd": 0.03439041490001 
},
{
 "name": "simplex",
"mean": 0.005337073225525,
"sd": 0.008548709505113 
},
{
 "name": "simplex",
"mean": 0.004929576325584,
"sd": 0.02404115872287 
},
{
 "name": "simplex",
"mean": 0.005336032777841,
"sd": 0.008537345803675 
},
{
 "name": "simplex",
"mean": 0.007232870583008,
"sd": 0.03442000831871 
},
{
 "name": "simplex",
"mean": 0.0051813426391,
"sd": 0.01054318583858 
},
{
 "name": "simplex",
"mean": 0.004916736570932,
"sd": 0.02416985184551 
},
{
 "name": "simplex",
"mean": 0.006476084120564,
"sd": 0.01798591350473 
},
{
 "name": "simplex",
"mean": 0.005311853903135,
"sd": 0.008391677477276 
},
{
 "name": "simplex",
"mean": 0.006464720600929,
"sd": 0.01809602644492 
},
{
 "name": "simplex",
"mean": 0.004887819410783,
"sd": 0.01278617595605 
},
{
 "name": "simplex",
"mean": 0.00720547728604,
"sd": 0.03228524815616 
},
{
 "name": "simplex",
"mean": 0.005337149861348,
"sd": 0.00854886608737 
},
{
 "name": "simplex",
"mean": 0.006469796620936,
"sd": 0.01803805111016 
},
{
 "name": "simplex",
"mean": 0.005503543279289,
"sd": 0.008769490023261 
},
{
 "name": "simplex",
"mean": 0.005329645481477,
"sd": 0.008498986827496 
},
{
 "name": "simplex",
"mean": 0.004942888355577,
"sd": 0.02299044579571 
},
{
 "name": "simplex",
"mean": 0.00491851431515,
"sd": 0.0241414143328 
},
{
 "name": "simplex",
"mean": 0.005839842757108,
"sd": 0.01040713242315 
},
{
 "name": "simplex",
"mean": 0.006502074410196,
"sd": 0.01827108460846 
},
{
 "name": "simplex",
"mean": 0.00649258537937,
"sd": 0.0158003744266 
},
{
 "name": "simplex",
"mean": 0.006487210888127,
"sd": 0.0182184535818 
},
{
 "name": "simplex",
"mean": 0.006500974593292,
"sd": 0.01589345103775 
},
{
 "name": "simplex",
"mean": 0.00504937277739,
"sd": 0.009681113110849 
},
{
 "name": "simplex",
"mean": 0.006475917269586,
"sd": 0.01577137715117 
},
{
 "name": "simplex",
"mean": 0.004909337963008,
"sd": 0.01280580979786 
},
{
 "name": "simplex",
"mean": 0.007122276078583,
"sd": 0.03324152637324 
},
{
 "name": "simplex",
"mean": 0.006487056343239,
"sd": 0.01821831073466 
},
{
 "name": "simplex",
"mean": 0.0064729671243,
"sd": 0.01576640040287 
},
{
 "name": "simplex",
"mean": 0.00532138390433,
"sd": 0.008444750544607 
},
{
 "name": "simplex",
"mean": 0.005134882445108,
"sd": 0.008735969180794 
},
{
 "name": "simplex",
"mean": 0.006493308416868,
"sd": 0.01587419314066 
},
{
 "name": "simplex",
"mean": 0.006487720614409,
"sd": 0.01821857218557 
},
{
 "name": "simplex",
"mean": 0.004892141800791,
"sd": 0.01279734523632 
},
{
 "name": "simplex",
"mean": 0.006461950587564,
"sd": 0.01791446567446 
},
{
 "name": "simplex",
"mean": 0.004912393805082,
"sd": 0.01253465438848 
},
{
 "name": "simplex",
"mean": 0.005336667742822,
"sd": 0.008544853366359 
},
{
 "name": "simplex",
"mean": 0.004919997943279,
"sd": 0.02413117464879 
},
{
 "name": "simplex",
"mean": 0.006681053211677,
"sd": 0.02710700994477 
},
{
 "name": "simplex",
"mean": 0.00570663021254,
"sd": 0.01357708052113 
},
{
 "name": "simplex",
"mean": 0.006379235027423,
"sd": 0.01684696581953 
},
{
 "name": "simplex",
"mean": 0.004961880670246,
"sd": 0.02346992552001 
},
{
 "name": "simplex",
"mean": 0.005350032414231,
"sd": 0.008524996788035 
},
{
 "name": "simplex",
"mean": 0.004955312894639,
"sd": 0.01571863222707 
},
{
 "name": "simplex",
"mean": 0.005397608947565,
"sd": 0.01776476371095 
},
{
 "name": "simplex",
"mean": 0.004916609511804,
"sd": 0.02417090525845 
},
{
 "name": "simplex",
"mean": 0.004954582572303,
"sd": 0.02379031031246 
},
{
 "name": "simplex",
"mean": 0.006170998893949,
"sd": 0.01862308511685 
},
{
 "name": "simplex",
"mean": 0.006290238270252,
"sd": 0.01478925092275 
},
{
 "name": "simplex",
"mean": 0.006482264685329,
"sd": 0.01715642898044 
},
{
 "name": "simplex",
"mean": 0.007314701432913,
"sd": 0.03551435285209 
},
{
 "name": "simplex",
"mean": 0.005903329584622,
"sd": 0.02078188687276 
},
{
 "name": "simplex",
"mean": 0.006627167163746,
"sd": 0.02666839538284 
},
{
 "name": "simplex",
"mean": 0.005085769888829,
"sd": 0.01229292848425 
},
{
 "name": "simplex",
"mean": 0.007301831815814,
"sd": 0.03512066719723 
},
{
 "name": "simplex",
"mean": 0.007009004291686,
"sd": 0.03125760158767 
},
{
 "name": "simplex",
"mean": 0.007308707722575,
"sd": 0.03533428401916 
},
{
 "name": "simplex",
"mean": 0.006280078119977,
"sd": 0.01607565522241 
},
{
 "name": "simplex",
"mean": 0.00649262255715,
"sd": 0.01580115607776 
},
{
 "name": "simplex",
"mean": 0.005658706010053,
"sd": 0.01789278121576 
},
{
 "name": "simplex",
"mean": 0.006492078667619,
"sd": 0.01579756116134 
},
{
 "name": "simplex",
"mean": 0.005197710791243,
"sd": 0.02186989620935 
},
{
 "name": "simplex",
"mean": 0.007292194260101,
"sd": 0.03487556355556 
},
{
 "name": "simplex",
"mean": 0.005979827817044,
"sd": 0.01267783030902 
},
{
 "name": "simplex",
"mean": 0.006492582737813,
"sd": 0.01580099358674 
},
{
 "name": "simplex",
"mean": 0.005313962012396,
"sd": 0.008428674891639 
},
{
 "name": "simplex",
"mean": 0.006453121471309,
"sd": 0.01780676091778 
},
{
 "name": "simplex",
"mean": 0.004918397026109,
"sd": 0.02409978155607 
},
{
 "name": "simplex",
"mean": 0.006347853107951,
"sd": 0.01468434294752 
},
{
 "name": "simplex",
"mean": 0.006508733031708,
"sd": 0.01598191273784 
},
{
 "name": "simplex",
"mean": 0.006332255155355,
"sd": 0.01404501445804 
},
{
 "name": "simplex",
"mean": 0.005511346134637,
"sd": 0.009838806443259 
},
{
 "name": "simplex",
"mean": 0.005334370741327,
"sd": 0.01355228202445 
},
{
 "name": "simplex",
"mean": 0.004888076565673,
"sd": 0.01277593787543 
},
{
 "name": "simplex",
"mean": 0.006746799721097,
"sd": 0.02110162879854 
},
{
 "name": "simplex",
"mean": 0.007084008407932,
"sd": 0.0289232014107 
},
{
 "name": "simplex",
"mean": 0.004905707178683,
"sd": 0.01273614554687 
},
{
 "name": "simplex",
"mean": 0.004967503357688,
"sd": 0.0235584286255 
},
{
 "name": "simplex",
"mean": 0.004887826164243,
"sd": 0.01278648588678 
},
{
 "name": "simplex",
"mean": 0.006489188678379,
"sd": 0.01779010634979 
},
{
 "name": "simplex",
"mean": 0.006497101716105,
"sd": 0.01825231593391 
},
{
 "name": "simplex",
"mean": 0.006491907370393,
"sd": 0.01550028155426 
},
{
 "name": "simplex",
"mean": 0.005578566505148,
"sd": 0.009269409133027 
},
{
 "name": "simplex",
"mean": 0.007299983000339,
"sd": 0.03529589004175 
},
{
 "name": "simplex",
"mean": 0.006319004667581,
"sd": 0.01540363023007 
},
{
 "name": "simplex",
"mean": 0.006488565833685,
"sd": 0.01583834531199 
},
{
 "name": "simplex",
"mean": 0.004917235648739,
"sd": 0.02366223963245 
},
{
 "name": "simplex",
"mean": 0.006645952864076,
"sd": 0.01800422508959 
},
{
 "name": "simplex",
"mean": 0.006470573156939,
"sd": 0.01808054561553 
},
{
 "name": "simplex",
"mean": 0.00709098164022,
"sd": 0.0289954457343 
},
{
 "name": "simplex",
"mean": 0.006483765129892,
"sd": 0.0180523723675 
},
{
 "name": "simplex",
"mean": 0.004887982794048,
"sd": 0.01278520713365 
},
{
 "name": "simplex",
"mean": 0.004916259981798,
"sd": 0.02320297775818 
},
{
 "name": "simplex",
"mean": 0.00649267098368,
"sd": 0.01579085734803 
},
{
 "name": "simplex",
"mean": 0.004913133973412,
"sd": 0.01219971716635 
},
{
 "name": "simplex",
"mean": 0.005337010261587,
"sd": 0.008548255678934 
},
{
 "name": "simplex",
"mean": 0.004916585432416,
"sd": 0.02417127943118 
},
{
 "name": "simplex",
"mean": 0.006127415278837,
"sd": 0.01579140179154 
},
{
 "name": "simplex",
"mean": 0.00648947305265,
"sd": 0.01822514018605 
},
{
 "name": "simplex",
"mean": 0.007071378732257,
"sd": 0.02948426920547 
},
{
 "name": "simplex",
"mean": 0.007315116365523,
"sd": 0.03551931631049 
},
{
 "name": "simplex",
"mean": 0.004917721451709,
"sd": 0.02415430616048 
},
{
 "name": "simplex",
"mean": 0.007296466695225,
"sd": 0.03496949600033 
},
{
 "name": "simplex",
"mean": 0.005333032743731,
"sd": 0.008497350317162 
},
{
 "name": "simplex",
"mean": 0.004902603323996,
"sd": 0.01252952773146 
},
{
 "name": "simplex",
"mean": 0.007065491425387,
"sd": 0.03193706774439 
},
{
 "name": "simplex",
"mean": 0.006205677938926,
"sd": 0.01382851246846 
},
{
 "name": "simplex",
"mean": 0.004916623416025,
"sd": 0.0241708614117 
},
{
 "name": "simplex",
"mean": 0.005547943428566,
"sd": 0.01413244850649 
},
{
 "name": "simplex",
"mean": 0.006487541504211,
"sd": 0.01786313303419 
},
{
 "name": "simplex",
"mean": 0.006427155599881,
"sd": 0.0174983013108 
},
{
 "name": "simplex",
"mean": 0.005198108784766,
"sd": 0.02140496263553 
},
{
 "name": "simplex",
"mean": 0.004930927403688,
"sd": 0.01239009417304 
},
{
 "name": "simplex",
"mean": 0.004916627186119,
"sd": 0.02417088700256 
},
{
 "name": "simplex",
"mean": 0.006460856302856,
"sd": 0.01790492539699 
},
{
 "name": "simplex",
"mean": 0.004918782068954,
"sd": 0.02413697206552 
},
{
 "name": "simplex",
"mean": 0.005844661028298,
"sd": 0.0136193925926 
},
{
 "name": "simplex",
"mean": 0.005314956686803,
"sd": 0.008392049986125 
},
{
 "name": "simplex",
"mean": 0.007314460205541,
"sd": 0.03549964647325 
},
{
 "name": "simplex",
"mean": 0.006109679905448,
"sd": 0.01608337444293 
},
{
 "name": "simplex",
"mean": 0.005337081607159,
"sd": 0.008548262027161 
},
{
 "name": "simplex",
"mean": 0.005337130345589,
"sd": 0.008548753407021 
},
{
 "name": "simplex",
"mean": 0.005357728527424,
"sd": 0.008602156083478 
},
{
 "name": "simplex",
"mean": 0.00649256048375,
"sd": 0.01580040172335 
},
{
 "name": "simplex",
"mean": 0.006431468089202,
"sd": 0.01757822647938 
},
{
 "name": "simplex",
"mean": 0.004888122935177,
"sd": 0.01276908482747 
},
{
 "name": "simplex",
"mean": 0.006605062354808,
"sd": 0.01879661737803 
},
{
 "name": "simplex",
"mean": 0.006492707828832,
"sd": 0.01580144363966 
},
{
 "name": "simplex",
"mean": 0.005337219421122,
"sd": 0.008548765343706 
},
{
 "name": "simplex",
"mean": 0.006492679907909,
"sd": 0.01580117609404 
},
{
 "name": "simplex",
"mean": 0.006459959002271,
"sd": 0.02463011742114 
},
{
 "name": "simplex",
"mean": 0.007315114190384,
"sd": 0.03551930389912 
},
{
 "name": "simplex",
"mean": 0.006484815976726,
"sd": 0.01578955629814 
},
{
 "name": "simplex",
"mean": 0.006491778191092,
"sd": 0.01579261967558 
},
{
 "name": "simplex",
"mean": 0.004919999917085,
"sd": 0.02413862151191 
},
{
 "name": "simplex",
"mean": 0.005293096985721,
"sd": 0.01467501442449 
},
{
 "name": "simplex",
"mean": 0.005112034406479,
"sd": 0.01538947790743 
},
{
 "name": "simplex",
"mean": 0.006490920209438,
"sd": 0.01823089245543 
},
{
 "name": "simplex",
"mean": 0.006492236070547,
"sd": 0.01579853836664 
},
{
 "name": "simplex",
"mean": 0.006487329729292,
"sd": 0.01821978954888 
},
{
 "name": "simplex",
"mean": 0.005357893482792,
"sd": 0.008665324071386 
},
{
 "name": "simplex",
"mean": 0.005377777462624,
"sd": 0.008783036033478 
},
{
 "name": "simplex",
"mean": 0.005299998101712,
"sd": 0.01747768984448 
},
{
 "name": "simplex",
"mean": 0.006428127976573,
"sd": 0.01537820790779 
},
{
 "name": "simplex",
"mean": 0.004920927745938,
"sd": 0.02387675835208 
},
{
 "name": "simplex",
"mean": 0.005361672243321,
"sd": 0.008560964754937 
},
{
 "name": "simplex",
"mean": 0.006487317610321,
"sd": 0.01821975317873 
},
{
 "name": "simplex",
"mean": 0.006492671205073,
"sd": 0.01580114264971 
},
{
 "name": "simplex",
"mean": 0.007290887853554,
"sd": 0.03520243815839 
},
{
 "name": "simplex",
"mean": 0.005352548420879,
"sd": 0.008497906740577 
},
{
 "name": "simplex",
"mean": 0.005129523857987,
"sd": 0.009930790185524 
},
{
 "name": "simplex",
"mean": 0.006389293867287,
"sd": 0.01566187289236 
},
{
 "name": "simplex",
"mean": 0.00528046634392,
"sd": 0.008265076891663 
},
{
 "name": "simplex",
"mean": 0.006483987272231,
"sd": 0.01820249569498 
},
{
 "name": "simplex",
"mean": 0.006490515531234,
"sd": 0.01578064637692 
},
{
 "name": "simplex",
"mean": 0.006557829713465,
"sd": 0.01680796683206 
},
{
 "name": "simplex",
"mean": 0.005338114437928,
"sd": 0.008549370652106 
},
{
 "name": "simplex",
"mean": 0.006411707552151,
"sd": 0.01567545633864 
},
{
 "name": "simplex",
"mean": 0.004888889346715,
"sd": 0.01278855922592 
},
{
 "name": "simplex",
"mean": 0.007240777077635,
"sd": 0.03366491888478 
},
{
 "name": "simplex",
"mean": 0.005143963875728,
"sd": 0.008689121418221 
},
{
 "name": "simplex",
"mean": 0.00618212120192,
"sd": 0.01670892144689 
},
{
 "name": "simplex",
"mean": 0.006493613305093,
"sd": 0.01580961821004 
},
{
 "name": "simplex",
"mean": 0.004916597217599,
"sd": 0.02417082444388 
},
{
 "name": "simplex",
"mean": 0.00637506052562,
"sd": 0.01699962525023 
},
{
 "name": "simplex",
"mean": 0.00488788323231,
"sd": 0.01278602602964 
},
{
 "name": "simplex",
"mean": 0.00558537346698,
"sd": 0.008972276890828 
},
{
 "name": "simplex",
"mean": 0.00488842801337,
"sd": 0.01278803455261 
},
{
 "name": "simplex",
"mean": 0.006487250624311,
"sd": 0.01821940526386 
},
{
 "name": "simplex",
"mean": 0.005337114144586,
"sd": 0.008548790730289 
},
{
 "name": "simplex",
"mean": 0.005004869245998,
"sd": 0.01151735252442 
},
{
 "name": "simplex",
"mean": 0.006524998130552,
"sd": 0.02332769084484 
},
{
 "name": "simplex",
"mean": 0.00490600412484,
"sd": 0.01241584834475 
},
{
 "name": "simplex",
"mean": 0.007315092090932,
"sd": 0.03551904358114 
},
{
 "name": "simplex",
"mean": 0.005337239690093,
"sd": 0.008549586767472 
},
{
 "name": "simplex",
"mean": 0.005081746990576,
"sd": 0.02254861219117 
},
{
 "name": "simplex",
"mean": 0.006492638836547,
"sd": 0.01580076584667 
},
{
 "name": "simplex",
"mean": 0.006487223468582,
"sd": 0.01821925191519 
},
{
 "name": "simplex",
"mean": 0.006553096861712,
"sd": 0.01656079483392 
},
{
 "name": "simplex",
"mean": 0.006491737858643,
"sd": 0.01579613429324 
},
{
 "name": "simplex",
"mean": 0.006485869060278,
"sd": 0.01820116202971 
},
{
 "name": "simplex",
"mean": 0.004887805036003,
"sd": 0.01278646944016 
},
{
 "name": "simplex",
"mean": 0.006483140267738,
"sd": 0.01813815543361 
},
{
 "name": "simplex",
"mean": 0.004937186782216,
"sd": 0.02340086099019 
},
{
 "name": "simplex",
"mean": 0.006411819011601,
"sd": 0.01498269515686 
},
{
 "name": "simplex",
"mean": 0.006286048587842,
"sd": 0.01390148319944 
},
{
 "name": "simplex",
"mean": 0.005270323895719,
"sd": 0.02087617702155 
},
{
 "name": "simplex",
"mean": 0.004921027775852,
"sd": 0.02171582093326 
},
{
 "name": "simplex",
"mean": 0.006239473626566,
"sd": 0.01529609726577 
},
{
 "name": "simplex",
"mean": 0.006042288479787,
"sd": 0.0160069348763 
},
{
 "name": "simplex",
"mean": 0.006532285040508,
"sd": 0.01842839035913 
},
{
 "name": "simplex",
"mean": 0.005074317474396,
"sd": 0.02263249133364 
},
{
 "name": "simplex",
"mean": 0.005311509155944,
"sd": 0.01857511780478 
},
{
 "name": "simplex",
"mean": 0.005693007854308,
"sd": 0.0142908735442 
},
{
 "name": "simplex",
"mean": 0.007315057118557,
"sd": 0.03551760986384 
},
{
 "name": "simplex",
"mean": 0.00720804852838,
"sd": 0.03424169183591 
},
{
 "name": "simplex",
"mean": 0.005682137474405,
"sd": 0.009527541917055 
},
{
 "name": "simplex",
"mean": 0.005390884386651,
"sd": 0.008612651943628 
},
{
 "name": "simplex",
"mean": 0.006487551914462,
"sd": 0.01821995999205 
},
{
 "name": "simplex",
"mean": 0.007291785868763,
"sd": 0.03482011074588 
},
{
 "name": "simplex",
"mean": 0.004916605415766,
"sd": 0.02417108062961 
},
{
 "name": "simplex",
"mean": 0.006492551587714,
"sd": 0.01580011944339 
},
{
 "name": "simplex",
"mean": 0.005236800231739,
"sd": 0.008241976321703 
},
{
 "name": "simplex",
"mean": 0.007315121940822,
"sd": 0.03551939670262 
},
{
 "name": "simplex",
"mean": 0.007315119342473,
"sd": 0.035519348646 
},
{
 "name": "simplex",
"mean": 0.006489872679106,
"sd": 0.0143299675426 
},
{
 "name": "simplex",
"mean": 0.006472428492464,
"sd": 0.01814278662604 
},
{
 "name": "simplex",
"mean": 0.004916033633753,
"sd": 0.0128121797735 
},
{
 "name": "simplex",
"mean": 0.004916981802017,
"sd": 0.02416751350807 
},
{
 "name": "simplex",
"mean": 0.005537975582457,
"sd": 0.008897103076069 
},
{
 "name": "simplex",
"mean": 0.005346545542126,
"sd": 0.008555522638529 
},
{
 "name": "simplex",
"mean": 0.005337073195741,
"sd": 0.008548712335759 
},
{
 "name": "simplex",
"mean": 0.004888141781781,
"sd": 0.01277530738501 
},
{
 "name": "simplex",
"mean": 0.007272548727972,
"sd": 0.03424751747418 
},
{
 "name": "simplex",
"mean": 0.007315109873268,
"sd": 0.03551920827052 
},
{
 "name": "simplex",
"mean": 0.006492931398702,
"sd": 0.01580386100136 
},
{
 "name": "simplex",
"mean": 0.005863796787986,
"sd": 0.01387767604273 
},
{
 "name": "simplex",
"mean": 0.007258979701041,
"sd": 0.03471628141544 
},
{
 "name": "simplex",
"mean": 0.006491014692076,
"sd": 0.01578975786681 
},
{
 "name": "simplex",
"mean": 0.004887804878261,
"sd": 0.01278647406699 
},
{
 "name": "simplex",
"mean": 0.006476394632974,
"sd": 0.01577040211689 
},
{
 "name": "simplex",
"mean": 0.005844105368349,
"sd": 0.01317091668757 
},
{
 "name": "simplex",
"mean": 0.006551319962739,
"sd": 0.01641250867103 
},
{
 "name": "simplex",
"mean": 0.005337069251958,
"sd": 0.008548683861633 
},
{
 "name": "simplex",
"mean": 0.004916585409148,
"sd": 0.02417128022511 
},
{
 "name": "simplex",
"mean": 0.004975261037806,
"sd": 0.02358929499325 
},
{
 "name": "simplex",
"mean": 0.006480226641555,
"sd": 0.01813397834871 
},
{
 "name": "simplex",
"mean": 0.006486581067892,
"sd": 0.01577910519723 
},
{
 "name": "simplex",
"mean": 0.006492682926801,
"sd": 0.01580118185488 
},
{
 "name": "simplex",
"mean": 0.007265046656574,
"sd": 0.03473707240296 
},
{
 "name": "simplex",
"mean": 0.006985109299287,
"sd": 0.0309545548652 
},
{
 "name": "simplex",
"mean": 0.00488780523612,
"sd": 0.01278632162827 
},
{
 "name": "simplex",
"mean": 0.006491634465631,
"sd": 0.01579072706183 
},
{
 "name": "simplex",
"mean": 0.00649257291134,
"sd": 0.01580021367906 
},
{
 "name": "simplex",
"mean": 0.005337624139942,
"sd": 0.008546115131153 
},
{
 "name": "simplex",
"mean": 0.005336968659921,
"sd": 0.008547730222933 
},
{
 "name": "simplex",
"mean": 0.005337073179153,
"sd": 0.00854871225667 
},
{
 "name": "simplex",
"mean": 0.004888413402684,
"sd": 0.01253567095435 
},
{
 "name": "simplex",
"mean": 0.004887804879086,
"sd": 0.01278647362591 
},
{
 "name": "simplex",
"mean": 0.004965821406871,
"sd": 0.02338312173544 
},
{
 "name": "simplex",
"mean": 0.006446294424649,
"sd": 0.0157243634196 
},
{
 "name": "simplex",
"mean": 0.005337074816725,
"sd": 0.008548714698704 
},
{
 "name": "simplex",
"mean": 0.006486563517905,
"sd": 0.01821075510651 
},
{
 "name": "simplex",
"mean": 0.006477543685812,
"sd": 0.01810393219228 
},
{
 "name": "simplex",
"mean": 0.005630445650161,
"sd": 0.01542648115948 
},
{
 "name": "simplex",
"mean": 0.007315108184978,
"sd": 0.03551898885872 
},
{
 "name": "simplex",
"mean": 0.004887809041878,
"sd": 0.01278646443716 
},
{
 "name": "simplex",
"mean": 0.005337069097273,
"sd": 0.008548660634042 
},
{
 "name": "simplex",
"mean": 0.005100020150905,
"sd": 0.01294141318417 
},
{
 "name": "simplex",
"mean": 0.006492683596728,
"sd": 0.01580118981651 
},
{
 "name": "simplex",
"mean": 0.004916592710735,
"sd": 0.02417104452344 
},
{
 "name": "simplex",
"mean": 0.005794770364828,
"sd": 0.012408094226 
},
{
 "name": "simplex",
"mean": 0.004913037704159,
"sd": 0.02127835677283 
},
{
 "name": "simplex",
"mean": 0.004996777884854,
"sd": 0.01997823666919 
},
{
 "name": "simplex",
"mean": 0.00489146458677,
"sd": 0.01162887132892 
},
{
 "name": "simplex",
"mean": 0.004898443702806,
"sd": 0.01274121966073 
},
{
 "name": "simplex",
"mean": 0.005337074544643,
"sd": 0.008548719060631 
},
{
 "name": "simplex",
"mean": 0.006868550598249,
"sd": 0.02306893020415 
},
{
 "name": "simplex",
"mean": 0.007313923668558,
"sd": 0.03548363498067 
},
{
 "name": "simplex",
"mean": 0.004887827582041,
"sd": 0.01278600596997 
},
{
 "name": "simplex",
"mean": 0.004916585964451,
"sd":  0.02417127656 
},
{
 "name": "simplex",
"mean": 0.004887827802455,
"sd": 0.01278156613512 
},
{
 "name": "simplex",
"mean": 0.004916427241212,
"sd": 0.01220736890196 
},
{
 "name": "simplex",
"mean": 0.007020749678612,
"sd": 0.02704123105281 
},
{
 "name": "simplex",
"mean": 0.007315121930813,
"sd": 0.03551939640309 
},
{
 "name": "simplex",
"mean": 0.005153823658923,
"sd": 0.01237784469839 
},
{
 "name": "simplex",
"mean": 0.006487241198491,
"sd": 0.01821884767251 
},
{
 "name": "simplex",
"mean": 0.004916585365854,
"sd": 0.02417128254867 
},
{
 "name": "simplex",
"mean": 0.005342898899105,
"sd": 0.00855102104811 
},
{
 "name": "simplex",
"mean": 0.005461026930098,
"sd": 0.009411901841168 
},
{
 "name": "simplex",
"mean": 0.006487317073171,
"sd": 0.01821975159432 
},
{
 "name": "simplex",
"mean": 0.006487301239455,
"sd": 0.01821955872355 
},
{
 "name": "simplex",
"mean": 0.004934561316452,
"sd": 0.01261991092514 
},
{
 "name": "simplex",
"mean": 0.004914736166787,
"sd": 0.01233121093596 
},
{
 "name": "simplex",
"mean": 0.006492664196247,
"sd": 0.01579875591552 
},
{
 "name": "simplex",
"mean": 0.00648731707343,
"sd": 0.01821975159473 
},
{
 "name": "simplex",
"mean": 0.004915216691228,
"sd": 0.02197852296021 
},
{
 "name": "simplex",
"mean": 0.004981528198894,
"sd": 0.01258247457053 
},
{
 "name": "simplex",
"mean": 0.006637049598524,
"sd": 0.01938622543799 
},
{
 "name": "simplex",
"mean": 0.005821666260708,
"sd": 0.01121595706116 
},
{
 "name": "simplex",
"mean": 0.006487351815129,
"sd": 0.01821984540697 
},
{
 "name": "simplex",
"mean": 0.005167719556041,
"sd": 0.0139232373744 
},
{
 "name": "simplex",
"mean": 0.006490239908228,
"sd": 0.01822875922553 
},
{
 "name": "simplex",
"mean": 0.004916870044964,
"sd": 0.024166834099 
},
{
 "name": "simplex",
"mean": 0.004917061192695,
"sd": 0.024166763959 
},
{
 "name": "simplex",
"mean": 0.005337152933976,
"sd": 0.008548973488346 
},
{
 "name": "simplex",
"mean": 0.006572882561513,
"sd": 0.02529175528882 
},
{
 "name": "simplex",
"mean": 0.006492682079172,
"sd": 0.01580117383121 
},
{
 "name": "simplex",
"mean": 0.005827902942465,
"sd": 0.01109869495193 
},
{
 "name": "simplex",
"mean": 0.006328782244764,
"sd": 0.01740468451874 
},
{
 "name": "simplex",
"mean": 0.0060566532666,
"sd": 0.01339536702032 
},
{
 "name": "simplex",
"mean": 0.004926450153095,
"sd": 0.02407273590737 
},
{
 "name": "simplex",
"mean": 0.005284555147392,
"sd": 0.008256015919511 
},
{
 "name": "simplex",
"mean": 0.00647937053749,
"sd": 0.01569506746888 
},
{
 "name": "simplex",
"mean": 0.006487317073161,
"sd": 0.01821975159427 
},
{
 "name": "simplex",
"mean": 0.007315121938789,
"sd": 0.03551939683643 
},
{
 "name": "simplex",
"mean": 0.005176210912861,
"sd": 0.02205396872866 
},
{
 "name": "simplex",
"mean": 0.005342268861339,
"sd": 0.008550831861515 
},
{
 "name": "simplex",
"mean": 0.004887806427641,
"sd": 0.01278645984837 
},
{
 "name": "simplex",
"mean": 0.007281735655387,
"sd": 0.03504087041204 
},
{
 "name": "simplex",
"mean": 0.004916585075265,
"sd": 0.02417100974329 
},
{
 "name": "simplex",
"mean": 0.006488295492382,
"sd": 0.01636331021801 
},
{
 "name": "simplex",
"mean": 0.00633240182596,
"sd": 0.01431703669187 
},
{
 "name": "simplex",
"mean": 0.005337074008231,
"sd": 0.008548712792314 
},
{
 "name": "simplex",
"mean": 0.006253126148243,
"sd": 0.01387101037627 
},
{
 "name": "simplex",
"mean": 0.005337073236102,
"sd": 0.008548712300724 
},
{
 "name": "simplex",
"mean": 0.005848367515905,
"sd": 0.01046204954528 
},
{
 "name": "simplex",
"mean": 0.005274136746981,
"sd": 0.008742318076816 
},
{
 "name": "simplex",
"mean": 0.006487296641206,
"sd": 0.01821956650276 
},
{
 "name": "simplex",
"mean": 0.004916598140041,
"sd": 0.02417088377035 
},
{
 "name": "simplex",
"mean": 0.005337095528191,
"sd": 0.008548721342817 
},
{
 "name": "simplex",
"mean": 0.004887805113972,
"sd": 0.01278647320456 
},
{
 "name": "simplex",
"mean": 0.005337073333145,
"sd": 0.008548712365606 
},
{
 "name": "simplex",
"mean": 0.007273152106506,
"sd": 0.03427161058459 
},
{
 "name": "simplex",
"mean": 0.006487317037947,
"sd": 0.01821975117467 
},
{
 "name": "simplex",
"mean": 0.006829344339864,
"sd": 0.02353342874789 
},
{
 "name": "simplex",
"mean": 0.006474455175535,
"sd": 0.01436586049937 
},
{
 "name": "simplex",
"mean": 0.007221881674739,
"sd": 0.03275775588893 
},
{
 "name": "simplex",
"mean": 0.007315121870751,
"sd": 0.03551939471344 
},
{
 "name": "simplex",
"mean": 0.005192154441506,
"sd": 0.008386055799247 
},
{
 "name": "simplex",
"mean": 0.004887895864388,
"sd": 0.01278613964571 
},
{
 "name": "simplex",
"mean": 0.006492284024456,
"sd": 0.01579862912079 
},
{
 "name": "simplex",
"mean": 0.0049165576255,
"sd": 0.02414310015229 
},
{
 "name": "simplex",
"mean": 0.004916306199551,
"sd": 0.02393935556755 
},
{
 "name": "simplex",
"mean": 0.004887805354128,
"sd": 0.01278646427198 
},
{
 "name": "simplex",
"mean": 0.006487316392725,
"sd": 0.01821972314161 
},
{
 "name": "simplex",
"mean": 0.006465577200423,
"sd": 0.01563200156225 
},
{
 "name": "simplex",
"mean": 0.004887825546677,
"sd": 0.01277761336048 
},
{
 "name": "simplex",
"mean": 0.007197222913912,
"sd": 0.03411045252084 
},
{
 "name": "simplex",
"mean": 0.004889337752278,
"sd": 0.01275635800864 
},
{
 "name": "simplex",
"mean": 0.005344761276057,
"sd": 0.00859004706651 
},
{
 "name": "simplex",
"mean": 0.004916588621849,
"sd": 0.02417106870658 
},
{
 "name": "simplex",
"mean": 0.006468921458072,
"sd": 0.01413975894067 
},
{
 "name": "simplex",
"mean": 0.004917411542541,
"sd": 0.02412693409245 
},
{
 "name": "simplex",
"mean": 0.007313821343251,
"sd": 0.03548573171916 
},
{
 "name": "simplex",
"mean": 0.005337073170732,
"sd": 0.008548712276578 
},
{
 "name": "simplex",
"mean": 0.004916694233766,
"sd": 0.02417019339049 
},
{
 "name": "simplex",
"mean": 0.005337069604718,
"sd": 0.00854867780386 
},
{
 "name": "simplex",
"mean": 0.007313530844866,
"sd": 0.03549761724367 
},
{
 "name": "simplex",
"mean": 0.005246192649074,
"sd": 0.009193752036453 
},
{
 "name": "simplex",
"mean": 0.004916585375383,
"sd": 0.02417128245863 
},
{
 "name": "simplex",
"mean": 0.006487260537631,
"sd": 0.01821819929201 
},
{
 "name": "simplex",
"mean": 0.005335396963385,
"sd": 0.008536671160993 
},
{
 "name": "simplex",
"mean": 0.006487017224701,
"sd": 0.01821819399787 
},
{
 "name": "simplex",
"mean": 0.004887804878513,
"sd": 0.0127864740618 
},
{
 "name": "simplex",
"mean": 0.00729728554099,
"sd": 0.03498444327852 
},
{
 "name": "simplex",
"mean": 0.00533707317352,
"sd": 0.008548712279751 
},
{
 "name": "simplex",
"mean": 0.006487216310233,
"sd": 0.01821875606573 
},
{
 "name": "simplex",
"mean": 0.005356073794909,
"sd": 0.008558559377298 
},
{
 "name": "simplex",
"mean": 0.0053360601847,
"sd": 0.008538845736292 
},
{
 "name": "simplex",
"mean": 0.004917783301046,
"sd": 0.02410781617867 
},
{
 "name": "simplex",
"mean": 0.004916587608084,
"sd": 0.02417124752321 
},
{
 "name": "simplex",
"mean": 0.00576896591981,
"sd": 0.009972821659686 
},
{
 "name": "simplex",
"mean": 0.006487590778706,
"sd": 0.01822057802999 
},
{
 "name": "simplex",
"mean": 0.00649268292084,
"sd": 0.01580118180398 
},
{
 "name": "simplex",
"mean": 0.006487317063114,
"sd": 0.01821975147293 
},
{
 "name": "simplex",
"mean": 0.006492777787918,
"sd": 0.01580220448812 
},
{
 "name": "simplex",
"mean": 0.004999110361422,
"sd": 0.01066188785246 
},
{
 "name": "simplex",
"mean": 0.006492500654675,
"sd": 0.01580084126176 
},
{
 "name": "simplex",
"mean": 0.004888099988651,
"sd": 0.01278672694166 
},
{
 "name": "simplex",
"mean": 0.007297233952613,
"sd": 0.03530776702032 
},
{
 "name": "simplex",
"mean": 0.006487317034425,
"sd": 0.01821975139321 
},
{
 "name": "simplex",
"mean": 0.006492430050549,
"sd": 0.01580070942436 
},
{
 "name": "simplex",
"mean": 0.005336485471609,
"sd": 0.008544465511587 
},
{
 "name": "simplex",
"mean": 0.005160761091795,
"sd": 0.008585187278195 
},
{
 "name": "simplex",
"mean": 0.006493006354607,
"sd": 0.01580505404465 
},
{
 "name": "simplex",
"mean": 0.006487317576504,
"sd": 0.01821975269392 
},
{
 "name": "simplex",
"mean": 0.004887812578451,
"sd": 0.01278649334803 
},
{
 "name": "simplex",
"mean": 0.006486732414229,
"sd": 0.01821269878593 
},
{
 "name": "simplex",
"mean": 0.004888308156885,
"sd": 0.01277731591837 
},
{
 "name": "simplex",
"mean": 0.005337072778892,
"sd": 0.00854870851813 
},
{
 "name": "simplex",
"mean": 0.004916592788979,
"sd": 0.02417120681561 
},
{
 "name": "simplex",
"mean": 0.007041781357518,
"sd": 0.03170767909924 
},
{
 "name": "simplex",
"mean": 0.00591710793805,
"sd": 0.01416408961998 
},
{
 "name": "simplex",
"mean": 0.006472077621261,
"sd": 0.01803382452733 
},
{
 "name": "simplex",
"mean": 0.004917936535239,
"sd": 0.02415017587375 
},
{
 "name": "simplex",
"mean": 0.005337256907344,
"sd": 0.008548208199334 
},
{
 "name": "simplex",
"mean": 0.004912225301299,
"sd": 0.01853750979351 
},
{
 "name": "simplex",
"mean": 0.005018250245693,
"sd": 0.02268874101715 
},
{
 "name": "simplex",
"mean": 0.004916585366225,
"sd": 0.02417128254287 
},
{
 "name": "simplex",
"mean": 0.004917003425688,
"sd": 0.02416728242798 
},
{
 "name": "simplex",
"mean": 0.006320811785961,
"sd": 0.02041055982109 
},
{
 "name": "simplex",
"mean": 0.006472440108802,
"sd": 0.01568307556797 
},
{
 "name": "simplex",
"mean": 0.006487335308186,
"sd": 0.01810894710699 
},
{
 "name": "simplex",
"mean": 0.007315121880786,
"sd": 0.03551939618133 
},
{
 "name": "simplex",
"mean": 0.005704636826871,
"sd": 0.02038767234967 
},
{
 "name": "simplex",
"mean": 0.00712819095171,
"sd": 0.03299311043673 
},
{
 "name": "simplex",
"mean": 0.005019327758497,
"sd": 0.01456421051648 
},
{
 "name": "simplex",
"mean": 0.007314913163675,
"sd": 0.03551311820147 
},
{
 "name": "simplex",
"mean": 0.007265035795092,
"sd": 0.03480273355826 
},
{
 "name": "simplex",
"mean": 0.007315079324713,
"sd": 0.03551811702232 
},
{
 "name": "simplex",
"mean": 0.006323938971109,
"sd": 0.01607871488179 
},
{
 "name": "simplex",
"mean": 0.006492682924163,
"sd": 0.01580118185658 
},
{
 "name": "simplex",
"mean": 0.005613090002523,
"sd": 0.0181911015632 
},
{
 "name": "simplex",
"mean": 0.006492682734429,
"sd": 0.0158011806411 
},
{
 "name": "simplex",
"mean": 0.0049582785358,
"sd": 0.02378520423438 
},
{
 "name": "simplex",
"mean": 0.007314586203606,
"sd": 0.03550344758584 
},
{
 "name": "simplex",
"mean": 0.006107968234806,
"sd": 0.01342978378511 
},
{
 "name": "simplex",
"mean": 0.006492682920453,
"sd": 0.01580118184764 
},
{
 "name": "simplex",
"mean": 0.005335651968795,
"sd": 0.008535457595924 
},
{
 "name": "simplex",
"mean": 0.006486225008224,
"sd": 0.01820657804736 
},
{
 "name": "simplex",
"mean": 0.004916588683639,
"sd": 0.02417111590434 
},
{
 "name": "simplex",
"mean": 0.006483098063397,
"sd": 0.01572432424738 
},
{
 "name": "simplex",
"mean": 0.006493008641435,
"sd": 0.01580469463787 
},
{
 "name": "simplex",
"mean": 0.006464237755513,
"sd": 0.01482596328771 
},
{
 "name": "simplex",
"mean": 0.005355380050924,
"sd": 0.008650826562953 
},
{
 "name": "simplex",
"mean": 0.00509484805944,
"sd": 0.01304850259008 
},
{
 "name": "simplex",
"mean": 0.004887804916004,
"sd": 0.01278646427949 
},
{
 "name": "simplex",
"mean": 0.006636811062972,
"sd": 0.01938354623414 
},
{
 "name": "simplex",
"mean": 0.007210866242503,
"sd": 0.03243778946117 
},
{
 "name": "simplex",
"mean": 0.004887961280764,
"sd": 0.01278591378635 
},
{
 "name": "simplex",
"mean": 0.004917484258482,
"sd": 0.02416088706718 
},
{
 "name": "simplex",
"mean": 0.00488780487816,
"sd": 0.01278647407151 
},
{
 "name": "simplex",
"mean": 0.00648732888104,
"sd": 0.01820178094853 
},
{
 "name": "simplex",
"mean": 0.006487435682893,
"sd": 0.01822010911711 
},
{
 "name": "simplex",
"mean": 0.006492674661396,
"sd": 0.01578868465789 
},
{
 "name": "simplex",
"mean": 0.00540883829955,
"sd": 0.008648407861758 
},
{
 "name": "simplex",
"mean": 0.007315064797313,
"sd": 0.0355185606638 
},
{
 "name": "simplex",
"mean": 0.006476848184898,
"sd": 0.01576759051546 
},
{
 "name": "simplex",
"mean": 0.006487760733556,
"sd": 0.01728055210882 
},
{
 "name": "simplex",
"mean": 0.004916578361384,
"sd": 0.02416238905057 
},
{
 "name": "simplex",
"mean": 0.006552901314532,
"sd": 0.01651612721573 
},
{
 "name": "simplex",
"mean": 0.006487197786447,
"sd": 0.01821880397887 
},
{
 "name": "simplex",
"mean": 0.007213902493087,
"sd": 0.03252835444413 
},
{
 "name": "simplex",
"mean": 0.006487308855784,
"sd": 0.01821829913601 
},
{
 "name": "simplex",
"mean": 0.004887804893389,
"sd": 0.01278647397746 
},
{
 "name": "simplex",
"mean": 0.004916539640285,
"sd": 0.02413186221124 
},
{
 "name": "simplex",
"mean": 0.006492682920008,
"sd": 0.01580116869368 
},
{
 "name": "simplex",
"mean": 0.004889401214818,
"sd": 0.01275309825401 
},
{
 "name": "simplex",
"mean": 0.005337073161815,
"sd": 0.008548712211943 
},
{
 "name": "simplex",
"mean": 0.004916585365854,
"sd": 0.02417128254867 
},
{
 "name": "simplex",
"mean": 0.006359469617343,
"sd": 0.01564762051307 
},
{
 "name": "simplex",
"mean": 0.006487323416104,
"sd": 0.01821977055234 
},
{
 "name": "simplex",
"mean": 0.007255478714997,
"sd": 0.03378027246232 
},
{
 "name": "simplex",
"mean": 0.007315121951207,
"sd": 0.03551939701501 
},
{
 "name": "simplex",
"mean": 0.004916586192481,
"sd": 0.02417127300174 
},
{
 "name": "simplex",
"mean": 0.007314709200151,
"sd": 0.03550698610815 
},
{
 "name": "simplex",
"mean": 0.005336980386122,
"sd": 0.008547790938899 
},
{
 "name": "simplex",
"mean": 0.004888186292662,
"sd": 0.01277866811957 
},
{
 "name": "simplex",
"mean": 0.007293507925479,
"sd": 0.03519494931105 
},
{
 "name": "simplex",
"mean": 0.006454019422672,
"sd": 0.01553104061649 
},
{
 "name": "simplex",
"mean": 0.00491658536673,
"sd": 0.02417128253982 
},
{
 "name": "simplex",
"mean": 0.005414860717586,
"sd": 0.01376666212729 
},
{
 "name": "simplex",
"mean": 0.006487322399263,
"sd": 0.01820799790182 
},
{
 "name": "simplex",
"mean": 0.006484629084002,
"sd": 0.01818733855749 
},
{
 "name": "simplex",
"mean": 0.004981216124078,
"sd": 0.02353004911055 
},
{
 "name": "simplex",
"mean": 0.004889256312448,
"sd": 0.01276122365384 
},
{
 "name": "simplex",
"mean": 0.004916585366582,
"sd": 0.02417128254179 
},
{
 "name": "simplex",
"mean": 0.006486839624467,
"sd": 0.01821406060233 
},
{
 "name": "simplex",
"mean": 0.004916588446281,
"sd": 0.02417123441557 
},
{
 "name": "simplex",
"mean": 0.005548749718225,
"sd": 0.01019479907491 
},
{
 "name": "simplex",
"mean": 0.005336415575484,
"sd": 0.008542915062066 
},
{
 "name": "simplex",
"mean": 0.007315121439481,
"sd": 0.03551938174098 
},
{
 "name": "simplex",
"mean": 0.006364185445598,
"sd": 0.01758103593227 
},
{
 "name": "simplex",
"mean": 0.005337073167705,
"sd": 0.008548712221096 
},
{
 "name": "simplex",
"mean": 0.005337073172144,
"sd": 0.00854871227738 
},
{
 "name": "simplex",
"mean": 0.005337241493274,
"sd": 0.008548965056554 
},
{
 "name": "simplex",
"mean": 0.006492682917486,
"sd": 0.01580118180003 
},
{
 "name": "simplex",
"mean": 0.006485221490586,
"sd": 0.0181948149186 
},
{
 "name": "simplex",
"mean": 0.00488780497729,
"sd": 0.01278645201283 
},
{
 "name": "simplex",
"mean": 0.006511185417738,
"sd": 0.01830156159531 
},
{
 "name": "simplex",
"mean": 0.006492682927603,
"sd": 0.01580118186789 
},
{
 "name": "simplex",
"mean": 0.005337073189277,
"sd": 0.008548712283437 
},
{
 "name": "simplex",
"mean": 0.006492682926824,
"sd": 0.01580118185955 
},
{
 "name": "simplex",
"mean": 0.006778741504846,
"sd": 0.02832238309562 
},
{
 "name": "simplex",
"mean": 0.007315121951195,
"sd": 0.03551939701491 
},
{
 "name": "simplex",
"mean": 0.006492639965047,
"sd": 0.01580110283334 
},
{
 "name": "simplex",
"mean": 0.006492682220283,
"sd": 0.01580117516772 
},
{
 "name": "simplex",
"mean": 0.004916590149287,
"sd": 0.02417123734137 
},
{
 "name": "simplex",
"mean": 0.005096702112246,
"sd": 0.01863813541163 
},
{
 "name": "simplex",
"mean": 0.005047521645759,
"sd": 0.0173870213835 
},
{
 "name": "simplex",
"mean": 0.006487333045142,
"sd": 0.01821979968228 
},
{
 "name": "simplex",
"mean": 0.006492682855788,
"sd": 0.01580118141813 
},
{
 "name": "simplex",
"mean": 0.006487317073366,
"sd": 0.01821975159491 
},
{
 "name": "simplex",
"mean": 0.005337301541453,
"sd": 0.008549931232453 
},
{
 "name": "simplex",
"mean": 0.005338137306476,
"sd": 0.008554303030595 
},
{
 "name": "simplex",
"mean": 0.005014491773706,
"sd": 0.02169777043234 
},
{
 "name": "simplex",
"mean": 0.006490307086362,
"sd": 0.01578595776452 
},
{
 "name": "simplex",
"mean": 0.004916626167119,
"sd": 0.02416887351962 
},
{
 "name": "simplex",
"mean": 0.005337628303531,
"sd": 0.008548918212251 
},
{
 "name": "simplex",
"mean": 0.006487317073171,
"sd": 0.01821975159432 
},
{
 "name": "simplex",
"mean": 0.006492682926768,
"sd": 0.01580118185941 
},
{
 "name": "simplex",
"mean": 0.007314903883015,
"sd": 0.03551675367626 
},
{
 "name": "simplex",
"mean": 0.005337516369129,
"sd": 0.008547289899151 
},
{
 "name": "simplex",
"mean": 0.005033310593975,
"sd": 0.0101782441162 
},
{
 "name": "simplex",
"mean": 0.006485013106634,
"sd": 0.01578715539102 
},
{
 "name": "simplex",
"mean": 0.005328021307761,
"sd": 0.008486329575574 
},
{
 "name": "simplex",
"mean": 0.006487310118029,
"sd": 0.01821971556817 
},
{
 "name": "simplex",
"mean": 0.00649267884785,
"sd": 0.0158011432267 
},
{
 "name": "simplex",
"mean": 0.006504361554542,
"sd": 0.01593177684663 
},
{
 "name": "simplex",
"mean": 0.005337074112374,
"sd": 0.008548712856575 
},
{
 "name": "simplex",
"mean": 0.006488249031758,
"sd": 0.01579299113761 
},
{
 "name": "simplex",
"mean": 0.004887805183458,
"sd": 0.01278647473893 
},
{
 "name": "simplex",
"mean": 0.007311138124376,
"sd": 0.0354044046742 
},
{
 "name": "simplex",
"mean": 0.005216968664207,
"sd": 0.008119965762122 
},
{
 "name": "simplex",
"mean": 0.006403047708421,
"sd": 0.01778831022727 
},
{
 "name": "simplex",
"mean": 0.006492684069276,
"sd": 0.01580119379566 
},
{
 "name": "simplex",
"mean": 0.004916585365971,
"sd": 0.02417128254258 
},
{
 "name": "simplex",
"mean": 0.006479187111343,
"sd": 0.01812358296325 
},
{
 "name": "simplex",
"mean": 0.004887804880727,
"sd": 0.01278647403899 
},
{
 "name": "simplex",
"mean": 0.005386269568734,
"sd": 0.008583179865022 
},
{
 "name": "simplex",
"mean": 0.004887805038045,
"sd": 0.01278647447189 
},
{
 "name": "simplex",
"mean": 0.006487317070435,
"sd": 0.01821975158015 
},
{
 "name": "simplex",
"mean": 0.005337073171532,
"sd": 0.008548712277722 
},
{
 "name": "simplex",
"mean": 0.00490620243924,
"sd": 0.01153413079311 
},
{
 "name": "simplex",
"mean": 0.006889890722614,
"sd": 0.02893093862354 
},
{
 "name": "simplex",
"mean": 0.00488860417428,
"sd": 0.01277002759535 
},
{
 "name": "simplex",
"mean": 0.007315121950852,
"sd": 0.03551939701085 
},
{
 "name": "simplex",
"mean": 0.005337073184343,
"sd": 0.008548712349177 
},
{
 "name": "simplex",
"mean": 0.004937822035889,
"sd": 0.0239594868781 
},
{
 "name": "simplex",
"mean": 0.006492682925186,
"sd": 0.01580118184399 
},
{
 "name": "simplex",
"mean": 0.006487317067901,
"sd": 0.018219751567 
},
{
 "name": "simplex",
"mean": 0.00649787973237,
"sd": 0.01585799961499 
},
{
 "name": "simplex",
"mean": 0.006492682554895,
"sd": 0.01580117961341 
},
{
 "name": "simplex",
"mean": 0.006487315717643,
"sd": 0.01821973530797 
},
{
 "name": "simplex",
"mean": 0.004887804878049,
"sd": 0.01278647407135 
},
{
 "name": "simplex",
"mean": 0.006487311576317,
"sd": 0.0182195257657 
},
{
 "name": "simplex",
"mean": 0.004916996429856,
"sd": 0.02415139903217 
},
{
 "name": "simplex",
"mean": 0.00648780163805,
"sd": 0.01575263886355 
},
{
 "name": "simplex",
"mean": 0.006440630645155,
"sd": 0.01531146352023 
},
{
 "name": "simplex",
"mean": 0.005002640140664,
"sd": 0.02332308015557 
},
{
 "name": "simplex",
"mean": 0.004916450764288,
"sd": 0.02393331133469 
},
{
 "name": "simplex",
"mean": 0.006404304903099,
"sd": 0.0172264259684 
},
{
 "name": "simplex",
"mean": 0.006283907238863,
"sd": 0.01719616322228 
},
{
 "name": "simplex",
"mean": 0.006490907836818,
"sd": 0.01823093762552 
},
{
 "name": "simplex",
"mean": 0.004935839709263,
"sd": 0.02397920973166 
},
{
 "name": "simplex",
"mean": 0.005075635123666,
"sd": 0.02176435726246 
},
{
 "name": "simplex",
"mean": 0.005781205013138,
"sd": 0.01454712125994 
},
{
 "name": "simplex",
"mean": 0.007315121947687,
"sd": 0.0355193969097 
},
{
 "name": "simplex",
"mean": 0.007310047849019,
"sd": 0.03545933648148 
},
{
 "name": "simplex",
"mean": 0.005457000081087,
"sd": 0.008748820432956 
},
{
 "name": "simplex",
"mean": 0.005339842511336,
"sd": 0.008550501264423 
},
{
 "name": "simplex",
"mean": 0.006487317173659,
"sd": 0.01821975187884 
},
{
 "name": "simplex",
"mean": 0.007314426030758,
"sd":  0.03549847019 
},
{
 "name": "simplex",
"mean": 0.004916585366109,
"sd": 0.02417128254612 
},
{
 "name": "simplex",
"mean": 0.006492682920108,
"sd": 0.01580118180252 
},
{
 "name": "simplex",
"mean": 0.005302813344554,
"sd": 0.008347683173215 
},
{
 "name": "simplex",
"mean": 0.00731512195122,
"sd": 0.0355193970152 
},
{
 "name": "simplex",
"mean": 0.007315121951217,
"sd": 0.03551939701516 
},
{
 "name": "simplex",
"mean": 0.006489760542614,
"sd": 0.01441191493619 
},
{
 "name": "simplex",
"mean": 0.006487175883294,
"sd": 0.01821902027456 
},
{
 "name": "simplex",
"mean": 0.004888320840161,
"sd": 0.01278691640427 
},
{
 "name": "simplex",
"mean": 0.004916585430949,
"sd": 0.02417128193355 
},
{
 "name": "simplex",
"mean": 0.005382057391656,
"sd": 0.008580262008841 
},
{
 "name": "simplex",
"mean": 0.005337152471983,
"sd": 0.008548761188721 
},
{
 "name": "simplex",
"mean": 0.005337073170732,
"sd": 0.00854871227658 
},
{
 "name": "simplex",
"mean": 0.004887805113783,
"sd": 0.01278646764055 
},
{
 "name": "simplex",
"mean": 0.007312695759857,
"sd": 0.03544645865173 
},
{
 "name": "simplex",
"mean": 0.007315121951148,
"sd": 0.03551939701408 
},
{
 "name": "simplex",
"mean": 0.006492683001942,
"sd": 0.01580118266889 
},
{
 "name": "simplex",
"mean": 0.00556745483818,
"sd": 0.01038128776414 
},
{
 "name": "simplex",
"mean": 0.007313745848205,
"sd": 0.03549960958113 
},
{
 "name": "simplex",
"mean": 0.006492681626527,
"sd": 0.01580117339627 
},
{
 "name": "simplex",
"mean": 0.004887804878049,
"sd": 0.01278647407135 
},
{
 "name": "simplex",
"mean": 0.006492512809256,
"sd": 0.01580086372585 
},
{
 "name": "simplex",
"mean": 0.006090345349588,
"sd": 0.01381128107623 
},
{
 "name": "simplex",
"mean": 0.006497689332533,
"sd": 0.01585317905785 
},
{
 "name": "simplex",
"mean": 0.005337073170697,
"sd": 0.008548712276332 
},
{
 "name": "simplex",
"mean": 0.004916585365854,
"sd": 0.02417128254867 
},
{
 "name": "simplex",
"mean": 0.00491893823859,
"sd": 0.0241477514986 
},
{
 "name": "simplex",
"mean": 0.006487273750037,
"sd": 0.01821922894868 
},
{
 "name": "simplex",
"mean": 0.006492638580479,
"sd": 0.0158009765317 
},
{
 "name": "simplex",
"mean": 0.006492682926829,
"sd": 0.01580118185956 
},
{
 "name": "simplex",
"mean": 0.007313788464393,
"sd": 0.03549855157577 
},
{
 "name": "simplex",
"mean": 0.007256178293819,
"sd": 0.03467679376917 
},
{
 "name": "simplex",
"mean": 0.004887804878053,
"sd": 0.01278647406947 
},
{
 "name": "simplex",
"mean": 0.00649268197619,
"sd": 0.01580117282252 
},
{
 "name": "simplex",
"mean": 0.006492682918059,
"sd": 0.01580118177695 
},
{
 "name": "simplex",
"mean": 0.005337073584806,
"sd": 0.008548710352023 
},
{
 "name": "simplex",
"mean": 0.005337073151011,
"sd": 0.008548712091997 
},
{
 "name": "simplex",
"mean": 0.005337073170732,
"sd": 0.00854871227658 
},
{
 "name": "simplex",
"mean": 0.004887818299884,
"sd": 0.01278071740003 
},
{
 "name": "simplex",
"mean": 0.004887804878049,
"sd": 0.01278647407135 
},
{
 "name": "simplex",
"mean": 0.004917499249731,
"sd": 0.02415622981796 
},
{
 "name": "simplex",
"mean": 0.006491254845121,
"sd": 0.01579852250039 
},
{
 "name": "simplex",
"mean": 0.005337073170733,
"sd": 0.008548712276581 
},
{
 "name": "simplex",
"mean": 0.006487316710892,
"sd": 0.01821974727812 
},
{
 "name": "simplex",
"mean": 0.006487255492139,
"sd": 0.01821901795221 
},
{
 "name": "simplex",
"mean": 0.005560115834847,
"sd": 0.01596728059177 
},
{
 "name": "simplex",
"mean": 0.007315121950994,
"sd": 0.03551939700847 
},
{
 "name": "simplex",
"mean": 0.004887804878056,
"sd": 0.01278647407133 
},
{
 "name": "simplex",
"mean": 0.005337073170256,
"sd": 0.008548712273003 
},
{
 "name": "simplex",
"mean": 0.00492334640902,
"sd": 0.0128175901079 
},
{
 "name": "simplex",
"mean": 0.00649268292683,
"sd": 0.01580118185957 
},
{
 "name": "simplex",
"mean": 0.004916585365896,
"sd": 0.02417128254678 
},
{
 "name": "simplex",
"mean": 0.005895804136027,
"sd": 0.01273848923915 
},
{
 "name": "simplex",
"mean": 0.004916027468901,
"sd": 0.0237084557273 
},
{
 "name": "simplex",
"mean": 0.004937923748963,
"sd": 0.02302971265385 
},
{
 "name": "simplex",
"mean": 0.004888403007967,
"sd": 0.01253977523585 
},
{
 "name": "simplex",
"mean": 0.004887876221283,
"sd": 0.01278620838602 
},
{
 "name": "simplex",
"mean": 0.005337073170733,
"sd": 0.008548712276585 
},
{
 "name": "simplex",
"mean": 0.006837928791669,
"sd": 0.02227584193139 
},
{
 "name": "simplex",
"mean": 0.007315120200243,
"sd": 0.0355193447527 
},
{
 "name": "simplex",
"mean": 0.004887804879196,
"sd": 0.01278647404774 
},
{
 "name": "simplex",
"mean": 0.004916585365854,
"sd": 0.02417128254867 
},
{
 "name": "simplex",
"mean": 0.004887804882459,
"sd": 0.01278647228242 
},
{
 "name": "simplex",
"mean": 0.004889875105726,
"sd": 0.01274390767501 
},
{
 "name": "simplex",
"mean": 0.007120172110039,
"sd": 0.02982266152441 
},
{
 "name": "simplex",
"mean": 0.00731512195122,
"sd": 0.0355193970152 
},
{
 "name": "simplex",
"mean": 0.00517993720747,
"sd": 0.01132812304084 
},
{
 "name": "simplex",
"mean": 0.006487317069505,
"sd": 0.01821975155065 
},
{
 "name": "simplex",
"mean": 0.004916585365854,
"sd": 0.02417128254867 
},
{
 "name": "simplex",
"mean": 0.00533710294792,
"sd": 0.008548723290347 
},
{
 "name": "simplex",
"mean": 0.005346563546703,
"sd": 0.008600496037732 
},
{
 "name": "simplex",
"mean": 0.006487317073171,
"sd": 0.01821975159432 
},
{
 "name": "simplex",
"mean": 0.006487317072954,
"sd": 0.0182197515917 
},
{
 "name": "simplex",
"mean": 0.004889248912188,
"sd": 0.01278117478495 
},
{
 "name": "simplex",
"mean": 0.004889083416819,
"sd": 0.01276045153535 
},
{
 "name": "simplex",
"mean": 0.006492682926202,
"sd": 0.01580118125006 
},
{
 "name": "simplex",
"mean": 0.006487317073171,
"sd": 0.01821975159432 
},
{
 "name": "simplex",
"mean": 0.004916308278355,
"sd": 0.02393729129998 
},
{
 "name": "simplex",
"mean": 0.004891503127495,
"sd": 0.01276907336579 
},
{
 "name": "simplex",
"mean": 0.00652580669811,
"sd": 0.01838463976626 
},
{
 "name": "simplex",
"mean": 0.005781357948427,
"sd": 0.01071206211627 
},
{
 "name": "simplex",
"mean": 0.006487317074666,
"sd": 0.01821975159882 
},
{
 "name": "simplex",
"mean": 0.004928250085285,
"sd": 0.01289747692574 
},
{
 "name": "simplex",
"mean": 0.006487327527163,
"sd": 0.01821978306733 
},
{
 "name": "simplex",
"mean": 0.004916585417463,
"sd": 0.02417128174225 
},
{
 "name": "simplex",
"mean": 0.004916585460078,
"sd": 0.02417128165829 
},
{
 "name": "simplex",
"mean": 0.005337073172749,
"sd": 0.008548712282785 
},
{
 "name": "simplex",
"mean": 0.006917524118181,
"sd": 0.0299990182133 
},
{
 "name": "simplex",
"mean": 0.006492682926829,
"sd": 0.01580118185955 
},
{
 "name": "simplex",
"mean": 0.005747228105364,
"sd": 0.01042957243907 
},
{
 "name": "simplex",
"mean": 0.006468284066906,
"sd": 0.01812139864864 
},
{
 "name": "simplex",
"mean": 0.006297638418938,
"sd": 0.01462545078144 
},
{
 "name": "simplex",
"mean": 0.004916647887238,
"sd": 0.02417065705943 
},
{
 "name": "simplex",
"mean": 0.005332997145592,
"sd": 0.008515405709914 
},
{
 "name": "simplex",
"mean": 0.00649261564308,
"sd": 0.01580062940644 
},
{
 "name": "simplex",
"mean": 0.006487317073171,
"sd": 0.01821975159432 
},
{
 "name": "simplex",
"mean": 0.00731512195122,
"sd": 0.0355193970152 
},
{
 "name": "simplex",
"mean": 0.004951379612697,
"sd": 0.02384805760971 
},
{
 "name": "simplex",
"mean": 0.005337096743742,
"sd": 0.008548720996654 
},
{
 "name": "simplex",
"mean": 0.00488780487805,
"sd": 0.01278647407135 
},
{
 "name": "simplex",
"mean": 0.007314644118195,
"sd": 0.03551252548826 
},
{
 "name": "simplex",
"mean": 0.00491658536585,
"sd": 0.02417128254559 
},
{
 "name": "simplex",
"mean": 0.006487541222271,
"sd": 0.01773457673069 
},
{
 "name": "simplex",
"mean": 0.006463555275191,
"sd": 0.01552632078013 
},
{
 "name": "simplex",
"mean": 0.005337073170732,
"sd": 0.008548712276581 
},
{
 "name": "simplex",
"mean": 0.006438737351101,
"sd": 0.0153034582236 
},
{
 "name": "simplex",
"mean": 0.005337073170732,
"sd": 0.00854871227658 
},
{
 "name": "simplex",
"mean": 0.005783618497396,
"sd": 0.01005889922616 
},
{
 "name": "simplex",
"mean": 0.005323461471795,
"sd": 0.008453174639325 
},
{
 "name": "simplex",
"mean": 0.006487317073009,
"sd": 0.01821975159271 
},
{
 "name": "simplex",
"mean": 0.004916585365953,
"sd": 0.02417128254546 
},
{
 "name": "simplex",
"mean": 0.005337073171038,
"sd": 0.008548712276697 
},
{
 "name": "simplex",
"mean": 0.004887804878049,
"sd": 0.01278647407135 
},
{
 "name": "simplex",
"mean": 0.005337073170732,
"sd": 0.00854871227658 
},
{
 "name": "simplex",
"mean": 0.007312750517234,
"sd": 0.0354486304848 
},
{
 "name": "simplex",
"mean": 0.006487317073171,
"sd": 0.01821975159432 
},
{
 "name": "simplex",
"mean": 0.006924909300147,
"sd": 0.02488316640619 
},
{
 "name": "simplex",
"mean": 0.00648893564857,
"sd": 0.01478624683188 
},
{
 "name": "simplex",
"mean": 0.00730199591647,
"sd": 0.03512543076093 
},
{
 "name": "simplex",
"mean": 0.00731512195122,
"sd": 0.0355193970152 
},
{
 "name": "simplex",
"mean": 0.005253997625717,
"sd": 0.00823282310319 
},
{
 "name": "simplex",
"mean": 0.004887804883205,
"sd": 0.01278647405241 
},
{
 "name": "simplex",
"mean": 0.00649268282943,
"sd": 0.01580118123893 
},
{
 "name": "simplex",
"mean": 0.004916585326613,
"sd": 0.02417124986317 
},
{
 "name": "simplex",
"mean": 0.004916582604801,
"sd": 0.02416898562277 
},
{
 "name": "simplex",
"mean": 0.004887804878049,
"sd": 0.01278647407134 
},
{
 "name": "simplex",
"mean": 0.00648731707317,
"sd": 0.01821975159427 
},
{
 "name": "simplex",
"mean": 0.006492234892256,
"sd": 0.01579832903749 
},
{
 "name": "simplex",
"mean": 0.004887804892913,
"sd": 0.01278646769044 
},
{
 "name": "simplex",
"mean": 0.007308904110582,
"sd": 0.03544579250066 
},
{
 "name": "simplex",
"mean": 0.004887809732349,
"sd": 0.01278637417887 
},
{
 "name": "simplex",
"mean": 0.005337105570841,
"sd": 0.008548884756205 
},
{
 "name": "simplex",
"mean": 0.00491658536586,
"sd": 0.02417128254726 
},
{
 "name": "simplex",
"mean": 0.006489586644662,
"sd": 0.01420631114431 
},
{
 "name": "simplex",
"mean": 0.004916586995521,
"sd": 0.0241711950584 
},
{
 "name": "simplex",
"mean": 0.007315120698049,
"sd": 0.03551935999879 
},
{
 "name": "simplex",
"mean": 0.005337073170732,
"sd": 0.00854871227658 
},
{
 "name": "simplex",
"mean": 0.004916585373375,
"sd": 0.02417128247343 
},
{
 "name": "simplex",
"mean": 0.005337073170701,
"sd": 0.008548712276285 
},
{
 "name": "simplex",
"mean": 0.007315121313521,
"sd": 0.03551938804051 
},
{
 "name": "simplex",
"mean": 0.005308189237831,
"sd": 0.008430897710715 
},
{
 "name": "simplex",
"mean": 0.004916585365854,
"sd": 0.02417128254867 
},
{
 "name": "simplex",
"mean": 0.006487317071294,
"sd": 0.01821975150792 
},
{
 "name": "simplex",
"mean": 0.005337066868008,
"sd": 0.008548666588608 
},
{
 "name": "simplex",
"mean": 0.00648731701719,
"sd": 0.01821975130435 
},
{
 "name": "simplex",
"mean": 0.004887804878049,
"sd": 0.01278647407135 
},
{
 "name": "simplex",
"mean": 0.007314721226245,
"sd": 0.03550734639624 
},
{
 "name": "simplex",
"mean": 0.005337073170732,
"sd": 0.00854871227658 
},
{
 "name": "simplex",
"mean": 0.006487317069804,
"sd": 0.01821975156048 
},
{
 "name": "simplex",
"mean": 0.005337321586669,
"sd": 0.008548805980405 
},
{
 "name": "simplex",
"mean": 0.00533707027585,
"sd": 0.00854868449403 
},
{
 "name": "simplex",
"mean": 0.004916588689601,
"sd": 0.02417110412031 
},
{
 "name": "simplex",
"mean": 0.004916585365857,
"sd": 0.02417128254862 
},
{
 "name": "simplex",
"mean": 0.005640551428604,
"sd": 0.009309901341207 
},
{
 "name": "simplex",
"mean": 0.006487317163729,
"sd": 0.01821975186693 
},
{
 "name": "simplex",
"mean": 0.006492682926829,
"sd": 0.01580118185956 
},
{
 "name": "simplex",
"mean": 0.006487317073171,
"sd": 0.01821975159432 
},
{
 "name": "simplex",
"mean": 0.006492682937815,
"sd": 0.01580118197793 
},
{
 "name": "simplex",
"mean": 0.004931831653282,
"sd": 0.0119045973787 
},
{
 "name": "simplex",
"mean": 0.006492682905745,
"sd": 0.01580118182014 
},
{
 "name": "simplex",
"mean": 0.004887804932515,
"sd": 0.01278647411799 
},
{
 "name": "simplex",
"mean": 0.007314988171906,
"sd": 0.03551781364893 
},
{
 "name": "simplex",
"mean": 0.006487317073171,
"sd": 0.01821975159432 
},
{
 "name": "simplex",
"mean": 0.006492682886244,
"sd": 0.01580118178368 
},
{
 "name": "simplex",
"mean": 0.005337072399934,
"sd": 0.008548706688957 
},
{
 "name": "simplex",
"mean": 0.005204885442519,
"sd": 0.008327336375534 
},
{
 "name": "simplex",
"mean": 0.006492683165615,
"sd": 0.01580118444941 
},
{
 "name": "simplex",
"mean": 0.006487317073171,
"sd": 0.01821975159432 
},
{
 "name": "simplex",
"mean": 0.004887804878073,
"sd": 0.01278647407142 
},
{
 "name": "simplex",
"mean": 0.006487316775691,
"sd": 0.0182197480056 
},
{
 "name": "simplex",
"mean": 0.004887805333485,
"sd": 0.01278646472166 
},
{
 "name": "simplex",
"mean": 0.005337073170731,
"sd": 0.008548712276577 
},
{
 "name": "simplex",
"mean": 0.004916585365889,
"sd": 0.02417128254832 
},
{
 "name": "simplex",
"mean": 0.007276094394188,
"sd": 0.03496033897359 
},
{
 "name": "simplex",
"mean": 0.0061763966296,
"sd": 0.01529665150503 
},
{
 "name": "simplex",
"mean": 0.006487102441223,
"sd": 0.01821716157167 
},
{
 "name": "simplex",
"mean": 0.004916586529826,
"sd": 0.02417126436106 
},
{
 "name": "simplex",
"mean": 0.005337073213473,
"sd": 0.008548712205635 
},
{
 "name": "simplex",
"mean": 0.004913844849836,
"sd": 0.02191937722473 
},
{
 "name": "simplex",
"mean": 0.004920117848388,
"sd": 0.02411785113253 
},
{
 "name": "simplex",
"mean": 0.004916585365854,
"sd": 0.02417128254867 
},
{
 "name": "simplex",
"mean": 0.004916585423207,
"sd": 0.02417128200572 
},
{
 "name": "simplex",
"mean": 0.006424359426031,
"sd": 0.02184496836794 
},
{
 "name": "simplex",
"mean": 0.006492473895118,
"sd": 0.01579986565385 
},
{
 "name": "simplex",
"mean": 0.00648731755044,
"sd": 0.01821869462525 
},
{
 "name": "simplex",
"mean": 0.00731512195122,
"sd": 0.0355193970152 
},
{
 "name": "simplex",
"mean": 0.005379944948924,
"sd": 0.02094651269768 
},
{
 "name": "simplex",
"mean": 0.007304966631583,
"sd": 0.03537683563122 
},
{
 "name": "simplex",
"mean": 0.004974042183111,
"sd": 0.01892225635647 
},
{
 "name": "simplex",
"mean": 0.007315121898611,
"sd": 0.03551939543305 
},
{
 "name": "simplex",
"mean": 0.007314031499378,
"sd": 0.03550371661424 
},
{
 "name": "simplex",
"mean": 0.007315121949038,
"sd": 0.0355193969496 
},
{
 "name": "simplex",
"mean": 0.006430882145548,
"sd": 0.01743457819698 
},
{
 "name": "simplex",
"mean": 0.006492682926829,
"sd": 0.01580118185956 
},
{
 "name": "simplex",
"mean": 0.005523983636542,
"sd": 0.01881509000727 
},
{
 "name": "simplex",
"mean": 0.006492682926829,
"sd": 0.01580118185956 
},
{
 "name": "simplex",
"mean": 0.004917335752328,
"sd": 0.02416419445977 
},
{
 "name": "simplex",
"mean": 0.007315121605186,
"sd": 0.03551938668695 
},
{
 "name": "simplex",
"mean": 0.006334145328638,
"sd": 0.01481642468984 
},
{
 "name": "simplex",
"mean": 0.006492682926829,
"sd": 0.01580118185956 
},
{
 "name": "simplex",
"mean": 0.005337068334642,
"sd": 0.008548665893703 
},
{
 "name": "simplex",
"mean": 0.006487316034273,
"sd": 0.0182197390613 
},
{
 "name": "simplex",
"mean": 0.004916585365875,
"sd": 0.02417128254751 
},
{
 "name": "simplex",
"mean": 0.006492647315996,
"sd": 0.01580088367682 
},
{
 "name": "simplex",
"mean": 0.006492683055926,
"sd": 0.01580118325056 
},
{
 "name": "simplex",
"mean": 0.006487979667866,
"sd": 0.0158040174595 
},
{
 "name": "simplex",
"mean": 0.005337245766548,
"sd": 0.008549633535966 
},
{
 "name": "simplex",
"mean": 0.004922401552458,
"sd": 0.01281849892634 
},
{
 "name": "simplex",
"mean": 0.004887804878049,
"sd": 0.01278647407135 
},
{
 "name": "simplex",
"mean": 0.006525676368807,
"sd": 0.01838391748997 
},
{
 "name": "simplex",
"mean": 0.007298287928716,
"sd": 0.03501442698666 
},
{
 "name": "simplex",
"mean": 0.004887804892849,
"sd": 0.01278647401699 
},
{
 "name": "simplex",
"mean": 0.004916585673054,
"sd": 0.02417127925183 
},
{
 "name": "simplex",
"mean": 0.004887804878049,
"sd": 0.01278647407135 
},
{
 "name": "simplex",
"mean": 0.006487317085575,
"sd": 0.01821972421046 
},
{
 "name": "simplex",
"mean": 0.00648731709017,
"sd": 0.0182197516455 
},
{
 "name": "simplex",
"mean": 0.006492682915076,
"sd": 0.01580116330845 
},
{
 "name": "simplex",
"mean": 0.005342134505125,
"sd": 0.008552108456738 
},
{
 "name": "simplex",
"mean": 0.007315121950284,
"sd": 0.03551939700166 
},
{
 "name": "simplex",
"mean": 0.006492542260699,
"sd": 0.0158009152724 
},
{
 "name": "simplex",
"mean": 0.006487360307835,
"sd": 0.01812455600267 
},
{
 "name": "simplex",
"mean": 0.004916585362072,
"sd": 0.02417127937767 
},
{
 "name": "simplex",
"mean": 0.006497898372864,
"sd": 0.01585789286017 
},
{
 "name": "simplex",
"mean": 0.006487317067815,
"sd": 0.01821975155318 
},
{
 "name": "simplex",
"mean": 0.007299235542946,
"sd": 0.0350459136325 
},
{
 "name": "simplex",
"mean": 0.00648731707319,
"sd": 0.01821975144065 
},
{
 "name": "simplex",
"mean": 0.004887804878049,
"sd": 0.01278647407135 
},
{
 "name": "simplex",
"mean": 0.004916585287884,
"sd": 0.02417121767915 
},
{
 "name": "simplex",
"mean": 0.006492682926829,
"sd": 0.01580118185954 
},
{
 "name": "simplex",
"mean": 0.004887810581615,
"sd": 0.01278635663507 
},
{
 "name": "simplex",
"mean": 0.005337073170732,
"sd": 0.00854871227658 
},
{
 "name": "simplex",
"mean": 0.004916585365854,
"sd": 0.02417128254867 
},
{
 "name": "simplex",
"mean": 0.006479360784085,
"sd": 0.01577722357834 
},
{
 "name": "simplex",
"mean": 0.00648731707322,
"sd": 0.01821975159447 
},
{
 "name": "simplex",
"mean": 0.007310480565984,
"sd": 0.03538094382559 
},
{
 "name": "simplex",
"mean": 0.00731512195122,
"sd": 0.0355193970152 
},
{
 "name": "simplex",
"mean": 0.004916585365854,
"sd": 0.02417128254867 
},
{
 "name": "simplex",
"mean": 0.007315121745292,
"sd": 0.03551939082217 
},
{
 "name": "simplex",
"mean": 0.005337073148609,
"sd": 0.008548712064274 
},
{
 "name": "simplex",
"mean": 0.004887805199102,
"sd": 0.01278646746233 
},
{
 "name": "simplex",
"mean": 0.007314947668883,
"sd": 0.03551669234017 
},
{
 "name": "simplex",
"mean": 0.006492057500792,
"sd": 0.01579706421212 
},
{
 "name": "simplex",
"mean": 0.004916585365854,
"sd": 0.02417128254867 
},
{
 "name": "simplex",
"mean": 0.005198959816541,
"sd": 0.01324224399945 
},
{
 "name": "simplex",
"mean": 0.006487317078455,
"sd": 0.01821973991283 
},
{
 "name": "simplex",
"mean": 0.006487310957184,
"sd": 0.01821967781265 
},
{
 "name": "simplex",
"mean": 0.004919445796116,
"sd": 0.02414267746576 
},
{
 "name": "simplex",
"mean": 0.004887808402896,
"sd": 0.01278640189147 
},
{
 "name": "simplex",
"mean": 0.004916585365854,
"sd": 0.02417128254867 
},
{
 "name": "simplex",
"mean": 0.006487316927668,
"sd": 0.01821974986083 
},
{
 "name": "simplex",
"mean": 0.00491658536586,
"sd": 0.02417128254858 
},
{
 "name": "simplex",
"mean": 0.005365078007677,
"sd": 0.008708454237682 
},
{
 "name": "simplex",
"mean": 0.005337072583225,
"sd": 0.008548706852685 
},
{
 "name": "simplex",
"mean": 0.007315121951219,
"sd": 0.03551939701519 
},
{
 "name": "simplex",
"mean": 0.00647654642118,
"sd": 0.01816402386198 
},
{
 "name": "simplex",
"mean": 0.005337073170732,
"sd": 0.00854871227658 
},
{
 "name": "simplex",
"mean": 0.005337073170732,
"sd": 0.00854871227658 
},
{
 "name": "simplex",
"mean": 0.005337073186101,
"sd": 0.008548712285953 
},
{
 "name": "simplex",
"mean": 0.006492682926829,
"sd": 0.01580118185956 
},
{
 "name": "simplex",
"mean": 0.006487314270372,
"sd": 0.01821971820226 
},
{
 "name": "simplex",
"mean": 0.004887804878049,
"sd": 0.01278647407132 
},
{
 "name": "simplex",
"mean": 0.006488047087129,
"sd": 0.01822195927554 
},
{
 "name": "simplex",
"mean": 0.006492682926829,
"sd": 0.01580118185956 
},
{
 "name": "simplex",
"mean": 0.005337073170732,
"sd": 0.00854871227658 
},
{
 "name": "simplex",
"mean": 0.006492682926829,
"sd": 0.01580118185956 
},
{
 "name": "simplex",
"mean": 0.007131911280743,
"sd": 0.03293628765082 
},
{
 "name": "simplex",
"mean": 0.00731512195122,
"sd": 0.0355193970152 
},
{
 "name": "simplex",
"mean": 0.00649268292565,
"sd": 0.01580118185735 
},
{
 "name": "simplex",
"mean": 0.006492682926829,
"sd": 0.01580118185955 
},
{
 "name": "simplex",
"mean": 0.004916585365863,
"sd": 0.02417128254858 
},
{
 "name": "simplex",
"mean": 0.004950479124654,
"sd": 0.02279666559782 
},
{
 "name": "simplex",
"mean": 0.004986599897922,
"sd": 0.02046038820633 
},
{
 "name": "simplex",
"mean": 0.006487317073479,
"sd": 0.01821975159525 
},
{
 "name": "simplex",
"mean": 0.006492682926829,
"sd": 0.01580118185956 
},
{
 "name": "simplex",
"mean": 0.006487317073171,
"sd": 0.01821975159432 
},
{
 "name": "simplex",
"mean": 0.005337073197126,
"sd": 0.008548712417403 
},
{
 "name": "simplex",
"mean": 0.005337073795757,
"sd": 0.00854871559995 
},
{
 "name": "simplex",
"mean": 0.004921376758633,
"sd": 0.02398690667332 
},
{
 "name": "simplex",
"mean": 0.006492679488934,
"sd": 0.01580115995209 
},
{
 "name": "simplex",
"mean": 0.004916585369846,
"sd": 0.0241712823324 
},
{
 "name": "simplex",
"mean": 0.005337073437885,
"sd": 0.008548712375358 
},
{
 "name": "simplex",
"mean": 0.006487317073171,
"sd": 0.01821975159432 
},
{
 "name": "simplex",
"mean": 0.006492682926829,
"sd": 0.01580118185956 
},
{
 "name": "simplex",
"mean": 0.00731512193222,
"sd": 0.03551939679007 
},
{
 "name": "simplex",
"mean": 0.005337073457116,
"sd": 0.008548711531205 
},
{
 "name": "simplex",
"mean": 0.004967160065185,
"sd": 0.01123748159578 
},
{
 "name": "simplex",
"mean": 0.0064926452395,
"sd": 0.01580111140705 
},
{
 "name": "simplex",
"mean": 0.005336883364044,
"sd": 0.008547337749865 
},
{
 "name": "simplex",
"mean": 0.00648731707314,
"sd": 0.01821975159416 
},
{
 "name": "simplex",
"mean": 0.006492682926815,
"sd": 0.01580118185942 
},
{
 "name": "simplex",
"mean": 0.006492858985038,
"sd": 0.01580307983807 
},
{
 "name": "simplex",
"mean": 0.005337073170732,
"sd": 0.008548712276581 
},
{
 "name": "simplex",
"mean": 0.006492670390333,
"sd": 0.01580115842221 
},
{
 "name": "simplex",
"mean": 0.004887804878049,
"sd": 0.01278647407135 
},
{
 "name": "simplex",
"mean": 0.007315104820346,
"sd": 0.03551888610853 
},
{
 "name": "simplex",
"mean": 0.005303897293028,
"sd": 0.008330881947745 
},
{
 "name": "simplex",
"mean": 0.006482385092311,
"sd": 0.01819422190968 
},
{
 "name": "simplex",
"mean": 0.006492682926831,
"sd": 0.01580118185957 
},
{
 "name": "simplex",
"mean": 0.004916585365854,
"sd": 0.02417128254867 
},
{
 "name": "simplex",
"mean": 0.00648727497743,
"sd": 0.01821925009077 
},
{
 "name": "simplex",
"mean": 0.004887804878049,
"sd": 0.01278647407135 
},
{
 "name": "simplex",
"mean": 0.005338812671728,
"sd": 0.008549387697123 
},
{
 "name": "simplex",
"mean": 0.004887804878049,
"sd": 0.01278647407135 
},
{
 "name": "simplex",
"mean": 0.006487317073171,
"sd": 0.01821975159432 
},
{
 "name": "simplex",
"mean": 0.005337073170732,
"sd": 0.00854871227658 
},
{
 "name": "simplex",
"mean": 0.004888738090251,
"sd": 0.01247582788049 
},
{
 "name": "simplex",
"mean": 0.007192910324841,
"sd": 0.03361235828197 
},
{
 "name": "simplex",
"mean": 0.004887806305151,
"sd": 0.01278644469364 
},
{
 "name": "simplex",
"mean": 0.00731512195122,
"sd": 0.0355193970152 
},
{
 "name": "simplex",
"mean": 0.005337073170732,
"sd": 0.00854871227658 
},
{
 "name": "simplex",
"mean": 0.004916879320967,
"sd": 0.02416834180415 
},
{
 "name": "simplex",
"mean": 0.006492682926829,
"sd": 0.01580118185956 
},
{
 "name": "simplex",
"mean": 0.006487317073171,
"sd": 0.01821975159432 
},
{
 "name": "simplex",
"mean": 0.006492716182763,
"sd": 0.01580154022211 
},
{
 "name": "simplex",
"mean": 0.006492682926829,
"sd": 0.01580118185956 
},
{
 "name": "simplex",
"mean": 0.00648731707317,
"sd": 0.01821975159431 
},
{
 "name": "simplex",
"mean": 0.004887804878049,
"sd": 0.01278647407135 
},
{
 "name": "simplex",
"mean": 0.006487317073162,
"sd": 0.01821975159156 
},
{
 "name": "simplex",
"mean": 0.004916585674112,
"sd": 0.02417126607562 
},
{
 "name": "simplex",
"mean": 0.006492667972483,
"sd": 0.01580104958345 
},
{
 "name": "simplex",
"mean": 0.006490117674391,
"sd": 0.01577689356044 
},
{
 "name": "simplex",
"mean": 0.004921572267823,
"sd": 0.02412142938314 
},
{
 "name": "simplex",
"mean": 0.004916582690838,
"sd": 0.0241690134855 
},
{
 "name": "simplex",
"mean": 0.006480397844216,
"sd": 0.0181363336683 
},
{
 "name": "simplex",
"mean": 0.006454085718393,
"sd": 0.01804839504979 
},
{
 "name": "simplex",
"mean": 0.006487333027658,
"sd":  0.01821979963 
},
{
 "name": "simplex",
"mean": 0.004916826403203,
"sd": 0.0241688711776 
},
{
 "name": "simplex",
"mean": 0.004936274017435,
"sd": 0.02386473043922 
},
{
 "name": "simplex",
"mean": 0.005873363564667,
"sd": 0.01442625263083 
},
{
 "name": "simplex",
"mean": 0.00731512195122,
"sd": 0.0355193970152 
},
{
 "name": "simplex",
"mean": 0.007315111302679,
"sd": 0.03551927098233 
},
{
 "name": "simplex",
"mean": 0.005349680195985,
"sd": 0.008558129994628 
},
{
 "name": "simplex",
"mean": 0.005337079870512,
"sd": 0.008548716403636 
},
{
 "name": "grid",
"mean": 0.006422443902439,
"sd": 0.01803755407838 
},
{
 "name": "grid",
"mean": 0.006422443902439,
"sd": 0.01803755407838 
},
{
 "name": "grid",
"mean": 0.006487317073171,
"sd": 0.01821975159432 
},
{
 "name": "grid",
"mean": 0.005283702439024,
"sd": 0.008463225153815 
},
{
 "name": "grid",
"mean": 0.005853073170732,
"sd": 0.0117484442904 
},
{
 "name": "grid",
"mean": 0.006103902439024,
"sd": 0.01381410569985 
},
{
 "name": "grid",
"mean": 0.006261753658537,
"sd": 0.01501374485399 
},
{
 "name": "grid",
"mean": 0.005283702439024,
"sd": 0.008463225153815 
},
{
 "name": "grid",
"mean": 0.005720487804878,
"sd": 0.01022431223912 
},
{
 "name": "grid",
"mean": 0.005971317073171,
"sd": 0.0119857865993 
},
{
 "name": "grid",
"mean": 0.005337073170732,
"sd": 0.00854871227658 
},
{
 "name": "grid",
"mean": 0.005680880487805,
"sd": 0.009662886144739 
},
{
 "name": "grid",
"mean": 0.005971317073171,
"sd": 0.0119857865993 
},
{
 "name": "grid",
"mean": 0.004838926829268,
"sd": 0.01265860933064 
},
{
 "name": "grid",
"mean": 0.005630685365854,
"sd": 0.01449484589609 
},
{
 "name": "grid",
"mean": 0.005954146341463,
"sd": 0.0156891908168 
},
{
 "name": "grid",
"mean": 0.006148313414634,
"sd": 0.01643611244905 
},
{
 "name": "grid",
"mean": 0.005061314634146,
"sd": 0.008951867652229 
},
{
 "name": "grid",
"mean": 0.005570731707317,
"sd": 0.01144696874357 
},
{
 "name": "grid",
"mean": 0.005857876829268,
"sd": 0.01310925507034 
},
{
 "name": "grid",
"mean": 0.005187317073171,
"sd": 0.008411441891538 
},
{
 "name": "grid",
"mean": 0.005567440243902,
"sd": 0.01024817481017 
},
{
 "name": "grid",
"mean": 0.005764390243902,
"sd": 0.01164215779135 
},
{
 "name": "grid",
"mean": 0.005895690243902,
"sd": 0.01266431334829 
},
{
 "name": "grid",
"mean": 0.005277003658537,
"sd": 0.008346457198086 
},
{
 "name": "grid",
"mean": 0.005702065853659,
"sd": 0.01077090938656 
},
{
 "name": "grid",
"mean": 0.005823512195122,
"sd": 0.01171364355193 
},
{
 "name": "grid",
"mean": 0.004838926829268,
"sd": 0.01265860933064 
},
{
 "name": "grid",
"mean": 0.005420975609756,
"sd": 0.01378388789465 
},
{
 "name": "grid",
"mean": 0.005744436585366,
"sd": 0.0147876710657 
},
{
 "name": "grid",
"mean": 0.00503756097561,
"sd": 0.01004440974994 
},
{
 "name": "grid",
"mean":       0.005454,
"sd": 0.01163219515768 
},
{
 "name": "grid",
"mean": 0.00567363804878,
"sd": 0.01277631524314 
},
{
 "name": "grid",
"mean": 0.005820063414634,
"sd": 0.01361580739306 
},
{
 "name": "grid",
"mean": 0.005163563414634,
"sd": 0.009132713463385 
},
{
 "name": "grid",
"mean": 0.005441288780488,
"sd": 0.01045872528613 
},
{
 "name": "grid",
"mean": 0.00562643902439,
"sd": 0.011561438431 
},
{
 "name": "grid",
"mean": 0.005432814634146,
"sd": 0.009794298372707 
},
{
 "name": "grid",
"mean": 0.005712658536585,
"sd": 0.01159765536313 
},
{
 "name": "grid",
"mean": 0.004887804878049,
"sd": 0.01278647407135 
},
{
 "name": "grid",
"mean": 0.005340559756098,
"sd": 0.01357276686552 
},
{
 "name": "grid",
"mean": 0.005744436585366,
"sd": 0.0147876710657 
},
{
 "name": "grid",
"mean": 0.005050123170732,
"sd": 0.01076005211893 
},
{
 "name": "grid",
"mean": 0.005550812195122,
"sd": 0.01263332317229 
},
{
 "name": "grid",
"mean": 0.005693866202091,
"sd": 0.01333103755828 
},
{
 "name": "grid",
"mean": 0.005357187804878,
"sd": 0.01070099376668 
},
{
 "name": "grid",
"mean": 0.005655938414634,
"sd": 0.01230663829511 
},
{
 "name": "grid",
"mean": 0.005163563414634,
"sd": 0.009132713463385 
},
{
 "name": "grid",
"mean": 0.005361938675958,
"sd": 0.01003412203416 
},
{
 "name": "grid",
"mean": 0.005510720121951,
"sd": 0.01085531578048 
},
{
 "name": "grid",
"mean": 0.00562643902439,
"sd": 0.011561438431 
},
{
 "name": "grid",
"mean": 0.004867419512195,
"sd": 0.02392956972319 
},
{
 "name": "grid",
"mean": 0.005644931707317,
"sd": 0.01483530841514 
},
{
 "name": "grid",
"mean": 0.005963739837398,
"sd": 0.01443802064666 
},
{
 "name": "grid",
"mean": 0.006155580487805,
"sd": 0.01497628253952 
},
{
 "name": "grid",
"mean": 0.00507556097561,
"sd": 0.01341563035442 
},
{
 "name": "grid",
"mean": 0.005580325203252,
"sd": 0.01157956457721 
},
{
 "name": "grid",
"mean": 0.005865143902439,
"sd": 0.01222198754646 
},
{
 "name": "grid",
"mean": 0.005196910569106,
"sd": 0.01070294668541 
},
{
 "name": "grid",
"mean": 0.005574707317073,
"sd": 0.01029226228033 
},
{
 "name": "grid",
"mean": 0.005770203902439,
"sd": 0.01098243035673 
},
{
 "name": "grid",
"mean": 0.00590053495935,
"sd": 0.01180049632146 
},
{
 "name": "grid",
"mean": 0.005284270731707,
"sd": 0.009692785383347 
},
{
 "name": "grid",
"mean": 0.005706910569106,
"sd": 0.01026016562594 
},
{
 "name": "grid",
"mean": 0.005827664808362,
"sd": 0.01101847096094 
},
{
 "name": "grid",
"mean": 0.004853173170732,
"sd": 0.01363530851133 
},
{
 "name": "grid",
"mean": 0.005430569105691,
"sd": 0.01262611102776 
},
{
 "name": "grid",
"mean": 0.005751703658537,
"sd": 0.01330046842661 
},
{
 "name": "grid",
"mean": 0.005047154471545,
"sd": 0.01053853747405 
},
{
 "name": "grid",
"mean": 0.005461267073171,
"sd": 0.01081278751418 
},
{
 "name": "grid",
"mean": 0.005679451707317,
"sd": 0.01166010219865 
},
{
 "name": "grid",
"mean": 0.005824908130081,
"sd": 0.01247720214925 
},
{
 "name": "grid",
"mean": 0.005170830487805,
"sd": 0.009401916276253 
},
{
 "name": "grid",
"mean": 0.005447102439024,
"sd": 0.00985254445242 
},
{
 "name": "grid",
"mean": 0.005631283739837,
"sd": 0.01069340216729 
},
{
 "name": "grid",
"mean": 0.005437659349593,
"sd": 0.009327480942906 
},
{
 "name": "grid",
"mean": 0.005716292073171,
"sd": 0.01083377809188 
},
{
 "name": "grid",
"mean": 0.004897398373984,
"sd": 0.0118336138122 
},
{
 "name": "grid",
"mean": 0.005347826829268,
"sd": 0.01210500903592 
},
{
 "name": "grid",
"mean": 0.005588699512195,
"sd": 0.01277594817442 
},
{
 "name": "grid",
"mean": 0.005749281300813,
"sd": 0.01343089943804 
},
{
 "name": "grid",
"mean": 0.005057390243902,
"sd": 0.01007328793377 
},
{
 "name": "grid",
"mean": 0.005356350243902,
"sd": 0.01066555427469 
},
{
 "name": "grid",
"mean": 0.005555656910569,
"sd": 0.01147633771516 
},
{
 "name": "grid",
"mean": 0.00512400097561,
"sd": 0.009188107495007 
},
{
 "name": "grid",
"mean": 0.005362032520325,
"sd": 0.00984928268355 
},
{
 "name": "grid",
"mean": 0.005532055052265,
"sd": 0.01064389551407 
},
{
 "name": "grid",
"mean": 0.00565957195122,
"sd": 0.01137955150179 
},
{
 "name": "grid",
"mean": 0.005168408130081,
"sd": 0.008734711546433 
},
{
 "name": "grid",
"mean": 0.005514353658537,
"sd": 0.01008583254642 
},
{
 "name": "grid",
"mean": 0.005629668834688,
"sd": 0.01077725519443 
},
{
 "name": "grid",
"mean":     0.00494395,
"sd": 0.01154016416178 
},
{
 "name": "grid",
"mean": 0.005480030081301,
"sd": 0.01254366098851 
},
{
 "name": "grid",
"mean": 0.005633195818815,
"sd": 0.01312968078309 
},
{
 "name": "grid",
"mean": 0.005286405691057,
"sd": 0.01073752598237 
},
{
 "name": "grid",
"mean": 0.005602851829268,
"sd": 0.01209441717314 
},
{
 "name": "grid",
"mean": 0.005092781300813,
"sd": 0.009339714762248 
},
{
 "name": "grid",
"mean": 0.005457633536585,
"sd": 0.01069438217384 
},
{
 "name": "grid",
"mean": 0.005579250948509,
"sd": 0.01134490262854 
},
{
 "name": "grid",
"mean": 0.005135304529617,
"sd": 0.008885530276143 
},
{
 "name": "grid",
"mean": 0.005312415243902,
"sd": 0.009504268988031 
},
{
 "name": "grid",
"mean": 0.00545016802168,
"sd": 0.01016588837797 
},
{
 "name": "grid",
"mean": 0.005560370243902,
"sd":  0.01078795978 
},
{
 "name": "grid",
"mean": 0.004867419512195,
"sd": 0.02392956972319 
},
{
 "name": "grid",
"mean": 0.005440162601626,
"sd": 0.01710419902119 
},
{
 "name": "grid",
"mean": 0.005758970731707,
"sd": 0.01513501161544 
},
{
 "name": "grid",
"mean": 0.00505674796748,
"sd": 0.01688026087587 
},
{
 "name": "grid",
"mean": 0.005468534146341,
"sd": 0.01387312707647 
},
{
 "name": "grid",
"mean": 0.005685265365854,
"sd": 0.01299240597726 
},
{
 "name": "grid",
"mean": 0.005829752845528,
"sd": 0.01295073532654 
},
{
 "name": "grid",
"mean": 0.005178097560976,
"sd": 0.01368665318986 
},
{
 "name": "grid",
"mean": 0.005452916097561,
"sd": 0.01203666097666 
},
{
 "name": "grid",
"mean": 0.005636128455285,
"sd": 0.01169536022299 
},
{
 "name": "grid",
"mean": 0.005442504065041,
"sd": 0.01094651224745 
},
{
 "name": "grid",
"mean": 0.005719925609756,
"sd": 0.01112297637411 
},
{
 "name": "grid",
"mean": 0.004906991869919,
"sd": 0.01674176641854 
},
{
 "name": "grid",
"mean": 0.005355093902439,
"sd": 0.01424013197081 
},
{
 "name": "grid",
"mean": 0.005594513170732,
"sd": 0.01355430909857 
},
{
 "name": "grid",
"mean": 0.00575412601626,
"sd": 0.01355925932339 
},
{
 "name": "grid",
"mean": 0.005064657317073,
"sd": 0.01345742952438 
},
{
 "name": "grid",
"mean": 0.005362163902439,
"sd": 0.01221525426898 
},
{
 "name": "grid",
"mean": 0.005560501626016,
"sd": 0.01206492477579 
},
{
 "name": "grid",
"mean": 0.005129814634146,
"sd": 0.01161222001576 
},
{
 "name": "grid",
"mean": 0.005366877235772,
"sd": 0.0110116338789 
},
{
 "name": "grid",
"mean": 0.005536207665505,
"sd": 0.01109937480214 
},
{
 "name": "grid",
"mean": 0.005663205487805,
"sd": 0.01144638617601 
},
{
 "name": "grid",
"mean": 0.005173252845528,
"sd": 0.01053254603176 
},
{
 "name": "grid",
"mean": 0.005517987195122,
"sd": 0.01044484913572 
},
{
 "name": "grid",
"mean": 0.005632898644986,
"sd": 0.01082531417141 
},
{
 "name": "grid",
"mean": 0.004951217073171,
"sd": 0.01391076928933 
},
{
 "name": "grid",
"mean": 0.005271411707317,
"sd": 0.01286337156326 
},
{
 "name": "grid",
"mean": 0.005484874796748,
"sd": 0.01275237213804 
},
{
 "name": "grid",
"mean": 0.005039062439024,
"sd": 0.0118537796563 
},
{
 "name": "grid",
"mean": 0.005291250406504,
"sd": 0.01144402277102 
},
{
 "name": "grid",
"mean": 0.00547138466899,
"sd": 0.01160048246344 
},
{
 "name": "grid",
"mean": 0.005606485365854,
"sd": 0.01195726554442 
},
{
 "name": "grid",
"mean": 0.00509762601626,
"sd": 0.01064392284879 
},
{
 "name": "grid",
"mean": 0.005305420905923,
"sd": 0.01053852787265 
},
{
 "name": "grid",
"mean": 0.005461267073171,
"sd": 0.01081278751418 
},
{
 "name": "grid",
"mean": 0.005582480758808,
"sd": 0.01122199746505 
},
{
 "name": "grid",
"mean": 0.005316048780488,
"sd": 0.009935944039139 
},
{
 "name": "grid",
"mean": 0.005453397831978,
"sd": 0.01025622736005 
},
{
 "name": "grid",
"mean": 0.005215623577236,
"sd": 0.01220470646455 
},
{
 "name": "grid",
"mean": 0.005549765243902,
"sd": 0.01263289087452 
},
{
 "name": "grid",
"mean": 0.005021999186992,
"sd": 0.01113244301635 
},
{
 "name": "grid",
"mean": 0.00540454695122,
"sd": 0.0113751995114 
},
{
 "name": "grid",
"mean": 0.005532062872629,
"sd": 0.01176260435315 
},
{
 "name": "grid",
"mean": 0.005259328658537,
"sd": 0.01034731035563 
},
{
 "name": "grid",
"mean": 0.005402979945799,
"sd": 0.0106933960322 
},
{
 "name": "grid",
"mean": 0.005114110365854,
"sd": 0.009623145788258 
},
{
 "name": "grid",
"mean": 0.00527389701897,
"sd": 0.009826191099257 
},
{
 "name": "grid",
"mean": 0.005506313968958,
"sd": 0.01061769472075 
},
{
 "name": "grid",
"mean": 0.004916585365854,
"sd": 0.02417128254867 
},
{
 "name": "grid",
"mean": 0.00536236097561,
"sd": 0.01878750501649 
},
{
 "name": "grid",
"mean": 0.005758970731707,
"sd": 0.01513501161544 
},
{
 "name": "grid",
"mean": 0.005071924390244,
"sd": 0.01883275233266 
},
{
 "name": "grid",
"mean": 0.005565346341463,
"sd": 0.01418340938757 
},
{
 "name": "grid",
"mean": 0.005706324041812,
"sd": 0.01358393875282 
},
{
 "name": "grid",
"mean": 0.00537172195122,
"sd": 0.01368409300985 
},
{
 "name": "grid",
"mean": 0.00566683902439,
"sd": 0.01249119843937 
},
{
 "name": "grid",
"mean": 0.005178097560976,
"sd": 0.01368665318986 
},
{
 "name": "grid",
"mean": 0.005374396515679,
"sd": 0.0123813743599 
},
{
 "name": "grid",
"mean": 0.005521620731707,
"sd": 0.01183006536899 
},
{
 "name": "grid",
"mean": 0.005636128455285,
"sd": 0.01169536022299 
},
{
 "name": "grid",
"mean": 0.004958484146341,
"sd": 0.01864892723386 
},
{
 "name": "grid",
"mean": 0.005489719512195,
"sd": 0.01447940498834 
},
{
 "name": "grid",
"mean": 0.005641501045296,
"sd": 0.01396417165716 
},
{
 "name": "grid",
"mean": 0.005296095121951,
"sd": 0.01372543112846 
},
{
 "name": "grid",
"mean": 0.005610118902439,
"sd": 0.01277349559418 
},
{
 "name": "grid",
"mean": 0.005102470731707,
"sd": 0.01345758243143 
},
{
 "name": "grid",
"mean": 0.005464900609756,
"sd": 0.01195607190623 
},
{
 "name": "grid",
"mean": 0.005585710569106,
"sd": 0.01190443047827 
},
{
 "name": "grid",
"mean": 0.005143609756098,
"sd": 0.01213961355548 
},
{
 "name": "grid",
"mean": 0.005319682317073,
"sd": 0.01142801630879 
},
{
 "name": "grid",
"mean": 0.005456627642276,
"sd": 0.01120673633403 
},
{
 "name": "grid",
"mean": 0.005566183902439,
"sd": 0.01125372115793 
},
{
 "name": "grid",
"mean": 0.005220468292683,
"sd": 0.01406414391363 
},
{
 "name": "grid",
"mean": 0.005553398780488,
"sd": 0.01322692824036 
},
{
 "name": "grid",
"mean": 0.005026843902439,
"sd": 0.01353396469846 
},
{
 "name": "grid",
"mean": 0.005408180487805,
"sd": 0.0122720069246 
},
{
 "name": "grid",
"mean": 0.005535292682927,
"sd": 0.012260885496 
},
{
 "name": "grid",
"mean": 0.005262962195122,
"sd": 0.0115809806508 
},
{
 "name": "grid",
"mean": 0.005406209756098,
"sd": 0.01144280002323 
},
{
 "name": "grid",
"mean": 0.005117743902439,
"sd": 0.01120279015718 
},
{
 "name": "grid",
"mean": 0.005277126829268,
"sd": 0.01085179648179 
},
{
 "name": "grid",
"mean": 0.00550895654102,
"sd": 0.01097838250125 
},
{
 "name": "grid",
"mean": 0.004951217073171,
"sd": 0.01391076928933 
},
{
 "name": "grid",
"mean": 0.005179927526132,
"sd": 0.01306013208997 
},
{
 "name": "grid",
"mean": 0.005351460365854,
"sd": 0.01276377460731 
},
{
 "name": "grid",
"mean": 0.005484874796748,
"sd": 0.01275237213804 
},
{
 "name": "grid",
"mean": 0.005013963763066,
"sd": 0.0124162852099 
},
{
 "name": "grid",
"mean": 0.005206242073171,
"sd": 0.01192878697323 
},
{
 "name": "grid",
"mean": 0.005355791869919,
"sd": 0.01183064830643 
},
{
 "name": "grid",
"mean": 0.005475431707317,
"sd": 0.01193504456762 
},
{
 "name": "grid",
"mean": 0.005061023780488,
"sd": 0.01138175780245 
},
{
 "name": "grid",
"mean": 0.005226708943089,
"sd": 0.01111399742839 
},
{
 "name": "grid",
"mean": 0.005467705543237,
"sd": 0.01130555847997 
},
{
 "name": "grid",
"mean": 0.00509762601626,
"sd": 0.01064392284879 
},
{
 "name": "grid",
"mean": 0.005243082439024,
"sd": 0.01051311064175 
},
{
 "name": "grid",
"mean": 0.005362092239468,
"sd": 0.01060392175728 
},
{
 "name": "grid",
"mean": 0.005461267073171,
"sd": 0.01081278751418 
},
{
 "name": "grid",
"mean": 0.007241970731707,
"sd": 0.03516420304505 
},
{
 "name": "grid",
"mean": 0.006832207317073,
"sd": 0.02398739311862 
},
{
 "name": "grid",
"mean": 0.00676325203252,
"sd": 0.02134092094039 
},
{
 "name": "grid",
"mean": 0.00676121097561,
"sd": 0.0203782291226 
},
{
 "name": "grid",
"mean": 0.006262836585366,
"sd": 0.0201757791468 
},
{
 "name": "grid",
"mean": 0.006379837398374,
"sd": 0.01796993206941 
},
{
 "name": "grid",
"mean": 0.006470774390244,
"sd": 0.01752128531683 
},
{
 "name": "grid",
"mean": 0.005996422764228,
"sd": 0.0156601049461 
},
{
 "name": "grid",
"mean": 0.006180337804878,
"sd": 0.01517182910903 
},
{
 "name": "grid",
"mean": 0.006254708292683,
"sd": 0.01522057885575 
},
{
 "name": "grid",
"mean": 0.006304288617886,
"sd": 0.01543590162315 
},
{
 "name": "grid",
"mean": 0.005889901219512,
"sd": 0.01359555442977 
},
{
 "name": "grid",
"mean": 0.006110664227642,
"sd": 0.01376435897454 
},
{
 "name": "grid",
"mean": 0.006173739372822,
"sd": 0.01411894920215 
},
{
 "name": "grid",
"mean": 0.006040448780488,
"sd": 0.02161911585369 
},
{
 "name": "grid",
"mean": 0.006230081300813,
"sd": 0.01931090794973 
},
{
 "name": "grid",
"mean": 0.006357334146341,
"sd": 0.0186727389285 
},
{
 "name": "grid",
"mean": 0.005846666666667,
"sd": 0.01632197704448 
},
{
 "name": "grid",
"mean": 0.006066897560976,
"sd": 0.01597874740554 
},
{
 "name": "grid",
"mean": 0.006163956097561,
"sd": 0.01600207465633 
},
{
 "name": "grid",
"mean": 0.006228661788618,
"sd": 0.01615502779414 
},
{
 "name": "grid",
"mean": 0.00577646097561,
"sd": 0.01390810197645 
},
{
 "name": "grid",
"mean": 0.005931606829268,
"sd": 0.0139938421784 
},
{
 "name": "grid",
"mean": 0.006035037398374,
"sd": 0.01431168869453 
},
{
 "name": "grid",
"mean": 0.00584141300813,
"sd": 0.01275338064749 
},
{
 "name": "grid",
"mean": 0.006019107317073,
"sd": 0.01358509404967 
},
{
 "name": "grid",
"mean": 0.005696910569106,
"sd": 0.01788968758199 
},
{
 "name": "grid",
"mean": 0.005953457317073,
"sd": 0.01729408852005 
},
{
 "name": "grid",
"mean": 0.006073203902439,
"sd": 0.01709939845841 
},
{
 "name": "grid",
"mean": 0.00615303495935,
"sd": 0.01708749289219 
},
{
 "name": "grid",
"mean": 0.005663020731707,
"sd": 0.01485465111926 
},
{
 "name": "grid",
"mean": 0.005840854634146,
"sd": 0.01488506636107 
},
{
 "name": "grid",
"mean": 0.005959410569106,
"sd": 0.0151152543743 
},
{
 "name": "grid",
"mean": 0.005608505365854,
"sd": 0.01307207132133 
},
{
 "name": "grid",
"mean": 0.005765786178862,
"sd": 0.01337700230431 
},
{
 "name": "grid",
"mean": 0.005878129616725,
"sd": 0.01376869339548 
},
{
 "name": "grid",
"mean": 0.005962387195122,
"sd": 0.01414976288725 
},
{
 "name": "grid",
"mean": 0.005572161788618,
"sd": 0.01197506746912 
},
{
 "name": "grid",
"mean": 0.005817168902439,
"sd": 0.01281032531056 
},
{
 "name": "grid",
"mean": 0.005898837940379,
"sd": 0.01323513868418 
},
{
 "name": "grid",
"mean": 0.005549580487805,
"sd": 0.01632529258897 
},
{
 "name": "grid",
"mean": 0.005883783739837,
"sd": 0.01613682194583 
},
{
 "name": "grid",
"mean": 0.005979270383275,
"sd": 0.01625386169742 
},
{
 "name": "grid",
"mean": 0.005690159349593,
"sd": 0.01426601870041 
},
{
 "name": "grid",
"mean": 0.005905667073171,
"sd": 0.01485039019456 
},
{
 "name": "grid",
"mean": 0.00549653495935,
"sd": 0.01267384436547 
},
{
 "name": "grid",
"mean": 0.005760448780488,
"sd": 0.01342712892905 
},
{
 "name": "grid",
"mean": 0.005848420054201,
"sd": 0.01380295602523 
},
{
 "name": "grid",
"mean": 0.005481379094077,
"sd": 0.01175024849663 
},
{
 "name": "grid",
"mean": 0.005615230487805,
"sd": 0.01216174038609 
},
{
 "name": "grid",
"mean": 0.005719337127371,
"sd": 0.01259243833222 
},
{
 "name": "grid",
"mean": 0.005802622439024,
"sd": 0.01299977519134 
},
{
 "name": "grid",
"mean": 0.006054695121951,
"sd": 0.02168257504081 
},
{
 "name": "grid",
"mean": 0.006239674796748,
"sd": 0.01821907586526 
},
{
 "name": "grid",
"mean": 0.006364601219512,
"sd": 0.01734736563444 
},
{
 "name": "grid",
"mean": 0.005856260162602,
"sd": 0.0163149619937 
},
{
 "name": "grid",
"mean": 0.006074164634146,
"sd": 0.0151974158604 
},
{
 "name": "grid",
"mean": 0.006169769756098,
"sd": 0.01499903512049 
},
{
 "name": "grid",
"mean": 0.006233506504065,
"sd": 0.01512028898905 
},
{
 "name": "grid",
"mean": 0.00578372804878,
"sd": 0.01387290689048 
},
{
 "name": "grid",
"mean": 0.005937420487805,
"sd": 0.01340508208247 
},
{
 "name": "grid",
"mean": 0.006039882113821,
"sd": 0.01352236215243 
},
{
 "name": "grid",
"mean": 0.005846257723577,
"sd": 0.01229106145299 
},
{
 "name": "grid",
"mean": 0.006022740853659,
"sd": 0.01288130761204 
},
{
 "name": "grid",
"mean": 0.005706504065041,
"sd": 0.01691691058698 
},
{
 "name": "grid",
"mean": 0.005960724390244,
"sd": 0.01598215916754 
},
{
 "name": "grid",
"mean": 0.006079017560976,
"sd": 0.0157779562196 
},
{
 "name": "grid",
"mean": 0.006157879674797,
"sd": 0.01584437159003 
},
{
 "name": "grid",
"mean": 0.005670287804878,
"sd": 0.01415576697367 
},
{
 "name": "grid",
"mean": 0.005846668292683,
"sd": 0.0138954532451 
},
{
 "name": "grid",
"mean": 0.005964255284553,
"sd": 0.01406855505555 
},
{
 "name": "grid",
"mean": 0.00561431902439,
"sd": 0.01254437780157 
},
{
 "name": "grid",
"mean": 0.005770630894309,
"sd": 0.01260115653463 
},
{
 "name": "grid",
"mean": 0.005882282229965,
"sd": 0.01291735631489 
},
{
 "name": "grid",
"mean": 0.005966020731707,
"sd": 0.01329529253745 
},
{
 "name": "grid",
"mean": 0.005577006504065,
"sd": 0.01156022599519 
},
{
 "name": "grid",
"mean": 0.005820802439024,
"sd": 0.01210366637258 
},
{
 "name": "grid",
"mean": 0.005902067750678,
"sd": 0.01250889823993 
},
{
 "name": "grid",
"mean": 0.005556847560976,
"sd": 0.01506463044972 
},
{
 "name": "grid",
"mean": 0.005755916097561,
"sd": 0.01477814866164 
},
{
 "name": "grid",
"mean": 0.005888628455285,
"sd": 0.01487527582004 
},
{
 "name": "grid",
"mean": 0.005523566829268,
"sd": 0.01311815607218 
},
{
 "name": "grid",
"mean": 0.005695004065041,
"sd": 0.01322073093972 
},
{
 "name": "grid",
"mean": 0.005817459233449,
"sd": 0.01352233927018 
},
{
 "name": "grid",
"mean": 0.005909300609756,
"sd": 0.0138657423622 
},
{
 "name": "grid",
"mean": 0.005501379674797,
"sd": 0.01192838617875 
},
{
 "name": "grid",
"mean": 0.005651495470383,
"sd": 0.01218394156063 
},
{
 "name": "grid",
"mean": 0.005764082317073,
"sd": 0.01256414759571 
},
{
 "name": "grid",
"mean": 0.005851649864499,
"sd": 0.01296200016558 
},
{
 "name": "grid",
"mean": 0.00561886402439,
"sd": 0.01145963498343 
},
{
 "name": "grid",
"mean": 0.005722566937669,
"sd": 0.01186085402077 
},
{
 "name": "grid",
"mean": 0.005619377235772,
"sd": 0.01410907273183 
},
{
 "name": "grid",
"mean": 0.005852580487805,
"sd": 0.01457429616111 
},
{
 "name": "grid",
"mean": 0.005425752845528,
"sd": 0.01261798695515 
},
{
 "name": "grid",
"mean": 0.005707362195122,
"sd": 0.01318613513101 
},
{
 "name": "grid",
"mean": 0.00580123197832,
"sd": 0.01353640259727 
},
{
 "name": "grid",
"mean": 0.005562143902439,
"sd": 0.01196681437933 
},
{
 "name": "grid",
"mean": 0.005672149051491,
"sd": 0.01235449270761 
},
{
 "name": "grid",
"mean": 0.005416925609756,
"sd": 0.01097276486034 
},
{
 "name": "grid",
"mean": 0.005543066124661,
"sd": 0.01132467395266 
},
{
 "name": "grid",
"mean": 0.005726543237251,
"sd": 0.01212357581234 
},
{
 "name": "grid",
"mean": 0.005716097560976,
"sd": 0.02039692712426 
},
{
 "name": "grid",
"mean": 0.005967991463415,
"sd": 0.01748462744774 
},
{
 "name": "grid",
"mean": 0.006084831219512,
"sd": 0.01629782976348 
},
{
 "name": "grid",
"mean": 0.006162724390244,
"sd": 0.01586998482488 
},
{
 "name": "grid",
"mean": 0.005677554878049,
"sd": 0.01655444619319 
},
{
 "name": "grid",
"mean": 0.00585248195122,
"sd": 0.01499076111823 
},
{
 "name": "grid",
"mean":      0.0059691,
"sd": 0.01446126472406 
},
{
 "name": "grid",
"mean": 0.005620132682927,
"sd": 0.01428161958361 
},
{
 "name": "grid",
"mean": 0.005775475609756,
"sd": 0.01343073754071 
},
{
 "name": "grid",
"mean": 0.005886434843206,
"sd": 0.01322169382904 
},
{
 "name": "grid",
"mean": 0.005969654268293,
"sd": 0.01329656103662 
},
{
 "name": "grid",
"mean": 0.005581851219512,
"sd": 0.01286957725593 
},
{
 "name": "grid",
"mean": 0.00582443597561,
"sd": 0.01234413879572 
},
{
 "name": "grid",
"mean": 0.005905297560976,
"sd": 0.01250327960242 
},
{
 "name": "grid",
"mean": 0.005564114634146,
"sd": 0.01677229484806 
},
{
 "name": "grid",
"mean": 0.005761729756098,
"sd": 0.01541696434036 
},
{
 "name": "grid",
"mean": 0.005893473170732,
"sd": 0.01496333012566 
},
{
 "name": "grid",
"mean": 0.005529380487805,
"sd": 0.01436450750377 
},
{
 "name": "grid",
"mean": 0.005699848780488,
"sd": 0.01370426112992 
},
{
 "name": "grid",
"mean": 0.00582161184669,
"sd": 0.01358335883959 
},
{
 "name": "grid",
"mean": 0.005912934146341,
"sd": 0.01369190459478 
},
{
 "name": "grid",
"mean": 0.005506224390244,
"sd": 0.01287233747907 
},
{
 "name": "grid",
"mean": 0.005655648083624,
"sd": 0.01255934218198 
},
{
 "name": "grid",
"mean": 0.005767715853659,
"sd": 0.01260605157016 
},
{
 "name": "grid",
"mean": 0.005854879674797,
"sd": 0.01280863490004 
},
{
 "name": "grid",
"mean": 0.005622497560976,
"sd": 0.01175683871376 
},
{
 "name": "grid",
"mean": 0.005725796747967,
"sd": 0.01188890404979 
},
{
 "name": "grid",
"mean": 0.005438628292683,
"sd": 0.01485382628991 
},
{
 "name": "grid",
"mean": 0.00562422195122,
"sd": 0.0142656391917 
},
{
 "name": "grid",
"mean": 0.005756788850174,
"sd": 0.01415221105339 
},
{
 "name": "grid",
"mean": 0.00585621402439,
"sd": 0.01424061997255 
},
{
 "name": "grid",
"mean": 0.005430597560976,
"sd": 0.01319271586161 
},
{
 "name": "grid",
"mean": 0.005590825087108,
"sd": 0.01296586962834 
},
{
 "name": "grid",
"mean": 0.005710995731707,
"sd": 0.01304241540165 
},
{
 "name": "grid",
"mean": 0.005804461788618,
"sd": 0.01324651233557 
},
{
 "name": "grid",
"mean": 0.005424861324042,
"sd": 0.01209202219904 
},
{
 "name": "grid",
"mean": 0.005565777439024,
"sd": 0.01205323099466 
},
{
 "name": "grid",
"mean": 0.005675378861789,
"sd": 0.01222652385333 
},
{
 "name": "grid",
"mean":     0.00576306,
"sd": 0.01248853745028 
},
{
 "name": "grid",
"mean": 0.005420559146341,
"sd": 0.01132795616635 
},
{
 "name": "grid",
"mean": 0.005546295934959,
"sd": 0.01138951874755 
},
{
 "name": "grid",
"mean": 0.005646885365854,
"sd": 0.01161009986469 
},
{
 "name": "grid",
"mean": 0.005729185809313,
"sd": 0.01189789033121 
},
{
 "name": "grid",
"mean": 0.005354970731707,
"sd": 0.01380862258289 
},
{
 "name": "grid",
"mean": 0.005654275609756,
"sd": 0.0136364934117 
},
{
 "name": "grid",
"mean": 0.005754043902439,
"sd": 0.01380430671846 
},
{
 "name": "grid",
"mean": 0.005509057317073,
"sd": 0.0125297561037 
},
{
 "name": "grid",
"mean": 0.00562496097561,
"sd": 0.01270077026934 
},
{
 "name": "grid",
"mean": 0.00536383902439,
"sd": 0.01165770385041 
},
{
 "name": "grid",
"mean": 0.00549587804878,
"sd": 0.01175906893324 
},
{
 "name": "grid",
"mean": 0.005601509268293,
"sd": 0.01199174525857 
},
{
 "name": "grid",
"mean": 0.00568793481153,
"sd": 0.01227687554643 
},
{
 "name": "grid",
"mean": 0.005366795121951,
"sd": 0.011020766498 
},
{
 "name": "grid",
"mean": 0.005582321507761,
"sd": 0.01144278431831 
},
{
 "name": "grid",
"mean": 0.005663143902439,
"sd": 0.01174438313719 
},
{
 "name": "grid",
"mean": 0.005571381707317,
"sd": 0.02072716887967 
},
{
 "name": "grid",
"mean": 0.005898317886179,
"sd": 0.01637923306182 
},
{
 "name": "grid",
"mean": 0.005991728222997,
"sd": 0.01569222597241 
},
{
 "name": "grid",
"mean": 0.005704693495935,
"sd": 0.01557486530183 
},
{
 "name": "grid",
"mean": 0.005916567682927,
"sd": 0.01435835678144 
},
{
 "name": "grid",
"mean": 0.005511069105691,
"sd": 0.01519402372769 
},
{
 "name": "grid",
"mean": 0.005771349390244,
"sd": 0.01354444481791 
},
{
 "name": "grid",
"mean": 0.005858109485095,
"sd": 0.01336654907463 
},
{
 "name": "grid",
"mean": 0.005493836933798,
"sd": 0.01374184376996 
},
{
 "name": "grid",
"mean": 0.005626131097561,
"sd": 0.01298491420774 
},
{
 "name": "grid",
"mean": 0.005729026558266,
"sd": 0.0126715447898 
},
{
 "name": "grid",
"mean": 0.005811342926829,
"sd": 0.01260887597193 
},
{
 "name": "grid",
"mean": 0.005629066666667,
"sd": 0.01580194922344 
},
{
 "name": "grid",
"mean": 0.005859847560976,
"sd": 0.01471954903068 
},
{
 "name": "grid",
"mean": 0.005435442276423,
"sd": 0.01518659008227 
},
{
 "name": "grid",
"mean": 0.005714629268293,
"sd": 0.01377750910645 
},
{
 "name": "grid",
"mean": 0.005807691598916,
"sd": 0.01364777671597 
},
{
 "name": "grid",
"mean": 0.00556941097561,
"sd": 0.01307060925454 
},
{
 "name": "grid",
"mean": 0.005678608672087,
"sd": 0.0128412658045 
},
{
 "name": "grid",
"mean": 0.005424192682927,
"sd": 0.01263836741776 
},
{
 "name": "grid",
"mean": 0.005549525745257,
"sd": 0.01223723256784 
},
{
 "name": "grid",
"mean": 0.005731828381375,
"sd": 0.01218852604446 
},
{
 "name": "grid",
"mean": 0.005359815447154,
"sd": 0.01544947590753 
},
{
 "name": "grid",
"mean": 0.005657909146341,
"sd": 0.01417197299848 
},
{
 "name": "grid",
"mean": 0.005757273712737,
"sd": 0.01405485034715 
},
{
 "name": "grid",
"mean": 0.005512690853659,
"sd": 0.01333157700071 
},
{
 "name": "grid",
"mean": 0.005628190785908,
"sd": 0.01314944871206 
},
{
 "name": "grid",
"mean": 0.005367472560976,
"sd": 0.0127469018815 
},
{
 "name": "grid",
"mean": 0.005499107859079,
"sd": 0.01242950682312 
},
{
 "name": "grid",
"mean": 0.005604416097561,
"sd": 0.0123717931746 
},
{
 "name": "grid",
"mean": 0.005690577383592,
"sd": 0.01245673932368 
},
{
 "name": "grid",
"mean": 0.005370024932249,
"sd": 0.01192865179246 
},
{
 "name": "grid",
"mean": 0.005584964079823,
"sd": 0.01176765561295 
},
{
 "name": "grid",
"mean": 0.005665566260163,
"sd": 0.01189851339144 
},
{
 "name": "grid",
"mean": 0.005299367944251,
"sd": 0.01425172865768 
},
{
 "name": "grid",
"mean": 0.005455970731707,
"sd": 0.01375784700107 
},
{
 "name": "grid",
"mean": 0.005577772899729,
"sd": 0.01358667469499 
},
{
 "name": "grid",
"mean": 0.005675214634146,
"sd": 0.01358980534839 
},
{
 "name": "grid",
"mean": 0.005310752439024,
"sd": 0.01303441498132 
},
{
 "name": "grid",
"mean": 0.0054486899729,
"sd": 0.01276382594209 
},
{
 "name": "grid",
"mean": 0.005649326385809,
"sd": 0.01281576770551 
},
{
 "name": "grid",
"mean": 0.00531960704607,
"sd": 0.01214282633398 
},
{
 "name": "grid",
"mean": 0.00554371308204,
"sd": 0.01205670721755 
},
{
 "name": "grid",
"mean": 0.005627752845528,
"sd": 0.01219854331955 
},
{
 "name": "grid",
"mean": 0.005326690731707,
"sd": 0.01147273722861 
},
{
 "name": "grid",
"mean": 0.005438099778271,
"sd": 0.01143001466757 
},
{
 "name": "grid",
"mean": 0.005530940650407,
"sd": 0.01152625036999 
},
{
 "name": "grid",
"mean": 0.005609498311445,
"sd": 0.0116992061879 
},
{
 "name": "grid",
"mean": 0.007241970731707,
"sd": 0.03516420304505 
},
{
 "name": "grid",
"mean": 0.00703918699187,
"sd": 0.0276644955127 
},
{
 "name": "grid",
"mean": 0.006970231707317,
"sd": 0.02447198691899 
},
{
 "name": "grid",
"mean": 0.006655772357724,
"sd": 0.02532629558057 
},
{
 "name": "grid",
"mean": 0.006679795121951,
"sd": 0.02226081252427 
},
{
 "name": "grid",
"mean": 0.006654274146341,
"sd": 0.02063573590021 
},
{
 "name": "grid",
"mean": 0.006637260162602,
"sd": 0.01974858847376 
},
{
 "name": "grid",
"mean": 0.006389358536585,
"sd": 0.02058337064471 
},
{
 "name": "grid",
"mean": 0.006421924878049,
"sd": 0.01895271336612 
},
{
 "name": "grid",
"mean": 0.006443635772358,
"sd": 0.0181496313901 
},
{
 "name": "grid",
"mean": 0.006250011382114,
"sd": 0.01681654741253 
},
{
 "name": "grid",
"mean": 0.006325556097561,
"sd": 0.01626827283507 
},
{
 "name": "grid",
"mean": 0.006506016260163,
"sd": 0.02619323328959 
},
{
 "name": "grid",
"mean": 0.006566354878049,
"sd": 0.02311194488181 
},
{
 "name": "grid",
"mean": 0.00656352195122,
"sd": 0.0214210786359 
},
{
 "name": "grid",
"mean": 0.006561633333333,
"sd": 0.0204625688208 
},
{
 "name": "grid",
"mean": 0.006275918292683,
"sd": 0.02111287756974 
},
{
 "name": "grid",
"mean": 0.006331172682927,
"sd": 0.01953583643071 
},
{
 "name": "grid",
"mean": 0.006368008943089,
"sd": 0.01872880366804 
},
{
 "name": "grid",
"mean": 0.006098823414634,
"sd": 0.01801591727591 
},
{
 "name": "grid",
"mean": 0.006174384552846,
"sd": 0.01722800700361 
},
{
 "name": "grid",
"mean": 0.006228356794425,
"sd": 0.01686249055768 
},
{
 "name": "grid",
"mean": 0.00626883597561,
"sd": 0.01670422053512 
},
{
 "name": "grid",
"mean": 0.005980760162602,
"sd": 0.0160257648238 
},
{
 "name": "grid",
"mean": 0.006123617682927,
"sd": 0.01550560346174 
},
{
 "name": "grid",
"mean": 0.006171236856369,
"sd": 0.01550536411451 
},
{
 "name": "grid",
"mean": 0.00616247804878,
"sd": 0.02205586566891 
},
{
 "name": "grid",
"mean": 0.006240420487805,
"sd": 0.02039644890383 
},
{
 "name": "grid",
"mean": 0.006292382113821,
"sd": 0.01950401702923 
},
{
 "name": "grid",
"mean": 0.006008071219512,
"sd": 0.01866424510818 
},
{
 "name": "grid",
"mean": 0.006098757723577,
"sd": 0.01786314109994 
},
{
 "name": "grid",
"mean": 0.006163533797909,
"sd": 0.01746309381998 
},
{
 "name": "grid",
"mean": 0.006212115853659,
"sd": 0.01726448829458 
},
{
 "name": "grid",
"mean": 0.005905133333333,
"sd": 0.01648519681492 
},
{
 "name": "grid",
"mean": 0.005997570034843,
"sd": 0.01611396334418 
},
{
 "name": "grid",
"mean": 0.006066897560976,
"sd": 0.01597874740554 
},
{
 "name": "grid",
"mean": 0.00612081897019,
"sd": 0.01596096307743 
},
{
 "name": "grid",
"mean": 0.005921679268293,
"sd": 0.01484737743917 
},
{
 "name": "grid",
"mean": 0.00599173604336,
"sd": 0.01486028900872 
},
{
 "name": "grid",
"mean": 0.006023130894309,
"sd": 0.01869917166073 
},
{
 "name": "grid",
"mean": 0.006155395731707,
"sd": 0.01793743064654 
},
{
 "name": "grid",
"mean": 0.005829506504065,
"sd": 0.01717492113715 
},
{
 "name": "grid",
"mean": 0.006010177439024,
"sd": 0.0165793331689 
},
{
 "name": "grid",
"mean": 0.006070401084011,
"sd": 0.0165156956336 
},
{
 "name": "grid",
"mean": 0.005864959146341,
"sd": 0.01535785121839 
},
{
 "name": "grid",
"mean": 0.005941318157182,
"sd": 0.01534851570357 
},
{
 "name": "grid",
"mean": 0.005719740853659,
"sd": 0.01430801647196 
},
{
 "name": "grid",
"mean": 0.005812235230352,
"sd": 0.01430388484393 
},
{
 "name": "grid",
"mean": 0.005946772505543,
"sd": 0.01453004525133 
},
{
 "name": "grid",
"mean": 0.006515609756098,
"sd": 0.02533430199428 
},
{
 "name": "grid",
"mean": 0.00657362195122,
"sd": 0.0220122370667 
},
{
 "name": "grid",
"mean": 0.006569335609756,
"sd": 0.0202878780039 
},
{
 "name": "grid",
"mean": 0.00656647804878,
"sd": 0.01936817683891 
},
{
 "name": "grid",
"mean": 0.006283185365854,
"sd": 0.02048197618776 
},
{
 "name": "grid",
"mean": 0.006336986341463,
"sd": 0.0186908596514 
},
{
 "name": "grid",
"mean": 0.006372853658537,
"sd": 0.01782047325304 
},
{
 "name": "grid",
"mean": 0.006104637073171,
"sd": 0.01752816448572 
},
{
 "name": "grid",
"mean": 0.006179229268293,
"sd": 0.01655285577331 
},
{
 "name": "grid",
"mean": 0.006232509407666,
"sd": 0.01611447325897 
},
{
 "name": "grid",
"mean": 0.006272469512195,
"sd": 0.01594014319926 
},
{
 "name": "grid",
"mean": 0.005985604878049,
"sd": 0.01563360406088 
},
{
 "name": "grid",
"mean": 0.006127251219512,
"sd": 0.01487703637977 
},
{
 "name": "grid",
"mean": 0.006174466666667,
"sd": 0.01485063041647 
},
{
 "name": "grid",
"mean": 0.006169745121951,
"sd": 0.020998137856 
},
{
 "name": "grid",
"mean": 0.006246234146341,
"sd": 0.01927081396664 
},
{
 "name": "grid",
"mean": 0.006297226829268,
"sd": 0.01840193854087 
},
{
 "name": "grid",
"mean": 0.006013884878049,
"sd": 0.01785125020814 
},
{
 "name": "grid",
"mean": 0.006103602439024,
"sd": 0.01696196039823 
},
{
 "name": "grid",
"mean": 0.00616768641115,
"sd": 0.01655267926518 
},
{
 "name": "grid",
"mean": 0.006215749390244,
"sd": 0.01637972667763 
},
{
 "name": "grid",
"mean": 0.00590997804878,
"sd": 0.01583570754437 
},
{
 "name": "grid",
"mean": 0.006001722648084,
"sd": 0.01537289906986 
},
{
 "name": "grid",
"mean": 0.006070531097561,
"sd": 0.01521178982945 
},
{
 "name": "grid",
"mean": 0.006124048780488,
"sd": 0.01520083760432 
},
{
 "name": "grid",
"mean": 0.005925312804878,
"sd": 0.01422561384236 
},
{
 "name": "grid",
"mean": 0.005994965853659,
"sd": 0.01420421812805 
},
{
 "name": "grid",
"mean": 0.005923132682927,
"sd": 0.0184937929987 
},
{
 "name": "grid",
"mean": 0.006027975609756,
"sd": 0.01759826108033 
},
{
 "name": "grid",
"mean": 0.006102863414634,
"sd": 0.01715777153294 
},
{
 "name": "grid",
"mean": 0.006159029268293,
"sd": 0.01694579642841 
},
{
 "name": "grid",
"mean": 0.005834351219512,
"sd": 0.01629138782088 
},
{
 "name": "grid",
"mean": 0.005936899651568,
"sd": 0.01585317689843 
},
{
 "name": "grid",
"mean": 0.00601381097561,
"sd": 0.01568847277221 
},
{
 "name": "grid",
"mean": 0.006073630894309,
"sd": 0.01566108354637 
},
{
 "name": "grid",
"mean": 0.005770935888502,
"sd": 0.01478282245354 
},
{
 "name": "grid",
"mean": 0.005868592682927,
"sd": 0.01459324415099 
},
{
 "name": "grid",
"mean": 0.00594454796748,
"sd": 0.01458412738494 
},
{
 "name": "grid",
"mean": 0.006005312195122,
"sd": 0.01466442050259 
},
{
 "name": "grid",
"mean": 0.005723374390244,
"sd": 0.01369904403129 
},
{
 "name": "grid",
"mean": 0.00581546504065,
"sd": 0.01365062125322 
},
{
 "name": "grid",
"mean": 0.005889137560976,
"sd": 0.01373090397875 
},
{
 "name": "grid",
"mean": 0.005949415077605,
"sd": 0.01387408308886 
},
{
 "name": "grid",
"mean": 0.005758724390244,
"sd": 0.01698024225479 
},
{
 "name": "grid",
"mean": 0.005957090853659,
"sd": 0.01629463395635 
},
{
 "name": "grid",
"mean": 0.00602321300813,
"sd": 0.01622200498357 
},
{
 "name": "grid",
"mean": 0.005811872560976,
"sd": 0.01510677828814 
},
{
 "name": "grid",
"mean": 0.005894130081301,
"sd": 0.01507692158229 
},
{
 "name": "grid",
"mean": 0.005666654268293,
"sd": 0.01409896593078 
},
{
 "name": "grid",
"mean": 0.005765047154472,
"sd": 0.01406019695433 
},
{
 "name": "grid",
"mean": 0.005843761463415,
"sd": 0.0141353917886 
},
{
 "name": "grid",
"mean": 0.005908164079823,
"sd": 0.01426615466951 
},
{
 "name": "grid",
"mean": 0.005635964227642,
"sd": 0.01320152141517 
},
{
 "name": "grid",
"mean": 0.005802550776053,
"sd": 0.01339130107606 
},
{
 "name": "grid",
"mean": 0.005865020731707,
"sd": 0.01356706306517 
},
{
 "name": "grid",
"mean": 0.006177012195122,
"sd": 0.02212060685982 
},
{
 "name": "grid",
"mean": 0.006252047804878,
"sd": 0.01966810340859 
},
{
 "name": "grid",
"mean": 0.006302071544715,
"sd": 0.01840126662391 
},
{
 "name": "grid",
"mean": 0.006019698536585,
"sd": 0.01868424738204 
},
{
 "name": "grid",
"mean": 0.006108447154472,
"sd": 0.01726484870082 
},
{
 "name": "grid",
"mean": 0.00617183902439,
"sd": 0.01654377989283 
},
{
 "name": "grid",
"mean": 0.006219382926829,
"sd": 0.0161868172859 
},
{
 "name": "grid",
"mean": 0.005914822764228,
"sd": 0.01647811161363 
},
{
 "name": "grid",
"mean": 0.006005875261324,
"sd": 0.0156098095492 
},
{
 "name": "grid",
"mean": 0.006074164634146,
"sd": 0.0151974158604 
},
{
 "name": "grid",
"mean": 0.006127278590786,
"sd": 0.01503111479027 
},
{
 "name": "grid",
"mean": 0.005928946341463,
"sd": 0.0144144468994 
},
{
 "name": "grid",
"mean": 0.005998195663957,
"sd": 0.01418616446711 
},
{
 "name": "grid",
"mean": 0.005928946341463,
"sd": 0.01897641060126 
},
{
 "name": "grid",
"mean": 0.006032820325203,
"sd": 0.01764905591872 
},
{
 "name": "grid",
"mean": 0.006107016027875,
"sd": 0.01696447673072 
},
{
 "name": "grid",
"mean": 0.006162662804878,
"sd": 0.01661485277815 
},
{
 "name": "grid",
"mean": 0.005839195934959,
"sd": 0.01666107222571 
},
{
 "name": "grid",
"mean": 0.005941052264808,
"sd": 0.01588591296109 
},
{
 "name": "grid",
"mean": 0.006017444512195,
"sd": 0.01551988354905 
},
{
 "name": "grid",
"mean": 0.006076860704607,
"sd": 0.01537292099383 
},
{
 "name": "grid",
"mean": 0.005775088501742,
"sd": 0.01507333800429 
},
{
 "name": "grid",
"mean": 0.005872226219512,
"sd": 0.01461323624347 
},
{
 "name": "grid",
"mean": 0.005947777777778,
"sd": 0.01443511119838 
},
{
 "name": "grid",
"mean": 0.00600821902439,
"sd": 0.0144106248573 
},
{
 "name": "grid",
"mean": 0.005727007926829,
"sd": 0.01393172628332 
},
{
 "name": "grid",
"mean": 0.005818694850949,
"sd": 0.01366139180395 
},
{
 "name": "grid",
"mean": 0.005892044390244,
"sd": 0.01359777888282 
},
{
 "name": "grid",
"mean": 0.005952057649667,
"sd": 0.0136484364326 
},
{
 "name": "grid",
"mean": 0.005763569105691,
"sd": 0.01708607969285 
},
{
 "name": "grid",
"mean": 0.005876229268293,
"sd": 0.01634445770685 
},
{
 "name": "grid",
"mean": 0.005960724390244,
"sd": 0.01598215916754 
},
{
 "name": "grid",
"mean": 0.006026442818428,
"sd": 0.01582399512747 
},
{
 "name": "grid",
"mean": 0.005710265505226,
"sd": 0.01538128227929 
},
{
 "name": "grid",
"mean": 0.005815506097561,
"sd": 0.01496577286134 
},
{
 "name": "grid",
"mean": 0.005897359891599,
"sd": 0.01480464258935 
},
{
 "name": "grid",
"mean": 0.005962842926829,
"sd": 0.01478162718671 
},
{
 "name": "grid",
"mean": 0.005670287804878,
"sd": 0.01415576697367 
},
{
 "name": "grid",
"mean": 0.00576827696477,
"sd": 0.01393454405022 
},
{
 "name": "grid",
"mean": 0.005846668292683,
"sd": 0.0138954532451 
},
{
 "name": "grid",
"mean": 0.005910806651885,
"sd": 0.01395568390102 
},
{
 "name": "grid",
"mean": 0.00563919403794,
"sd": 0.01324315101709 
},
{
 "name": "grid",
"mean": 0.005730493658537,
"sd": 0.01314173726215 
},
{
 "name": "grid",
"mean": 0.005805193348115,
"sd": 0.01317788347476 
},
{
 "name": "grid",
"mean": 0.005867443089431,
"sd": 0.01328941672467 
},
{
 "name": "grid",
"mean": 0.00575878597561,
"sd": 0.01546154361328 
},
{
 "name": "grid",
"mean": 0.00584694200542,
"sd": 0.01528601592812 
},
{
 "name": "grid",
"mean": 0.005613567682927,
"sd": 0.01453739212275 
},
{
 "name": "grid",
"mean": 0.005717859078591,
"sd": 0.01433140759385 
},
{
 "name": "grid",
"mean": 0.005801292195122,
"sd": 0.01429154703522 
},
{
 "name": "grid",
"mean": 0.005869555654102,
"sd": 0.01434244641256 
},
{
 "name": "grid",
"mean": 0.005588776151762,
"sd": 0.01354000219987 
},
{
 "name": "grid",
"mean": 0.005685117560976,
"sd": 0.01346193117172 
},
{
 "name": "grid",
"mean": 0.005763942350333,
"sd": 0.01350607980977 
},
{
 "name": "grid",
"mean": 0.005829629674797,
"sd": 0.0136165592604 
},
{
 "name": "grid",
"mean": 0.005658329046563,
"sd": 0.01277823829334 
},
{
 "name": "grid",
"mean": 0.005732817479675,
"sd": 0.01287452845817 
},
{
 "name": "grid",
"mean": 0.005795846153846,
"sd": 0.01302182472336 
},
{
 "name": "grid",
"mean": 0.00603766504065,
"sd": 0.01884224418983 
},
{
 "name": "grid",
"mean": 0.006166296341463,
"sd": 0.01698326726338 
},
{
 "name": "grid",
"mean": 0.005844040650407,
"sd": 0.01820779909288 
},
{
 "name": "grid",
"mean": 0.00602107804878,
"sd": 0.01609627629861 
},
{
 "name": "grid",
"mean": 0.006080090514905,
"sd": 0.01568246435898 
},
{
 "name": "grid",
"mean": 0.005875859756098,
"sd": 0.01541477505569 
},
{
 "name": "grid",
"mean": 0.005951007588076,
"sd": 0.01491991707385 
},
{
 "name": "grid",
"mean": 0.005730641463415,
"sd": 0.01496686022346 
},
{
 "name": "grid",
"mean": 0.005821924661247,
"sd": 0.01433469978392 
},
{
 "name": "grid",
"mean": 0.005954700221729,
"sd": 0.01387411696288 
},
{
 "name": "grid",
"mean": 0.005768413821138,
"sd": 0.01836546272289 
},
{
 "name": "grid",
"mean": 0.005964357926829,
"sd": 0.0163959886983 
},
{
 "name": "grid",
"mean": 0.006029672628726,
"sd": 0.01600624954051 
},
{
 "name": "grid",
"mean": 0.005819139634146,
"sd": 0.01559546992818 
},
{
 "name": "grid",
"mean": 0.005900589701897,
"sd": 0.01515246337895 
},
{
 "name": "grid",
"mean": 0.005673921341463,
"sd": 0.01501583679858 
},
{
 "name": "grid",
"mean": 0.005771506775068,
"sd": 0.01446408392648 
},
{
 "name": "grid",
"mean": 0.005849575121951,
"sd": 0.01419112963024 
},
{
 "name": "grid",
"mean": 0.005913449223947,
"sd": 0.01408618920017 
},
{
 "name": "grid",
"mean": 0.005642423848238,
"sd": 0.01396559187787 
},
{
 "name": "grid",
"mean": 0.005807835920177,
"sd": 0.01343160266633 
},
{
 "name": "grid",
"mean": 0.005869865447154,
"sd": 0.01340110614571 
},
{
 "name": "grid",
"mean": 0.005762419512195,
"sd": 0.01592103527671 
},
{
 "name": "grid",
"mean": 0.005850171815718,
"sd": 0.01550065164041 
},
{
 "name": "grid",
"mean": 0.005617201219512,
"sd": 0.01521844585733 
},
{
 "name": "grid",
"mean": 0.005721088888889,
"sd": 0.01471786885513 
},
{
 "name": "grid",
"mean": 0.00580419902439,
"sd": 0.01447291640799 
},
{
 "name": "grid",
"mean": 0.005872198226164,
"sd": 0.01438101841762 
},
{
 "name": "grid",
"mean": 0.00559200596206,
"sd": 0.01411299334943 
},
{
 "name": "grid",
"mean": 0.005688024390244,
"sd": 0.01379063341317 
},
{
 "name": "grid",
"mean": 0.005766584922395,
"sd": 0.01366066754344 
},
{
 "name": "grid",
"mean": 0.00583205203252,
"sd": 0.01364725633795 
},
{
 "name": "grid",
"mean": 0.005660971618625,
"sd": 0.01306042664157 
},
{
 "name": "grid",
"mean": 0.005735239837398,
"sd": 0.01300724066372 
},
{
 "name": "grid",
"mean": 0.00579808217636,
"sd": 0.01304610681683 
},
{
 "name": "grid",
"mean": 0.005560481097561,
"sd": 0.01556869049091 
},
{
 "name": "grid",
"mean": 0.00567067100271,
"sd": 0.01508977923326 
},
{
 "name": "grid",
"mean": 0.005830947228381,
"sd": 0.01475364413067 
},
{
 "name": "grid",
"mean": 0.005541588075881,
"sd": 0.01438732914513 
},
{
 "name": "grid",
"mean": 0.005725333924612,
"sd": 0.01397437244173 
},
{
 "name": "grid",
"mean": 0.005794238617886,
"sd": 0.01396336176316 
},
{
 "name": "grid",
"mean": 0.005619720620843,
"sd": 0.01330627350046 
},
{
 "name": "grid",
"mean": 0.005697426422764,
"sd": 0.0132694545351 
},
{
 "name": "grid",
"mean": 0.005763177485929,
"sd": 0.0133154630103 
},
{
 "name": "grid",
"mean": 0.005514107317073,
"sd": 0.01276681256699 
},
{
 "name": "grid",
"mean": 0.005600614227642,
"sd": 0.01267586561885 
},
{
 "name": "grid",
"mean": 0.005673812382739,
"sd": 0.01269357371551 
},
{
 "name": "grid",
"mean": 0.005736553658537,
"sd": 0.01277764240813 
},
{
 "name": "grid",
"mean": 0.00731512195122,
"sd": 0.0355193970152 
},
{
 "name": "grid",
"mean": 0.007179252439024,
"sd": 0.02982363988228 
},
{
 "name": "grid",
"mean": 0.006970231707317,
"sd": 0.02447198691899 
},
{
 "name": "grid",
"mean": 0.006888815853659,
"sd": 0.02812574514216 
},
{
 "name": "grid",
"mean": 0.006776607317073,
"sd": 0.02294542155821 
},
{
 "name": "grid",
"mean": 0.006744547735192,
"sd": 0.02172087441436 
},
{
 "name": "grid",
"mean": 0.006582982926829,
"sd": 0.02163541937646 
},
{
 "name": "grid",
"mean": 0.006575284756098,
"sd": 0.01969514985696 
},
{
 "name": "grid",
"mean": 0.006389358536585,
"sd": 0.02058337064471 
},
{
 "name": "grid",
"mean": 0.006412620209059,
"sd": 0.01937017816094 
},
{
 "name": "grid",
"mean": 0.006430066463415,
"sd": 0.01862245400437 
},
{
 "name": "grid",
"mean": 0.006443635772358,
"sd": 0.0181496313901 
},
{
 "name": "grid",
"mean": 0.006775375609756,
"sd": 0.02875096944143 
},
{
 "name": "grid",
"mean": 0.006700980487805,
"sd": 0.02353387876347 
},
{
 "name": "grid",
"mean": 0.006679724738676,
"sd": 0.02227403335078 
},
{
 "name": "grid",
"mean": 0.006507356097561,
"sd": 0.02209279162904 
},
{
 "name": "grid",
"mean": 0.006518564634146,
"sd": 0.02014060012756 
},
{
 "name": "grid",
"mean": 0.006313731707317,
"sd": 0.02088837496469 
},
{
 "name": "grid",
"mean": 0.006373346341463,
"sd": 0.01898435398702 
},
{
 "name": "grid",
"mean": 0.006393217886179,
"sd": 0.01851292717597 
},
{
 "name": "grid",
"mean": 0.006181833449477,
"sd": 0.01872039677495 
},
{
 "name": "grid",
"mean": 0.00622812804878,
"sd": 0.01797314815789 
},
{
 "name": "grid",
"mean": 0.00626413495935,
"sd": 0.01751662806116 
},
{
 "name": "grid",
"mean": 0.006292940487805,
"sd": 0.01723656284168 
},
{
 "name": "grid",
"mean": 0.006431729268293,
"sd": 0.02272380114624 
},
{
 "name": "grid",
"mean": 0.006461844512195,
"sd": 0.02068927037976 
},
{
 "name": "grid",
"mean": 0.006238104878049,
"sd": 0.02138347250646 
},
{
 "name": "grid",
"mean": 0.006316626219512,
"sd": 0.01945951924387 
},
{
 "name": "grid",
"mean":      0.0063428,
"sd": 0.01896648780127 
},
{
 "name": "grid",
"mean": 0.006171407926829,
"sd": 0.01836208880147 
},
{
 "name": "grid",
"mean": 0.006213717073171,
"sd": 0.01790431038895 
},
{
 "name": "grid",
"mean": 0.006026189634146,
"sd": 0.0174220020864 
},
{
 "name": "grid",
"mean": 0.006084634146341,
"sd": 0.01695925873311 
},
{
 "name": "grid",
"mean": 0.006169644345898,
"sd": 0.01653227280241 
},
{
 "name": "grid",
"mean": 0.00616247804878,
"sd": 0.02205586566891 
},
{
 "name": "grid",
"mean": 0.006218151219512,
"sd": 0.0208340312044 
},
{
 "name": "grid",
"mean": 0.006259906097561,
"sd": 0.02003989450668 
},
{
 "name": "grid",
"mean": 0.006292382113821,
"sd": 0.01950401702923 
},
{
 "name": "grid",
"mean": 0.006052187456446,
"sd": 0.01962065578067 
},
{
 "name": "grid",
"mean": 0.006114687804878,
"sd": 0.01886679213165 
},
{
 "name": "grid",
"mean": 0.006163299186992,
"sd": 0.01838413001047 
},
{
 "name": "grid",
"mean": 0.006202188292683,
"sd": 0.0180688238775 
},
{
 "name": "grid",
"mean": 0.005969469512195,
"sd": 0.01783762188281 
},
{
 "name": "grid",
"mean": 0.006034216260163,
"sd": 0.01737127650727 
},
{
 "name": "grid",
"mean": 0.006128393348115,
"sd": 0.01691804185359 
},
{
 "name": "grid",
"mean": 0.005905133333333,
"sd": 0.01648519681492 
},
{
 "name": "grid",
"mean": 0.00596983902439,
"sd": 0.01620285369635 
},
{
 "name": "grid",
"mean": 0.006022780044346,
"sd": 0.01605029754545 
},
{
 "name": "grid",
"mean": 0.006066897560976,
"sd": 0.01597874740554 
},
{
 "name": "grid",
"mean": 0.006782642682927,
"sd": 0.02784082429419 
},
{
 "name": "grid",
"mean": 0.006705825203252,
"sd": 0.02257024512525 
},
{
 "name": "grid",
"mean": 0.006683877351916,
"sd": 0.02134434175601 
},
{
 "name": "grid",
"mean": 0.006512200813008,
"sd": 0.021308580106 
},
{
 "name": "grid",
"mean": 0.006522198170732,
"sd": 0.01934902661878 
},
{
 "name": "grid",
"mean": 0.006318576422764,
"sd": 0.02031452637017 
},
{
 "name": "grid",
"mean": 0.006376979878049,
"sd": 0.01830278790528 
},
{
 "name": "grid",
"mean": 0.006396447696477,
"sd": 0.01782870893788 
},
{
 "name": "grid",
"mean": 0.006185986062718,
"sd": 0.01824296074898 
},
{
 "name": "grid",
"mean": 0.006231761585366,
"sd": 0.01742026678074 
},
{
 "name": "grid",
"mean": 0.006267364769648,
"sd": 0.01692882601054 
},
{
 "name": "grid",
"mean": 0.006295847317073,
"sd": 0.01663798464838 
},
{
 "name": "grid",
"mean": 0.00643657398374,
"sd": 0.02176600019211 
},
{
 "name": "grid",
"mean": 0.00646547804878,
"sd": 0.01979804724906 
},
{
 "name": "grid",
"mean": 0.006242949593496,
"sd": 0.02061630789139 
},
{
 "name": "grid",
"mean": 0.006320259756098,
"sd": 0.01866641225402 
},
{
 "name": "grid",
"mean": 0.006346029810298,
"sd": 0.01819478670817 
},
{
 "name": "grid",
"mean": 0.006175041463415,
"sd": 0.01768540792837 
},
{
 "name": "grid",
"mean": 0.006216946883469,
"sd": 0.01721933320535 
},
{
 "name": "grid",
"mean": 0.006029823170732,
"sd": 0.01688131500635 
},
{
 "name": "grid",
"mean": 0.00608786395664,
"sd": 0.01637608207119 
},
{
 "name": "grid",
"mean": 0.00617228691796,
"sd": 0.0159340811535 
},
{
 "name": "grid",
"mean": 0.006167322764228,
"sd": 0.02111075429023 
},
{
 "name": "grid",
"mean": 0.006263539634146,
"sd": 0.01914510740389 
},
{
 "name": "grid",
"mean": 0.006295611924119,
"sd": 0.01865254669441 
},
{
 "name": "grid",
"mean": 0.006118321341463,
"sd": 0.01807592050801 
},
{
 "name": "grid",
"mean": 0.00616652899729,
"sd": 0.01760981419111 
},
{
 "name": "grid",
"mean": 0.00597310304878,
"sd": 0.01717000589727 
},
{
 "name": "grid",
"mean": 0.006037446070461,
"sd": 0.01668859097673 
},
{
 "name": "grid",
"mean": 0.006088920487805,
"sd": 0.0164097379763 
},
{
 "name": "grid",
"mean": 0.006131035920177,
"sd": 0.01625568660332 
},
{
 "name": "grid",
"mean": 0.005908363143631,
"sd": 0.01590999802063 
},
{
 "name": "grid",
"mean": 0.006025422616408,
"sd": 0.01545092173164 
},
{
 "name": "grid",
"mean": 0.006069319918699,
"sd": 0.01538710747126 
},
{
 "name": "grid",
"mean": 0.005991517073171,
"sd": 0.01935980560692 
},
{
 "name": "grid",
"mean": 0.006061601219512,
"sd": 0.01858390272707 
},
{
 "name": "grid",
"mean": 0.006116111111111,
"sd": 0.01809379754285 
},
{
 "name": "grid",
"mean": 0.00615971902439,
"sd": 0.01777936551403 
},
{
 "name": "grid",
"mean": 0.005916382926829,
"sd": 0.01758683309806 
},
{
 "name": "grid",
"mean": 0.005987028184282,
"sd": 0.01710327183442 
},
{
 "name": "grid",
"mean": 0.006089784922395,
"sd": 0.01664521287177 
},
{
 "name": "grid",
"mean": 0.005857945257453,
"sd": 0.01624419159723 
},
{
 "name": "grid",
"mean": 0.005984171618625,
"sd": 0.01579112630434 
},
{
 "name": "grid",
"mean": 0.006031506504065,
"sd": 0.01572060484077 
},
{
 "name": "grid",
"mean": 0.005811195121951,
"sd": 0.01519923002148 
},
{
 "name": "grid",
"mean": 0.005878558314856,
"sd": 0.01502720824689 
},
{
 "name": "grid",
"mean": 0.005934694308943,
"sd": 0.01495904482055 
},
{
 "name": "grid",
"mean": 0.005982193996248,
"sd": 0.01495556302874 
},
{
 "name": "grid",
"mean": 0.006441418699187,
"sd": 0.02174619506277 
},
{
 "name": "grid",
"mean": 0.006469111585366,
"sd": 0.01947727287786 
},
{
 "name": "grid",
"mean": 0.006247794308943,
"sd": 0.02084615108904 
},
{
 "name": "grid",
"mean": 0.006323893292683,
"sd": 0.01848463474145 
},
{
 "name": "grid",
"mean": 0.006349259620596,
"sd": 0.0179144827904 
},
{
 "name": "grid",
"mean":    0.006178675,
"sd": 0.01765972026635 
},
{
 "name": "grid",
"mean": 0.006220176693767,
"sd": 0.01705879151703 
},
{
 "name": "grid",
"mean": 0.006033456707317,
"sd": 0.01702692437998 
},
{
 "name": "grid",
"mean": 0.006091093766938,
"sd": 0.01634905188933 
},
{
 "name": "grid",
"mean": 0.006174929490022,
"sd": 0.01571290900331 
},
{
 "name": "grid",
"mean": 0.006172167479675,
"sd": 0.02113332185238 
},
{
 "name": "grid",
"mean": 0.006267173170732,
"sd": 0.01884031902669 
},
{
 "name": "grid",
"mean": 0.006298841734417,
"sd": 0.01827523303681 
},
{
 "name": "grid",
"mean": 0.006121954878049,
"sd": 0.01791665920137 
},
{
 "name": "grid",
"mean": 0.006169758807588,
"sd": 0.01734331865304 
},
{
 "name": "grid",
"mean": 0.005976736585366,
"sd": 0.01717329812365 
},
{
 "name": "grid",
"mean": 0.006040675880759,
"sd": 0.01654728728635 
},
{
 "name": "grid",
"mean": 0.006091827317073,
"sd": 0.01617402385331 
},
{
 "name": "grid",
"mean": 0.006133678492239,
"sd": 0.01595920495132 
},
{
 "name": "grid",
"mean": 0.00591159295393,
"sd": 0.01590755124015 
},
{
 "name": "grid",
"mean": 0.00602806518847,
"sd": 0.01524045873207 
},
{
 "name": "grid",
"mean": 0.006071742276423,
"sd": 0.01512096016771 
},
{
 "name": "grid",
"mean": 0.006065234756098,
"sd": 0.01829767536407 
},
{
 "name": "grid",
"mean": 0.006119340921409,
"sd": 0.01772734807149 
},
{
 "name": "grid",
"mean": 0.005920016463415,
"sd": 0.01745237728173 
},
{
 "name": "grid",
"mean": 0.00599025799458,
"sd": 0.01685270806907 
},
{
 "name": "grid",
"mean": 0.006046451219512,
"sd": 0.01649061060855 
},
{
 "name": "grid",
"mean": 0.006092427494457,
"sd": 0.01627760104782 
},
{
 "name": "grid",
"mean": 0.005861175067751,
"sd": 0.01612402320017 
},
{
 "name": "grid",
"mean": 0.005930276585366,
"sd": 0.01572635186565 
},
{
 "name": "grid",
"mean": 0.005986814190687,
"sd": 0.01550317962739 
},
{
 "name": "grid",
"mean": 0.006033928861789,
"sd": 0.01539070001773 
},
{
 "name": "grid",
"mean": 0.005881200886918,
"sd": 0.01482894536266 
},
{
 "name": "grid",
"mean": 0.005937116666667,
"sd": 0.01470058364902 
},
{
 "name": "grid",
"mean": 0.005984430018762,
"sd": 0.01465753301981 
},
{
 "name": "grid",
"mean": 0.005863296341463,
"sd": 0.01785794128287 
},
{
 "name": "grid",
"mean": 0.005939840108401,
"sd": 0.01725962502063 
},
{
 "name": "grid",
"mean": 0.006051176496674,
"sd": 0.01666396498218 
},
{
 "name": "grid",
"mean": 0.005810757181572,
"sd": 0.01644986005194 
},
{
 "name": "grid",
"mean": 0.005945563192905,
"sd": 0.01583947393747 
},
{
 "name": "grid",
"mean": 0.005996115447154,
"sd": 0.01572176085013 
},
{
 "name": "grid",
"mean": 0.005839949889135,
"sd": 0.01510797470301 
},
{
 "name": "grid",
"mean": 0.005899303252033,
"sd": 0.0149856439506 
},
{
 "name": "grid",
"mean": 0.00594952532833,
"sd": 0.01494267699695 
},
{
 "name": "grid",
"mean": 0.005734336585366,
"sd": 0.0144835639247 
},
{
 "name": "grid",
"mean": 0.005802491056911,
"sd": 0.01433392753566 
},
{
 "name": "grid",
"mean": 0.005860160225141,
"sd": 0.0142811086369 
},
{
 "name": "grid",
"mean": 0.005909590940767,
"sd": 0.01429034427506 
},
{
 "name": "grid",
"mean": 0.006177012195122,
"sd": 0.02212060685982 
},
{
 "name": "grid",
"mean": 0.006230609059233,
"sd": 0.02030824957893 
},
{
 "name": "grid",
"mean": 0.006270806707317,
"sd": 0.01915371562913 
},
{
 "name": "grid",
"mean": 0.006302071544715,
"sd": 0.01840126662391 
},
{
 "name": "grid",
"mean": 0.006064645296167,
"sd": 0.01965304750387 
},
{
 "name": "grid",
"mean": 0.006125588414634,
"sd": 0.0184054114194 
},
{
 "name": "grid",
"mean": 0.006172988617886,
"sd": 0.01760771633144 
},
{
 "name": "grid",
"mean": 0.006210908780488,
"sd": 0.01709438734414 
},
{
 "name": "grid",
"mean": 0.005980370121951,
"sd": 0.01784712728223 
},
{
 "name": "grid",
"mean": 0.006043905691057,
"sd": 0.0169609018256 
},
{
 "name": "grid",
"mean": 0.006136321064302,
"sd": 0.01604888679499 
},
{
 "name": "grid",
"mean": 0.005914822764228,
"sd": 0.01647811161363 
},
{
 "name": "grid",
"mean": 0.005978559512195,
"sd": 0.01583395302787 
},
{
 "name": "grid",
"mean": 0.006030707760532,
"sd": 0.01543482592444 
},
{
 "name": "grid",
"mean": 0.006074164634146,
"sd": 0.0151974158604 
},
{
 "name": "grid",
"mean": 0.005999822299652,
"sd": 0.01984242562254 
},
{
 "name": "grid",
"mean": 0.006068868292683,
"sd": 0.01864760372652 
},
{
 "name": "grid",
"mean": 0.006122570731707,
"sd": 0.01787982260052 
},
{
 "name": "grid",
"mean": 0.006165532682927,
"sd": 0.01738154440413 
},
{
 "name": "grid",
"mean":     0.00592365,
"sd": 0.01798218465598 
},
{
 "name": "grid",
"mean": 0.005993487804878,
"sd": 0.01714822024388 
},
{
 "name": "grid",
"mean": 0.006095070066519,
"sd": 0.01628739233717 
},
{
 "name": "grid",
"mean": 0.005864404878049,
"sd": 0.01657257717206 
},
{
 "name": "grid",
"mean": 0.005989456762749,
"sd": 0.0156127882233 
},
{
 "name": "grid",
"mean": 0.006036351219512,
"sd": 0.01539634794005 
},
{
 "name": "grid",
"mean": 0.005817008780488,
"sd": 0.01547497202792 
},
{
 "name": "grid",
"mean": 0.00588384345898,
"sd": 0.01504659172003 
},
{
 "name": "grid",
"mean": 0.00593953902439,
"sd": 0.01479455811031 
},
{
 "name": "grid",
"mean": 0.005986666041276,
"sd": 0.01465985347503 
},
{
 "name": "grid",
"mean": 0.005866929878049,
"sd": 0.0182443215142 
},
{
 "name": "grid",
"mean": 0.005943069918699,
"sd": 0.01743933938527 
},
{
 "name": "grid",
"mean": 0.006053819068736,
"sd": 0.01659683375807 
},
{
 "name": "grid",
"mean": 0.00581398699187,
"sd": 0.0167765401607 
},
{
 "name": "grid",
"mean": 0.005948205764967,
"sd": 0.01586656297864 
},
{
 "name": "grid",
"mean": 0.005998537804878,
"sd": 0.01565897562099 
},
{
 "name": "grid",
"mean": 0.005842592461197,
"sd": 0.0152381596671 
},
{
 "name": "grid",
"mean": 0.005901725609756,
"sd": 0.01500657139678 
},
{
 "name": "grid",
"mean": 0.005951761350844,
"sd": 0.01488370501136 
},
{
 "name": "grid",
"mean": 0.005736979157428,
"sd": 0.01472467176707 
},
{
 "name": "grid",
"mean": 0.005804913414634,
"sd": 0.01444600450894 
},
{
 "name": "grid",
"mean": 0.005862396247655,
"sd": 0.01429701872182 
},
{
 "name": "grid",
"mean": 0.005911667247387,
"sd": 0.01423475417577 
},
{
 "name": "grid",
"mean": 0.005763569105691,
"sd": 0.01708607969285 
},
{
 "name": "grid",
"mean": 0.005842431219512,
"sd": 0.01653716745335 
},
{
 "name": "grid",
"mean": 0.005906954767184,
"sd": 0.01619258612039 
},
{
 "name": "grid",
"mean": 0.005960724390244,
"sd": 0.01598215916754 
},
{
 "name": "grid",
"mean": 0.005726256585366,
"sd": 0.01588516889916 
},
{
 "name": "grid",
"mean": 0.005801341463415,
"sd": 0.01550698092965 
},
{
 "name": "grid",
"mean": 0.005863912195122,
"sd": 0.01528349852109 
},
{
 "name": "grid",
"mean": 0.005916856660413,
"sd": 0.01516251144031 
},
{
 "name": "grid",
"mean": 0.005695728159645,
"sd": 0.01492962923726 
},
{
 "name": "grid",
"mean":      0.0057671,
"sd": 0.01467097256591 
},
{
 "name": "grid",
"mean": 0.005827491557223,
"sd": 0.01453326966923 
},
{
 "name": "grid",
"mean": 0.005879255749129,
"sd": 0.01447623952149 
},
{
 "name": "grid",
"mean": 0.005670287804878,
"sd": 0.01415576697367 
},
{
 "name": "grid",
"mean": 0.005738126454034,
"sd": 0.01398245710078 
},
{
 "name": "grid",
"mean": 0.005796273867596,
"sd": 0.01390635501553 
},
{
 "name": "grid",
"mean": 0.005846668292683,
"sd": 0.0138954532451 
},
{
 "name": "grid",
"mean": 0.006427756097561,
"sd": 0.01564317004096 
},
{
 "name": "grid",
"mean":      0.0064251,
"sd": 0.01411694423817 
},
{
 "name": "grid",
"mean": 0.006489105691057,
"sd": 0.01505883687726 
},
{
 "name": "grid",
"mean": 0.006553545121951,
"sd": 0.01583401516907 
},
{
 "name": "grid",
"mean": 0.005855729268293,
"sd": 0.01080216758902 
},
{
 "name": "grid",
"mean": 0.006105691056911,
"sd": 0.0115381424445 
},
{
 "name": "grid",
"mean": 0.006263108536585,
"sd": 0.01283537672658 
},
{
 "name": "grid",
"mean": 0.005722276422764,
"sd": 0.009712145509187 
},
{
 "name": "grid",
"mean": 0.00597267195122,
"sd": 0.01049969034931 
},
{
 "name": "grid",
"mean": 0.006088575609756,
"sd": 0.011556919857 
},
{
 "name": "grid",
"mean": 0.006165844715447,
"sd": 0.01246270807502 
},
{
 "name": "grid",
"mean": 0.005682235365854,
"sd": 0.009338406798195 
},
{
 "name": "grid",
"mean": 0.005972220325203,
"sd": 0.01079000089612 
},
{
 "name": "grid",
"mean": 0.006055073170732,
"sd": 0.01161029707051 
},
{
 "name": "grid",
"mean": 0.005633341463415,
"sd": 0.01201709327275 
},
{
 "name": "grid",
"mean": 0.00595593495935,
"sd": 0.01297452060461 
},
{
 "name": "grid",
"mean": 0.006149668292683,
"sd": 0.01406911199924 
},
{
 "name": "grid",
"mean": 0.005572520325203,
"sd": 0.01003546534523 
},
{
 "name": "grid",
"mean": 0.005859231707317,
"sd": 0.01126484550996 
},
{
 "name": "grid",
"mean": 0.005997823414634,
"sd": 0.01235045854749 
},
{
 "name": "grid",
"mean": 0.006090217886179,
"sd": 0.01320101728925 
},
{
 "name": "grid",
"mean": 0.005568795121951,
"sd": 0.009344480652171 
},
{
 "name": "grid",
"mean": 0.005765474146341,
"sd": 0.01032336878395 
},
{
 "name": "grid",
"mean": 0.005896593495935,
"sd": 0.01131463103444 
},
{
 "name": "grid",
"mean": 0.005702969105691,
"sd": 0.00978568688498 
},
{
 "name": "grid",
"mean": 0.005915274390244,
"sd": 0.01141300503093 
},
{
 "name": "grid",
"mean": 0.005422764227642,
"sd": 0.01181360649513 
},
{
 "name": "grid",
"mean": 0.005745791463415,
"sd": 0.01273495780316 
},
{
 "name": "grid",
"mean": 0.005907071219512,
"sd": 0.01354355632082 
},
{
 "name": "grid",
"mean": 0.006014591056911,
"sd": 0.01419486698002 
},
{
 "name": "grid",
"mean": 0.005455354878049,
"sd": 0.01029863848972 
},
{
 "name": "grid",
"mean": 0.00567472195122,
"sd": 0.01126412810022 
},
{
 "name": "grid",
"mean": 0.005820966666667,
"sd": 0.0121613016472 
},
{
 "name": "grid",
"mean": 0.005442372682927,
"sd": 0.009511641342269 
},
{
 "name": "grid",
"mean": 0.005627342276423,
"sd": 0.01040600986539 
},
{
 "name": "grid",
"mean": 0.005759463414634,
"sd": 0.01125946623183 
},
{
 "name": "grid",
"mean": 0.005858554268293,
"sd": 0.0119915392032 
},
{
 "name": "grid",
"mean": 0.005433717886179,
"sd": 0.009091622477529 
},
{
 "name": "grid",
"mean": 0.00571333597561,
"sd": 0.01064248879955 
},
{
 "name": "grid",
"mean": 0.00580654200542,
"sd": 0.01133184354026 
},
{
 "name": "grid",
"mean": 0.005341914634146,
"sd": 0.01197638300065 
},
{
 "name": "grid",
"mean": 0.005745339837398,
"sd": 0.01326850655741 
},
{
 "name": "grid",
"mean": 0.005860604181185,
"sd": 0.01383230585143 
},
{
 "name": "grid",
"mean": 0.005551715447154,
"sd": 0.01136177983539 
},
{
 "name": "grid",
"mean": 0.005801834146341,
"sd": 0.01272771113854 
},
{
 "name": "grid",
"mean": 0.005358091056911,
"sd": 0.009803651879902 
},
{
 "name": "grid",
"mean": 0.005656615853659,
"sd": 0.01128384880093 
},
{
 "name": "grid",
"mean": 0.005756124119241,
"sd": 0.01192005777757 
},
{
 "name": "grid",
"mean": 0.005362712891986,
"sd": 0.009330883622244 
},
{
 "name": "grid",
"mean": 0.005511397560976,
"sd": 0.01002608060476 
},
{
 "name": "grid",
"mean": 0.005627041192412,
"sd": 0.0107024358634 
},
{
 "name": "grid",
"mean": 0.005719556097561,
"sd": 0.01131355928424 
},
{
 "name": "grid",
"mean": 0.005647587804878,
"sd": 0.01743188260541 
},
{
 "name": "grid",
"mean": 0.005965528455285,
"sd": 0.01408636734171 
},
{
 "name": "grid",
"mean": 0.006156935365854,
"sd": 0.01381943542277 
},
{
 "name": "grid",
"mean": 0.005582113821138,
"sd": 0.01309754233952 
},
{
 "name": "grid",
"mean": 0.005866498780488,
"sd": 0.01197141562812 
},
{
 "name": "grid",
"mean": 0.006003637073171,
"sd": 0.01214642938612 
},
{
 "name": "grid",
"mean": 0.006095062601626,
"sd": 0.01265022411906 
},
{
 "name": "grid",
"mean": 0.005576062195122,
"sd": 0.01127468541723 
},
{
 "name": "grid",
"mean": 0.005771287804878,
"sd": 0.01079526790003 
},
{
 "name": "grid",
"mean": 0.005901438211382,
"sd": 0.01114333060832 
},
{
 "name": "grid",
"mean": 0.005707813821138,
"sd": 0.01011455575993 
},
{
 "name": "grid",
"mean": 0.005918907926829,
"sd": 0.0110374014692 
},
{
 "name": "grid",
"mean": 0.005432357723577,
"sd": 0.01329534658719 
},
{
 "name": "grid",
"mean": 0.005753058536585,
"sd": 0.01262141163893 
},
{
 "name": "grid",
"mean": 0.005912884878049,
"sd": 0.01288718944934 
},
{
 "name": "grid",
"mean": 0.006019435772358,
"sd": 0.01336708805354 
},
{
 "name": "grid",
"mean": 0.00546262195122,
"sd": 0.0112500667142 
},
{
 "name": "grid",
"mean": 0.005680535609756,
"sd": 0.01115778927169 
},
{
 "name": "grid",
"mean": 0.005825811382114,
"sd": 0.01163931617468 
},
{
 "name": "grid",
"mean": 0.005448186341463,
"sd": 0.01015140118814 
},
{
 "name": "grid",
"mean": 0.00563218699187,
"sd": 0.0103079141143 
},
{
 "name": "grid",
"mean": 0.005763616027875,
"sd": 0.01083382131876 
},
{
 "name": "grid",
"mean": 0.005862187804878,
"sd": 0.01142542201634 
},
{
 "name": "grid",
"mean": 0.005438562601626,
"sd": 0.009540297697255 
},
{
 "name": "grid",
"mean": 0.005716969512195,
"sd": 0.01028839709786 
},
{
 "name": "grid",
"mean": 0.005809771815718,
"sd": 0.01085214388297 
},
{
 "name": "grid",
"mean": 0.005349181707317,
"sd": 0.01202658013915 
},
{
 "name": "grid",
"mean": 0.005589783414634,
"sd": 0.01201571369331 
},
{
 "name": "grid",
"mean": 0.005750184552846,
"sd": 0.01245203482099 
},
{
 "name": "grid",
"mean": 0.005357434146341,
"sd": 0.01059936826124 
},
{
 "name": "grid",
"mean": 0.005556560162602,
"sd": 0.01088492419616 
},
{
 "name": "grid",
"mean": 0.005698793031359,
"sd": 0.01142792110303 
},
{
 "name": "grid",
"mean": 0.005805467682927,
"sd": 0.01199639925348 
},
{
 "name": "grid",
"mean": 0.005362935772358,
"sd": 0.009792586620492 
},
{
 "name": "grid",
"mean": 0.005532829268293,
"sd": 0.0101723292057 
},
{
 "name": "grid",
"mean": 0.005660249390244,
"sd": 0.01072797740445 
},
{
 "name": "grid",
"mean": 0.005759353929539,
"sd": 0.01129753691417 
},
{
 "name": "grid",
"mean": 0.005515031097561,
"sd": 0.009702156406 
},
{
 "name": "grid",
"mean": 0.00563027100271,
"sd": 0.01023265351752 
},
{
 "name": "grid",
"mean": 0.005480933333333,
"sd": 0.01178938854758 
},
{
 "name": "grid",
"mean": 0.005748747560976,
"sd": 0.01272573029671 
},
{
 "name": "grid",
"mean": 0.005287308943089,
"sd": 0.0104427894496 
},
{
 "name": "grid",
"mean": 0.005603529268293,
"sd": 0.0113571639627 
},
{
 "name": "grid",
"mean": 0.00570893604336,
"sd": 0.01188189747745 
},
{
 "name": "grid",
"mean": 0.00545831097561,
"sd": 0.01019277050469 
},
{
 "name": "grid",
"mean": 0.005579853116531,
"sd": 0.01072311663299 
},
{
 "name": "grid",
"mean": 0.005313092682927,
"sd": 0.009309479225063 
},
{
 "name": "grid",
"mean": 0.005450770189702,
"sd": 0.009747124102498 
},
{
 "name": "grid",
"mean": 0.005651028381375,
"sd": 0.01076612870993 
},
{
 "name": "grid",
"mean": 0.005441951219512,
"sd": 0.01943406837498 
},
{
 "name": "grid",
"mean": 0.005760325609756,
"sd": 0.01582276160103 
},
{
 "name": "grid",
"mean": 0.005918698536585,
"sd": 0.01445172182458 
},
{
 "name": "grid",
"mean": 0.006024280487805,
"sd": 0.01405754057431 
},
{
 "name": "grid",
"mean": 0.00546988902439,
"sd": 0.01552433311992 
},
{
 "name": "grid",
"mean": 0.005686349268293,
"sd": 0.01349936364088 
},
{
 "name": "grid",
"mean": 0.005830656097561,
"sd": 0.01283747302979 
},
{
 "name": "grid",
"mean":       0.005454,
"sd": 0.01325712822768 
},
{
 "name": "grid",
"mean": 0.005637031707317,
"sd": 0.01208187712955 
},
{
 "name": "grid",
"mean": 0.005767768641115,
"sd": 0.0117746212722 
},
{
 "name": "grid",
"mean": 0.005865821341463,
"sd": 0.01186457929697 
},
{
 "name": "grid",
"mean": 0.005443407317073,
"sd": 0.01187971256164 
},
{
 "name": "grid",
"mean": 0.00572060304878,
"sd": 0.01104193731603 
},
{
 "name": "grid",
"mean": 0.005813001626016,
"sd": 0.01121085198119 
},
{
 "name": "grid",
"mean": 0.005356448780488,
"sd": 0.01548490817929 
},
{
 "name": "grid",
"mean": 0.005595597073171,
"sd": 0.01377547626857 
},
{
 "name": "grid",
"mean": 0.005755029268293,
"sd": 0.01325909822043 
},
{
 "name": "grid",
"mean": 0.005363247804878,
"sd": 0.01314142016257 
},
{
 "name": "grid",
"mean": 0.005561404878049,
"sd": 0.01223207741968 
},
{
 "name": "grid",
"mean": 0.005702945644599,
"sd": 0.01206508545177 
},
{
 "name": "grid",
"mean": 0.005809101219512,
"sd": 0.01221954488207 
},
{
 "name": "grid",
"mean": 0.005367780487805,
"sd": 0.01172299128951 
},
{
 "name": "grid",
"mean": 0.005536981881533,
"sd": 0.01122852856586 
},
{
 "name": "grid",
"mean": 0.005663882926829,
"sd": 0.01124003881094 
},
{
 "name": "grid",
"mean": 0.005762583739837,
"sd": 0.0114776622047 
},
{
 "name": "grid",
"mean": 0.005518664634146,
"sd": 0.01054637868715 
},
{
 "name": "grid",
"mean": 0.005633500813008,
"sd": 0.01065026239399 
},
{
 "name": "grid",
"mean": 0.005272495609756,
"sd": 0.01347460799665 
},
{
 "name": "grid",
"mean": 0.00548577804878,
"sd": 0.01271044087473 
},
{
 "name": "grid",
"mean": 0.005638122648084,
"sd": 0.01259261126581 
},
{
 "name": "grid",
"mean": 0.005752381097561,
"sd": 0.01274847450066 
},
{
 "name": "grid",
"mean": 0.005292153658537,
"sd": 0.01191675732749 
},
{
 "name": "grid",
"mean": 0.005472158885017,
"sd": 0.01156229579832 
},
{
 "name": "grid",
"mean": 0.005607162804878,
"sd": 0.01163657402289 
},
{
 "name": "grid",
"mean": 0.005712165853659,
"sd": 0.01189412002183 
},
{
 "name": "grid",
"mean": 0.005306195121951,
"sd": 0.01091210510799 
},
{
 "name": "grid",
"mean": 0.005461944512195,
"sd": 0.01077787286439 
},
{
 "name": "grid",
"mean": 0.005583082926829,
"sd": 0.010949627183 
},
{
 "name": "grid",
"mean": 0.005679993658537,
"sd": 0.01125421832989 
},
{
 "name": "grid",
"mean": 0.005316726219512,
"sd": 0.0102362916902 
},
{
 "name": "grid",
"mean":       0.005454,
"sd": 0.010224188725 
},
{
 "name": "grid",
"mean": 0.00556381902439,
"sd": 0.01044306009823 
},
{
 "name": "grid",
"mean": 0.005653670953437,
"sd": 0.01076476785489 
},
{
 "name": "grid",
"mean": 0.005216526829268,
"sd": 0.01244464992805 
},
{
 "name": "grid",
"mean": 0.005550442682927,
"sd": 0.01221222859461 
},
{
 "name": "grid",
"mean": 0.00566174796748,
"sd": 0.01244521138307 
},
{
 "name": "grid",
"mean": 0.005405224390244,
"sd": 0.01121411045458 
},
{
 "name": "grid",
"mean": 0.00553266504065,
"sd": 0.01140352645894 
},
{
 "name": "grid",
"mean": 0.005260006097561,
"sd": 0.01049954546267 
},
{
 "name": "grid",
"mean": 0.005403582113821,
"sd": 0.01055523568259 
},
{
 "name": "grid",
"mean": 0.005518442926829,
"sd": 0.01080313861713 
},
{
 "name": "grid",
"mean": 0.005612419955654,
"sd": 0.01113195505791 
},
{
 "name": "grid",
"mean": 0.005274499186992,
"sd": 0.009949926643701 
},
{
 "name": "grid",
"mean": 0.005506806651885,
"sd": 0.01034822854766 
},
{
 "name": "grid",
"mean": 0.00559392195122,
"sd": 0.0106856705546 
},
{
 "name": "grid",
"mean": 0.005363715853659,
"sd": 0.02070905609174 
},
{
 "name": "grid",
"mean": 0.00575987398374,
"sd": 0.0154371243945 
},
{
 "name": "grid",
"mean": 0.005873062020906,
"sd": 0.01460600580718 
},
{
 "name": "grid",
"mean": 0.005566249593496,
"sd": 0.01491698598118 
},
{
 "name": "grid",
"mean": 0.005812734756098,
"sd": 0.01334936991225 
},
{
 "name": "grid",
"mean": 0.005372625203252,
"sd": 0.01485640960477 
},
{
 "name": "grid",
"mean": 0.005667516463415,
"sd": 0.01269142086631 
},
{
 "name": "grid",
"mean": 0.005765813550136,
"sd": 0.01242557626411 
},
{
 "name": "grid",
"mean": 0.005375170731707,
"sd": 0.0133334205746 
},
{
 "name": "grid",
"mean": 0.005522298170732,
"sd": 0.0123209206536 
},
{
 "name": "grid",
"mean": 0.005636730623306,
"sd": 0.01186190568854 
},
{
 "name": "grid",
"mean": 0.005728276585366,
"sd": 0.01172856199085 
},
{
 "name": "grid",
"mean": 0.005490622764228,
"sd": 0.01502902330994 
},
{
 "name": "grid",
"mean": 0.005756014634146,
"sd": 0.01365971121424 
},
{
 "name": "grid",
"mean": 0.005296998373984,
"sd": 0.01472131001074 
},
{
 "name": "grid",
"mean": 0.005610796341463,
"sd": 0.01285765354012 
},
{
 "name": "grid",
"mean": 0.005715395663957,
"sd": 0.01266161604909 
},
{
 "name": "grid",
"mean": 0.00546557804878,
"sd": 0.01232547095465 
},
{
 "name": "grid",
"mean": 0.005586312737127,
"sd": 0.01197327302873 
},
{
 "name": "grid",
"mean": 0.005320359756098,
"sd": 0.01209882843687 
},
{
 "name": "grid",
"mean": 0.005457229810298,
"sd": 0.01151596580193 
},
{
 "name": "grid",
"mean": 0.005656313525499,
"sd": 0.01132574922411 
},
{
 "name": "grid",
"mean": 0.005221371544715,
"sd": 0.01486609083522 
},
{
 "name": "grid",
"mean": 0.005554076219512,
"sd": 0.01319938010847 
},
{
 "name": "grid",
"mean": 0.005664977777778,
"sd": 0.01303525720195 
},
{
 "name": "grid",
"mean": 0.005408857926829,
"sd": 0.01251745471306 
},
{
 "name": "grid",
"mean": 0.005535894850949,
"sd": 0.01223493201225 
},
{
 "name": "grid",
"mean": 0.005263639634146,
"sd": 0.01212502145705 
},
{
 "name": "grid",
"mean": 0.005406811924119,
"sd": 0.01164837532245 
},
{
 "name": "grid",
"mean": 0.005521349756098,
"sd": 0.01151049220514 
},
{
 "name": "grid",
"mean": 0.005615062527716,
"sd": 0.01156551341368 
},
{
 "name": "grid",
"mean": 0.00527772899729,
"sd": 0.01130889867519 
},
{
 "name": "grid",
"mean": 0.005509449223947,
"sd": 0.01095528344951 
},
{
 "name": "grid",
"mean": 0.005596344308943,
"sd": 0.01106157380447 
},
{
 "name": "grid",
"mean": 0.00518070174216,
"sd": 0.0135552202988 
},
{
 "name": "grid",
"mean": 0.005352137804878,
"sd": 0.01288849876718 
},
{
 "name": "grid",
"mean": 0.00548547696477,
"sd": 0.01263755077282 
},
{
 "name": "grid",
"mean": 0.005592148292683,
"sd": 0.01261691386064 
},
{
 "name": "grid",
"mean": 0.005206919512195,
"sd": 0.01234130993297 
},
{
 "name": "grid",
"mean": 0.00535639403794,
"sd": 0.01193447362983 
},
{
 "name": "grid",
"mean": 0.005573811529933,
"sd": 0.01190432549194 
},
{
 "name": "grid",
"mean": 0.005227311111111,
"sd": 0.0114617227061 
},
{
 "name": "grid",
"mean": 0.005468198226164,
"sd": 0.01121530308804 
},
{
 "name": "grid",
"mean": 0.005558530894309,
"sd": 0.01134221003261 
},
{
 "name": "grid",
"mean": 0.005243624390244,
"sd": 0.01080902742194 
},
{
 "name": "grid",
"mean": 0.005362584922395,
"sd": 0.01067772637869 
},
{
 "name": "grid",
"mean": 0.005461718699187,
"sd": 0.01073193103898 
},
{
 "name": "grid",
"mean": 0.005545601125704,
"sd": 0.01089264443445 
},
{
 "name": "grid",
"mean": 0.006834863414634,
"sd": 0.02369137300172 
},
{
 "name": "grid",
"mean": 0.006765040650407,
"sd": 0.02002524761409 
},
{
 "name": "grid",
"mean": 0.006762565853659,
"sd": 0.01888085310535 
},
{
 "name": "grid",
"mean": 0.00638162601626,
"sd": 0.01777594129284 
},
{
 "name": "grid",
"mean": 0.006472129268293,
"sd": 0.01659698426888 
},
{
 "name": "grid",
"mean": 0.006488141463415,
"sd": 0.01623694739674 
},
{
 "name": "grid",
"mean": 0.006498816260163,
"sd": 0.01620341897323 
},
{
 "name": "grid",
"mean": 0.006181692682927,
"sd": 0.01502993353399 
},
{
 "name": "grid",
"mean": 0.006255792195122,
"sd": 0.01453571806513 
},
{
 "name": "grid",
"mean": 0.006305191869919,
"sd": 0.01455567473624 
},
{
 "name": "grid",
"mean": 0.006111567479675,
"sd": 0.01323515477897 
},
{
 "name": "grid",
"mean": 0.006221723170732,
"sd": 0.01369100210908 
},
{
 "name": "grid",
"mean": 0.006231869918699,
"sd": 0.01859744761414 
},
{
 "name": "grid",
"mean": 0.00635868902439,
"sd": 0.01748128672599 
},
{
 "name": "grid",
"mean": 0.006397389268293,
"sd": 0.01706571216492 
},
{
 "name": "grid",
"mean": 0.006423189430894,
"sd": 0.01695554963014 
},
{
 "name": "grid",
"mean": 0.006068252439024,
"sd": 0.01547561407535 
},
{
 "name": "grid",
"mean":     0.00616504,
"sd": 0.01510965644637 
},
{
 "name": "grid",
"mean": 0.00622956504065,
"sd": 0.01514782278298 
},
{
 "name": "grid",
"mean": 0.005932690731707,
"sd": 0.0136199478443 
},
{
 "name": "grid",
"mean": 0.006035940650407,
"sd": 0.01361643056237 
},
{
 "name": "grid",
"mean": 0.006109690592334,
"sd": 0.01384241192767 
},
{
 "name": "grid",
"mean": 0.00616500304878,
"sd": 0.0141314238702 
},
{
 "name": "grid",
"mean": 0.005842316260163,
"sd": 0.0124636512245 
},
{
 "name": "grid",
"mean": 0.006019784756098,
"sd": 0.01290994989206 
},
{
 "name": "grid",
"mean": 0.006078940921409,
"sd": 0.01324961763093 
},
{
 "name": "grid",
"mean": 0.005954812195122,
"sd": 0.01648391697793 
},
{
 "name": "grid",
"mean": 0.006074287804878,
"sd": 0.01603867194987 
},
{
 "name": "grid",
"mean": 0.006153938211382,
"sd": 0.01597889324401 
},
{
 "name": "grid",
"mean": 0.005841938536585,
"sd": 0.01427777316658 
},
{
 "name": "grid",
"mean": 0.005960313821138,
"sd": 0.01428021942051 
},
{
 "name": "grid",
"mean": 0.006044867595819,
"sd": 0.01447268313519 
},
{
 "name": "grid",
"mean": 0.006108282926829,
"sd": 0.01471760959483 
},
{
 "name": "grid",
"mean": 0.005766689430894,
"sd": 0.01290383967984 
},
{
 "name": "grid",
"mean": 0.005878903832753,
"sd": 0.01309361883288 
},
{
 "name": "grid",
"mean": 0.005963064634146,
"sd": 0.01339562448532 
},
{
 "name": "grid",
"mean": 0.00602852303523,
"sd": 0.01371906679611 
},
{
 "name": "grid",
"mean": 0.005817846341463,
"sd": 0.01225332257441 
},
{
 "name": "grid",
"mean": 0.005899440108401,
"sd": 0.01259742389113 
},
{
 "name": "grid",
"mean": 0.00588468699187,
"sd": 0.01518952857521 
},
{
 "name": "grid",
"mean": 0.006051562804878,
"sd": 0.0154329586765 
},
{
 "name": "grid",
"mean": 0.005691062601626,
"sd": 0.01363653846552 
},
{
 "name": "grid",
"mean": 0.005906344512195,
"sd": 0.01403125062944 
},
{
 "name": "grid",
"mean": 0.005978105149051,
"sd": 0.01430220451586 
},
{
 "name": "grid",
"mean": 0.005761126219512,
"sd": 0.01278445190599 
},
{
 "name": "grid",
"mean": 0.005849022222222,
"sd": 0.01310603807135 
},
{
 "name": "grid",
"mean": 0.005615907926829,
"sd": 0.0117420127083 
},
{
 "name": "grid",
"mean": 0.005719939295393,
"sd": 0.01204984934041 
},
{
 "name": "grid",
"mean": 0.005871257649667,
"sd": 0.0127490505858 
},
{
 "name": "grid",
"mean": 0.006241463414634,
"sd": 0.01930467418944 
},
{
 "name": "grid",
"mean": 0.006365956097561,
"sd": 0.01722640234206 
},
{
 "name": "grid",
"mean": 0.006403202926829,
"sd": 0.01643384236664 
},
{
 "name": "grid",
"mean": 0.006428034146341,
"sd": 0.01618713412824 
},
{
 "name": "grid",
"mean": 0.006075519512195,
"sd": 0.01593829814894 
},
{
 "name": "grid",
"mean": 0.006170853658537,
"sd": 0.01490296301573 
},
{
 "name": "grid",
"mean": 0.006234409756098,
"sd": 0.01464174893581 
},
{
 "name": "grid",
"mean": 0.005938504390244,
"sd": 0.01393784465001 
},
{
 "name": "grid",
"mean": 0.006040785365854,
"sd": 0.01344332871367 
},
{
 "name": "grid",
"mean": 0.006113843205575,
"sd": 0.01342609238667 
},
{
 "name": "grid",
"mean": 0.006168636585366,
"sd": 0.01359957955323 
},
{
 "name": "grid",
"mean": 0.00584716097561,
"sd": 0.01269055303362 
},
{
 "name": "grid",
"mean": 0.006023418292683,
"sd": 0.01256039493194 
},
{
 "name": "grid",
"mean": 0.006082170731707,
"sd": 0.0127957799472 
},
{
 "name": "grid",
"mean": 0.005962079268293,
"sd": 0.01633882093493 
},
{
 "name": "grid",
"mean": 0.006080101463415,
"sd": 0.01544944659934 
},
{
 "name": "grid",
"mean": 0.006158782926829,
"sd": 0.01522080115491 
},
{
 "name": "grid",
"mean": 0.005847752195122,
"sd": 0.01415150714207 
},
{
 "name": "grid",
"mean": 0.005965158536585,
"sd": 0.0138081155123 
},
{
 "name": "grid",
"mean": 0.006049020209059,
"sd": 0.01384936931474 
},
{
 "name": "grid",
"mean": 0.006111916463415,
"sd": 0.01403692145894 
},
{
 "name": "grid",
"mean": 0.005771534146341,
"sd": 0.01279218881175 
},
{
 "name": "grid",
"mean": 0.005883056445993,
"sd": 0.01270527423904 
},
{
 "name": "grid",
"mean": 0.005966698170732,
"sd": 0.01287304076028 
},
{
 "name": "grid",
"mean": 0.006031752845528,
"sd": 0.01313699451618 
},
{
 "name": "grid",
"mean": 0.005821479878049,
"sd": 0.01192734791904 
},
{
 "name": "grid",
"mean": 0.005902669918699,
"sd": 0.01215242451314 
},
{
 "name": "grid",
"mean":       0.005757,
"sd": 0.01477123749136 
},
{
 "name": "grid",
"mean": 0.005889531707317,
"sd": 0.01445284218759 
},
{
 "name": "grid",
"mean": 0.005984197212544,
"sd": 0.01447180343755 
},
{
 "name": "grid",
"mean": 0.006055196341463,
"sd": 0.01462118510254 
},
{
 "name": "grid",
"mean": 0.005695907317073,
"sd": 0.01321021116762 
},
{
 "name": "grid",
"mean": 0.005818233449477,
"sd": 0.01317768252192 
},
{
 "name": "grid",
"mean": 0.00590997804878,
"sd": 0.01335380566443 
},
{
 "name": "grid",
"mean": 0.00598133495935,
"sd": 0.01360548048568 
},
{
 "name": "grid",
"mean": 0.005652269686411,
"sd": 0.01217144472466 
},
{
 "name": "grid",
"mean": 0.005764759756098,
"sd": 0.01227744408413 
},
{
 "name": "grid",
"mean": 0.00585225203252,
"sd": 0.01252768058554 
},
{
 "name": "grid",
"mean": 0.005922245853659,
"sd": 0.012827670651 
},
{
 "name": "grid",
"mean": 0.005619541463415,
"sd": 0.01144611621247 
},
{
 "name": "grid",
"mean": 0.005723169105691,
"sd": 0.01161860222855 
},
{
 "name": "grid",
"mean": 0.005806071219512,
"sd": 0.01190003581947 
},
{
 "name": "grid",
"mean": 0.005873900221729,
"sd": 0.01221876643139 
},
{
 "name": "grid",
"mean": 0.005620280487805,
"sd": 0.01391613774696 
},
{
 "name": "grid",
"mean": 0.005853257926829,
"sd": 0.01398536244684 
},
{
 "name": "grid",
"mean": 0.005930917073171,
"sd": 0.01418863656035 
},
{
 "name": "grid",
"mean": 0.005708039634146,
"sd": 0.01280105182532 
},
{
 "name": "grid",
"mean": 0.005801834146341,
"sd": 0.01303395465798 
},
{
 "name": "grid",
"mean": 0.005562821341463,
"sd": 0.01183257911423 
},
{
 "name": "grid",
"mean": 0.005672751219512,
"sd": 0.01202771781742 
},
{
 "name": "grid",
"mean": 0.005760695121951,
"sd": 0.01230955700809 
},
{
 "name": "grid",
"mean": 0.005832649223947,
"sd": 0.01261782699974 
},
{
 "name": "grid",
"mean": 0.005543668292683,
"sd": 0.01120997203607 
},
{
 "name": "grid",
"mean": 0.005727035920177,
"sd": 0.01174576964448 
},
{
 "name": "grid",
"mean": 0.005795798780488,
"sd": 0.01206660566539 
},
{
 "name": "grid",
"mean": 0.005969346341463,
"sd": 0.01887149945194 
},
{
 "name": "grid",
"mean": 0.006085915121951,
"sd": 0.01674074909435 
},
{
 "name": "grid",
"mean": 0.006163627642276,
"sd": 0.01580414995524 
},
{
 "name": "grid",
"mean": 0.005853565853659,
"sd": 0.01602486754677 
},
{
 "name": "grid",
"mean": 0.005970003252033,
"sd": 0.01480385256684 
},
{
 "name": "grid",
"mean": 0.0060531728223,
"sd": 0.01431172319801 
},
{
 "name": "grid",
"mean":     0.00611555,
"sd": 0.01417557807486 
},
{
 "name": "grid",
"mean": 0.005776378861789,
"sd": 0.01423101262119 
},
{
 "name": "grid",
"mean": 0.005887209059233,
"sd": 0.01349366703433 
},
{
 "name": "grid",
"mean": 0.005970331707317,
"sd": 0.01324659651883 
},
{
 "name": "grid",
"mean": 0.006034982655827,
"sd": 0.01324780293225 
},
{
 "name": "grid",
"mean": 0.005825113414634,
"sd": 0.01256439654356 
},
{
 "name": "grid",
"mean": 0.005905899728997,
"sd": 0.01245887558548 
},
{
 "name": "grid",
"mean": 0.005762813658537,
"sd": 0.01619785467632 
},
{
 "name": "grid",
"mean": 0.005894376422764,
"sd": 0.01512607436774 
},
{
 "name": "grid",
"mean": 0.005988349825784,
"sd": 0.01470212126531 
},
{
 "name": "grid",
"mean": 0.006058829878049,
"sd": 0.01458994900429 
},
{
 "name": "grid",
"mean": 0.00570075203252,
"sd": 0.01431135166158 
},
{
 "name": "grid",
"mean": 0.005822386062718,
"sd": 0.01371150092721 
},
{
 "name": "grid",
"mean": 0.005913611585366,
"sd": 0.01353724399846 
},
{
 "name": "grid",
"mean": 0.005984564769648,
"sd": 0.01357280261887 
},
{
 "name": "grid",
"mean": 0.005656422299652,
"sd": 0.01304349162525 
},
{
 "name": "grid",
"mean": 0.005768393292683,
"sd": 0.01270880693828 
},
{
 "name": "grid",
"mean": 0.005855481842818,
"sd": 0.01267569146953 
},
{
 "name": "grid",
"mean": 0.005925152682927,
"sd": 0.01279430847082 
},
{
 "name": "grid",
"mean":    0.005623175,
"sd": 0.0121505970269 
},
{
 "name": "grid",
"mean": 0.005726398915989,
"sd": 0.01197250152707 
},
{
 "name": "grid",
"mean": 0.00580897804878,
"sd": 0.01202069190155 
},
{
 "name": "grid",
"mean": 0.005876542793792,
"sd": 0.01218523222256 
},
{
 "name": "grid",
"mean": 0.005625125203252,
"sd": 0.01467608526616 
},
{
 "name": "grid",
"mean": 0.005757563066202,
"sd": 0.01414264671125 
},
{
 "name": "grid",
"mean": 0.005856891463415,
"sd": 0.01398924303003 
},
{
 "name": "grid",
"mean": 0.005934146883469,
"sd": 0.01402203598615 
},
{
 "name": "grid",
"mean": 0.005591599303136,
"sd": 0.01329440224081 
},
{
 "name": "grid",
"mean": 0.005711673170732,
"sd": 0.01303153010292 
},
{
 "name": "grid",
"mean": 0.00580506395664,
"sd": 0.01303083118893 
},
{
 "name": "grid",
"mean": 0.005879776585366,
"sd": 0.01315931179974 
},
{
 "name": "grid",
"mean": 0.005566454878049,
"sd": 0.0123210822856 
},
{
 "name": "grid",
"mean": 0.00567598102981,
"sd": 0.01221487147033 
},
{
 "name": "grid",
"mean": 0.00576360195122,
"sd": 0.01230137506216 
},
{
 "name": "grid",
"mean": 0.005835291796009,
"sd": 0.01248356912109 
},
{
 "name": "grid",
"mean": 0.005546898102981,
"sd": 0.01161116117724 
},
{
 "name": "grid",
"mean": 0.005647427317073,
"sd": 0.01159804675701 
},
{
 "name": "grid",
"mean": 0.005729678492239,
"sd": 0.01173391531505 
},
{
 "name": "grid",
"mean": 0.005798221138211,
"sd": 0.01194475379105 
},
{
 "name": "grid",
"mean": 0.00565495304878,
"sd": 0.01351980286804 
},
{
 "name": "grid",
"mean": 0.005754646070461,
"sd": 0.01351339345188 
},
{
 "name": "grid",
"mean": 0.005509734756098,
"sd": 0.01267432124436 
},
{
 "name": "grid",
"mean": 0.005625563143631,
"sd": 0.01259941312622 
},
{
 "name": "grid",
"mean": 0.005718225853659,
"sd": 0.01269374997707 
},
{
 "name": "grid",
"mean": 0.005794040798226,
"sd": 0.01287099341722 
},
{
 "name": "grid",
"mean": 0.005496480216802,
"sd": 0.0118783026965 
},
{
 "name": "grid",
"mean": 0.005602051219512,
"sd": 0.01190276377444 
},
{
 "name": "grid",
"mean": 0.005688427494457,
"sd": 0.01205490212184 
},
{
 "name": "grid",
"mean": 0.005760407723577,
"sd": 0.01226934014275 
},
{
 "name": "grid",
"mean": 0.005582814190687,
"sd": 0.01136396137405 
},
{
 "name": "grid",
"mean": 0.005663595528455,
"sd": 0.01154807912145 
},
{
 "name": "grid",
"mean": 0.005731948968105,
"sd": 0.01178167052795 
},
{
 "name": "grid",
"mean": 0.005899221138211,
"sd": 0.0170429520264 
},
{
 "name": "grid",
"mean": 0.006062463414634,
"sd": 0.01534401798572 
},
{
 "name": "grid",
"mean": 0.005705596747967,
"sd": 0.01663942616758 
},
{
 "name": "grid",
"mean": 0.005917245121951,
"sd": 0.01454903922179 
},
{
 "name": "grid",
"mean": 0.005987794579946,
"sd": 0.0142087532552 
},
{
 "name": "grid",
"mean": 0.005772026829268,
"sd": 0.01399202030456 
},
{
 "name": "grid",
"mean": 0.005858711653117,
"sd": 0.01352624685787 
},
{
 "name": "grid",
"mean": 0.005626808536585,
"sd": 0.01370201290774 
},
{
 "name": "grid",
"mean": 0.005729628726287,
"sd": 0.01304781569221 
},
{
 "name": "grid",
"mean": 0.005879185365854,
"sd": 0.0126523983686 
},
{
 "name": "grid",
"mean": 0.005629969918699,
"sd": 0.01669930169809 
},
{
 "name": "grid",
"mean":    0.005860525,
"sd": 0.0148085176905 
},
{
 "name": "grid",
"mean": 0.005937376693767,
"sd": 0.0145077015893 
},
{
 "name": "grid",
"mean": 0.005715306707317,
"sd": 0.01411592517922 
},
{
 "name": "grid",
"mean": 0.005808293766938,
"sd": 0.0137214057361 
},
{
 "name": "grid",
"mean": 0.005570088414634,
"sd": 0.01367819164713 
},
{
 "name": "grid",
"mean": 0.005679210840108,
"sd": 0.01312615711584 
},
{
 "name": "grid",
"mean": 0.005766508780488,
"sd": 0.0128900225962 
},
{
 "name": "grid",
"mean": 0.005837934368071,
"sd": 0.01284099321524 
},
{
 "name": "grid",
"mean": 0.005550127913279,
"sd": 0.01274874429777 
},
{
 "name": "grid",
"mean": 0.005732321064302,
"sd": 0.01224042898134 
},
{
 "name": "grid",
"mean": 0.005800643495935,
"sd": 0.01225516080484 
},
{
 "name": "grid",
"mean": 0.005658586585366,
"sd": 0.01440136607638 
},
{
 "name": "grid",
"mean": 0.005757875880759,
"sd": 0.01404544162909 
},
{
 "name": "grid",
"mean": 0.005513368292683,
"sd": 0.013823819497 
},
{
 "name": "grid",
"mean": 0.00562879295393,
"sd": 0.01334265119041 
},
{
 "name": "grid",
"mean": 0.005721132682927,
"sd": 0.01314810237214 
},
{
 "name": "grid",
"mean": 0.005796683370288,
"sd": 0.01312106660648 
},
{
 "name": "grid",
"mean": 0.0054997100271,
"sd": 0.01284498759464 
},
{
 "name": "grid",
"mean": 0.00560495804878,
"sd": 0.01253627055186 
},
{
 "name": "grid",
"mean": 0.005691070066519,
"sd": 0.01244637725551 
},
{
 "name": "grid",
"mean": 0.005762830081301,
"sd": 0.01248616839455 
},
{
 "name": "grid",
"mean": 0.005585456762749,
"sd": 0.01190894554543 
},
{
 "name": "grid",
"mean": 0.005666017886179,
"sd": 0.0118879658578 
},
{
 "name": "grid",
"mean": 0.005734184990619,
"sd": 0.0119708515023 
},
{
 "name": "grid",
"mean": 0.005456648170732,
"sd": 0.01413365961009 
},
{
 "name": "grid",
"mean": 0.005578375067751,
"sd": 0.01369074559019 
},
{
 "name": "grid",
"mean": 0.005755432372506,
"sd": 0.01348692073421 
},
{
 "name": "grid",
"mean": 0.005449292140921,
"sd": 0.01308192513777 
},
{
 "name": "grid",
"mean": 0.005649819068736,
"sd": 0.0127459747288 
},
{
 "name": "grid",
"mean": 0.005725016666667,
"sd": 0.0127941292221 
},
{
 "name": "grid",
"mean": 0.005544205764967,
"sd": 0.01213191955159 
},
{
 "name": "grid",
"mean": 0.005628204471545,
"sd": 0.01213553960114 
},
{
 "name": "grid",
"mean": 0.005699280300188,
"sd": 0.01223106045025 
},
{
 "name": "grid",
"mean": 0.005438592461197,
"sd": 0.01166481168078 
},
{
 "name": "grid",
"mean": 0.005531392276423,
"sd": 0.01159090565346 
},
{
 "name": "grid",
"mean": 0.005609915196998,
"sd": 0.01164199572112 
},
{
 "name": "grid",
"mean": 0.005677220557491,
"sd": 0.01176777502734 
},
{
 "name": "grid",
"mean": 0.007040975609756,
"sd": 0.02759830362319 
},
{
 "name": "grid",
"mean": 0.006971586585366,
"sd": 0.02385838682877 
},
{
 "name": "grid",
"mean": 0.006887707317073,
"sd": 0.02181706181634 
},
{
 "name": "grid",
"mean": 0.006831787804878,
"sd": 0.02066190188612 
},
{
 "name": "grid",
"mean":     0.00668115,
"sd": 0.02220671764137 
},
{
 "name": "grid",
"mean": 0.00665535804878,
"sd": 0.02016576045473 
},
{
 "name": "grid",
"mean": 0.006638163414634,
"sd": 0.01909038826182 
},
{
 "name": "grid",
"mean": 0.006423008780488,
"sd": 0.01890686854717 
},
{
 "name": "grid",
"mean": 0.00644453902439,
"sd": 0.01777516336952 
},
{
 "name": "grid",
"mean": 0.006459917770035,
"sd": 0.01719597999306 
},
{
 "name": "grid",
"mean": 0.006471451829268,
"sd": 0.01690118395315 
},
{
 "name": "grid",
"mean": 0.006250914634146,
"sd": 0.0167766121738 
},
{
 "name": "grid",
"mean": 0.006326233536585,
"sd": 0.01581530255368 
},
{
 "name": "grid",
"mean": 0.006351339837398,
"sd": 0.01569838971518 
},
{
 "name": "grid",
"mean": 0.006567709756098,
"sd": 0.0228082504638 
},
{
 "name": "grid",
"mean": 0.006564605853659,
"sd": 0.0207918530717 
},
{
 "name": "grid",
"mean": 0.006562536585366,
"sd": 0.01969832236396 
},
{
 "name": "grid",
"mean": 0.006332256585366,
"sd": 0.0193009710902 
},
{
 "name": "grid",
"mean": 0.006368912195122,
"sd": 0.01822598156424 
},
{
 "name": "grid",
"mean": 0.006395094773519,
"sd": 0.01765976484408 
},
{
 "name": "grid",
"mean": 0.006414731707317,
"sd": 0.01735725162079 
},
{
 "name": "grid",
"mean": 0.006175287804878,
"sd": 0.01703918068441 
},
{
 "name": "grid",
"mean": 0.006229131010453,
"sd": 0.01644967032187 
},
{
 "name": "grid",
"mean": 0.006269513414634,
"sd": 0.01617445570099 
},
{
 "name": "grid",
"mean": 0.00630092195122,
"sd": 0.0160649678814 
},
{
 "name": "grid",
"mean": 0.006124295121951,
"sd": 0.01515975877822 
},
{
 "name": "grid",
"mean": 0.00617183902439,
"sd": 0.0150530205246 
},
{
 "name": "grid",
"mean": 0.006241504390244,
"sd": 0.01998769505268 
},
{
 "name": "grid",
"mean": 0.006293285365854,
"sd": 0.01888639725697 
},
{
 "name": "grid",
"mean": 0.006330271777003,
"sd": 0.01827883616243 
},
{
 "name": "grid",
"mean": 0.006358011585366,
"sd": 0.01793197251767 
},
{
 "name": "grid",
"mean": 0.00609966097561,
"sd": 0.01753545742099 
},
{
 "name": "grid",
"mean": 0.006164308013937,
"sd": 0.01695403992101 
},
{
 "name": "grid",
"mean": 0.006212793292683,
"sd": 0.01666611920394 
},
{
 "name": "grid",
"mean": 0.006250504065041,
"sd": 0.01653501679099 
},
{
 "name": "grid",
"mean": 0.005998344250871,
"sd": 0.01584416662237 
},
{
 "name": "grid",
"mean":    0.006067575,
"sd": 0.01555087958478 
},
{
 "name": "grid",
"mean": 0.006121421138211,
"sd": 0.01544829761299 
},
{
 "name": "grid",
"mean": 0.00616449804878,
"sd": 0.01544714742199 
},
{
 "name": "grid",
"mean": 0.005922356707317,
"sd": 0.01462075982961 
},
{
 "name": "grid",
"mean": 0.005992338211382,
"sd": 0.01449511894692 
},
{
 "name": "grid",
"mean": 0.006048323414634,
"sd": 0.01450302471302 
},
{
 "name": "grid",
"mean": 0.006094129490022,
"sd": 0.0145812123801 
},
{
 "name": "grid",
"mean": 0.006024034146341,
"sd": 0.01824638270266 
},
{
 "name": "grid",
"mean": 0.006156073170732,
"sd": 0.01727898524669 
},
{
 "name": "grid",
"mean": 0.006200086178862,
"sd": 0.0171000058887 
},
{
 "name": "grid",
"mean": 0.006010854878049,
"sd": 0.01607789523678 
},
{
 "name": "grid",
"mean": 0.006071003252033,
"sd": 0.01594948918405 
},
{
 "name": "grid",
"mean": 0.005865636585366,
"sd": 0.0150432887006 
},
{
 "name": "grid",
"mean": 0.005941920325203,
"sd": 0.01491903697816 
},
{
 "name": "grid",
"mean": 0.006002947317073,
"sd": 0.0149169738393 
},
{
 "name": "grid",
"mean": 0.006052878492239,
"sd": 0.01497979199498 
},
{
 "name": "grid",
"mean": 0.005812837398374,
"sd": 0.01403511947807 
},
{
 "name": "grid",
"mean": 0.00594726518847,
"sd": 0.01409737033071 
},
{
 "name": "grid",
"mean": 0.005997675609756,
"sd": 0.01421769646818 
},
{
 "name": "grid",
"mean": 0.006574976829268,
"sd": 0.0225718078667 
},
{
 "name": "grid",
"mean": 0.006570419512195,
"sd": 0.02024669023849 
},
{
 "name": "grid",
"mean": 0.006567381300813,
"sd": 0.01901892520684 
},
{
 "name": "grid",
"mean": 0.006338070243902,
"sd": 0.01910808054117 
},
{
 "name": "grid",
"mean": 0.006373756910569,
"sd": 0.01778408403202 
},
{
 "name": "grid",
"mean": 0.00639924738676,
"sd": 0.01709576707526 
},
{
 "name": "grid",
"mean": 0.006418365243902,
"sd": 0.01673945887184 
},
{
 "name": "grid",
"mean": 0.006180132520325,
"sd": 0.01687639025631 
},
{
 "name": "grid",
"mean": 0.006233283623693,
"sd": 0.01608179956677 
},
{
 "name": "grid",
"mean": 0.00627314695122,
"sd": 0.01569692988845 
},
{
 "name": "grid",
"mean": 0.006304151761518,
"sd": 0.01553294216972 
},
{
 "name": "grid",
"mean": 0.006127928658537,
"sd": 0.01484736232867 
},
{
 "name": "grid",
"mean": 0.006175068834688,
"sd": 0.0146424392785 
},
{
 "name": "grid",
"mean": 0.00624731804878,
"sd": 0.0194871502291 
},
{
 "name": "grid",
"mean": 0.006298130081301,
"sd": 0.01822653899909 
},
{
 "name": "grid",
"mean": 0.006334424390244,
"sd": 0.01755598739692 
},
{
 "name": "grid",
"mean": 0.006361645121951,
"sd": 0.01719495990988 
},
{
 "name": "grid",
"mean": 0.006104505691057,
"sd": 0.01712876581397 
},
{
 "name": "grid",
"mean": 0.006168460627178,
"sd": 0.01640643098712 
},
{
 "name": "grid",
"mean": 0.006216426829268,
"sd": 0.01605353009319 
},
{
 "name": "grid",
"mean": 0.006253733875339,
"sd": 0.0158991853177 
},
{
 "name": "grid",
"mean": 0.006002496864111,
"sd": 0.01550496017774 
},
{
 "name": "grid",
"mean": 0.006071208536585,
"sd": 0.01508745989882 
},
{
 "name": "grid",
"mean": 0.006124650948509,
"sd": 0.01492131744588 
},
{
 "name": "grid",
"mean": 0.006167404878049,
"sd": 0.01489209744306 
},
{
 "name": "grid",
"mean": 0.005925990243902,
"sd": 0.01433225381922 
},
{
 "name": "grid",
"mean": 0.00599556802168,
"sd": 0.01409690711325 
},
{
 "name": "grid",
"mean": 0.006051230243902,
"sd": 0.0140441772623 
},
{
 "name": "grid",
"mean": 0.006096772062084,
"sd": 0.01409193557967 
},
{
 "name": "grid",
"mean": 0.006028878861789,
"sd": 0.01761409420713 
},
{
 "name": "grid",
"mean": 0.006103637630662,
"sd": 0.01690564014887 
},
{
 "name": "grid",
"mean": 0.006159706707317,
"sd": 0.01654374016725 
},
{
 "name": "grid",
"mean": 0.00620331598916,
"sd": 0.01636996423187 
},
{
 "name": "grid",
"mean": 0.005937673867596,
"sd": 0.01586293604795 
},
{
 "name": "grid",
"mean": 0.006014488414634,
"sd": 0.01547501256919 
},
{
 "name": "grid",
"mean": 0.006074233062331,
"sd": 0.01531568589663 
},
{
 "name": "grid",
"mean": 0.006122028780488,
"sd": 0.01528109176449 
},
{
 "name": "grid",
"mean": 0.005869270121951,
"sd": 0.0145987370997 
},
{
 "name": "grid",
"mean": 0.005945150135501,
"sd": 0.01440070120864 
},
{
 "name": "grid",
"mean": 0.006005854146341,
"sd": 0.01436418292378 
},
{
 "name": "grid",
"mean": 0.006055521064302,
"sd": 0.01441573344349 
},
{
 "name": "grid",
"mean": 0.005816067208672,
"sd": 0.01365304403426 
},
{
 "name": "grid",
"mean": 0.005889679512195,
"sd": 0.01357137632672 
},
{
 "name": "grid",
"mean": 0.005949907760532,
"sd": 0.01361053918901 
},
{
 "name": "grid",
"mean": 0.00600009796748,
"sd": 0.01371555229367 
},
{
 "name": "grid",
"mean": 0.005957768292683,
"sd": 0.0159993084717 
},
{
 "name": "grid",
"mean": 0.006023815176152,
"sd": 0.01581690831088 
},
{
 "name": "grid",
"mean":     0.00581255,
"sd": 0.0150163257827 
},
{
 "name": "grid",
"mean": 0.005894732249322,
"sd": 0.01482286862617 
},
{
 "name": "grid",
"mean": 0.00596047804878,
"sd": 0.01477840494077 
},
{
 "name": "grid",
"mean": 0.006014270066519,
"sd": 0.01481578391134 
},
{
 "name": "grid",
"mean": 0.005765649322493,
"sd": 0.0139812637264 
},
{
 "name": "grid",
"mean": 0.005844303414634,
"sd": 0.01391428739673 
},
{
 "name": "grid",
"mean": 0.005908656762749,
"sd": 0.01395542124806 
},
{
 "name": "grid",
"mean": 0.005962284552846,
"sd": 0.01405518374649 
},
{
 "name": "grid",
"mean": 0.00580304345898,
"sd": 0.01319700234285 
},
{
 "name": "grid",
"mean": 0.005865472357724,
"sd": 0.01329144169982 
},
{
 "name": "grid",
"mean": 0.005918296810507,
"sd": 0.01343019995252 
},
{
 "name": "grid",
"mean": 0.006253131707317,
"sd": 0.02049660879971 
},
{
 "name": "grid",
"mean": 0.006302974796748,
"sd": 0.01869406472491 
},
{
 "name": "grid",
"mean": 0.006338577003484,
"sd": 0.01769133835255 
},
{
 "name": "grid",
"mean": 0.006365278658537,
"sd": 0.01712490502407 
},
{
 "name": "grid",
"mean": 0.006109350406504,
"sd": 0.0179178006232 
},
{
 "name": "grid",
"mean": 0.006172613240418,
"sd": 0.0167802384674 
},
{
 "name": "grid",
"mean": 0.006220060365854,
"sd": 0.01616034793683 
},
{
 "name": "grid",
"mean": 0.006256963685637,
"sd": 0.01583405297789 
},
{
 "name": "grid",
"mean": 0.006006649477352,
"sd": 0.01613827380684 
},
{
 "name": "grid",
"mean": 0.006074842073171,
"sd": 0.01539213287332 
},
{
 "name": "grid",
"mean": 0.006127880758808,
"sd": 0.01500657708985 
},
{
 "name": "grid",
"mean": 0.006170311707317,
"sd": 0.01483121668834 
},
{
 "name": "grid",
"mean": 0.005929623780488,
"sd": 0.01485076088742 
},
{
 "name": "grid",
"mean": 0.005998797831978,
"sd": 0.01434896977088 
},
{
 "name": "grid",
"mean": 0.006054137073171,
"sd": 0.0141127716466 
},
{
 "name": "grid",
"mean": 0.006099414634146,
"sd": 0.0140347857567 
},
{
 "name": "grid",
"mean": 0.006033723577236,
"sd": 0.01814752757981 
},
{
 "name": "grid",
"mean": 0.006107790243902,
"sd": 0.01708522691143 
},
{
 "name": "grid",
"mean": 0.006163340243902,
"sd": 0.01650188062437 
},
{
 "name": "grid",
"mean": 0.006206545799458,
"sd": 0.01618941268124 
},
{
 "name": "grid",
"mean": 0.005941826480836,
"sd": 0.01629023375821 
},
{
 "name": "grid",
"mean": 0.00601812195122,
"sd": 0.01561851563501 
},
{
 "name": "grid",
"mean": 0.006077462872629,
"sd": 0.01527449148705 
},
{
 "name": "grid",
"mean": 0.006124935609756,
"sd": 0.01512000797338 
},
{
 "name": "grid",
"mean": 0.005872903658537,
"sd": 0.01494758952929 
},
{
 "name": "grid",
"mean": 0.005948379945799,
"sd": 0.01451683686955 
},
{
 "name": "grid",
"mean": 0.00600876097561,
"sd": 0.0143238826105 
},
{
 "name": "grid",
"mean": 0.006058163636364,
"sd": 0.01427074777864 
},
{
 "name": "grid",
"mean": 0.00581929701897,
"sd": 0.01394211140722 
},
{
 "name": "grid",
"mean": 0.005892586341463,
"sd": 0.01366627614767 
},
{
 "name": "grid",
"mean": 0.005952550332594,
"sd": 0.01357127012248 
},
{
 "name": "grid",
"mean": 0.006002520325203,
"sd": 0.01358409028824 
},
{
 "name": "grid",
"mean": 0.005877003484321,
"sd": 0.01662475687065 
},
{
 "name": "grid",
"mean": 0.005961401829268,
"sd": 0.01598798288186 
},
{
 "name": "grid",
"mean": 0.00602704498645,
"sd": 0.01565575479782 
},
{
 "name": "grid",
"mean": 0.006079559512195,
"sd": 0.0154998373929 
},
{
 "name": "grid",
"mean": 0.005816183536585,
"sd": 0.01519779774425 
},
{
 "name": "grid",
"mean": 0.005897962059621,
"sd": 0.01480756582886 
},
{
 "name": "grid",
"mean": 0.005963384878049,
"sd": 0.01463412340942 
},
{
 "name": "grid",
"mean": 0.006016912638581,
"sd": 0.0145875506151 
},
{
 "name": "grid",
"mean": 0.005768879132791,
"sd": 0.01412943058115 
},
{
 "name": "grid",
"mean": 0.005847210243902,
"sd": 0.01389621155204 
},
{
 "name": "grid",
"mean": 0.005911299334812,
"sd": 0.01382514688982 
},
{
 "name": "grid",
"mean": 0.005964706910569,
"sd": 0.01384973627189 
},
{
 "name": "grid",
"mean": 0.005731035609756,
"sd": 0.01330737922419 
},
{
 "name": "grid",
"mean": 0.005805686031042,
"sd": 0.01317700646677 
},
{
 "name": "grid",
"mean": 0.005867894715447,
"sd": 0.01317297742788 
},
{
 "name": "grid",
"mean": 0.005920532833021,
"sd": 0.01324406360585 
},
{
 "name": "grid",
"mean": 0.005759463414634,
"sd": 0.01559400428218 
},
{
 "name": "grid",
"mean": 0.005847544173442,
"sd": 0.01521411491748 
},
{
 "name": "grid",
"mean": 0.005918008780488,
"sd": 0.01503735975222 
},
{
 "name": "grid",
"mean": 0.005975661640798,
"sd": 0.014980066208 
},
{
 "name": "grid",
"mean": 0.005718461246612,
"sd": 0.0144422636893 
},
{
 "name": "grid",
"mean": 0.005801834146341,
"sd": 0.01422754204654 
},
{
 "name": "grid",
"mean": 0.005870048337029,
"sd": 0.01416168634643 
},
{
 "name": "grid",
"mean": 0.005926893495935,
"sd": 0.01418353930417 
},
{
 "name": "grid",
"mean": 0.005685659512195,
"sd": 0.01355574549168 
},
{
 "name": "grid",
"mean": 0.005764435033259,
"sd": 0.01344860944768 
},
{
 "name": "grid",
"mean": 0.005830081300813,
"sd": 0.01345537715961 
},
{
 "name": "grid",
"mean": 0.005885628142589,
"sd": 0.0135293179558 
},
{
 "name": "grid",
"mean": 0.00565882172949,
"sd": 0.01285836658758 
},
{
 "name": "grid",
"mean": 0.005733269105691,
"sd": 0.01282254870996 
},
{
 "name": "grid",
"mean": 0.0057962630394,
"sd": 0.01287714559028 
},
{
 "name": "grid",
"mean": 0.005850257839721,
"sd": 0.0129851885075 
},
{
 "name": "grid",
"mean": 0.006038568292683,
"sd": 0.01975246337292 
},
{
 "name": "grid",
"mean": 0.006166973780488,
"sd": 0.01715848208361 
},
{
 "name": "grid",
"mean": 0.006209775609756,
"sd": 0.01657631634067 
},
{
 "name": "grid",
"mean": 0.006021755487805,
"sd": 0.01648892856188 
},
{
 "name": "grid",
"mean": 0.006080692682927,
"sd": 0.01583053291407 
},
{
 "name": "grid",
"mean": 0.005876537195122,
"sd": 0.01603815632855 
},
{
 "name": "grid",
"mean": 0.005951609756098,
"sd": 0.01525295828834 
},
{
 "name": "grid",
"mean": 0.006011667804878,
"sd": 0.01480025994491 
},
{
 "name": "grid",
"mean": 0.006060806208426,
"sd": 0.01455736179932 
},
{
 "name": "grid",
"mean": 0.005822526829268,
"sd": 0.01486321486196 
},
{
 "name": "grid",
"mean": 0.005955192904656,
"sd": 0.01398333427355 
},
{
 "name": "grid",
"mean": 0.006004942682927,
"sd": 0.01383388215855 
},
{
 "name": "grid",
"mean": 0.005965035365854,
"sd": 0.01669544966576 
},
{
 "name": "grid",
"mean": 0.006030274796748,
"sd": 0.01608062668293 
},
{
 "name": "grid",
"mean": 0.005819817073171,
"sd": 0.01612267938004 
},
{
 "name": "grid",
"mean": 0.005901191869919,
"sd": 0.01540669996123 
},
{
 "name": "grid",
"mean": 0.005966291707317,
"sd": 0.01499813664283 
},
{
 "name": "grid",
"mean": 0.006019555210643,
"sd": 0.01478199857718 
},
{
 "name": "grid",
"mean": 0.005772108943089,
"sd": 0.01491179761665 
},
{
 "name": "grid",
"mean": 0.005850117073171,
"sd": 0.0144094488675 
},
{
 "name": "grid",
"mean": 0.005913941906874,
"sd": 0.01413991724823 
},
{
 "name": "grid",
"mean": 0.005967129268293,
"sd": 0.01401855384243 
},
{
 "name": "grid",
"mean": 0.005808328603104,
"sd": 0.01362085889218 
},
{
 "name": "grid",
"mean": 0.005870317073171,
"sd": 0.0134473007905 
},
{
 "name": "grid",
"mean": 0.005922768855535,
"sd": 0.01339155673507 
},
{
 "name": "grid",
"mean": 0.00576309695122,
"sd": 0.01634981382469 
},
{
 "name": "grid",
"mean": 0.00585077398374,
"sd": 0.01567673131467 
},
{
 "name": "grid",
"mean": 0.005920915609756,
"sd": 0.01529121239502 
},
{
 "name": "grid",
"mean": 0.00597830421286,
"sd": 0.01508514306119 
},
{
 "name": "grid",
"mean": 0.005721691056911,
"sd": 0.01508270844889 
},
{
 "name": "grid",
"mean": 0.00580474097561,
"sd": 0.0146240530377 
},
{
 "name": "grid",
"mean": 0.005872690909091,
"sd": 0.01438068990101 
},
{
 "name": "grid",
"mean": 0.005929315853659,
"sd": 0.01427351794899 
},
{
 "name": "grid",
"mean": 0.005688566341463,
"sd": 0.01410458070167 
},
{
 "name": "grid",
"mean": 0.005767077605322,
"sd": 0.01379158084501 
},
{
 "name": "grid",
"mean": 0.005832503658537,
"sd": 0.01364571761238 
},
{
 "name": "grid",
"mean": 0.005887864165103,
"sd": 0.01360676540144 
},
{
 "name": "grid",
"mean": 0.005661464301552,
"sd": 0.01333311372871 
},
{
 "name": "grid",
"mean": 0.005735691463415,
"sd": 0.01312151276611 
},
{
 "name": "grid",
"mean": 0.005798499061914,
"sd": 0.01304362297435 
},
{
 "name": "grid",
"mean": 0.005852334146341,
"sd": 0.01305213161015 
},
{
 "name": "grid",
"mean": 0.005671273170732,
"sd": 0.01537186761639 
},
{
 "name": "grid",
"mean": 0.005831439911308,
"sd": 0.01470151638889 
},
{
 "name": "grid",
"mean": 0.005891502439024,
"sd": 0.01459509109247 
},
{
 "name": "grid",
"mean": 0.005725826607539,
"sd": 0.01404816597673 
},
{
 "name": "grid",
"mean": 0.005794690243902,
"sd": 0.01391586239922 
},
{
 "name": "grid",
"mean": 0.005852959474672,
"sd": 0.0138822924961 
},
{
 "name": "grid",
"mean": 0.005620213303769,
"sd": 0.0135176961724 
},
{
 "name": "grid",
"mean": 0.00569787804878,
"sd": 0.0133334902493 
},
{
 "name": "grid",
"mean": 0.005763594371482,
"sd": 0.01327193223268 
},
{
 "name": "grid",
"mean": 0.005819922648084,
"sd": 0.01328905401477 
},
{
 "name": "grid",
"mean": 0.005601065853659,
"sd": 0.01286113881972 
},
{
 "name": "grid",
"mean": 0.005674229268293,
"sd": 0.0127494835435 
},
{
 "name": "grid",
"mean": 0.005736940766551,
"sd": 0.01273755030494 
},
{
 "name": "grid",
"mean": 0.005791290731707,
"sd": 0.01279002044869 
},
{
 "name": "grid",
"mean": 0.007180607317073,
"sd": 0.0298148295422 
},
{
 "name": "grid",
"mean": 0.00697113495935,
"sd": 0.02396142504565 
},
{
 "name": "grid",
"mean": 0.006911285714286,
"sd": 0.02254636799562 
},
{
 "name": "grid",
"mean": 0.006777510569106,
"sd": 0.02266881675064 
},
{
 "name": "grid",
"mean": 0.006721180487805,
"sd": 0.02039590855418 
},
{
 "name": "grid",
"mean": 0.006583886178862,
"sd": 0.02162372285064 
},
{
 "name": "grid",
"mean": 0.006575962195122,
"sd": 0.0193348397815 
},
{
 "name": "grid",
"mean": 0.006573320867209,
"sd": 0.0187536389867 
},
{
 "name": "grid",
"mean": 0.006413394425087,
"sd": 0.01935799511487 
},
{
 "name": "grid",
"mean": 0.006430743902439,
"sd": 0.01842675795133 
},
{
 "name": "grid",
"mean": 0.006444237940379,
"sd": 0.01783920256273 
},
{
 "name": "grid",
"mean": 0.006455033170732,
"sd": 0.01746447116289 
},
{
 "name": "grid",
"mean": 0.006701883739837,
"sd": 0.02315377584695 
},
{
 "name": "grid",
"mean": 0.006664460365854,
"sd": 0.02085636146408 
},
{
 "name": "grid",
"mean": 0.006508259349593,
"sd": 0.02196489162771 
},
{
 "name": "grid",
"mean": 0.006519242073171,
"sd": 0.01971537001163 
},
{
 "name": "grid",
"mean": 0.00652290298103,
"sd": 0.01913128039317 
},
{
 "name": "grid",
"mean": 0.006374023780488,
"sd": 0.01871550831438 
},
{
 "name": "grid",
"mean": 0.006393820054201,
"sd": 0.01814598984046 
},
{
 "name": "grid",
"mean": 0.006228805487805,
"sd": 0.01788046766252 
},
{
 "name": "grid",
"mean": 0.006264737127371,
"sd": 0.0172847753929 
},
{
 "name": "grid",
"mean": 0.006317001330377,
"sd": 0.01669074257421 
},
{
 "name": "grid",
"mean": 0.006432632520325,
"sd": 0.02248571023371 
},
{
 "name": "grid",
"mean": 0.00646252195122,
"sd": 0.02020427899612 
},
{
 "name": "grid",
"mean": 0.006472485094851,
"sd": 0.0195957300856 
},
{
 "name": "grid",
"mean": 0.006317303658537,
"sd": 0.0191220392237 
},
{
 "name": "grid",
"mean": 0.006343402168022,
"sd": 0.01854714480381 
},
{
 "name": "grid",
"mean": 0.006172085365854,
"sd": 0.01819225972734 
},
{
 "name": "grid",
"mean": 0.006214319241192,
"sd": 0.0176129491694 
},
{
 "name": "grid",
"mean": 0.006248106341463,
"sd": 0.01724845496699 
},
{
 "name": "grid",
"mean": 0.006275750332594,
"sd": 0.0170200681156 
},
{
 "name": "grid",
"mean": 0.006085236314363,
"sd": 0.01681222269062 
},
{
 "name": "grid",
"mean": 0.006170137028825,
"sd": 0.01620815414056 
},
{
 "name": "grid",
"mean": 0.006201974796748,
"sd": 0.01608341101281 
},
{
 "name": "grid",
"mean": 0.00621892543554,
"sd": 0.0205494599216 
},
{
 "name": "grid",
"mean": 0.006260583536585,
"sd": 0.01963903781803 
},
{
 "name": "grid",
"mean": 0.006292984281843,
"sd": 0.0190367026379 
},
{
 "name": "grid",
"mean": 0.006318904878049,
"sd": 0.01862824401757 
},
{
 "name": "grid",
"mean": 0.006115365243902,
"sd": 0.01862425284091 
},
{
 "name": "grid",
"mean": 0.006163901355014,
"sd": 0.01803741473637 
},
{
 "name": "grid",
"mean": 0.006234499334812,
"sd": 0.01741401474394 
},
{
 "name": "grid",
"mean": 0.006034818428184,
"sd": 0.01716147047034 
},
{
 "name": "grid",
"mean": 0.006128886031042,
"sd": 0.01655543338267 
},
{
 "name": "grid",
"mean": 0.006164161382114,
"sd": 0.01642194191475 
},
{
 "name": "grid",
"mean": 0.00597038097561,
"sd": 0.01601920195271 
},
{
 "name": "grid",
"mean": 0.006023272727273,
"sd": 0.01578219245849 
},
{
 "name": "grid",
"mean": 0.006067349186992,
"sd": 0.01565526164007 
},
{
 "name": "grid",
"mean": 0.006104644652908,
"sd": 0.01559927029647 
},
{
 "name": "grid",
"mean": 0.006706728455285,
"sd": 0.02256005124292 
},
{
 "name": "grid",
"mean": 0.006668093902439,
"sd": 0.02018927906301 
},
{
 "name": "grid",
"mean": 0.006513104065041,
"sd": 0.02158023793053 
},
{
 "name": "grid",
"mean": 0.006522875609756,
"sd": 0.01916142691619 
},
{
 "name": "grid",
"mean": 0.006526132791328,
"sd": 0.01855274834696 
},
{
 "name": "grid",
"mean": 0.006377657317073,
"sd": 0.01829152835166 
},
{
 "name": "grid",
"mean": 0.006397049864499,
"sd": 0.01766618325295 
},
{
 "name": "grid",
"mean": 0.00623243902439,
"sd": 0.01760301602027 
},
{
 "name": "grid",
"mean": 0.006267966937669,
"sd": 0.01691741840567 
},
{
 "name": "grid",
"mean": 0.006319643902439,
"sd": 0.01624077317213 
},
{
 "name": "grid",
"mean": 0.006437477235772,
"sd": 0.02191531088748 
},
{
 "name": "grid",
"mean": 0.006466155487805,
"sd": 0.01954106099871 
},
{
 "name": "grid",
"mean": 0.006475714905149,
"sd": 0.01893091211226 
},
{
 "name": "grid",
"mean": 0.006320937195122,
"sd": 0.01857788606059 
},
{
 "name": "grid",
"mean": 0.00634663197832,
"sd": 0.01797225299958 
},
{
 "name": "grid",
"mean": 0.006175718902439,
"sd": 0.01778451774999 
},
{
 "name": "grid",
"mean": 0.006217549051491,
"sd": 0.01714175538398 
},
{
 "name": "grid",
"mean": 0.006251013170732,
"sd": 0.01674419507115 
},
{
 "name": "grid",
"mean": 0.006278392904656,
"sd": 0.01650190452046 
},
{
 "name": "grid",
"mean": 0.006088466124661,
"sd": 0.01645883144978 
},
{
 "name": "grid",
"mean": 0.006172779600887,
"sd": 0.01576153923513 
},
{
 "name": "grid",
"mean": 0.006204397154472,
"sd": 0.01562006898384 
},
{
 "name": "grid",
"mean": 0.006264217073171,
"sd": 0.01898296555491 
},
{
 "name": "grid",
"mean": 0.006296214092141,
"sd": 0.01837361039377 
},
{
 "name": "grid",
"mean": 0.006118998780488,
"sd": 0.0180933484996 
},
{
 "name": "grid",
"mean": 0.006167131165312,
"sd": 0.01746883921853 
},
{
 "name": "grid",
"mean": 0.006205637073171,
"sd": 0.01707676974631 
},
{
 "name": "grid",
"mean": 0.006237141906874,
"sd": 0.01683230085989 
},
{
 "name": "grid",
"mean": 0.006038048238482,
"sd": 0.01670169403589 
},
{
 "name": "grid",
"mean": 0.006089462439024,
"sd": 0.01628572493569 
},
{
 "name": "grid",
"mean": 0.006131528603104,
"sd": 0.01603909416075 
},
{
 "name": "grid",
"mean": 0.006166583739837,
"sd": 0.01590114750323 
},
{
 "name": "grid",
"mean": 0.006025915299335,
"sd": 0.01534077866603 
},
{
 "name": "grid",
"mean": 0.006069771544715,
"sd": 0.01519379609871 
},
{
 "name": "grid",
"mean": 0.006106880675422,
"sd": 0.0151298912284 
},
{
 "name": "grid",
"mean": 0.006062278658537,
"sd": 0.01852314060829 
},
{
 "name": "grid",
"mean": 0.006116713279133,
"sd": 0.01789303617357 
},
{
 "name": "grid",
"mean": 0.006195890909091,
"sd": 0.01722797758109 
},
{
 "name": "grid",
"mean": 0.005987630352304,
"sd": 0.01704933630708 
},
{
 "name": "grid",
"mean": 0.006090277605322,
"sd": 0.01638726102177 
},
{
 "name": "grid",
"mean": 0.006128770325203,
"sd": 0.01624119254639 
},
{
 "name": "grid",
"mean": 0.005984664301552,
"sd": 0.01563464727392 
},
{
 "name": "grid",
"mean": 0.006031958130081,
"sd": 0.01549011075166 
},
{
 "name": "grid",
"mean": 0.006071975984991,
"sd": 0.01542369935011 
},
{
 "name": "grid",
"mean": 0.005879050997783,
"sd": 0.01498341837788 
},
{
 "name": "grid",
"mean": 0.005935145934959,
"sd": 0.01481914034167 
},
{
 "name": "grid",
"mean": 0.005982610881801,
"sd": 0.01474822516163 
},
{
 "name": "grid",
"mean": 0.006023295121951,
"sd": 0.01473789646372 
},
{
 "name": "grid",
"mean": 0.00644232195122,
"sd": 0.02228688285499 
},
{
 "name": "grid",
"mean": 0.00646978902439,
"sd": 0.01946735621523 
},
{
 "name": "grid",
"mean": 0.006478944715447,
"sd": 0.01874358705447 
},
{
 "name": "grid",
"mean": 0.006324570731707,
"sd": 0.01865765351647 
},
{
 "name": "grid",
"mean": 0.006349861788618,
"sd": 0.01790427328433 
},
{
 "name": "grid",
"mean": 0.006179352439024,
"sd": 0.01803065575805 
},
{
 "name": "grid",
"mean": 0.006220778861789,
"sd": 0.01720521442169 
},
{
 "name": "grid",
"mean":     0.00625392,
"sd": 0.01668104186912 
},
{
 "name": "grid",
"mean": 0.006281035476718,
"sd": 0.0163512891202 
},
{
 "name": "grid",
"mean": 0.006091695934959,
"sd": 0.01666407093232 
},
{
 "name": "grid",
"mean": 0.006175422172949,
"sd": 0.01570253630546 
},
{
 "name": "grid",
"mean": 0.006206819512195,
"sd": 0.01548336696885 
},
{
 "name": "grid",
"mean": 0.006267850609756,
"sd": 0.01893406649277 
},
{
 "name": "grid",
"mean": 0.006299443902439,
"sd": 0.01820271674592 
},
{
 "name": "grid",
"mean": 0.006122632317073,
"sd": 0.01820330859561 
},
{
 "name": "grid",
"mean": 0.00617036097561,
"sd": 0.01742206025032 
},
{
 "name": "grid",
"mean": 0.006208543902439,
"sd": 0.01692387684972 
},
{
 "name": "grid",
"mean": 0.006239784478936,
"sd": 0.01660802408237 
},
{
 "name": "grid",
"mean": 0.00604127804878,
"sd": 0.01679085837842 
},
{
 "name": "grid",
"mean": 0.006092369268293,
"sd": 0.01624091640605 
},
{
 "name": "grid",
"mean": 0.006134171175166,
"sd": 0.01590108101526 
},
{
 "name": "grid",
"mean": 0.006169006097561,
"sd": 0.01569874071275 
},
{
 "name": "grid",
"mean": 0.006028557871397,
"sd": 0.01529781194519 
},
{
 "name": "grid",
"mean": 0.006072193902439,
"sd": 0.01506828839446 
},
{
 "name": "grid",
"mean": 0.006109116697936,
"sd": 0.01494602302775 
},
{
 "name": "grid",
"mean": 0.006065912195122,
"sd": 0.01850063864646 
},
{
 "name": "grid",
"mean": 0.006119943089431,
"sd": 0.01774025922309 
},
{
 "name": "grid",
"mean": 0.006163167804878,
"sd": 0.01724989051681 
},
{
 "name": "grid",
"mean": 0.006198533481153,
"sd": 0.01693374240415 
},
{
 "name": "grid",
"mean": 0.005990860162602,
"sd": 0.01702511157081 
},
{
 "name": "grid",
"mean": 0.006046993170732,
"sd": 0.01650036720069 
},
{
 "name": "grid",
"mean": 0.006092920177384,
"sd": 0.01617351131774 
},
{
 "name": "grid",
"mean": 0.006131192682927,
"sd": 0.01597611259946 
},
{
 "name": "grid",
"mean": 0.005930818536585,
"sd": 0.01587450885709 
},
{
 "name": "grid",
"mean": 0.005987306873614,
"sd": 0.01551045016119 
},
{
 "name": "grid",
"mean": 0.006034380487805,
"sd": 0.01529710027302 
},
{
 "name": "grid",
"mean": 0.006074212007505,
"sd": 0.01518333121396 
},
{
 "name": "grid",
"mean": 0.005881693569845,
"sd": 0.01495748706781 
},
{
 "name": "grid",
"mean": 0.005937568292683,
"sd": 0.01470586866977 
},
{
 "name": "grid",
"mean": 0.005984846904315,
"sd": 0.01457281002181 
},
{
 "name": "grid",
"mean": 0.006025371428571,
"sd": 0.01451871216515 
},
{
 "name": "grid",
"mean": 0.005940442276423,
"sd": 0.01736248129675 
},
{
 "name": "grid",
"mean": 0.006051669179601,
"sd": 0.01651617144189 
},
{
 "name": "grid",
"mean": 0.006093379268293,
"sd": 0.01631232031826 
},
{
 "name": "grid",
"mean": 0.005946055875831,
"sd": 0.01579836820551 
},
{
 "name": "grid",
"mean": 0.005996567073171,
"sd": 0.01558907133018 
},
{
 "name": "grid",
"mean": 0.006039307317073,
"sd": 0.01547408144338 
},
{
 "name": "grid",
"mean": 0.005840442572062,
"sd": 0.01518399539446 
},
{
 "name": "grid",
"mean": 0.005899754878049,
"sd": 0.01494799950852 
},
{
 "name": "grid",
"mean": 0.005949942213884,
"sd": 0.0148227712095 
},
{
 "name": "grid",
"mean": 0.005992959930314,
"sd": 0.01477122009753 
},
{
 "name": "grid",
"mean": 0.005802942682927,
"sd": 0.0144000842801 
},
{
 "name": "grid",
"mean": 0.005860577110694,
"sd": 0.01424643468865 
},
{
 "name": "grid",
"mean": 0.00590997804878,
"sd": 0.01418137946049 
},
{
 "name": "grid",
"mean": 0.005952792195122,
"sd": 0.01417550129386 
},
{
 "name": "grid",
"mean": 0.006231383275261,
"sd": 0.02094632036437 
},
{
 "name": "grid",
"mean": 0.006271484146341,
"sd": 0.01949690961408 
},
{
 "name": "grid",
"mean": 0.006302673712737,
"sd": 0.01853763904638 
},
{
 "name": "grid",
"mean": 0.006327625365854,
"sd": 0.01789394154545 
},
{
 "name": "grid",
"mean": 0.006126265853659,
"sd": 0.01894297615421 
},
{
 "name": "grid",
"mean": 0.006173590785908,
"sd": 0.01790116894341 
},
{
 "name": "grid",
"mean": 0.006242427050998,
"sd": 0.01675554369605 
},
{
 "name": "grid",
"mean": 0.006044507859079,
"sd": 0.01742053652882 
},
{
 "name": "grid",
"mean": 0.006136813747228,
"sd": 0.01615109539188 
},
{
 "name": "grid",
"mean": 0.006171428455285,
"sd": 0.01582694159945 
},
{
 "name": "grid",
"mean": 0.005979101463415,
"sd": 0.01623161659839 
},
{
 "name": "grid",
"mean": 0.006031200443459,
"sd": 0.01565657304418 
},
{
 "name": "grid",
"mean": 0.006074616260163,
"sd": 0.01528701549574 
},
{
 "name": "grid",
"mean": 0.00611135272045,
"sd": 0.01505812807326 
},
{
 "name": "grid",
"mean": 0.006069545731707,
"sd": 0.01910301652913 
},
{
 "name": "grid",
"mean": 0.006123172899729,
"sd": 0.01810603980853 
},
{
 "name": "grid",
"mean": 0.006201176053215,
"sd": 0.01700357965349 
},
{
 "name": "grid",
"mean": 0.0059940899729,
"sd": 0.01753809584696 
},
{
 "name": "grid",
"mean": 0.006095562749446,
"sd": 0.01634148958454 
},
{
 "name": "grid",
"mean": 0.00613361504065,
"sd": 0.01603538788529 
},
{
 "name": "grid",
"mean": 0.005989949445676,
"sd": 0.01578377654344 
},
{
 "name": "grid",
"mean": 0.006036802845528,
"sd": 0.01544333755397 
},
{
 "name": "grid",
"mean": 0.006076448030019,
"sd": 0.01523385088628 
},
{
 "name": "grid",
"mean": 0.005884336141907,
"sd": 0.01534182783855 
},
{
 "name": "grid",
"mean": 0.005939990650407,
"sd": 0.01494509546901 
},
{
 "name": "grid",
"mean": 0.005987082926829,
"sd": 0.01470092035919 
},
{
 "name": "grid",
"mean": 0.006027447735192,
"sd": 0.01456189867402 
},
{
 "name": "grid",
"mean": 0.005943672086721,
"sd": 0.01775878241291 
},
{
 "name": "grid",
"mean": 0.006054311751663,
"sd": 0.01660403689429 
},
{
 "name": "grid",
"mean": 0.006095801626016,
"sd": 0.01630475867412 
},
{
 "name": "grid",
"mean": 0.005948698447894,
"sd": 0.01598719125801 
},
{
 "name": "grid",
"mean": 0.005998989430894,
"sd": 0.01566430379542 
},
{
 "name": "grid",
"mean": 0.006041543339587,
"sd": 0.01546469889575 
},
{
 "name": "grid",
"mean": 0.005843085144124,
"sd": 0.01548054542889 
},
{
 "name": "grid",
"mean": 0.005902177235772,
"sd": 0.01511263924558 
},
{
 "name": "grid",
"mean": 0.005952178236398,
"sd": 0.01488750694886 
},
{
 "name": "grid",
"mean": 0.005995036236934,
"sd": 0.01476040495206 
},
{
 "name": "grid",
"mean": 0.00580536504065,
"sd": 0.01465979229514 
},
{
 "name": "grid",
"mean": 0.005862813133208,
"sd": 0.01439089363327 
},
{
 "name": "grid",
"mean": 0.005912054355401,
"sd": 0.01423730315395 
},
{
 "name": "grid",
"mean": 0.005954730081301,
"sd": 0.01416355981399 
},
{
 "name": "grid",
"mean": 0.005842973170732,
"sd": 0.01675380256926 
},
{
 "name": "grid",
"mean": 0.005907447450111,
"sd": 0.01626395789863 
},
{
 "name": "grid",
"mean": 0.00596117601626,
"sd": 0.01594722729928 
},
{
 "name": "grid",
"mean": 0.006006638649156,
"sd": 0.01574824810586 
},
{
 "name": "grid",
"mean": 0.005801834146341,
"sd": 0.01569669557537 
},
{
 "name": "grid",
"mean": 0.005864363821138,
"sd": 0.0153459362017 
},
{
 "name": "grid",
"mean": 0.005917273545966,
"sd": 0.01513018090619 
},
{
 "name": "grid",
"mean": 0.005962624738676,
"sd": 0.01500703046491 
},
{
 "name": "grid",
"mean": 0.005767551626016,
"sd": 0.01483838437553 
},
{
 "name": "grid",
"mean": 0.005827908442777,
"sd": 0.01458822934081 
},
{
 "name": "grid",
"mean": 0.005879642857143,
"sd": 0.01444617682411 
},
{
 "name": "grid",
"mean": 0.005924479349593,
"sd": 0.0143787744233 
},
{
 "name": "grid",
"mean": 0.005738543339587,
"sd": 0.01413115358142 
},
{
 "name": "grid",
"mean": 0.00579666097561,
"sd": 0.01395498901194 
},
{
 "name": "grid",
"mean": 0.005847029593496,
"sd": 0.01386738640509 
},
{
 "name": "grid",
"mean": 0.005891102134146,
"sd": 0.01384129764388 
},
{
 "name": "grid",
"mean": 0.006427756097561,
"sd": 0.01564317004096 
},
{
 "name": "grid",
"mean": 0.006490894308943,
"sd": 0.01411882554474 
},
{
 "name": "grid",
"mean":      0.0065549,
"sd": 0.01440213503086 
},
{
 "name": "grid",
"mean": 0.006107479674797,
"sd": 0.01237705104546 
},
{
 "name": "grid",
"mean": 0.006264463414634,
"sd": 0.01219435970606 
},
{
 "name": "grid",
"mean": 0.006322008780488,
"sd": 0.01269536243274 
},
{
 "name": "grid",
"mean": 0.006360372357724,
"sd": 0.01328808602637 
},
{
 "name": "grid",
"mean": 0.005974026829268,
"sd": 0.01102039319688 
},
{
 "name": "grid",
"mean": 0.006089659512195,
"sd": 0.01109494287107 
},
{
 "name": "grid",
"mean": 0.00616674796748,
"sd": 0.01165352386895 
},
{
 "name": "grid",
"mean": 0.005973123577236,
"sd": 0.01044130170405 
},
{
 "name": "grid",
"mean": 0.006117890243902,
"sd": 0.01155690178097 
},
{
 "name": "grid",
"mean": 0.005957723577236,
"sd": 0.01297262918927 
},
{
 "name": "grid",
"mean": 0.006151023170732,
"sd": 0.01305201839606 
},
{
 "name": "grid",
"mean": 0.006231256585366,
"sd": 0.01354043409124 
},
{
 "name": "grid",
"mean": 0.006284745528455,
"sd": 0.01406211249707 
},
{
 "name": "grid",
"mean": 0.005860586585366,
"sd": 0.01124998097698 
},
{
 "name": "grid",
"mean": 0.005998907317073,
"sd": 0.01160531915631 
},
{
 "name": "grid",
"mean": 0.006091121138211,
"sd": 0.01223206390596 
},
{
 "name": "grid",
"mean": 0.00576655804878,
"sd": 0.01030347442289 
},
{
 "name": "grid",
"mean": 0.005897496747967,
"sd": 0.01074655951036 
},
{
 "name": "grid",
"mean": 0.005991024390244,
"sd": 0.01137343737356 
},
{
 "name": "grid",
"mean": 0.006061170121951,
"sd": 0.01198728075179 
},
{
 "name": "grid",
"mean": 0.005703872357724,
"sd": 0.009764142031454 
},
{
 "name": "grid",
"mean": 0.005915951829268,
"sd": 0.01078153402945 
},
{
 "name": "grid",
"mean": 0.00598664498645,
"sd": 0.01136324182079 
},
{
 "name": "grid",
"mean": 0.005747146341463,
"sd": 0.01225986283382 
},
{
 "name": "grid",
"mean": 0.005908155121951,
"sd": 0.01257743097207 
},
{
 "name": "grid",
"mean": 0.006015494308943,
"sd": 0.01310426581066 
},
{
 "name": "grid",
"mean": 0.005675805853659,
"sd": 0.01091258308777 
},
{
 "name": "grid",
"mean": 0.005821869918699,
"sd": 0.01141209566844 
},
{
 "name": "grid",
"mean": 0.005926201393728,
"sd": 0.01201796147627 
},
{
 "name": "grid",
"mean":     0.00600445,
"sd": 0.01258908696482 
},
{
 "name": "grid",
"mean": 0.005628245528455,
"sd": 0.01013581999868 
},
{
 "name": "grid",
"mean": 0.005760237630662,
"sd": 0.01066301191837 
},
{
 "name": "grid",
"mean": 0.005859231707317,
"sd": 0.01126484550996 
},
{
 "name": "grid",
"mean": 0.005936227100271,
"sd": 0.01183672638117 
},
{
 "name": "grid",
"mean": 0.005714013414634,
"sd": 0.0101575003674 
},
{
 "name": "grid",
"mean": 0.005807144173442,
"sd": 0.01072365768907 
},
{
 "name": "grid",
"mean": 0.005746243089431,
"sd": 0.0123799410331 
},
{
 "name": "grid",
"mean": 0.005947729878049,
"sd": 0.01333913832043 
},
{
 "name": "grid",
"mean": 0.005552618699187,
"sd": 0.01088167499433 
},
{
 "name": "grid",
"mean": 0.005802511585366,
"sd": 0.01192515418922 
},
{
 "name": "grid",
"mean": 0.005885809214092,
"sd": 0.0124407697525 
},
{
 "name": "grid",
"mean": 0.005657293292683,
"sd": 0.01069356953204 
},
{
 "name": "grid",
"mean": 0.005756726287263,
"sd": 0.01124252550907 
},
{
 "name": "grid",
"mean":    0.005512075,
"sd": 0.009714010079842 
},
{
 "name": "grid",
"mean": 0.005627643360434,
"sd": 0.01020938112199 
},
{
 "name": "grid",
"mean": 0.005795742793792,
"sd": 0.01123810699744 
},
{
 "name": "grid",
"mean": 0.005967317073171,
"sd": 0.01631436472275 
},
{
 "name": "grid",
"mean": 0.006158290243902,
"sd": 0.01422271787879 
},
{
 "name": "grid",
"mean": 0.006237070243902,
"sd": 0.01372127027192 
},
{
 "name": "grid",
"mean": 0.006289590243902,
"sd": 0.01379850544857 
},
{
 "name": "grid",
"mean": 0.005867853658537,
"sd": 0.01348605785437 
},
{
 "name": "grid",
"mean": 0.00600472097561,
"sd": 0.01243291119171 
},
{
 "name": "grid",
"mean": 0.006095965853659,
"sd": 0.01235600100934 
},
{
 "name": "grid",
"mean": 0.005772371707317,
"sd": 0.01187515804735 
},
{
 "name": "grid",
"mean": 0.005902341463415,
"sd": 0.01135462653115 
},
{
 "name": "grid",
"mean": 0.005995177003484,
"sd": 0.01145938893154 
},
{
 "name": "grid",
"mean": 0.006064803658537,
"sd": 0.01179578590625 
},
{
 "name": "grid",
"mean": 0.005708717073171,
"sd": 0.01091646008429 
},
{
 "name": "grid",
"mean": 0.005919585365854,
"sd": 0.01084123759344 
},
{
 "name": "grid",
"mean": 0.005989874796748,
"sd": 0.01119632044098 
},
{
 "name": "grid",
"mean": 0.005754413414634,
"sd": 0.01364986281639 
},
{
 "name": "grid",
"mean": 0.005913968780488,
"sd": 0.01287382738068 
},
{
 "name": "grid",
"mean": 0.00602033902439,
"sd": 0.01289157234936 
},
{
 "name": "grid",
"mean": 0.005681619512195,
"sd": 0.01189924689647 
},
{
 "name": "grid",
"mean": 0.005826714634146,
"sd": 0.01162318422624 
},
{
 "name": "grid",
"mean": 0.005930354006969,
"sd": 0.01183608041218 
},
{
 "name": "grid",
"mean": 0.006008083536585,
"sd": 0.01221091660626 
},
{
 "name": "grid",
"mean": 0.005633090243902,
"sd": 0.01086227231598 
},
{
 "name": "grid",
"mean": 0.005764390243902,
"sd": 0.01081646387289 
},
{
 "name": "grid",
"mean": 0.005862865243902,
"sd": 0.01110690818031 
},
{
 "name": "grid",
"mean": 0.005939456910569,
"sd": 0.01151219493549 
},
{
 "name": "grid",
"mean": 0.00571764695122,
"sd": 0.01027067523979 
},
{
 "name": "grid",
"mean": 0.00581037398374,
"sd": 0.01058479460053 
},
{
 "name": "grid",
"mean": 0.005590867317073,
"sd": 0.01241319606252 
},
{
 "name": "grid",
"mean": 0.005751087804878,
"sd": 0.01222901899801 
},
{
 "name": "grid",
"mean": 0.005865531010453,
"sd": 0.0124479313559 
},
{
 "name": "grid",
"mean": 0.005951363414634,
"sd": 0.01279569041991 
},
{
 "name": "grid",
"mean": 0.005557463414634,
"sd": 0.01118429457598 
},
{
 "name": "grid",
"mean": 0.005699567247387,
"sd": 0.01124514762337 
},
{
 "name": "grid",
"mean": 0.005806145121951,
"sd": 0.01156942850068 
},
{
 "name": "grid",
"mean": 0.00588903902439,
"sd": 0.0119742852361 
},
{
 "name": "grid",
"mean": 0.005533603484321,
"sd": 0.0104033755688 
},
{
 "name": "grid",
"mean": 0.005660926829268,
"sd": 0.01057544769442 
},
{
 "name": "grid",
"mean": 0.005759956097561,
"sd": 0.01093725908973 
},
{
 "name": "grid",
"mean": 0.005839179512195,
"sd": 0.01135716802123 
},
{
 "name": "grid",
"mean": 0.005515708536585,
"sd": 0.009884075456883 
},
{
 "name": "grid",
"mean": 0.005630873170732,
"sd": 0.01010342620901 
},
{
 "name": "grid",
"mean": 0.005723004878049,
"sd": 0.01046977585309 
},
{
 "name": "grid",
"mean": 0.005798385365854,
"sd": 0.01088333620112 
},
{
 "name": "grid",
"mean": 0.005481836585366,
"sd": 0.01185190097239 
},
{
 "name": "grid",
"mean":    0.005749425,
"sd": 0.01220644223668 
},
{
 "name": "grid",
"mean": 0.005838621138211,
"sd": 0.01256647187821 
},
{
 "name": "grid",
"mean": 0.005604206707317,
"sd": 0.01108380384759 
},
{
 "name": "grid",
"mean": 0.005709538211382,
"sd": 0.01144068310563 
},
{
 "name": "grid",
"mean": 0.005458988414634,
"sd": 0.01022597765406 
},
{
 "name": "grid",
"mean": 0.005580455284553,
"sd": 0.01049179201404 
},
{
 "name": "grid",
"mean": 0.005677628780488,
"sd": 0.01087075489246 
},
{
 "name": "grid",
"mean": 0.005757134368071,
"sd": 0.01127992152233 
},
{
 "name": "grid",
"mean": 0.005451372357724,
"sd": 0.009771469935133 
},
{
 "name": "grid",
"mean": 0.005651521064302,
"sd": 0.01043743008532 
},
{
 "name": "grid",
"mean": 0.005726576829268,
"sd": 0.01084021888064 
},
{
 "name": "grid",
"mean": 0.005761680487805,
"sd": 0.01778404184996 
},
{
 "name": "grid",
"mean": 0.005919782439024,
"sd": 0.01527743338071 
},
{
 "name": "grid",
"mean": 0.006025183739837,
"sd": 0.01422723101512 
},
{
 "name": "grid",
"mean": 0.005687433170732,
"sd": 0.01497399617546 
},
{
 "name": "grid",
"mean": 0.005831559349593,
"sd": 0.01348004098975 
},
{
 "name": "grid",
"mean": 0.005934506620209,
"sd": 0.01290070223661 
},
{
 "name": "grid",
"mean": 0.006011717073171,
"sd": 0.01277546773998 
},
{
 "name": "grid",
"mean": 0.00563793495935,
"sd": 0.01322851776292 
},
{
 "name": "grid",
"mean": 0.005768542857143,
"sd": 0.01228681714081 
},
{
 "name": "grid",
"mean": 0.005866498780488,
"sd": 0.01197141562812 
},
{
 "name": "grid",
"mean": 0.005942686720867,
"sd": 0.01197952417647 
},
{
 "name": "grid",
"mean": 0.005721280487805,
"sd": 0.01145789875597 
},
{
 "name": "grid",
"mean": 0.005813603794038,
"sd": 0.0112975548545 
},
{
 "name": "grid",
"mean": 0.00559668097561,
"sd": 0.0149788429071 
},
{
 "name": "grid",
"mean": 0.005755932520325,
"sd": 0.01369618490096 
},
{
 "name": "grid",
"mean": 0.005869683623693,
"sd": 0.01322819708803 
},
{
 "name": "grid",
"mean": 0.00595499695122,
"sd": 0.01315338334292 
},
{
 "name": "grid",
"mean": 0.005562308130081,
"sd": 0.01317257185058 
},
{
 "name": "grid",
"mean": 0.005703719860627,
"sd": 0.01241459682365 
},
{
 "name": "grid",
"mean": 0.005809778658537,
"sd": 0.0122056858505 
},
{
 "name": "grid",
"mean": 0.005892268834688,
"sd": 0.01226988542115 
},
{
 "name": "grid",
"mean": 0.005537756097561,
"sd": 0.01198055752091 
},
{
 "name": "grid",
"mean": 0.005664560365854,
"sd": 0.01152442954733 
},
{
 "name": "grid",
"mean": 0.005763185907859,
"sd": 0.0114633642279 
},
{
 "name": "grid",
"mean": 0.005842086341463,
"sd": 0.01160408441858 
},
{
 "name": "grid",
"mean": 0.005519342073171,
"sd": 0.0111585223617 
},
{
 "name": "grid",
"mean": 0.00563410298103,
"sd": 0.01088498074778 
},
{
 "name": "grid",
"mean": 0.005725911707317,
"sd": 0.01090993595381 
},
{
 "name": "grid",
"mean": 0.005801027937916,
"sd": 0.01109146671069 
},
{
 "name": "grid",
"mean": 0.005486681300813,
"sd": 0.01342830005306 
},
{
 "name": "grid",
"mean": 0.005638896864111,
"sd": 0.01278129664059 
},
{
 "name": "grid",
"mean": 0.005753058536585,
"sd": 0.01262141163893 
},
{
 "name": "grid",
"mean": 0.005841850948509,
"sd": 0.01269925237442 
},
{
 "name": "grid",
"mean": 0.005472933101045,
"sd": 0.01213970126786 
},
{
 "name": "grid",
"mean": 0.005607840243902,
"sd": 0.01178977517255 
},
{
 "name": "grid",
"mean": 0.00571276802168,
"sd": 0.01178399452732 
},
{
 "name": "grid",
"mean": 0.005796710243902,
"sd": 0.01194866926804 
},
{
 "name": "grid",
"mean": 0.00546262195122,
"sd": 0.0112500667142 
},
{
 "name": "grid",
"mean": 0.005583685094851,
"sd": 0.01107562632339 
},
{
 "name": "grid",
"mean": 0.005680535609756,
"sd": 0.01115778927169 
},
{
 "name": "grid",
"mean": 0.005759776940133,
"sd": 0.01136919368603 
},
{
 "name": "grid",
"mean": 0.005454602168022,
"sd": 0.01061564530189 
},
{
 "name": "grid",
"mean": 0.00556436097561,
"sd": 0.0105478123024 
},
{
 "name": "grid",
"mean": 0.005654163636364,
"sd": 0.01067958831512 
},
{
 "name": "grid",
"mean": 0.005728999186992,
"sd": 0.01091399516227 
},
{
 "name": "grid",
"mean": 0.005551120121951,
"sd": 0.01224101346234 
},
{
 "name": "grid",
"mean": 0.005662350135501,
"sd": 0.01224729229199 
},
{
 "name": "grid",
"mean": 0.005405901829268,
"sd": 0.01154437831109 
},
{
 "name": "grid",
"mean": 0.005533267208672,
"sd": 0.01142523631696 
},
{
 "name": "grid",
"mean": 0.005635159512195,
"sd": 0.01153024222553 
},
{
 "name": "grid",
"mean": 0.00571852594235,
"sd": 0.01174563136695 
},
{
 "name": "grid",
"mean": 0.005404184281843,
"sd": 0.01083011249231 
},
{
 "name": "grid",
"mean": 0.005518984878049,
"sd": 0.01081943647082 
},
{
 "name": "grid",
"mean": 0.005612912638581,
"sd": 0.01098033791463 
},
{
 "name": "grid",
"mean": 0.005691185772358,
"sd": 0.01122641216112 
},
{
 "name": "grid",
"mean": 0.005507299334812,
"sd": 0.01036033515644 
},
{
 "name": "grid",
"mean": 0.005594373577236,
"sd": 0.01055141605647 
},
{
 "name": "grid",
"mean": 0.005668051782364,
"sd": 0.01081125515886 
},
{
 "name": "grid",
"mean": 0.005760777235772,
"sd": 0.01635153404257 
},
{
 "name": "grid",
"mean": 0.005958630487805,
"sd": 0.01434496466578 
},
{
 "name": "grid",
"mean": 0.005567152845528,
"sd": 0.01623870261665 
},
{
 "name": "grid",
"mean": 0.005813412195122,
"sd": 0.01369637091288 
},
{
 "name": "grid",
"mean": 0.005895498644986,
"sd": 0.01327676526993 
},
{
 "name": "grid",
"mean": 0.005668193902439,
"sd": 0.01331426215374 
},
{
 "name": "grid",
"mean": 0.005766415718157,
"sd": 0.01271808275248 
},
{
 "name": "grid",
"mean": 0.005522975609756,
"sd": 0.013221762848 
},
{
 "name": "grid",
"mean": 0.005637332791328,
"sd": 0.01238717954379 
},
{
 "name": "grid",
"mean": 0.005803670509978,
"sd": 0.01183283269844 
},
{
 "name": "grid",
"mean": 0.00549152601626,
"sd": 0.01618399015965 
},
{
 "name": "grid",
"mean": 0.005756692073171,
"sd": 0.01389559460026 
},
{
 "name": "grid",
"mean": 0.005845080758808,
"sd": 0.01353446093217 
},
{
 "name": "grid",
"mean": 0.005611473780488,
"sd": 0.01336531359002 
},
{
 "name": "grid",
"mean": 0.005715997831978,
"sd": 0.01286046852055 
},
{
 "name": "grid",
"mean": 0.005466255487805,
"sd": 0.01311648203672 
},
{
 "name": "grid",
"mean": 0.005586914905149,
"sd": 0.01240230316097 
},
{
 "name": "grid",
"mean": 0.005683442439024,
"sd": 0.01207681375021 
},
{
 "name": "grid",
"mean": 0.005762419512195,
"sd": 0.01198758397419 
},
{
 "name": "grid",
"mean": 0.00545783197832,
"sd": 0.01218433634261 
},
{
 "name": "grid",
"mean": 0.005656806208426,
"sd": 0.01147122205006 
},
{
 "name": "grid",
"mean": 0.005731421544715,
"sd": 0.01145241842595 
},
{
 "name": "grid",
"mean": 0.005554753658537,
"sd": 0.01358863229906 
},
{
 "name": "grid",
"mean": 0.005665579945799,
"sd": 0.01314205106156 
},
{
 "name": "grid",
"mean": 0.005409535365854,
"sd": 0.01318811885632 
},
{
 "name": "grid",
"mean": 0.00553649701897,
"sd": 0.01256470708813 
},
{
 "name": "grid",
"mean": 0.005638066341463,
"sd": 0.01229683325504 
},
{
 "name": "grid",
"mean": 0.005721168514412,
"sd": 0.01224139572388 
},
{
 "name": "grid",
"mean": 0.005407414092141,
"sd": 0.01221661481758 
},
{
 "name": "grid",
"mean": 0.005521891707317,
"sd": 0.01179265933708 
},
{
 "name": "grid",
"mean": 0.005615555210643,
"sd": 0.01164265682969 
},
{
 "name": "grid",
"mean": 0.005693608130081,
"sd": 0.01165893639348 
},
{
 "name": "grid",
"mean": 0.005509941906874,
"sd": 0.01119874574057 
},
{
 "name": "grid",
"mean": 0.005596795934959,
"sd": 0.01112783697827 
},
{
 "name": "grid",
"mean": 0.005670287804878,
"sd": 0.01119094744344 
},
{
 "name": "grid",
"mean": 0.005352815243902,
"sd": 0.01343384333151 
},
{
 "name": "grid",
"mean": 0.005486079132791,
"sd": 0.01286881651868 
},
{
 "name": "grid",
"mean": 0.00567991751663,
"sd": 0.01258827743819 
},
{
 "name": "grid",
"mean": 0.005356996205962,
"sd": 0.01239811021727 
},
{
 "name": "grid",
"mean": 0.00557430421286,
"sd": 0.01191542448457 
},
{
 "name": "grid",
"mean": 0.005655794715447,
"sd": 0.01194879133643 
},
{
 "name": "grid",
"mean": 0.005468690909091,
"sd": 0.01138642664565 
},
{
 "name": "grid",
"mean": 0.005558982520325,
"sd": 0.01135049867579 
},
{
 "name": "grid",
"mean": 0.005635383114447,
"sd": 0.01143379904819 
},
{
 "name": "grid",
"mean": 0.005363077605322,
"sd": 0.01102201624881 
},
{
 "name": "grid",
"mean": 0.005462170325203,
"sd": 0.01088072697821 
},
{
 "name": "grid",
"mean": 0.005546018011257,
"sd": 0.01089866741614 
},
{
 "name": "grid",
"mean": 0.005617887456446,
"sd": 0.01101414522625 
},
{
 "name": "grid",
"mean": 0.006766829268293,
"sd": 0.0205997481576 
},
{
 "name": "grid",
"mean": 0.006763920731707,
"sd": 0.01850195010137 
},
{
 "name": "grid",
"mean": 0.006721574634146,
"sd": 0.01759052135601 
},
{
 "name": "grid",
"mean": 0.006693343902439,
"sd": 0.01721710310843 
},
{
 "name": "grid",
"mean": 0.006473484146341,
"sd": 0.01698662668819 
},
{
 "name": "grid",
"mean": 0.006489225365854,
"sd": 0.01594920189556 
},
{
 "name": "grid",
"mean": 0.006499719512195,
"sd": 0.01561639784318 
},
{
 "name": "grid",
"mean": 0.006256876097561,
"sd": 0.01481437530768 
},
{
 "name": "grid",
"mean": 0.006306095121951,
"sd": 0.01432829013306 
},
{
 "name": "grid",
"mean": 0.006341251567944,
"sd": 0.014265852789 
},
{
 "name": "grid",
"mean": 0.006367618902439,
"sd": 0.01437820268525 
},
{
 "name": "grid",
"mean": 0.006112470731707,
"sd": 0.01344294197967 
},
{
 "name": "grid",
"mean": 0.006222400609756,
"sd": 0.01329599183566 
},
{
 "name": "grid",
"mean": 0.006259043902439,
"sd": 0.01348720156193 
},
{
 "name": "grid",
"mean": 0.006360043902439,
"sd": 0.01752543134577 
},
{
 "name": "grid",
"mean": 0.006398473170732,
"sd": 0.01657079834203 
},
{
 "name": "grid",
"mean": 0.006424092682927,
"sd": 0.0162383115613 
},
{
 "name": "grid",
"mean": 0.006166123902439,
"sd": 0.0151358706281 
},
{
 "name": "grid",
"mean": 0.006230468292683,
"sd": 0.01475668560694 
},
{
 "name": "grid",
"mean": 0.006276428571429,
"sd": 0.0147278733503 
},
{
 "name": "grid",
"mean": 0.006310898780488,
"sd": 0.0148403570195 
},
{
 "name": "grid",
"mean": 0.006036843902439,
"sd": 0.01363163791453 
},
{
 "name": "grid",
"mean": 0.006110464808362,
"sd": 0.01352474550184 
},
{
 "name": "grid",
"mean": 0.006165680487805,
"sd": 0.01364375096707 
},
{
 "name": "grid",
"mean": 0.00620862601626,
"sd": 0.01385181431105 
},
{
 "name": "grid",
"mean": 0.006020462195122,
"sd": 0.01264650623815 
},
{
 "name": "grid",
"mean": 0.006079543089431,
"sd": 0.01283703398237 
},
{
 "name": "grid",
"mean": 0.006075371707317,
"sd": 0.01583180676089 
},
{
 "name": "grid",
"mean": 0.006154841463415,
"sd": 0.01544342152077 
},
{
 "name": "grid",
"mean": 0.006211605574913,
"sd": 0.01537494525928 
},
{
 "name": "grid",
"mean": 0.006254178658537,
"sd": 0.01544010913861 
},
{
 "name": "grid",
"mean": 0.005961217073171,
"sd": 0.01411417874331 
},
{
 "name": "grid",
"mean": 0.006045641811847,
"sd": 0.01403555086319 
},
{
 "name": "grid",
"mean": 0.006108960365854,
"sd": 0.01414841940809 
},
{
 "name": "grid",
"mean": 0.006158208130081,
"sd": 0.01433599481412 
},
{
 "name": "grid",
"mean": 0.00587967804878,
"sd": 0.01295681143122 
},
{
 "name": "grid",
"mean": 0.005963742073171,
"sd": 0.01303169247938 
},
{
 "name": "grid",
"mean": 0.006029125203252,
"sd": 0.01323519225568 
},
{
 "name": "grid",
"mean": 0.006081431707317,
"sd": 0.01348556775565 
},
{
 "name": "grid",
"mean": 0.005818523780488,
"sd": 0.01213831461124 
},
{
 "name": "grid",
"mean": 0.005900042276423,
"sd": 0.01228964344738 
},
{
 "name": "grid",
"mean": 0.005965257073171,
"sd": 0.01253607286272 
},
{
 "name": "grid",
"mean": 0.006018614634146,
"sd": 0.01281593333111 
},
{
 "name": "grid",
"mean": 0.005885590243902,
"sd": 0.01486197016431 
},
{
 "name": "grid",
"mean": 0.006052240243902,
"sd": 0.014793947843 
},
{
 "name": "grid",
"mean": 0.006107790243902,
"sd": 0.01492811336976 
},
{
 "name": "grid",
"mean": 0.00590702195122,
"sd": 0.01357840887073 
},
{
 "name": "grid",
"mean": 0.005978707317073,
"sd": 0.01375612665295 
},
{
 "name": "grid",
"mean": 0.005761803658537,
"sd": 0.01255993303066 
},
{
 "name": "grid",
"mean": 0.005849624390244,
"sd": 0.0127211999233 
},
{
 "name": "grid",
"mean": 0.00591988097561,
"sd": 0.01296048879489 
},
{
 "name": "grid",
"mean": 0.005977363636364,
"sd": 0.01322531203714 
},
{
 "name": "grid",
"mean": 0.005720541463415,
"sd": 0.01185927020394 
},
{
 "name": "grid",
"mean": 0.005871750332594,
"sd": 0.01233694984176 
},
{
 "name": "grid",
"mean": 0.005928453658537,
"sd": 0.01262279755439 
},
{
 "name": "grid",
"mean": 0.00636731097561,
"sd": 0.01836267770781 
},
{
 "name": "grid",
"mean": 0.006404286829268,
"sd": 0.01668280337384 
},
{
 "name": "grid",
"mean": 0.006428937398374,
"sd": 0.01598441338114 
},
{
 "name": "grid",
"mean": 0.006171937560976,
"sd": 0.01574112752868 
},
{
 "name": "grid",
"mean": 0.00623531300813,
"sd": 0.01483138927034 
},
{
 "name": "grid",
"mean": 0.006280581184669,
"sd": 0.0145128611638 
},
{
 "name": "grid",
"mean": 0.006314532317073,
"sd": 0.01446947607411 
},
{
 "name": "grid",
"mean": 0.006041688617886,
"sd": 0.0140862883198 
},
{
 "name": "grid",
"mean": 0.006114617421603,
"sd": 0.01357447209621 
},
{
 "name": "grid",
"mean": 0.00616931402439,
"sd": 0.01345833470567 
},
{
 "name": "grid",
"mean": 0.006211855826558,
"sd": 0.01353205707841 
},
{
 "name": "grid",
"mean": 0.006024095731707,
"sd": 0.01267888969252 
},
{
 "name": "grid",
"mean": 0.006082772899729,
"sd": 0.01267484722682 
},
{
 "name": "grid",
"mean": 0.006081185365854,
"sd": 0.01603073407886 
},
{
 "name": "grid",
"mean": 0.006159686178862,
"sd": 0.01523591439704 
},
{
 "name": "grid",
"mean": 0.006215758188153,
"sd": 0.01495997031907 
},
{
 "name": "grid",
"mean": 0.006257812195122,
"sd": 0.01492321078182 
},
{
 "name": "grid",
"mean": 0.005966061788618,
"sd": 0.01425606852021 
},
{
 "name": "grid",
"mean": 0.006049794425087,
"sd": 0.013857962488 
},
{
 "name": "grid",
"mean": 0.006112593902439,
"sd": 0.01379595276807 
},
{
 "name": "grid",
"mean": 0.006161437940379,
"sd": 0.01389074405663 
},
{
 "name": "grid",
"mean": 0.005883830662021,
"sd": 0.01305986465526 
},
{
 "name": "grid",
"mean": 0.005967375609756,
"sd": 0.01287714413656 
},
{
 "name": "grid",
"mean": 0.00603235501355,
"sd": 0.01293138967296 
},
{
 "name": "grid",
"mean": 0.006084338536585,
"sd": 0.01309752095949 
},
{
 "name": "grid",
"mean": 0.005822157317073,
"sd": 0.01221391849668 
},
{
 "name": "grid",
"mean": 0.005903272086721,
"sd": 0.0121533683638 
},
{
 "name": "grid",
"mean": 0.005968163902439,
"sd": 0.01227104831637 
},
{
 "name": "grid",
"mean": 0.006021257206208,
"sd": 0.01247441528771 
},
{
 "name": "grid",
"mean": 0.00589043495935,
"sd": 0.01470806196883 
},
{
 "name": "grid",
"mean": 0.005984971428571,
"sd": 0.01434931868157 
},
{
 "name": "grid",
"mean": 0.006055873780488,
"sd": 0.0142894066443 
},
{
 "name": "grid",
"mean": 0.006111020054201,
"sd": 0.0143690224437 
},
{
 "name": "grid",
"mean": 0.005819007665505,
"sd": 0.01337980365927 
},
{
 "name": "grid",
"mean": 0.005910655487805,
"sd": 0.01324932934645 
},
{
 "name": "grid",
"mean": 0.005981937127371,
"sd": 0.01332177763634 
},
{
 "name": "grid",
"mean": 0.006038962439024,
"sd":  0.01348795829 
},
{
 "name": "grid",
"mean": 0.005765437195122,
"sd": 0.01244060889225 
},
{
 "name": "grid",
"mean": 0.005852854200542,
"sd": 0.01243728941859 
},
{
 "name": "grid",
"mean": 0.005922787804878,
"sd": 0.01258221869001 
},
{
 "name": "grid",
"mean": 0.005980006208426,
"sd": 0.01279531463838 
},
{
 "name": "grid",
"mean": 0.005723771273713,
"sd": 0.01175236282864 
},
{
 "name": "grid",
"mean": 0.005806613170732,
"sd": 0.01182091173616 
},
{
 "name": "grid",
"mean": 0.005874392904656,
"sd": 0.01200430147437 
},
{
 "name": "grid",
"mean": 0.00593087601626,
"sd": 0.01224088534933 
},
{
 "name": "grid",
"mean": 0.005853935365854,
"sd": 0.01378136077772 
},
{
 "name": "grid",
"mean": 0.005931519241192,
"sd": 0.01383468517089 
},
{
 "name": "grid",
"mean": 0.005708717073171,
"sd": 0.01284581581351 
},
{
 "name": "grid",
"mean": 0.005802436314363,
"sd": 0.01285876383812 
},
{
 "name": "grid",
"mean": 0.005877411707317,
"sd": 0.01300101892068 
},
{
 "name": "grid",
"mean": 0.005938755210643,
"sd": 0.01320198759588 
},
{
 "name": "grid",
"mean": 0.005673353387534,
"sd": 0.01206285854996 
},
{
 "name": "grid",
"mean": 0.005761237073171,
"sd": 0.01215737870261 
},
{
 "name": "grid",
"mean": 0.005833141906874,
"sd": 0.01234862650968 
},
{
 "name": "grid",
"mean": 0.005893062601626,
"sd": 0.01258286790688 
},
{
 "name": "grid",
"mean": 0.005727528603104,
"sd": 0.01161229676645 
},
{
 "name": "grid",
"mean": 0.005796250406504,
"sd": 0.01182883865825 
},
{
 "name": "grid",
"mean": 0.005854399624765,
"sd": 0.01207920992452 
},
{
 "name": "grid",
"mean": 0.00608699902439,
"sd": 0.0179845389878 
},
{
 "name": "grid",
"mean": 0.006164530894309,
"sd": 0.01635596271321 
},
{
 "name": "grid",
"mean": 0.006219910801394,
"sd": 0.01555269749481 
},
{
 "name": "grid",
"mean": 0.006261445731707,
"sd": 0.01518196916471 
},
{
 "name": "grid",
"mean": 0.005970906504065,
"sd": 0.01578009972163 
},
{
 "name": "grid",
"mean": 0.006053947038328,
"sd": 0.01475681123415 
},
{
 "name": "grid",
"mean": 0.006116227439024,
"sd": 0.0142815815533 
},
{
 "name": "grid",
"mean": 0.006164667750678,
"sd": 0.01410463115558 
},
{
 "name": "grid",
"mean": 0.005887983275261,
"sd": 0.01427988390566 
},
{
 "name": "grid",
"mean": 0.005971009146341,
"sd": 0.01361254536094 
},
{
 "name": "grid",
"mean": 0.006035584823848,
"sd": 0.01333518616875 
},
{
 "name": "grid",
"mean": 0.006087245365854,
"sd": 0.01327628684167 
},
{
 "name": "grid",
"mean": 0.005825790853659,
"sd": 0.01321005852896 
},
{
 "name": "grid",
"mean": 0.005906501897019,
"sd": 0.01276437290117 
},
{
 "name": "grid",
"mean": 0.005971070731707,
"sd": 0.01261088221228 
},
{
 "name": "grid",
"mean": 0.006023899778271,
"sd": 0.01262519684362 
},
{
 "name": "grid",
"mean": 0.005895279674797,
"sd": 0.01592251946488 
},
{
 "name": "grid",
"mean": 0.005989124041812,
"sd": 0.01501073564457 
},
{
 "name": "grid",
"mean": 0.006059507317073,
"sd": 0.01459445475392 
},
{
 "name": "grid",
"mean": 0.006114249864499,
"sd": 0.01444454463271 
},
{
 "name": "grid",
"mean": 0.005823160278746,
"sd": 0.01435524468584 
},
{
 "name": "grid",
"mean": 0.00591428902439,
"sd": 0.01379133737727 
},
{
 "name": "grid",
"mean": 0.005985166937669,
"sd": 0.01357439960897 
},
{
 "name": "grid",
"mean": 0.006041869268293,
"sd": 0.0135481441256 
},
{
 "name": "grid",
"mean": 0.005769070731707,
"sd": 0.0132389689681 
},
{
 "name": "grid",
"mean": 0.00585608401084,
"sd": 0.01288794922114 
},
{
 "name": "grid",
"mean": 0.005925694634146,
"sd": 0.01279376426036 
},
{
 "name": "grid",
"mean": 0.005982648780488,
"sd": 0.01284340079021 
},
{
 "name": "grid",
"mean": 0.005727001084011,
"sd": 0.01241569977061 
},
{
 "name": "grid",
"mean":     0.00580952,
"sd": 0.01220012776294 
},
{
 "name": "grid",
"mean": 0.005877035476718,
"sd": 0.01218309670503 
},
{
 "name": "grid",
"mean": 0.005933298373984,
"sd": 0.01227929894379 
},
{
 "name": "grid",
"mean": 0.00575833728223,
"sd": 0.01463946741859 
},
{
 "name": "grid",
"mean": 0.005857568902439,
"sd": 0.01413357250812 
},
{
 "name": "grid",
"mean": 0.005934749051491,
"sd": 0.01394206918647 
},
{
 "name": "grid",
"mean": 0.005996493170732,
"sd": 0.01392211519944 
},
{
 "name": "grid",
"mean": 0.005712350609756,
"sd": 0.01344218144783 
},
{
 "name": "grid",
"mean": 0.005805666124661,
"sd": 0.01315101093301 
},
{
 "name": "grid",
"mean": 0.005880318536585,
"sd": 0.01308843471007 
},
{
 "name": "grid",
"mean": 0.005941397782705,
"sd": 0.01315194480817 
},
{
 "name": "grid",
"mean": 0.005676583197832,
"sd": 0.01255915360225 
},
{
 "name": "grid",
"mean": 0.005764143902439,
"sd": 0.01240255915019 
},
{
 "name": "grid",
"mean": 0.005835784478936,
"sd": 0.01242020437452 
},
{
 "name": "grid",
"mean": 0.00589548495935,
"sd": 0.01253500473045 
},
{
 "name": "grid",
"mean": 0.005647969268293,
"sd": 0.01189009430444 
},
{
 "name": "grid",
"mean": 0.005730171175166,
"sd": 0.01181990103576 
},
{
 "name": "grid",
"mean": 0.005798672764228,
"sd": 0.01188768732915 
},
{
 "name": "grid",
"mean": 0.00585663564728,
"sd": 0.01203340366406 
},
{
 "name": "grid",
"mean": 0.005655630487805,
"sd": 0.01381200472606 
},
{
 "name": "grid",
"mean": 0.005755248238482,
"sd": 0.01354543376423 
},
{
 "name": "grid",
"mean": 0.005834942439024,
"sd": 0.0134875686394 
},
{
 "name": "grid",
"mean": 0.005900146784922,
"sd": 0.01354465652861 
},
{
 "name": "grid",
"mean": 0.005626165311653,
"sd": 0.01284503450689 
},
{
 "name": "grid",
"mean": 0.005718767804878,
"sd": 0.01271945340603 
},
{
 "name": "grid",
"mean": 0.005794533481153,
"sd": 0.0127498418793 
},
{
 "name": "grid",
"mean": 0.005857671544715,
"sd": 0.01286628972098 
},
{
 "name": "grid",
"mean": 0.005602593170732,
"sd": 0.01211152347455 
},
{
 "name": "grid",
"mean": 0.005688920177384,
"sd": 0.01207559417747 
},
{
 "name": "grid",
"mean": 0.005760859349593,
"sd": 0.01216118519829 
},
{
 "name": "grid",
"mean": 0.005821730956848,
"sd": 0.01231409473899 
},
{
 "name": "grid",
"mean": 0.005583306873614,
"sd": 0.01154306028979 
},
{
 "name": "grid",
"mean": 0.005664047154472,
"sd": 0.01156475824511 
},
{
 "name": "grid",
"mean": 0.005732365853659,
"sd": 0.01168449098529 
},
{
 "name": "grid",
"mean": 0.005790924738676,
"sd": 0.01185895374971 
},
{
 "name": "grid",
"mean": 0.005900124390244,
"sd": 0.0182339456744 
},
{
 "name": "grid",
"mean": 0.006063140853659,
"sd": 0.01566185795923 
},
{
 "name": "grid",
"mean": 0.006117479674797,
"sd": 0.01514518932721 
},
{
 "name": "grid",
"mean": 0.005917922560976,
"sd": 0.01511098815531 
},
{
 "name": "grid",
"mean": 0.005988396747967,
"sd": 0.01447807710118 
},
{
 "name": "grid",
"mean": 0.005772704268293,
"sd": 0.01480731464242 
},
{
 "name": "grid",
"mean": 0.005859313821138,
"sd": 0.01400243295372 
},
{
 "name": "grid",
"mean": 0.005928601463415,
"sd": 0.01356756426785 
},
{
 "name": "grid",
"mean": 0.00598529135255,
"sd": 0.01336441094728 
},
{
 "name": "grid",
"mean": 0.005730230894309,
"sd": 0.01373815839127 
},
{
 "name": "grid",
"mean": 0.00587967804878,
"sd": 0.01285200784732 
},
{
 "name": "grid",
"mean": 0.005935720731707,
"sd": 0.01273423509072 
},
{
 "name": "grid",
"mean": 0.005861202439024,
"sd": 0.01526677951147 
},
{
 "name": "grid",
"mean": 0.005937978861789,
"sd": 0.0146942131152 
},
{
 "name": "grid",
"mean": 0.005715984146341,
"sd": 0.01482748185266 
},
{
 "name": "grid",
"mean": 0.005808895934959,
"sd": 0.01411049949958 
},
{
 "name": "grid",
"mean": 0.005883225365854,
"sd": 0.01373382586062 
},
{
 "name": "grid",
"mean": 0.005944040354767,
"sd": 0.01356747938785 
},
{
 "name": "grid",
"mean": 0.00567981300813,
"sd": 0.01372981968073 
},
{
 "name": "grid",
"mean": 0.005767050731707,
"sd": 0.01322404285591 
},
{
 "name": "grid",
"mean": 0.005838427050998,
"sd": 0.01297906181822 
},
{
 "name": "grid",
"mean": 0.005897907317073,
"sd": 0.01289813067083 
},
{
 "name": "grid",
"mean": 0.005732813747228,
"sd": 0.0125298185839 
},
{
 "name": "grid",
"mean": 0.005801095121951,
"sd": 0.0123753969006 
},
{
 "name": "grid",
"mean": 0.005858871669794,
"sd": 0.01235281004654 
},
{
 "name": "grid",
"mean": 0.00565926402439,
"sd": 0.01500363819214 
},
{
 "name": "grid",
"mean": 0.00575847804878,
"sd": 0.01434657093174 
},
{
 "name": "grid",
"mean": 0.005837849268293,
"sd": 0.01400492094978 
},
{
 "name": "grid",
"mean": 0.005902789356984,
"sd": 0.01385672666157 
},
{
 "name": "grid",
"mean": 0.005629395121951,
"sd": 0.01385491735141 
},
{
 "name": "grid",
"mean": 0.005721674634146,
"sd": 0.01340704234075 
},
{
 "name": "grid",
"mean": 0.005797176053215,
"sd": 0.0131985357338 
},
{
 "name": "grid",
"mean": 0.005860093902439,
"sd": 0.01313897685546 
},
{
 "name": "grid",
"mean":      0.0056055,
"sd": 0.0129766820345 
},
{
 "name": "grid",
"mean": 0.005691562749446,
"sd": 0.01267101324212 
},
{
 "name": "grid",
"mean": 0.005763281707317,
"sd": 0.01255323202421 
},
{
 "name": "grid",
"mean": 0.005823966979362,
"sd": 0.01255384765289 
},
{
 "name": "grid",
"mean": 0.005585949445676,
"sd": 0.01229099536126 
},
{
 "name": "grid",
"mean": 0.005666469512195,
"sd": 0.01208431832156 
},
{
 "name": "grid",
"mean": 0.005734601876173,
"sd": 0.01202925985419 
},
{
 "name": "grid",
"mean": 0.005793001045296,
"sd": 0.01207089620035 
},
{
 "name": "grid",
"mean": 0.005578977235772,
"sd": 0.01410990272932 
},
{
 "name": "grid",
"mean": 0.005755925055432,
"sd": 0.01350592481887 
},
{
 "name": "grid",
"mean": 0.005822280487805,
"sd": 0.0134526412784 
},
{
 "name": "grid",
"mean": 0.005650311751663,
"sd": 0.01290643867534 
},
{
 "name": "grid",
"mean": 0.005725468292683,
"sd": 0.01280963413257 
},
{
 "name": "grid",
"mean": 0.005789062288931,
"sd": 0.01282068535979 
},
{
 "name": "grid",
"mean": 0.005544698447894,
"sd": 0.012446006294 
},
{
 "name": "grid",
"mean": 0.005628656097561,
"sd": 0.01227583355431 
},
{
 "name": "grid",
"mean": 0.005699697185741,
"sd": 0.01224369549918 
},
{
 "name": "grid",
"mean": 0.005760589547038,
"sd": 0.01229857247481 
},
{
 "name": "grid",
"mean": 0.005531843902439,
"sd": 0.01186598693298 
},
{
 "name": "grid",
"mean": 0.005610332082552,
"sd": 0.01176542353909 
},
{
 "name": "grid",
"mean": 0.005677607665505,
"sd": 0.0117780366832 
},
{
 "name": "grid",
"mean": 0.005735913170732,
"sd": 0.01186263756195 
},
{
 "name": "grid",
"mean": 0.006972941463415,
"sd": 0.02416998659772 
},
{
 "name": "grid",
"mean": 0.006888791219512,
"sd": 0.0216315969417 
},
{
 "name": "grid",
"mean": 0.006832691056911,
"sd": 0.02022550009023 
},
{
 "name": "grid",
"mean": 0.00665644195122,
"sd": 0.02039705200305 
},
{
 "name": "grid",
"mean": 0.006639066666667,
"sd": 0.01893966008857 
},
{
 "name": "grid",
"mean": 0.006626655749129,
"sd": 0.01812847706224 
},
{
 "name": "grid",
"mean": 0.006617347560976,
"sd": 0.01766465489632 
},
{
 "name": "grid",
"mean": 0.006445442276423,
"sd": 0.01795370070577 
},
{
 "name": "grid",
"mean": 0.006460691986063,
"sd": 0.01707015613298 
},
{
 "name": "grid",
"mean": 0.006472129268293,
"sd": 0.01659698426888 
},
{
 "name": "grid",
"mean": 0.006481024932249,
"sd": 0.01635184426698 
},
{
 "name": "grid",
"mean": 0.00632691097561,
"sd": 0.01570815637277 
},
{
 "name": "grid",
"mean": 0.00635194200542,
"sd": 0.01543785709337 
},
{
 "name": "grid",
"mean": 0.006565689756098,
"sd": 0.02083979931545 
},
{
 "name": "grid",
"mean": 0.006563439837398,
"sd": 0.01942067733539 
},
{
 "name": "grid",
"mean": 0.006561832752613,
"sd": 0.01861289193462 
},
{
 "name": "grid",
"mean": 0.006560627439024,
"sd": 0.01813598851938 
},
{
 "name": "grid",
"mean": 0.006369815447154,
"sd": 0.01826023943234 
},
{
 "name": "grid",
"mean": 0.006395868989547,
"sd": 0.017429502426 
},
{
 "name": "grid",
"mean": 0.006415409146341,
"sd": 0.01697642457468 
},
{
 "name": "grid",
"mean": 0.00643060704607,
"sd": 0.01673362260614 
},
{
 "name": "grid",
"mean": 0.006229905226481,
"sd": 0.01647452249961 
},
{
 "name": "grid",
"mean": 0.006270190853659,
"sd": 0.01597968518774 
},
{
 "name": "grid",
"mean": 0.006301524119241,
"sd": 0.01573822198363 
},
{
 "name": "grid",
"mean": 0.006326590731707,
"sd": 0.01563997635572 
},
{
 "name": "grid",
"mean": 0.006124972560976,
"sd": 0.01517788264725 
},
{
 "name": "grid",
"mean": 0.006172441192412,
"sd": 0.01488571772446 
},
{
 "name": "grid",
"mean": 0.006210416097561,
"sd": 0.01477511678742 
},
{
 "name": "grid",
"mean": 0.006241486474501,
"sd": 0.01476810465089 
},
{
 "name": "grid",
"mean": 0.006294188617886,
"sd": 0.01878342209028 
},
{
 "name": "grid",
"mean": 0.006331045993031,
"sd": 0.01795182079652 
},
{
 "name": "grid",
"mean": 0.00635868902439,
"sd": 0.01748128672599 
},
{
 "name": "grid",
"mean": 0.006380189159892,
"sd": 0.0172141010217 
},
{
 "name": "grid",
"mean": 0.006165082229965,
"sd": 0.01686681551221 
},
{
 "name": "grid",
"mean": 0.006213470731707,
"sd": 0.01638938041859 
},
{
 "name": "grid",
"mean": 0.006251106233062,
"sd": 0.01614731137509 
},
{
 "name": "grid",
"mean": 0.006281214634146,
"sd": 0.01603910425869 
},
{
 "name": "grid",
"mean": 0.006068252439024,
"sd": 0.01547561407535 
},
{
 "name": "grid",
"mean": 0.006122023306233,
"sd": 0.01521057232939 
},
{
 "name": "grid",
"mean":     0.00616504,
"sd": 0.01510965644637 
},
{
 "name": "grid",
"mean": 0.006200235476718,
"sd": 0.0151022204842 
},
{
 "name": "grid",
"mean": 0.005992940379404,
"sd": 0.01442923462336 
},
{
 "name": "grid",
"mean": 0.006048865365854,
"sd": 0.0142963313201 
},
{
 "name": "grid",
"mean": 0.006094622172949,
"sd": 0.01428455027042 
},
{
 "name": "grid",
"mean": 0.006132752845528,
"sd": 0.0143416791682 
},
{
 "name": "grid",
"mean": 0.006156750609756,
"sd": 0.01692721276671 
},
{
 "name": "grid",
"mean": 0.006200688346883,
"sd": 0.01665711655567 
},
{
 "name": "grid",
"mean": 0.006011532317073,
"sd": 0.01591472368918 
},
{
 "name": "grid",
"mean": 0.006071605420054,
"sd": 0.01564666938586 
},
{
 "name": "grid",
"mean": 0.006119663902439,
"sd": 0.01553319108249 
},
{
 "name": "grid",
"mean": 0.006158984478936,
"sd": 0.01550872567262 
},
{
 "name": "grid",
"mean": 0.005942522493225,
"sd": 0.01477810708704 
},
{
 "name": "grid",
"mean": 0.006003489268293,
"sd": 0.0146532260381 
},
{
 "name": "grid",
"mean": 0.006053371175166,
"sd": 0.01463915324959 
},
{
 "name": "grid",
"mean": 0.006094939430894,
"sd": 0.01468827093321 
},
{
 "name": "grid",
"mean": 0.005947757871397,
"sd": 0.01386541752217 
},
{
 "name": "grid",
"mean": 0.005998127235772,
"sd": 0.01391513459165 
},
{
 "name": "grid",
"mean": 0.006040747467167,
"sd": 0.01401198912978 
},
{
 "name": "grid",
"mean": 0.006571503414634,
"sd": 0.02090015489055 
},
{
 "name": "grid",
"mean": 0.006568284552846,
"sd": 0.0191870889138 
},
{
 "name": "grid",
"mean": 0.006565985365854,
"sd": 0.01821820519839 
},
{
 "name": "grid",
"mean": 0.00656426097561,
"sd": 0.01765580632645 
},
{
 "name": "grid",
"mean": 0.006374660162602,
"sd": 0.01829780439714 
},
{
 "name": "grid",
"mean": 0.006400021602787,
"sd": 0.01723036492251 
},
{
 "name": "grid",
"mean": 0.006419042682927,
"sd": 0.01663904143895 
},
{
 "name": "grid",
"mean": 0.006433836856369,
"sd": 0.01631770256245 
},
{
 "name": "grid",
"mean": 0.006234057839721,
"sd": 0.01649674144184 
},
{
 "name": "grid",
"mean": 0.006273824390244,
"sd": 0.01580678141089 
},
{
 "name": "grid",
"mean": 0.006304753929539,
"sd": 0.01544549851222 
},
{
 "name": "grid",
"mean": 0.006329497560976,
"sd": 0.01527542488072 
},
{
 "name": "grid",
"mean": 0.006128606097561,
"sd": 0.01518938487008 
},
{
 "name": "grid",
"mean": 0.00617567100271,
"sd": 0.0147334614721 
},
{
 "name": "grid",
"mean": 0.006213322926829,
"sd": 0.01451808552572 
},
{
 "name": "grid",
"mean": 0.006244129046563,
"sd": 0.01444543998099 
},
{
 "name": "grid",
"mean": 0.006299033333333,
"sd": 0.01859068930165 
},
{
 "name": "grid",
"mean": 0.006335198606272,
"sd": 0.01758023534938 
},
{
 "name": "grid",
"mean": 0.006362322560976,
"sd": 0.01701263655924 
},
{
 "name": "grid",
"mean": 0.00638341897019,
"sd": 0.01669631035432 
},
{
 "name": "grid",
"mean": 0.006169234843206,
"sd": 0.0167009262203 
},
{
 "name": "grid",
"mean": 0.006217104268293,
"sd": 0.01607144980427 
},
{
 "name": "grid",
"mean": 0.00625433604336,
"sd": 0.01574152698437 
},
{
 "name": "grid",
"mean": 0.006284121463415,
"sd": 0.01558509107119 
},
{
 "name": "grid",
"mean": 0.00607188597561,
"sd": 0.01533034998036 
},
{
 "name": "grid",
"mean": 0.006125253116531,
"sd": 0.01493452441279 
},
{
 "name": "grid",
"mean": 0.006167946829268,
"sd": 0.01475414865639 
},
{
 "name": "grid",
"mean": 0.00620287804878,
"sd": 0.01470031269773 
},
{
 "name": "grid",
"mean": 0.005996170189702,
"sd": 0.01430034343495 
},
{
 "name": "grid",
"mean": 0.006051772195122,
"sd": 0.01405379878299 
},
{
 "name": "grid",
"mean": 0.006097264745011,
"sd": 0.01397004762195 
},
{
 "name": "grid",
"mean": 0.006135175203252,
"sd": 0.01398302473693 
},
{
 "name": "grid",
"mean": 0.00610441184669,
"sd": 0.01708163884735 
},
{
 "name": "grid",
"mean": 0.006160384146341,
"sd": 0.01647379582212 
},
{
 "name": "grid",
"mean": 0.006203918157182,
"sd": 0.01614644707592 
},
{
 "name": "grid",
"mean": 0.006238745365854,
"sd": 0.01598224601052 
},
{
 "name": "grid",
"mean": 0.006015165853659,
"sd": 0.01561983156271 
},
{
 "name": "grid",
"mean": 0.006074835230352,
"sd": 0.01525401539889 
},
{
 "name": "grid",
"mean": 0.006122570731707,
"sd": 0.0150856111328 
},
{
 "name": "grid",
"mean": 0.006161627050998,
"sd": 0.01503299738705 
},
{
 "name": "grid",
"mean": 0.005945752303523,
"sd": 0.01452162779059 
},
{
 "name": "grid",
"mean": 0.006006396097561,
"sd": 0.01430921583245 
},
{
 "name": "grid",
"mean": 0.006056013747228,
"sd": 0.01424313766658 
},
{
 "name": "grid",
"mean": 0.006097361788618,
"sd": 0.01426332334529 
},
{
 "name": "grid",
"mean": 0.005890221463415,
"sd": 0.01367355736486 
},
{
 "name": "grid",
"mean": 0.005950400443459,
"sd": 0.01356110947018 
},
{
 "name": "grid",
"mean": 0.006000549593496,
"sd": 0.0135619308268 
},
{
 "name": "grid",
"mean": 0.006042983489681,
"sd": 0.01362984796263 
},
{
 "name": "grid",
"mean": 0.005958445731707,
"sd": 0.01604979549131 
},
{
 "name": "grid",
"mean": 0.006024417344173,
"sd": 0.01568469910487 
},
{
 "name": "grid",
"mean": 0.006077194634146,
"sd": 0.01550635641786 
},
{
 "name": "grid",
"mean": 0.006120376053215,
"sd": 0.01543846453922 
},
{
 "name": "grid",
"mean": 0.005895334417344,
"sd": 0.01486389447239 
},
{
 "name": "grid",
"mean":     0.00596102,
"sd": 0.01466215266184 
},
{
 "name": "grid",
"mean": 0.006014762749446,
"sd": 0.01459572096473 
},
{
 "name": "grid",
"mean": 0.006059548373984,
"sd": 0.01460923377393 
},
{
 "name": "grid",
"mean": 0.005844845365854,
"sd": 0.01394793428366 
},
{
 "name": "grid",
"mean": 0.005909149445676,
"sd": 0.01385224577297 
},
{
 "name": "grid",
"mean": 0.005962736178862,
"sd": 0.01385913179676 
},
{
 "name": "grid",
"mean": 0.00600807879925,
"sd": 0.01392646941938 
},
{
 "name": "grid",
"mean": 0.005803536141907,
"sd": 0.0132247916714 
},
{
 "name": "grid",
"mean": 0.00586592398374,
"sd": 0.01319914961109 
},
{
 "name": "grid",
"mean": 0.00591871369606,
"sd": 0.01325436382498 
},
{
 "name": "grid",
"mean": 0.005963962020906,
"sd": 0.01335717234782 
},
{
 "name": "grid",
"mean": 0.00630387804878,
"sd": 0.01949772093133 
},
{
 "name": "grid",
"mean": 0.006339351219512,
"sd": 0.018070290456 
},
{
 "name": "grid",
"mean": 0.006365956097561,
"sd": 0.01722640234206 
},
{
 "name": "grid",
"mean": 0.006386648780488,
"sd": 0.01672615052432 
},
{
 "name": "grid",
"mean": 0.006173387456446,
"sd": 0.01743635067623 
},
{
 "name": "grid",
"mean": 0.006220737804878,
"sd": 0.01647591954855 
},
{
 "name": "grid",
"mean": 0.006257565853659,
"sd": 0.01591890402657 
},
{
 "name": "grid",
"mean": 0.006287028292683,
"sd": 0.01560664862806 
},
{
 "name": "grid",
"mean": 0.006075519512195,
"sd": 0.01593829814894 
},
{
 "name": "grid",
"mean": 0.006128482926829,
"sd": 0.01527332166677 
},
{
 "name": "grid",
"mean": 0.006170853658537,
"sd": 0.01490296301573 
},
{
 "name": "grid",
"mean": 0.006205520620843,
"sd": 0.01471539983201 
},
{
 "name": "grid",
"mean":      0.0059994,
"sd": 0.01481055904923 
},
{
 "name": "grid",
"mean": 0.00605467902439,
"sd": 0.01434097802375 
},
{
 "name": "grid",
"mean": 0.006099907317073,
"sd": 0.01409601907058 
},
{
 "name": "grid",
"mean": 0.006137597560976,
"sd": 0.0139929929823 
},
{
 "name": "grid",
"mean": 0.00610856445993,
"sd": 0.01762346896852 
},
{
 "name": "grid",
"mean": 0.006164017682927,
"sd": 0.01672501370346 
},
{
 "name": "grid",
"mean": 0.00620714796748,
"sd": 0.01620221592418 
},
{
 "name": "grid",
"mean": 0.006241652195122,
"sd": 0.01590650994531 
},
{
 "name": "grid",
"mean": 0.006018799390244,
"sd": 0.01606750023634 
},
{
 "name": "grid",
"mean": 0.00607806504065,
"sd": 0.01546310237827 
},
{
 "name": "grid",
"mean": 0.006125477560976,
"sd": 0.01512949175355 
},
{
 "name": "grid",
"mean": 0.00616426962306,
"sd": 0.01496272427858 
},
{
 "name": "grid",
"mean": 0.005948982113821,
"sd": 0.01489693675735 
},
{
 "name": "grid",
"mean": 0.006009302926829,
"sd": 0.01448518213837 
},
{
 "name": "grid",
"mean": 0.00605865631929,
"sd": 0.01427763293619 
},
{
 "name": "grid",
"mean": 0.006099784146341,
"sd": 0.0141977862738 
},
{
 "name": "grid",
"mean": 0.005893128292683,
"sd": 0.0139919262871 
},
{
 "name": "grid",
"mean": 0.005953043015521,
"sd": 0.01371055235967 
},
{
 "name": "grid",
"mean": 0.00600297195122,
"sd": 0.01358891523181 
},
{
 "name": "grid",
"mean": 0.006045219512195,
"sd": 0.01356846684625 
},
{
 "name": "grid",
"mean": 0.005962079268293,
"sd": 0.01633882093493 
},
{
 "name": "grid",
"mean": 0.006027647154472,
"sd": 0.01576770590327 
},
{
 "name": "grid",
"mean": 0.006080101463415,
"sd": 0.01544944659934 
},
{
 "name": "grid",
"mean": 0.006123018625277,
"sd": 0.01528681141767 
},
{
 "name": "grid",
"mean": 0.005898564227642,
"sd": 0.01510511913297 
},
{
 "name": "grid",
"mean": 0.005963926829268,
"sd": 0.01472949482903 
},
{
 "name": "grid",
"mean": 0.006017405321508,
"sd": 0.01454191221073 
},
{
 "name": "grid",
"mean": 0.006061970731707,
"sd": 0.01447136220611 
},
{
 "name": "grid",
"mean": 0.005847752195122,
"sd": 0.01415150714207 
},
{
 "name": "grid",
"mean": 0.005911792017738,
"sd": 0.01390714124084 
},
{
 "name": "grid",
"mean": 0.005965158536585,
"sd": 0.0138081155123 
},
{
 "name": "grid",
"mean": 0.006010314821764,
"sd": 0.01380036727446 
},
{
 "name": "grid",
"mean": 0.005806178713969,
"sd": 0.01339816045447 
},
{
 "name": "grid",
"mean": 0.005868346341463,
"sd": 0.01324401666789 
},
{
 "name": "grid",
"mean": 0.005920949718574,
"sd": 0.01320588309161 
},
{
 "name": "grid",
"mean": 0.005848146341463,
"sd": 0.01543017683494 
},
{
 "name": "grid",
"mean": 0.005918550731707,
"sd": 0.01506904773659 
},
{
 "name": "grid",
"mean": 0.005976154323725,
"sd": 0.01488445427586 
},
{
 "name": "grid",
"mean": 0.006024157317073,
"sd": 0.01480990950933 
},
{
 "name": "grid",
"mean": 0.005802376097561,
"sd": 0.01441308339645 
},
{
 "name": "grid",
"mean": 0.005870541019956,
"sd": 0.01418806783321 
},
{
 "name": "grid",
"mean": 0.005927345121951,
"sd": 0.0140974961385 
},
{
 "name": "grid",
"mean": 0.005975410131332,
"sd": 0.01409115639749 
},
{
 "name": "grid",
"mean": 0.005764927716186,
"sd": 0.01360942019015 
},
{
 "name": "grid",
"mean": 0.005830532926829,
"sd": 0.01347744666743 
},
{
 "name": "grid",
"mean": 0.005886045028143,
"sd": 0.01345139414975 
},
{
 "name": "grid",
"mean": 0.005733720731707,
"sd": 0.01296302177616 
},
{
 "name": "grid",
"mean": 0.005796679924953,
"sd": 0.01289567081315 
},
{
 "name": "grid",
"mean": 0.005897414634146,
"sd": 0.01298709456156 
},
{
 "name": "grid",
"mean": 0.006167651219512,
"sd": 0.01765080552911 
},
{
 "name": "grid",
"mean": 0.006210377777778,
"sd": 0.01681878934358 
},
{
 "name": "grid",
"mean": 0.006022432926829,
"sd": 0.01719984614744 
},
{
 "name": "grid",
"mean": 0.006081294850949,
"sd": 0.01625072032199 
},
{
 "name": "grid",
"mean": 0.006128384390244,
"sd": 0.01566070233475 
},
{
 "name": "grid",
"mean": 0.006166912195122,
"sd": 0.0153034928752 
},
{
 "name": "grid",
"mean": 0.005952211924119,
"sd": 0.01585924343304 
},
{
 "name": "grid",
"mean": 0.006012209756098,
"sd": 0.01516303283812 
},
{
 "name": "grid",
"mean": 0.006061298891353,
"sd": 0.01473961678196 
},
{
 "name": "grid",
"mean": 0.006102206504065,
"sd": 0.01449653509221 
},
{
 "name": "grid",
"mean": 0.005955685587583,
"sd": 0.01429952694671 
},
{
 "name": "grid",
"mean": 0.006005394308943,
"sd": 0.01399388862978 
},
{
 "name": "grid",
"mean": 0.006047455534709,
"sd": 0.01383211663461 
},
{
 "name": "grid",
"mean": 0.005965712804878,
"sd": 0.01731481797847 
},
{
 "name": "grid",
"mean": 0.00603087696477,
"sd": 0.01642519830461 
},
{
 "name": "grid",
"mean": 0.006083008292683,
"sd": 0.01587305674783 
},
{
 "name": "grid",
"mean": 0.006125661197339,
"sd": 0.01553861925194 
},
{
 "name": "grid",
"mean": 0.00590179403794,
"sd": 0.01593579981074 
},
{
 "name": "grid",
"mean": 0.005966833658537,
"sd": 0.01529599733865 
},
{
 "name": "grid",
"mean": 0.00602004789357,
"sd": 0.01491043325849 
},
{
 "name": "grid",
"mean": 0.006064393089431,
"sd": 0.01469178645088 
},
{
 "name": "grid",
"mean": 0.00585065902439,
"sd": 0.01486659535431 
},
{
 "name": "grid",
"mean": 0.0059144345898,
"sd": 0.01439979551861 
},
{
 "name": "grid",
"mean": 0.005967580894309,
"sd": 0.0141311792226 
},
{
 "name": "grid",
"mean": 0.006012550844278,
"sd": 0.01399454694848 
},
{
 "name": "grid",
"mean": 0.005808821286031,
"sd": 0.01401955638823 
},
{
 "name": "grid",
"mean": 0.005870768699187,
"sd": 0.01367582015537 
},
{
 "name": "grid",
"mean": 0.005923185741088,
"sd": 0.01349096575548 
},
{
 "name": "grid",
"mean": 0.005968114634146,
"sd": 0.01341323294526 
},
{
 "name": "grid",
"mean": 0.005851376151762,
"sd": 0.01612648947586 
},
{
 "name": "grid",
"mean": 0.005921457560976,
"sd": 0.01552411574778 
},
{
 "name": "grid",
"mean": 0.005978796895787,
"sd": 0.01516077494372 
},
{
 "name": "grid",
"mean": 0.006026579674797,
"sd": 0.01495384750181 
},
{
 "name": "grid",
"mean": 0.005805282926829,
"sd": 0.01501332506232 
},
{
 "name": "grid",
"mean": 0.005873183592018,
"sd": 0.01458407045093 
},
{
 "name": "grid",
"mean": 0.005929767479675,
"sd": 0.01433950626801 
},
{
 "name": "grid",
"mean": 0.005977646153846,
"sd": 0.01421727629855 
},
{
 "name": "grid",
"mean": 0.005767570288248,
"sd": 0.0141315917102 
},
{
 "name": "grid",
"mean": 0.005832955284553,
"sd": 0.01382467064746 
},
{
 "name": "grid",
"mean": 0.005888281050657,
"sd": 0.01366469402167 
},
{
 "name": "grid",
"mean": 0.005736143089431,
"sd": 0.01342079694388 
},
{
 "name": "grid",
"mean": 0.005798915947467,
"sd": 0.01320211290885 
},
{
 "name": "grid",
"mean": 0.005899352520325,
"sd": 0.01308533187568 
},
{
 "name": "grid",
"mean": 0.005831932594235,
"sd": 0.01484922458309 
},
{
 "name": "grid",
"mean": 0.005891954065041,
"sd": 0.01461583252331 
},
{
 "name": "grid",
"mean": 0.005942741463415,
"sd": 0.01449752578069 
},
{
 "name": "grid",
"mean": 0.005726319290466,
"sd": 0.01432896252583 
},
{
 "name": "grid",
"mean": 0.005795141869919,
"sd": 0.0140458138479 
},
{
 "name": "grid",
"mean": 0.005853376360225,
"sd": 0.013899830062 
},
{
 "name": "grid",
"mean": 0.005903291637631,
"sd": 0.01384528634681 
},
{
 "name": "grid",
"mean": 0.005698329674797,
"sd": 0.01358099431366 
},
{
 "name": "grid",
"mean": 0.005764011257036,
"sd": 0.01338698259276 
},
{
 "name": "grid",
"mean": 0.005869101788618,
"sd": 0.01329531578174 
},
{
 "name": "grid",
"mean": 0.005674646153846,
"sd": 0.01296905298474 
},
{
 "name": "grid",
"mean": 0.005737327874564,
"sd": 0.01283996779717 
},
{
 "name": "grid",
"mean": 0.00579165203252,
"sd": 0.01280261671543 
},
{
 "name": "grid",
"mean": 0.005839185670732,
"sd": 0.01282726521035 
},
{
 "name": "grid",
"mean": 0.006972038211382,
"sd": 0.02385902889212 
},
{
 "name": "grid",
"mean": 0.006867076219512,
"sd": 0.02120103627701 
},
{
 "name": "grid",
"mean": 0.006778413821138,
"sd": 0.02282739052688 
},
{
 "name": "grid",
"mean": 0.006721857926829,
"sd": 0.02015621396031 
},
{
 "name": "grid",
"mean": 0.00670300596206,
"sd": 0.01944872455144 
},
{
 "name": "grid",
"mean": 0.006576639634146,
"sd": 0.01925950203934 
},
{
 "name": "grid",
"mean": 0.00657392303523,
"sd": 0.01854611364427 
},
{
 "name": "grid",
"mean": 0.006431421341463,
"sd": 0.01853241247079 
},
{
 "name": "grid",
"mean": 0.006444840108401,
"sd": 0.01777299162267 
},
{
 "name": "grid",
"mean": 0.006464358314856,
"sd": 0.01696588790433 
},
{
 "name": "grid",
"mean": 0.00670278699187,
"sd": 0.02319876818477 
},
{
 "name": "grid",
"mean": 0.006665137804878,
"sd": 0.02055194951076 
},
{
 "name": "grid",
"mean": 0.006652588075881,
"sd": 0.01983802818147 
},
{
 "name": "grid",
"mean": 0.006519919512195,
"sd": 0.01956791107481 
},
{
 "name": "grid",
"mean": 0.006523505149051,
"sd": 0.01886758309203 
},
{
 "name": "grid",
"mean": 0.006374701219512,
"sd": 0.01874273444739 
},
{
 "name": "grid",
"mean": 0.006394422222222,
"sd": 0.01801775448343 
},
{
 "name": "grid",
"mean": 0.00641019902439,
"sd": 0.01754602308879 
},
{
 "name": "grid",
"mean": 0.006423107317073,
"sd": 0.01723777590635 
},
{
 "name": "grid",
"mean": 0.006265339295393,
"sd": 0.01730632014036 
},
{
 "name": "grid",
"mean": 0.006317494013304,
"sd": 0.01648611813098 
},
{
 "name": "grid",
"mean": 0.00633705203252,
"sd": 0.01629077292614 
},
{
 "name": "grid",
"mean": 0.006463199390244,
"sd": 0.01998837558679 
},
{
 "name": "grid",
"mean": 0.006473087262873,
"sd": 0.01927933831931 
},
{
 "name": "grid",
"mean": 0.006317981097561,
"sd": 0.0190732054695 
},
{
 "name": "grid",
"mean": 0.006344004336043,
"sd": 0.01835972646579 
},
{
 "name": "grid",
"mean": 0.006364822926829,
"sd": 0.01788903933621 
},
{
 "name": "grid",
"mean": 0.00638185631929,
"sd": 0.01757566055301 
},
{
 "name": "grid",
"mean": 0.006214921409214,
"sd": 0.01756933923206 
},
{
 "name": "grid",
"mean": 0.006248648292683,
"sd": 0.01708383349948 
},
{
 "name": "grid",
"mean": 0.006276243015521,
"sd": 0.01677402574539 
},
{
 "name": "grid",
"mean": 0.006299238617886,
"sd": 0.01657947362274 
},
{
 "name": "grid",
"mean": 0.006170629711752,
"sd": 0.01606215411651 
},
{
 "name": "grid",
"mean": 0.006202426422764,
"sd": 0.01586364803446 
},
{
 "name": "grid",
"mean": 0.006229331332083,
"sd": 0.01575268738688 
},
{
 "name": "grid",
"mean": 0.00626126097561,
"sd": 0.0195177234568 
},
{
 "name": "grid",
"mean": 0.006293586449864,
"sd": 0.01879360181123 
},
{
 "name": "grid",
"mean": 0.006340605321508,
"sd": 0.01797582068817 
},
{
 "name": "grid",
"mean": 0.006164503523035,
"sd": 0.01793138252253 
},
{
 "name": "grid",
"mean": 0.006234992017738,
"sd": 0.01712913156722 
},
{
 "name": "grid",
"mean": 0.006261425203252,
"sd": 0.01692449453407 
},
{
 "name": "grid",
"mean": 0.006129378713969,
"sd": 0.01636596854463 
},
{
 "name": "grid",
"mean": 0.00616461300813,
"sd": 0.01616716259544 
},
{
 "name": "grid",
"mean": 0.006194426641651,
"sd": 0.01605181062744 
},
{
 "name": "grid",
"mean": 0.0060237654102,
"sd": 0.01569851788811 
},
{
 "name": "grid",
"mean": 0.006067800813008,
"sd": 0.01548586359035 
},
{
 "name": "grid",
"mean": 0.006105061538462,
"sd": 0.01536988756975 
},
{
 "name": "grid",
"mean": 0.006136999303136,
"sd": 0.01531826368554 
},
{
 "name": "grid",
"mean": 0.006707631707317,
"sd": 0.02298537539729 
},
{
 "name": "grid",
"mean": 0.006668771341463,
"sd": 0.02011778351184 
},
{
 "name": "grid",
"mean": 0.006655817886179,
"sd": 0.01936001383902 
},
{
 "name": "grid",
"mean": 0.00652355304878,
"sd": 0.01926372289495 
},
{
 "name": "grid",
"mean": 0.00652673495935,
"sd": 0.01848964486262 
},
{
 "name": "grid",
"mean": 0.006378334756098,
"sd": 0.01858287549634 
},
{
 "name": "grid",
"mean": 0.00639765203252,
"sd": 0.01775216194522 
},
{
 "name": "grid",
"mean": 0.006413105853659,
"sd": 0.01721311529523 
},
{
 "name": "grid",
"mean": 0.006425749889135,
"sd": 0.01686339170207 
},
{
 "name": "grid",
"mean": 0.006268569105691,
"sd": 0.01716470202473 
},
{
 "name": "grid",
"mean": 0.006320136585366,
"sd": 0.01619002399017 
},
{
 "name": "grid",
"mean": 0.006339474390244,
"sd": 0.01595517048968 
},
{
 "name": "grid",
"mean": 0.006466832926829,
"sd": 0.01956779862359 
},
{
 "name": "grid",
"mean": 0.006476317073171,
"sd": 0.01880857186265 
},
{
 "name": "grid",
"mean": 0.006321614634146,
"sd": 0.01878818890851 
},
{
 "name": "grid",
"mean": 0.006347234146341,
"sd": 0.0179935418157 
},
{
 "name": "grid",
"mean": 0.006367729756098,
"sd": 0.01747451268861 
},
{
 "name": "grid",
"mean": 0.006384498891353,
"sd": 0.01713432852396 
},
{
 "name": "grid",
"mean": 0.006218151219512,
"sd": 0.01732016668277 
},
{
 "name": "grid",
"mean": 0.006251555121951,
"sd": 0.01676124156323 
},
{
 "name": "grid",
"mean": 0.006278885587583,
"sd": 0.01640551840307 
},
{
 "name": "grid",
"mean": 0.00630166097561,
"sd": 0.01618372635847 
},
{
 "name": "grid",
"mean": 0.006173272283814,
"sd": 0.01577522150408 
},
{
 "name": "grid",
"mean": 0.006204848780488,
"sd": 0.01553342490895 
},
{
 "name": "grid",
"mean": 0.006231567354597,
"sd": 0.01539542535144 
},
{
 "name": "grid",
"mean": 0.006264894512195,
"sd": 0.019113506035 
},
{
 "name": "grid",
"mean": 0.006296816260163,
"sd": 0.01833236684895 
},
{
 "name": "grid",
"mean": 0.006322353658537,
"sd": 0.01781590622658 
},
{
 "name": "grid",
"mean": 0.00634324789357,
"sd": 0.01747168589743 
},
{
 "name": "grid",
"mean": 0.006167733333333,
"sd": 0.01757922598148 
},
{
 "name": "grid",
"mean": 0.00620617902439,
"sd": 0.01703938897211 
},
{
 "name": "grid",
"mean": 0.0062376345898,
"sd": 0.01669217035637 
},
{
 "name": "grid",
"mean": 0.006263847560976,
"sd": 0.01647205023326 
},
{
 "name": "grid",
"mean": 0.006090004390244,
"sd": 0.01638015742269 
},
{
 "name": "grid",
"mean": 0.006132021286031,
"sd": 0.01600493841403 
},
{
 "name": "grid",
"mean": 0.006167035365854,
"sd": 0.01577545489814 
},
{
 "name": "grid",
"mean": 0.006196662664165,
"sd": 0.01564306989085 
},
{
 "name": "grid",
"mean": 0.006026407982262,
"sd": 0.01542233171055 
},
{
 "name": "grid",
"mean": 0.006070223170732,
"sd": 0.01516237808515 
},
{
 "name": "grid",
"mean": 0.006107297560976,
"sd": 0.01501639292575 
},
{
 "name": "grid",
"mean": 0.006139075609756,
"sd": 0.01494689723364 
},
{
 "name": "grid",
"mean": 0.006117315447154,
"sd": 0.01793739201121 
},
{
 "name": "grid",
"mean": 0.006196383592018,
"sd": 0.01704639047425 
},
{
 "name": "grid",
"mean": 0.006226034146341,
"sd": 0.01681706827835 
},
{
 "name": "grid",
"mean": 0.006090770288248,
"sd": 0.01630711052371 
},
{
 "name": "grid",
"mean": 0.00612922195122,
"sd": 0.0160783261453 
},
{
 "name": "grid",
"mean": 0.006161757973734,
"sd": 0.01594226636412 
},
{
 "name": "grid",
"mean": 0.005985156984479,
"sd": 0.01566604731218 
},
{
 "name": "grid",
"mean": 0.006032409756098,
"sd": 0.01541776739311 
},
{
 "name": "grid",
"mean": 0.006072392870544,
"sd": 0.01527666136135 
},
{
 "name": "grid",
"mean": 0.006106664111498,
"sd": 0.01520750910387 
},
{
 "name": "grid",
"mean": 0.005935597560976,
"sd": 0.01484583189786 
},
{
 "name": "grid",
"mean": 0.005983027767355,
"sd": 0.01468250363766 
},
{
 "name": "grid",
"mean": 0.006023682229965,
"sd": 0.01460439298553 
},
{
 "name": "grid",
"mean": 0.006058916097561,
"sd": 0.014583696466 
},
{
 "name": "grid",
"mean": 0.006470466463415,
"sd": 0.01974201218725 
},
{
 "name": "grid",
"mean": 0.006479546883469,
"sd": 0.01882518768687 
},
{
 "name": "grid",
"mean": 0.006325248170732,
"sd": 0.01912301325355 
},
{
 "name": "grid",
"mean": 0.00635046395664,
"sd": 0.01813867042261 
},
{
 "name": "grid",
"mean": 0.006370636585366,
"sd": 0.01748512735047 
},
{
 "name": "grid",
"mean": 0.006387141463415,
"sd": 0.01704958219449 
},
{
 "name": "grid",
"mean": 0.00622138102981,
"sd": 0.01760257064291 
},
{
 "name": "grid",
"mean": 0.00625446195122,
"sd": 0.01688346202265 
},
{
 "name": "grid",
"mean": 0.006281528159645,
"sd": 0.01641145323812 
},
{
 "name": "grid",
"mean": 0.006304083333333,
"sd": 0.01610542469056 
},
{
 "name": "grid",
"mean": 0.006175914855876,
"sd": 0.01587904597543 
},
{
 "name": "grid",
"mean": 0.006207271138211,
"sd": 0.01553566549016 
},
{
 "name": "grid",
"mean": 0.006233803377111,
"sd": 0.01532268594461 
},
{
 "name": "grid",
"mean": 0.00626852804878,
"sd": 0.01931826635121 
},
{
 "name": "grid",
"mean": 0.006300046070461,
"sd": 0.01837138279994 
},
{
 "name": "grid",
"mean": 0.006325260487805,
"sd": 0.01773950542798 
},
{
 "name": "grid",
"mean": 0.006345890465632,
"sd": 0.01731505575304 
},
{
 "name": "grid",
"mean": 0.006170963143631,
"sd": 0.01775048504966 
},
{
 "name": "grid",
"mean": 0.006209085853659,
"sd": 0.01706942591505 
},
{
 "name": "grid",
"mean": 0.006240277161863,
"sd": 0.01662142061054 
},
{
 "name": "grid",
"mean": 0.006266269918699,
"sd": 0.01632960547348 
},
{
 "name": "grid",
"mean": 0.006092911219512,
"sd": 0.01652498314946 
},
{
 "name": "grid",
"mean": 0.006134663858093,
"sd": 0.01602787727108 
},
{
 "name": "grid",
"mean": 0.006169457723577,
"sd": 0.01570956589082 
},
{
 "name": "grid",
"mean": 0.006198898686679,
"sd": 0.01551271310129 
},
{
 "name": "grid",
"mean": 0.006029050554324,
"sd": 0.01554589450126 
},
{
 "name": "grid",
"mean": 0.006072645528455,
"sd": 0.01517962778312 
},
{
 "name": "grid",
"mean": 0.00610953358349,
"sd": 0.0149547413905 
},
{
 "name": "grid",
"mean": 0.006141151916376,
"sd": 0.01482696026789 
},
{
 "name": "grid",
"mean": 0.006120545257453,
"sd": 0.01799968832021 
},
{
 "name": "grid",
"mean": 0.006163709756098,
"sd": 0.01733954952416 
},
{
 "name": "grid",
"mean": 0.00619902616408,
"sd": 0.0169017989782 
},
{
 "name": "grid",
"mean": 0.006228456504065,
"sd": 0.01661316441639 
},
{
 "name": "grid",
"mean": 0.006047535121951,
"sd": 0.0167249329312 
},
{
 "name": "grid",
"mean": 0.00609341286031,
"sd": 0.01625130635997 
},
{
 "name": "grid",
"mean": 0.006131644308943,
"sd": 0.01594659604067 
},
{
 "name": "grid",
"mean": 0.006163993996248,
"sd": 0.01575650780868 
},
{
 "name": "grid",
"mean": 0.005987799556541,
"sd": 0.01570668225419 
},
{
 "name": "grid",
"mean": 0.006034832113821,
"sd": 0.0153651167337 
},
{
 "name": "grid",
"mean": 0.006074628893058,
"sd": 0.01515591189117 
},
{
 "name": "grid",
"mean": 0.005938019918699,
"sd": 0.0148787060188 
},
{
 "name": "grid",
"mean": 0.005985263789869,
"sd": 0.01463266200582 
},
{
 "name": "grid",
"mean": 0.00606085398374,
"sd": 0.01442709419139 
},
{
 "name": "grid",
"mean": 0.006052161862528,
"sd": 0.01654631157858 
},
{
 "name": "grid",
"mean": 0.006093830894309,
"sd": 0.01624399263238 
},
{
 "name": "grid",
"mean": 0.006129089305816,
"sd": 0.01605162036193 
},
{
 "name": "grid",
"mean": 0.005946548558758,
"sd": 0.01594328256306 
},
{
 "name": "grid",
"mean": 0.005997018699187,
"sd": 0.01561481448486 
},
{
 "name": "grid",
"mean": 0.006039724202627,
"sd": 0.01541177302602 
},
{
 "name": "grid",
"mean": 0.006076328919861,
"sd": 0.01529464393701 
},
{
 "name": "grid",
"mean": 0.005900206504065,
"sd": 0.01507560147225 
},
{
 "name": "grid",
"mean": 0.005950359099437,
"sd": 0.01484486391192 
},
{
 "name": "grid",
"mean": 0.006030603252033,
"sd": 0.01465219433729 
},
{
 "name": "grid",
"mean": 0.005860993996248,
"sd": 0.01435953434192 
},
{
 "name": "grid",
"mean": 0.005910365156794,
"sd": 0.0142004102161 
},
{
 "name": "grid",
"mean": 0.005953153495935,
"sd": 0.01412288105648 
},
{
 "name": "grid",
"mean": 0.005990593292683,
"sd": 0.01410183889317 
},
{
 "name": "grid",
"mean": 0.006272161585366,
"sd": 0.02011341429336 
},
{
 "name": "grid",
"mean": 0.006303275880759,
"sd": 0.0189075530974 
},
{
 "name": "grid",
"mean": 0.006348533037694,
"sd": 0.01751525541665 
},
{
 "name": "grid",
"mean": 0.00617419295393,
"sd": 0.01843057457977 
},
{
 "name": "grid",
"mean": 0.006242919733925,
"sd": 0.01692147641212 
},
{
 "name": "grid",
"mean": 0.006268692276423,
"sd": 0.01650518837612 
},
{
 "name": "grid",
"mean": 0.006137306430155,
"sd": 0.0164331772626 
},
{
 "name": "grid",
"mean": 0.006171880081301,
"sd": 0.01597352797888 
},
{
 "name": "grid",
"mean": 0.006201134709193,
"sd": 0.01566769034918 
},
{
 "name": "grid",
"mean": 0.006031693126386,
"sd": 0.01605998210601 
},
{
 "name": "grid",
"mean": 0.006075067886179,
"sd": 0.01553647780066 
},
{
 "name": "grid",
"mean": 0.006111769606004,
"sd": 0.01518848723787 
},
{
 "name": "grid",
"mean": 0.006143228222997,
"sd": 0.01496449945561 
},
{
 "name": "grid",
"mean": 0.006123775067751,
"sd": 0.01856834277844 
},
{
 "name": "grid",
"mean": 0.006201668736142,
"sd": 0.01712261248016 
},
{
 "name": "grid",
"mean": 0.006230878861789,
"sd": 0.01672179209154 
},
{
 "name": "grid",
"mean": 0.006096055432373,
"sd": 0.01657436973539 
},
{
 "name": "grid",
"mean": 0.006134066666667,
"sd": 0.01614041298459 
},
{
 "name": "grid",
"mean": 0.006166230018762,
"sd": 0.01585158795791 
},
{
 "name": "grid",
"mean": 0.005990442128603,
"sd": 0.01613680158607 
},
{
 "name": "grid",
"mean": 0.006037254471545,
"sd": 0.01564940016331 
},
{
 "name": "grid",
"mean": 0.006076864915572,
"sd": 0.01532711874811 
},
{
 "name": "grid",
"mean": 0.006110816724739,
"sd": 0.01512098814332 
},
{
 "name": "grid",
"mean": 0.005940442276423,
"sd": 0.01525748056617 
},
{
 "name": "grid",
"mean": 0.005987499812383,
"sd": 0.01488446514475 
},
{
 "name": "grid",
"mean": 0.006027834843206,
"sd": 0.01464518954448 
},
{
 "name": "grid",
"mean": 0.006062791869919,
"sd": 0.01450096508944 
},
{
 "name": "grid",
"mean": 0.00605480443459,
"sd": 0.01678789769949 
},
{
 "name": "grid",
"mean": 0.006096253252033,
"sd": 0.01636893927793 
},
{
 "name": "grid",
"mean": 0.00613132532833,
"sd": 0.01608828356456 
},
{
 "name": "grid",
"mean": 0.00594919113082,
"sd": 0.01628904740838 
},
{
 "name": "grid",
"mean": 0.005999441056911,
"sd": 0.01582704062807 
},
{
 "name": "grid",
"mean": 0.006041960225141,
"sd": 0.01552142715793 
},
{
 "name": "grid",
"mean": 0.006078405226481,
"sd": 0.01532556911091 
},
{
 "name": "grid",
"mean": 0.005902628861789,
"sd": 0.01538000164493 
},
{
 "name": "grid",
"mean": 0.005952595121951,
"sd": 0.01503248280158 
},
{
 "name": "grid",
"mean": 0.006032541138211,
"sd": 0.0146782661674 
},
{
 "name": "grid",
"mean": 0.005863230018762,
"sd": 0.01462926459315 
},
{
 "name": "grid",
"mean": 0.005912441463415,
"sd": 0.01436718200452 
},
{
 "name": "grid",
"mean": 0.005955091382114,
"sd": 0.01420855829169 
},
{
 "name": "grid",
"mean": 0.005992410060976,
"sd": 0.0141237764009 
},
{
 "name": "grid",
"mean": 0.005907940133038,
"sd": 0.01651463367375 
},
{
 "name": "grid",
"mean": 0.005961627642276,
"sd": 0.01606725275721 
},
{
 "name": "grid",
"mean": 0.006007055534709,
"sd": 0.01576935446604 
},
{
 "name": "grid",
"mean": 0.006045993728223,
"sd": 0.01557634753034 
},
{
 "name": "grid",
"mean": 0.005864815447154,
"sd": 0.01556817569658 
},
{
 "name": "grid",
"mean": 0.00591769043152,
"sd": 0.01523704312098 
},
{
 "name": "grid",
"mean": 0.00596301184669,
"sd": 0.01502529620904 
},
{
 "name": "grid",
"mean": 0.006002290406504,
"sd": 0.01489797795102 
},
{
 "name": "grid",
"mean": 0.00582832532833,
"sd": 0.01478652870222 
},
{
 "name": "grid",
"mean": 0.005880029965157,
"sd": 0.01454183901657 
},
{
 "name": "grid",
"mean": 0.005924840650407,
"sd": 0.01439462457546 
},
{
 "name": "grid",
"mean":     0.00596405,
"sd": 0.01431679178202 
},
{
 "name": "grid",
"mean": 0.005797048083624,
"sd": 0.01413291441385 
},
{
 "name": "grid",
"mean": 0.005847390894309,
"sd": 0.01395343148512 
},
{
 "name": "grid",
"mean": 0.005891440853659,
"sd": 0.01385507848708 
},
{
 "name": "grid",
"mean": 0.005930308464849,
"sd": 0.01381480519508 
},
{
 "name": "grid",
"mean": 0.006492682926829,
"sd": 0.01580118185956 
},
{
 "name": "grid",
"mean": 0.006556254878049,
"sd": 0.01444811760315 
},
{
 "name": "grid",
"mean":      0.0065549,
"sd": 0.01440213503086 
},
{
 "name": "grid",
"mean": 0.006265818292683,
"sd": 0.01331461878249 
},
{
 "name": "grid",
"mean": 0.006361275609756,
"sd": 0.01283663676014 
},
{
 "name": "grid",
"mean": 0.00638854912892,
"sd": 0.01316747829315 
},
{
 "name": "grid",
"mean": 0.006167651219512,
"sd": 0.011667123363 
},
{
 "name": "grid",
"mean": 0.00626378597561,
"sd": 0.0122941859514 
},
{
 "name": "grid",
"mean": 0.005974026829268,
"sd": 0.01102039319688 
},
{
 "name": "grid",
"mean": 0.006056621602787,
"sd": 0.01097700642089 
},
{
 "name": "grid",
"mean": 0.006118567682927,
"sd": 0.01125942295293 
},
{
 "name": "grid",
"mean": 0.00616674796748,
"sd": 0.01165352386895 
},
{
 "name": "grid",
"mean": 0.00615237804878,
"sd": 0.01368910543937 
},
{
 "name": "grid",
"mean": 0.006285648780488,
"sd": 0.01344693366071 
},
{
 "name": "grid",
"mean": 0.006323726132404,
"sd": 0.01376259621524 
},
{
 "name": "grid",
"mean": 0.006092024390244,
"sd": 0.01203376667259 
},
{
 "name": "grid",
"mean": 0.006207065853659,
"sd": 0.01274871803024 
},
{
 "name": "grid",
"mean":      0.0058984,
"sd": 0.0110809670235 
},
{
 "name": "grid",
"mean": 0.006061847560976,
"sd": 0.01157680724507 
},
{
 "name": "grid",
"mean": 0.006116330081301,
"sd": 0.01200404817777 
},
{
 "name": "grid",
"mean": 0.005825834843206,
"sd": 0.01045403756143 
},
{
 "name": "grid",
"mean": 0.005916629268293,
"sd": 0.01064807758045 
},
{
 "name": "grid",
"mean": 0.005987247154472,
"sd": 0.0110190880877 
},
{
 "name": "grid",
"mean": 0.006043741463415,
"sd": 0.01144156144851 
},
{
 "name": "grid",
"mean": 0.006016397560976,
"sd": 0.01271931363299 
},
{
 "name": "grid",
"mean": 0.006150345731707,
"sd": 0.01336300506472 
},
{
 "name": "grid",
"mean": 0.005822773170732,
"sd": 0.01150679018403 
},
{
 "name": "grid",
"mean": 0.006005127439024,
"sd": 0.01208005035106 
},
{
 "name": "grid",
"mean": 0.006065912195122,
"sd": 0.01249278033953 
},
{
 "name": "grid",
"mean": 0.005859909146341,
"sd": 0.01100689621117 
},
{
 "name": "grid",
"mean": 0.005936829268293,
"sd": 0.01140725975944 
},
{
 "name": "grid",
"mean": 0.005714690853659,
"sd": 0.01020991417778 
},
{
 "name": "grid",
"mean": 0.005807746341463,
"sd": 0.01050699981496 
},
{
 "name": "grid",
"mean": 0.005943099778271,
"sd": 0.01132365762617 
},
{
 "name": "grid",
"mean": 0.005747146341463,
"sd": 0.01225986283382 
},
{
 "name": "grid",
"mean": 0.00586215261324,
"sd": 0.01242654034682 
},
{
 "name": "grid",
"mean": 0.005948407317073,
"sd": 0.01274715877038 
},
{
 "name": "grid",
"mean": 0.006015494308943,
"sd": 0.01310426581066 
},
{
 "name": "grid",
"mean": 0.005696188850174,
"sd": 0.0112776135055 
},
{
 "name": "grid",
"mean": 0.00580318902439,
"sd": 0.01155764950091 
},
{
 "name": "grid",
"mean": 0.005886411382114,
"sd": 0.01193778379857 
},
{
 "name": "grid",
"mean": 0.005952989268293,
"sd": 0.01233487786014 
},
{
 "name": "grid",
"mean": 0.005657970731707,
"sd": 0.01060824692631 
},
{
 "name": "grid",
"mean": 0.005757328455285,
"sd": 0.01093229330429 
},
{
 "name": "grid",
"mean": 0.005901848780488,
"sd": 0.01173730254188 
},
{
 "name": "grid",
"mean": 0.005628245528455,
"sd": 0.01013581999868 
},
{
 "name": "grid",
"mean":     0.00572064,
"sd": 0.01046970009494 
},
{
 "name": "grid",
"mean": 0.005796235476718,
"sd": 0.01086303340548 
},
{
 "name": "grid",
"mean": 0.005859231707317,
"sd": 0.01126484550996 
},
{
 "name": "grid",
"mean": 0.006159645121951,
"sd": 0.01606909344269 
},
{
 "name": "grid",
"mean": 0.006290493495935,
"sd": 0.01381165559812 
},
{
 "name": "grid",
"mean": 0.006327878745645,
"sd": 0.01374053137441 
},
{
 "name": "grid",
"mean": 0.006096869105691,
"sd": 0.01285087446928 
},
{
 "name": "grid",
"mean": 0.006210699390244,
"sd": 0.01272216941823 
},
{
 "name": "grid",
"mean": 0.005903244715447,
"sd": 0.01239003201287 
},
{
 "name": "grid",
"mean": 0.006065481097561,
"sd": 0.01179794645208 
},
{
 "name": "grid",
"mean": 0.006119559891599,
"sd": 0.01197479400107 
},
{
 "name": "grid",
"mean": 0.005829987456446,
"sd": 0.01147100899638 
},
{
 "name": "grid",
"mean": 0.005920262804878,
"sd": 0.01115329197417 
},
{
 "name": "grid",
"mean": 0.00599047696477,
"sd": 0.01119541525318 
},
{
 "name": "grid",
"mean": 0.006046648292683,
"sd": 0.01141077155102 
},
{
 "name": "grid",
"mean": 0.006021242276423,
"sd": 0.01317337675953 
},
{
 "name": "grid",
"mean": 0.006153979268293,
"sd": 0.01315558421677 
},
{
 "name": "grid",
"mean": 0.005827617886179,
"sd": 0.01243200567497 
},
{
 "name": "grid",
"mean": 0.00600876097561,
"sd": 0.01209431268879 
},
{
 "name": "grid",
"mean": 0.00606914200542,
"sd": 0.01231082017561 
},
{
 "name": "grid",
"mean": 0.005863542682927,
"sd": 0.01128458410127 
},
{
 "name": "grid",
"mean": 0.005940059078591,
"sd": 0.01141187143709 
},
{
 "name": "grid",
"mean": 0.005718324390244,
"sd": 0.01078321440286 
},
{
 "name": "grid",
"mean": 0.005810976151762,
"sd": 0.01072943527321 
},
{
 "name": "grid",
"mean": 0.005945742350333,
"sd": 0.01117945022675 
},
{
 "name": "grid",
"mean": 0.005751991056911,
"sd":  0.01280141645 
},
{
 "name": "grid",
"mean": 0.005952040853659,
"sd": 0.01257022466309 
},
{
 "name": "grid",
"mean": 0.006018724119241,
"sd": 0.01278267301043 
},
{
 "name": "grid",
"mean": 0.005806822560976,
"sd": 0.01161658489206 
},
{
 "name": "grid",
"mean": 0.005889641192412,
"sd": 0.01178151648005 
},
{
 "name": "grid",
"mean": 0.005661604268293,
"sd": 0.0109428504798 
},
{
 "name": "grid",
"mean": 0.005760558265583,
"sd": 0.01097392305114 
},
{
 "name": "grid",
"mean": 0.005839721463415,
"sd": 0.0111904838473 
},
{
 "name": "grid",
"mean": 0.00590449135255,
"sd": 0.0114877092969 
},
{
 "name": "grid",
"mean": 0.005631475338753,
"sd": 0.01040506121942 
},
{
 "name": "grid",
"mean": 0.00579887804878,
"sd": 0.01073780298631 
},
{
 "name": "grid",
"mean": 0.005861654065041,
"sd": 0.01104227216549 
},
{
 "name": "grid",
"mean": 0.005635518466899,
"sd": 0.01219227970607 
},
{
 "name": "grid",
"mean": 0.005750102439024,
"sd": 0.01213282903978 
},
{
 "name": "grid",
"mean": 0.005839223306233,
"sd": 0.01229053641261 
},
{
 "name": "grid",
"mean":     0.00591052,
"sd": 0.01254088876027 
},
{
 "name": "grid",
"mean": 0.005604884146341,
"sd": 0.01130802945375 
},
{
 "name": "grid",
"mean": 0.005710140379404,
"sd": 0.01137598465408 
},
{
 "name": "grid",
"mean": 0.005863240354767,
"sd": 0.01189193770037 
},
{
 "name": "grid",
"mean": 0.005581057452575,
"sd": 0.01067633943235 
},
{
 "name": "grid",
"mean": 0.005757627050998,
"sd": 0.01107086506799 
},
{
 "name": "grid",
"mean": 0.005823840650407,
"sd": 0.01137886375711 
},
{
 "name": "grid",
"mean": 0.005561996097561,
"sd": 0.01021311172255 
},
{
 "name": "grid",
"mean": 0.005652013747228,
"sd": 0.0103860812514 
},
{
 "name": "grid",
"mean": 0.005727028455285,
"sd": 0.01065598548106 
},
{
 "name": "grid",
"mean": 0.005790502439024,
"sd": 0.01096722345965 
},
{
 "name": "grid",
"mean": 0.00602608699187,
"sd": 0.01506802511012 
},
{
 "name": "grid",
"mean": 0.006157612804878,
"sd": 0.01382221420983 
},
{
 "name": "grid",
"mean": 0.005832462601626,
"sd": 0.01478018671676 
},
{
 "name": "grid",
"mean": 0.006012394512195,
"sd": 0.01304231191919 
},
{
 "name": "grid",
"mean": 0.006072371815718,
"sd": 0.0128685217734 
},
{
 "name": "grid",
"mean": 0.005867176219512,
"sd": 0.01253062769086 
},
{
 "name": "grid",
"mean": 0.005943288888889,
"sd": 0.01220212566015 
},
{
 "name": "grid",
"mean": 0.005721957926829,
"sd": 0.01232062482391 
},
{
 "name": "grid",
"mean": 0.00581420596206,
"sd": 0.01176436755225 
},
{
 "name": "grid",
"mean": 0.005948384922395,
"sd": 0.01158260167912 
},
{
 "name": "grid",
"mean": 0.005756835772358,
"sd": 0.01480536723845 
},
{
 "name": "grid",
"mean": 0.005955674390244,
"sd": 0.01330472687795 
},
{
 "name": "grid",
"mean": 0.006021953929539,
"sd": 0.01317677891591 
},
{
 "name": "grid",
"mean": 0.005810456097561,
"sd": 0.01264102626277 
},
{
 "name": "grid",
"mean": 0.00589287100271,
"sd": 0.0123957010923 
},
{
 "name": "grid",
"mean": 0.005665237804878,
"sd": 0.01226547056782 
},
{
 "name": "grid",
"mean": 0.005763788075881,
"sd": 0.01182771169015 
},
{
 "name": "grid",
"mean": 0.005842628292683,
"sd": 0.01170980018353 
},
{
 "name": "grid",
"mean": 0.005907133924612,
"sd": 0.0117725242734 
},
{
 "name": "grid",
"mean": 0.005634705149051,
"sd": 0.01150441555951 
},
{
 "name": "grid",
"mean": 0.005801520620843,
"sd": 0.01118110013032 
},
{
 "name": "grid",
"mean": 0.005864076422764,
"sd": 0.01128735829012 
},
{
 "name": "grid",
"mean": 0.00575373597561,
"sd": 0.01293181152676 
},
{
 "name": "grid",
"mean": 0.005842453116531,
"sd": 0.01273164460628 
},
{
 "name": "grid",
"mean": 0.005608517682927,
"sd": 0.01239931571447 
},
{
 "name": "grid",
"mean": 0.005713370189702,
"sd": 0.01204446003828 
},
{
 "name": "grid",
"mean": 0.005797252195122,
"sd": 0.01197451058856 
},
{
 "name": "grid",
"mean": 0.005865882926829,
"sd": 0.01206197967748 
},
{
 "name": "grid",
"mean": 0.005584287262873,
"sd": 0.011587006349 
},
{
 "name": "grid",
"mean": 0.005681077560976,
"sd": 0.01137868520203 
},
{
 "name": "grid",
"mean": 0.00576026962306,
"sd": 0.01138986168036 
},
{
 "name": "grid",
"mean": 0.00582626300813,
"sd": 0.01152419599844 
},
{
 "name": "grid",
"mean": 0.00565465631929,
"sd": 0.01086863854301 
},
{
 "name": "grid",
"mean": 0.005729450813008,
"sd": 0.01093053003208 
},
{
 "name": "grid",
"mean": 0.005792738461538,
"sd": 0.01109216043277 
},
{
 "name": "grid",
"mean": 0.005551797560976,
"sd": 0.0127161936861 
},
{
 "name": "grid",
"mean": 0.005662952303523,
"sd": 0.01240657508216 
},
{
 "name": "grid",
"mean": 0.005824631929047,
"sd": 0.01244402425157 
},
{
 "name": "grid",
"mean": 0.005533869376694,
"sd": 0.0118256369285 
},
{
 "name": "grid",
"mean": 0.005719018625277,
"sd": 0.01170060929751 
},
{
 "name": "grid",
"mean": 0.005788449593496,
"sd": 0.01184397045765 
},
{
 "name": "grid",
"mean": 0.005613405321508,
"sd": 0.01109574403162 
},
{
 "name": "grid",
"mean": 0.005691637398374,
"sd": 0.01118531525588 
},
{
 "name": "grid",
"mean": 0.005757833771107,
"sd": 0.01136076804226 
},
{
 "name": "grid",
"mean": 0.005507792017738,
"sd": 0.01065305461453 
},
{
 "name": "grid",
"mean": 0.005594825203252,
"sd": 0.01065077988425 
},
{
 "name": "grid",
"mean": 0.005668468667917,
"sd": 0.01077355173539 
},
{
 "name": "grid",
"mean": 0.005731591637631,
"sd": 0.01096690156276 
},
{
 "name": "grid",
"mean": 0.005761680487805,
"sd": 0.01778404184996 
},
{
 "name": "grid",
"mean": 0.005874610452962,
"sd": 0.01589406818503 
},
{
 "name": "grid",
"mean": 0.005959307926829,
"sd": 0.01481572422686 
},
{
 "name": "grid",
"mean": 0.006025183739837,
"sd": 0.01422723101512 
},
{
 "name": "grid",
"mean": 0.005708646689895,
"sd": 0.01575725429923 
},
{
 "name": "grid",
"mean": 0.005814089634146,
"sd": 0.01442673778964 
},
{
 "name": "grid",
"mean": 0.005896100813008,
"sd": 0.01367693339245 
},
{
 "name": "grid",
"mean": 0.005961709756098,
"sd": 0.01328923351874 
},
{
 "name": "grid",
"mean": 0.005668871341463,
"sd": 0.01430462434774 
},
{
 "name": "grid",
"mean": 0.005767017886179,
"sd": 0.01333860438615 
},
{
 "name": "grid",
"mean": 0.005909776496674,
"sd": 0.01255543147775 
},
{
 "name": "grid",
"mean": 0.00563793495935,
"sd": 0.01322851776292 
},
{
 "name": "grid",
"mean": 0.005729360487805,
"sd": 0.01251059766745 
},
{
 "name": "grid",
"mean": 0.005804163192905,
"sd": 0.01213075662202 
},
{
 "name": "grid",
"mean": 0.005866498780488,
"sd": 0.01197141562812 
},
{
 "name": "grid",
"mean": 0.00564382369338,
"sd": 0.01573089428618 
},
{
 "name": "grid",
"mean": 0.005757369512195,
"sd": 0.01451697888927 
},
{
 "name": "grid",
"mean": 0.005845682926829,
"sd": 0.01384514679963 
},
{
 "name": "grid",
"mean": 0.005916333658537,
"sd": 0.01350674007963 
},
{
 "name": "grid",
"mean": 0.005612151219512,
"sd": 0.0142512893487 
},
{
 "name": "grid",
"mean":      0.0057166,
"sd": 0.01338957933728 
},
{
 "name": "grid",
"mean": 0.005868525498891,
"sd": 0.01272737970508 
},
{
 "name": "grid",
"mean": 0.005587517073171,
"sd": 0.01315633060699 
},
{
 "name": "grid",
"mean": 0.005762912195122,
"sd": 0.01221947272959 
},
{
 "name": "grid",
"mean": 0.005828685365854,
"sd": 0.01210674600365 
},
{
 "name": "grid",
"mean": 0.005567809756098,
"sd": 0.01232524668988 
},
{
 "name": "grid",
"mean": 0.005657298891353,
"sd": 0.0118661532412 
},
{
 "name": "grid",
"mean": 0.005731873170732,
"sd": 0.01165506754039 
},
{
 "name": "grid",
"mean": 0.005794974484053,
"sd": 0.01160541583402 
},
{
 "name": "grid",
"mean": 0.005555431097561,
"sd": 0.0143608324344 
},
{
 "name": "grid",
"mean": 0.005666182113821,
"sd": 0.01357656571287 
},
{
 "name": "grid",
"mean": 0.005827274501109,
"sd": 0.01299218567386 
},
{
 "name": "grid",
"mean": 0.005537099186992,
"sd": 0.01322362362381 
},
{
 "name": "grid",
"mean": 0.005721661197339,
"sd": 0.01240722093045 
},
{
 "name": "grid",
"mean": 0.00579087195122,
"sd": 0.01232484125671 
},
{
 "name": "grid",
"mean": 0.00561604789357,
"sd": 0.01196838055767 
},
{
 "name": "grid",
"mean": 0.005694059756098,
"sd": 0.01180386633172 
},
{
 "name": "grid",
"mean": 0.005760069793621,
"sd": 0.01178514970087 
},
{
 "name": "grid",
"mean": 0.0055104345898,
"sd": 0.01169212959439 
},
{
 "name": "grid",
"mean": 0.005597247560976,
"sd": 0.01141301898486 
},
{
 "name": "grid",
"mean": 0.005670704690432,
"sd": 0.01131837980278 
},
{
 "name": "grid",
"mean": 0.005733667944251,
"sd": 0.01134343038845 
},
{
 "name": "grid",
"mean": 0.005486681300813,
"sd": 0.01342830005306 
},
{
 "name": "grid",
"mean": 0.005593232195122,
"sd": 0.0129253151402 
},
{
 "name": "grid",
"mean": 0.005680410199557,
"sd": 0.01268960631803 
},
{
 "name": "grid",
"mean": 0.005753058536585,
"sd": 0.01262141163893 
},
{
 "name": "grid",
"mean": 0.005477057560976,
"sd": 0.01251527560145 
},
{
 "name": "grid",
"mean": 0.005574796895787,
"sd": 0.01217136120013 
},
{
 "name": "grid",
"mean": 0.005656246341463,
"sd": 0.01203710214018 
},
{
 "name": "grid",
"mean": 0.005725165103189,
"sd": 0.01203567769674 
},
{
 "name": "grid",
"mean": 0.005469183592018,
"sd": 0.01180757035738 
},
{
 "name": "grid",
"mean": 0.005559434146341,
"sd": 0.01157496512758 
},
{
 "name": "grid",
"mean":      0.0056358,
"sd": 0.01151119499192 
},
{
 "name": "grid",
"mean": 0.005701256445993,
"sd": 0.01155548768402 
},
{
 "name": "grid",
"mean": 0.00546262195122,
"sd": 0.0112500667142 
},
{
 "name": "grid",
"mean": 0.005546434896811,
"sd": 0.01109669715997 
},
{
 "name": "grid",
"mean": 0.00561827456446,
"sd": 0.01108230981509 
},
{
 "name": "grid",
"mean": 0.005680535609756,
"sd": 0.01115778927169 
},
{
 "name": "grid",
"mean": 0.006765275609756,
"sd": 0.01930748178494 
},
{
 "name": "grid",
"mean": 0.006694247154472,
"sd": 0.01689586404087 
},
{
 "name": "grid",
"mean": 0.006673953310105,
"sd": 0.01656715236778 
},
{
 "name": "grid",
"mean": 0.006500622764228,
"sd": 0.0156532616337 
},
{
 "name": "grid",
"mean": 0.006513514634146,
"sd": 0.01521244825041 
},
{
 "name": "grid",
"mean": 0.006306998373984,
"sd": 0.01478390179382 
},
{
 "name": "grid",
"mean": 0.006368296341463,
"sd": 0.01415681272661 
},
{
 "name": "grid",
"mean": 0.00638872899729,
"sd": 0.01419512215125 
},
{
 "name": "grid",
"mean": 0.006176062020906,
"sd": 0.01358560161908 
},
{
 "name": "grid",
"mean": 0.00622307804878,
"sd": 0.01331459901671 
},
{
 "name": "grid",
"mean": 0.006259646070461,
"sd": 0.01329859246184 
},
{
 "name": "grid",
"mean": 0.006288900487805,
"sd": 0.01340953072004 
},
{
 "name": "grid",
"mean": 0.006424995934959,
"sd": 0.01611541139612 
},
{
 "name": "grid",
"mean": 0.006456794512195,
"sd": 0.01568986768393 
},
{
 "name": "grid",
"mean": 0.006231371544715,
"sd": 0.0150297913539 
},
{
 "name": "grid",
"mean": 0.006311576219512,
"sd": 0.01452700142126 
},
{
 "name": "grid",
"mean": 0.006338311111111,
"sd": 0.01457591190413 
},
{
 "name": "grid",
"mean": 0.006166357926829,
"sd": 0.01355588537287 
},
{
 "name": "grid",
"mean": 0.006209228184282,
"sd": 0.01358459279447 
},
{
 "name": "grid",
"mean": 0.006021139634146,
"sd": 0.0128201685553 
},
{
 "name": "grid",
"mean": 0.006080145257453,
"sd": 0.01276090005567 
},
{
 "name": "grid",
"mean": 0.006165971618625,
"sd": 0.01303662626963 
},
{
 "name": "grid",
"mean": 0.006155744715447,
"sd": 0.01554043532965 
},
{
 "name": "grid",
"mean": 0.006254856097561,
"sd": 0.01504358613326 
},
{
 "name": "grid",
"mean": 0.006287893224932,
"sd": 0.01506959709654 
},
{
 "name": "grid",
"mean": 0.006109637804878,
"sd": 0.01396075926472 
},
{
 "name": "grid",
"mean": 0.006158810298103,
"sd": 0.01399677088418 
},
{
 "name": "grid",
"mean": 0.005964419512195,
"sd": 0.01309055194944 
},
{
 "name": "grid",
"mean": 0.006029727371274,
"sd": 0.01307447471224 
},
{
 "name": "grid",
"mean": 0.006081973658537,
"sd": 0.01319190898181 
},
{
 "name": "grid",
"mean": 0.006124720620843,
"sd": 0.01337206381455 
},
{
 "name": "grid",
"mean": 0.005900644444444,
"sd": 0.01233651644924 
},
{
 "name": "grid",
"mean": 0.006019107317073,
"sd": 0.01255918685368 
},
{
 "name": "grid",
"mean": 0.006063530894309,
"sd": 0.01276953460179 
},
{
 "name": "grid",
"mean": 0.005981593031359,
"sd": 0.01467983946595 
},
{
 "name": "grid",
"mean": 0.006052917682927,
"sd": 0.01451553857047 
},
{
 "name": "grid",
"mean": 0.006108392411924,
"sd": 0.01452438855319 
},
{
 "name": "grid",
"mean": 0.006152772195122,
"sd": 0.01461765446419 
},
{
 "name": "grid",
"mean": 0.005907699390244,
"sd": 0.0135286979652 
},
{
 "name": "grid",
"mean": 0.005979309485095,
"sd": 0.01351750870679 
},
{
 "name": "grid",
"mean": 0.00608346962306,
"sd": 0.0137889055653 
},
{
 "name": "grid",
"mean": 0.005850226558266,
"sd": 0.01267689535009 
},
{
 "name": "grid",
"mean": 0.00597785631929,
"sd": 0.01291773677759 
},
{
 "name": "grid",
"mean": 0.006025717479675,
"sd": 0.01312173886304 
},
{
 "name": "grid",
"mean": 0.005804248292683,
"sd": 0.01202890330503 
},
{
 "name": "grid",
"mean": 0.005872243015521,
"sd": 0.01215576774805 
},
{
 "name": "grid",
"mean": 0.005928905284553,
"sd": 0.01235088358843 
},
{
 "name": "grid",
"mean": 0.005976850281426,
"sd": 0.01257656874276 
},
{
 "name": "grid",
"mean": 0.006429840650407,
"sd": 0.01639546443364 
},
{
 "name": "grid",
"mean": 0.00646042804878,
"sd": 0.01546543265802 
},
{
 "name": "grid",
"mean": 0.006236216260163,
"sd": 0.01566496174524 
},
{
 "name": "grid",
"mean": 0.006315209756098,
"sd": 0.01448746771728 
},
{
 "name": "grid",
"mean": 0.006341540921409,
"sd": 0.01437933269929 
},
{
 "name": "grid",
"mean": 0.006169991463415,
"sd": 0.01372808326073 
},
{
 "name": "grid",
"mean": 0.00621245799458,
"sd": 0.01354502359261 
},
{
 "name": "grid",
"mean": 0.006024773170732,
"sd": 0.01322498600236 
},
{
 "name": "grid",
"mean": 0.006083375067751,
"sd": 0.01289905324653 
},
{
 "name": "grid",
"mean": 0.006168614190687,
"sd": 0.01288097207022 
},
{
 "name": "grid",
"mean": 0.006160589430894,
"sd": 0.01588788879091 
},
{
 "name": "grid",
"mean": 0.006258489634146,
"sd": 0.01484379138724 
},
{
 "name": "grid",
"mean": 0.00629112303523,
"sd": 0.0147508973206 
},
{
 "name": "grid",
"mean": 0.006113271341463,
"sd": 0.01395624410262 
},
{
 "name": "grid",
"mean": 0.006162040108401,
"sd": 0.01382115450995 
},
{
 "name": "grid",
"mean": 0.00596805304878,
"sd": 0.01330720826284 
},
{
 "name": "grid",
"mean": 0.006032957181572,
"sd": 0.01306426834911 
},
{
 "name": "grid",
"mean": 0.006084880487805,
"sd": 0.01303587212453 
},
{
 "name": "grid",
"mean": 0.006127363192905,
"sd": 0.01312349862777 
},
{
 "name": "grid",
"mean": 0.005903874254743,
"sd": 0.01251164890381 
},
{
 "name": "grid",
"mean": 0.006021749889135,
"sd": 0.01241930047434 
},
{
 "name": "grid",
"mean": 0.006065953252033,
"sd": 0.01254723107118 
},
{
 "name": "grid",
"mean": 0.006056551219512,
"sd": 0.01434400636602 
},
{
 "name": "grid",
"mean": 0.006111622222222,
"sd": 0.0142218421352 
},
{
 "name": "grid",
"mean": 0.005911332926829,
"sd": 0.01356173360882 
},
{
 "name": "grid",
"mean": 0.005982539295393,
"sd": 0.01336579493355 
},
{
 "name": "grid",
"mean": 0.006039504390244,
"sd": 0.01335917239238 
},
{
 "name": "grid",
"mean": 0.006086112195122,
"sd": 0.01345348842695 
},
{
 "name": "grid",
"mean": 0.005853456368564,
"sd": 0.01269817348983 
},
{
 "name": "grid",
"mean": 0.005923329756098,
"sd": 0.01261803091915 
},
{
 "name": "grid",
"mean": 0.005980498891353,
"sd": 0.01268156464694 
},
{
 "name": "grid",
"mean": 0.006028139837398,
"sd": 0.01282216420156 
},
{
 "name": "grid",
"mean": 0.005874885587583,
"sd": 0.01203364334928 
},
{
 "name": "grid",
"mean": 0.005931327642276,
"sd": 0.01213960858673 
},
{
 "name": "grid",
"mean": 0.00597908630394,
"sd": 0.01230721490685 
},
{
 "name": "grid",
"mean": 0.005854612804878,
"sd": 0.01397915358457 
},
{
 "name": "grid",
"mean": 0.005932121409214,
"sd": 0.01379469734286 
},
{
 "name": "grid",
"mean": 0.006044861197339,
"sd": 0.01386469780906 
},
{
 "name": "grid",
"mean": 0.005803038482385,
"sd": 0.01302404297945 
},
{
 "name": "grid",
"mean": 0.00593924789357,
"sd": 0.01303336101619 
},
{
 "name": "grid",
"mean": 0.005990326422764,
"sd": 0.01317014383446 
},
{
 "name": "grid",
"mean": 0.0058336345898,
"sd": 0.01231535272507 
},
{
 "name": "grid",
"mean": 0.005893514227642,
"sd": 0.01243290546283 
},
{
 "name": "grid",
"mean": 0.005944181613508,
"sd": 0.0126031042094 
},
{
 "name": "grid",
"mean": 0.005728021286031,
"sd": 0.01173149970935 
},
{
 "name": "grid",
"mean": 0.00579670203252,
"sd": 0.01179821094371 
},
{
 "name": "grid",
"mean": 0.005854816510319,
"sd": 0.01194494921587 
},
{
 "name": "grid",
"mean": 0.005904628919861,
"sd": 0.01213429010987 
},
{
 "name": "grid",
"mean": 0.006165434146341,
"sd": 0.01746698313236 
},
{
 "name": "grid",
"mean": 0.006262123170732,
"sd": 0.01542243559235 
},
{
 "name": "grid",
"mean": 0.006294352845528,
"sd": 0.01505460106829 
},
{
 "name": "grid",
"mean": 0.006116904878049,
"sd": 0.01476940816381 
},
{
 "name": "grid",
"mean": 0.006165269918699,
"sd": 0.01430716097776 
},
{
 "name": "grid",
"mean": 0.005971686585366,
"sd": 0.01436265125292 
},
{
 "name": "grid",
"mean": 0.00603618699187,
"sd": 0.01374642326011 
},
{
 "name": "grid",
"mean": 0.006087787317073,
"sd": 0.01344885426107 
},
{
 "name": "grid",
"mean": 0.006130005764967,
"sd": 0.01334398781297 
},
{
 "name": "grid",
"mean": 0.005907104065041,
"sd": 0.01339585402032 
},
{
 "name": "grid",
"mean": 0.006024392461197,
"sd": 0.01277366599945 
},
{
 "name": "grid",
"mean": 0.006068375609756,
"sd": 0.01273745082581 
},
{
 "name": "grid",
"mean": 0.006060184756098,
"sd": 0.01497614420067 
},
{
 "name": "grid",
"mean": 0.00611485203252,
"sd": 0.01456432459968 
},
{
 "name": "grid",
"mean": 0.005914966463415,
"sd": 0.01443261189711 
},
{
 "name": "grid",
"mean": 0.005985769105691,
"sd": 0.01389682457344 
},
{
 "name": "grid",
"mean": 0.006042411219512,
"sd": 0.01364982268366 
},
{
 "name": "grid",
"mean": 0.006088754767184,
"sd": 0.01357499370042 
},
{
 "name": "grid",
"mean": 0.005856686178862,
"sd": 0.01342904950806 
},
{
 "name": "grid",
"mean": 0.005926236585366,
"sd": 0.01306927170632 
},
{
 "name": "grid",
"mean": 0.005983141463415,
"sd": 0.01293050305598 
},
{
 "name": "grid",
"mean": 0.006030562195122,
"sd": 0.01292568803635 
},
{
 "name": "grid",
"mean": 0.005877528159645,
"sd": 0.01242079401318 
},
{
 "name": "grid",
"mean":     0.00593375,
"sd": 0.01235449392904 
},
{
 "name": "grid",
"mean": 0.005981322326454,
"sd": 0.01239591130767 
},
{
 "name": "grid",
"mean": 0.005858246341463,
"sd": 0.01466192726316 
},
{
 "name": "grid",
"mean": 0.005935351219512,
"sd": 0.01417600606205 
},
{
 "name": "grid",
"mean": 0.005997035121951,
"sd": 0.01395508162239 
},
{
 "name": "grid",
"mean": 0.006047503769401,
"sd": 0.01389108552424 
},
{
 "name": "grid",
"mean": 0.005806268292683,
"sd": 0.01359814951799 
},
{
 "name": "grid",
"mean": 0.005880860487805,
"sd": 0.01328857191566 
},
{
 "name": "grid",
"mean": 0.005941890465632,
"sd": 0.01317925018191 
},
{
 "name": "grid",
"mean": 0.005992748780488,
"sd": 0.01318992325362 
},
{
 "name": "grid",
"mean": 0.005764685853659,
"sd": 0.01278524551225 
},
{
 "name": "grid",
"mean": 0.005836277161863,
"sd": 0.01259300492321 
},
{
 "name": "grid",
"mean": 0.005895936585366,
"sd": 0.01255772400819 
},
{
 "name": "grid",
"mean": 0.005946417636023,
"sd": 0.01261754189392 
},
{
 "name": "grid",
"mean": 0.005730663858093,
"sd": 0.01215054196146 
},
{
 "name": "grid",
"mean": 0.005799124390244,
"sd": 0.01203806390965 
},
{
 "name": "grid",
"mean": 0.005857052532833,
"sd": 0.01205236661011 
},
{
 "name": "grid",
"mean": 0.005906705226481,
"sd": 0.01214437611928 
},
{
 "name": "grid",
"mean": 0.005755850406504,
"sd": 0.01389819426416 
},
{
 "name": "grid",
"mean": 0.005900639467849,
"sd": 0.01351483338024 
},
{
 "name": "grid",
"mean": 0.005954935365854,
"sd": 0.01352570316879 
},
{
 "name": "grid",
"mean": 0.00579502616408,
"sd": 0.01285903503128 
},
{
 "name": "grid",
"mean": 0.005858123170732,
"sd": 0.01283858445622 
},
{
 "name": "grid",
"mean": 0.005911512945591,
"sd": 0.0129038682998 
},
{
 "name": "grid",
"mean": 0.00568941286031,
"sd": 0.01233773082452 
},
{
 "name": "grid",
"mean": 0.00576131097561,
"sd": 0.01225602093121 
},
{
 "name": "grid",
"mean": 0.005822147842402,
"sd": 0.01228825045286 
},
{
 "name": "grid",
"mean": 0.005874293728223,
"sd": 0.0123893856652 
},
{
 "name": "grid",
"mean": 0.005664498780488,
"sd": 0.01179351690086 
},
{
 "name": "grid",
"mean": 0.005732782739212,
"sd": 0.01176738656125 
},
{
 "name": "grid",
"mean": 0.00579131184669,
"sd": 0.01183475903651 
},
{
 "name": "grid",
"mean": 0.005842037073171,
"sd": 0.01195913701012 
},
{
 "name": "grid",
"mean": 0.00599405087108,
"sd": 0.01765942495234 
},
{
 "name": "grid",
"mean": 0.006063818292683,
"sd": 0.01631882316416 
},
{
 "name": "grid",
"mean": 0.006118081842818,
"sd": 0.01550916293109 
},
{
 "name": "grid",
"mean": 0.006161492682927,
"sd": 0.0150334087877 
},
{
 "name": "grid",
"mean":      0.0059186,
"sd": 0.01600514516403 
},
{
 "name": "grid",
"mean": 0.005988998915989,
"sd": 0.01503844469007 
},
{
 "name": "grid",
"mean": 0.006091397339246,
"sd": 0.01414164863183 
},
{
 "name": "grid",
"mean": 0.00585991598916,
"sd": 0.01476451685136 
},
{
 "name": "grid",
"mean": 0.005985784035477,
"sd": 0.01363801325759 
},
{
 "name": "grid",
"mean": 0.006032984552846,
"sd": 0.01342298699967 
},
{
 "name": "grid",
"mean": 0.005812968780488,
"sd": 0.01380908939095 
},
{
 "name": "grid",
"mean": 0.005880170731707,
"sd": 0.01327272935477 
},
{
 "name": "grid",
"mean": 0.005936172357724,
"sd": 0.01297438230611 
},
{
 "name": "grid",
"mean": 0.005983558348968,
"sd": 0.01283523729791 
},
{
 "name": "grid",
"mean": 0.005861879878049,
"sd": 0.01606275832641 
},
{
 "name": "grid",
"mean": 0.00593858102981,
"sd": 0.01517169889673 
},
{
 "name": "grid",
"mean": 0.006050146341463,
"sd": 0.01435675233799 
},
{
 "name": "grid",
"mean": 0.005809498102981,
"sd": 0.01479018159282 
},
{
 "name": "grid",
"mean": 0.005944533037694,
"sd": 0.01378181556558 
},
{
 "name": "grid",
"mean": 0.005995171138211,
"sd": 0.01359901153141 
},
{
 "name": "grid",
"mean": 0.005838919733925,
"sd": 0.01333871441316 
},
{
 "name": "grid",
"mean": 0.005898358943089,
"sd": 0.01308638141975 
},
{
 "name": "grid",
"mean": 0.005948653658537,
"sd": 0.01297921847179 
},
{
 "name": "grid",
"mean": 0.005733306430155,
"sd": 0.01304089427268 
},
{
 "name": "grid",
"mean": 0.005801546747967,
"sd": 0.01269132917754 
},
{
 "name": "grid",
"mean": 0.005859288555347,
"sd": 0.01251922086239 
},
{
 "name": "grid",
"mean": 0.005908781533101,
"sd": 0.01246592256842 
},
{
 "name": "grid",
"mean": 0.005759080216802,
"sd": 0.01493947184843 
},
{
 "name": "grid",
"mean": 0.005903282039911,
"sd": 0.01401231011699 
},
{
 "name": "grid",
"mean": 0.005957357723577,
"sd": 0.01384772746049 
},
{
 "name": "grid",
"mean": 0.005797668736142,
"sd": 0.01349594827082 
},
{
 "name": "grid",
"mean": 0.005860545528455,
"sd": 0.01327561929447 
},
{
 "name": "grid",
"mean": 0.005913748968105,
"sd": 0.01318865478887 
},
{
 "name": "grid",
"mean": 0.005692055432373,
"sd": 0.01311858086704 
},
{
 "name": "grid",
"mean": 0.005763733333333,
"sd": 0.01281486531653 
},
{
 "name": "grid",
"mean": 0.005824383864916,
"sd": 0.01267460053165 
},
{
 "name": "grid",
"mean": 0.005876370034843,
"sd": 0.01264258487958 
},
{
 "name": "grid",
"mean": 0.005666921138211,
"sd": 0.01247780718346 
},
{
 "name": "grid",
"mean": 0.005735018761726,
"sd": 0.0122608885212 
},
{
 "name": "grid",
"mean": 0.00579338815331,
"sd": 0.01217818801155 
},
{
 "name": "grid",
"mean": 0.00584397495935,
"sd": 0.01218531651808 
},
{
 "name": "grid",
"mean": 0.005676840487805,
"sd": 0.01413407438299 
},
{
 "name": "grid",
"mean": 0.005756417738359,
"sd": 0.01374129896053 
},
{
 "name": "grid",
"mean": 0.005822732113821,
"sd": 0.01353885752899 
},
{
 "name": "grid",
"mean": 0.005878844277674,
"sd": 0.01346049127995 
},
{
 "name": "grid",
"mean": 0.00565080443459,
"sd": 0.01328882102975 
},
{
 "name": "grid",
"mean": 0.005725919918699,
"sd": 0.01301697416527 
},
{
 "name": "grid",
"mean": 0.005789479174484,
"sd": 0.01289665901033 
},
{
 "name": "grid",
"mean": 0.005843958536585,
"sd": 0.01287600198285 
},
{
 "name": "grid",
"mean": 0.005629107723577,
"sd": 0.0126126429921 
},
{
 "name": "grid",
"mean": 0.005700114071295,
"sd": 0.01242746180664 
},
{
 "name": "grid",
"mean": 0.005760976655052,
"sd": 0.01236586238722 
},
{
 "name": "grid",
"mean": 0.005813724227642,
"sd": 0.0123862114552 
},
{
 "name": "grid",
"mean": 0.005610748968105,
"sd": 0.01206394651339 
},
{
 "name": "grid",
"mean": 0.005677994773519,
"sd": 0.01194178839739 
},
{
 "name": "grid",
"mean": 0.005736274471545,
"sd": 0.01192250512575 
},
{
 "name": "grid",
"mean": 0.005787269207317,
"sd": 0.01197200257536 
},
{
 "name": "grid",
"mean": 0.006833594308943,
"sd": 0.0202745934916 
},
{
 "name": "grid",
"mean": 0.006763243292683,
"sd": 0.01854256567469 
},
{
 "name": "grid",
"mean": 0.006639969918699,
"sd": 0.01930829219678 
},
{
 "name": "grid",
"mean":    0.006618025,
"sd": 0.01749836204861 
},
{
 "name": "grid",
"mean": 0.0066107100271,
"sd": 0.01710526930512 
},
{
 "name": "grid",
"mean": 0.006472806707317,
"sd": 0.01662603508246 
},
{
 "name": "grid",
"mean": 0.006481627100271,
"sd": 0.01620808842839 
},
{
 "name": "grid",
"mean": 0.006327588414634,
"sd": 0.01595380350691 
},
{
 "name": "grid",
"mean": 0.006352544173442,
"sd": 0.01546057820034 
},
{
 "name": "grid",
"mean": 0.00638884345898,
"sd": 0.01508567852381 
},
{
 "name": "grid",
"mean": 0.006564343089431,
"sd": 0.01965027061845 
},
{
 "name": "grid",
"mean": 0.006561304878049,
"sd": 0.01789362068063 
},
{
 "name": "grid",
"mean": 0.006560292140921,
"sd": 0.01749883178405 
},
{
 "name": "grid",
"mean": 0.006416086585366,
"sd": 0.01691978388723 
},
{
 "name": "grid",
"mean": 0.006431209214092,
"sd": 0.01652434238587 
},
{
 "name": "grid",
"mean": 0.006270868292683,
"sd": 0.01613204723877 
},
{
 "name": "grid",
"mean": 0.006302126287263,
"sd": 0.01568802486132 
},
{
 "name": "grid",
"mean": 0.006327132682927,
"sd": 0.01545710285882 
},
{
 "name": "grid",
"mean": 0.006347592461197,
"sd": 0.01535433423929 
},
{
 "name": "grid",
"mean": 0.006173043360434,
"sd": 0.01501298811356 
},
{
 "name": "grid",
"mean": 0.006241979157428,
"sd": 0.01460672871156 
},
{
 "name": "grid",
"mean": 0.006267830081301,
"sd": 0.01458019414934 
},
{
 "name": "grid",
"mean": 0.006359366463415,
"sd": 0.01734331054777 
},
{
 "name": "grid",
"mean": 0.006380791327913,
"sd": 0.01694359719588 
},
{
 "name": "grid",
"mean": 0.006214148170732,
"sd": 0.01645051183487 
},
{
 "name": "grid",
"mean": 0.006251708401084,
"sd": 0.01602743284738 
},
{
 "name": "grid",
"mean": 0.006281756585366,
"sd": 0.0158025186352 
},
{
 "name": "grid",
"mean": 0.006306341463415,
"sd": 0.01569703230555 
},
{
 "name": "grid",
"mean": 0.006122625474255,
"sd": 0.01526064142503 
},
{
 "name": "grid",
"mean": 0.00616558195122,
"sd": 0.01500428127764 
},
{
 "name": "grid",
"mean": 0.006200728159645,
"sd": 0.01489331024066 
},
{
 "name": "grid",
"mean": 0.006230016666667,
"sd": 0.01487041356883 
},
{
 "name": "grid",
"mean": 0.006095114855876,
"sd": 0.01419102941897 
},
{
 "name": "grid",
"mean": 0.006133204471545,
"sd": 0.01415571298066 
},
{
 "name": "grid",
"mean": 0.006165434146341,
"sd": 0.01418814671275 
},
{
 "name": "grid",
"mean": 0.00615742804878,
"sd": 0.01690127275925 
},
{
 "name": "grid",
"mean": 0.006201290514905,
"sd": 0.01647188262381 
},
{
 "name": "grid",
"mean": 0.006265090465632,
"sd": 0.01610904796482 
},
{
 "name": "grid",
"mean": 0.006072207588076,
"sd": 0.01562255981509 
},
{
 "name": "grid",
"mean": 0.006159477161863,
"sd": 0.0152554323458 
},
{
 "name": "grid",
"mean": 0.006192203252033,
"sd": 0.01522321400146 
},
{
 "name": "grid",
"mean": 0.006053863858093,
"sd": 0.01449537047144 
},
{
 "name": "grid",
"mean": 0.006095391056911,
"sd": 0.01446248604551 
},
{
 "name": "grid",
"mean": 0.00613052945591,
"sd": 0.0144918554229 
},
{
 "name": "grid",
"mean": 0.005948250554324,
"sd": 0.01384427944944 
},
{
 "name": "grid",
"mean": 0.005998578861789,
"sd": 0.01378683989523 
},
{
 "name": "grid",
"mean": 0.00604116435272,
"sd": 0.01380936853104 
},
{
 "name": "grid",
"mean": 0.006077666202091,
"sd": 0.01388037979208 
},
{
 "name": "grid",
"mean": 0.006569187804878,
"sd": 0.01985952841712 
},
{
 "name": "grid",
"mean": 0.006564938414634,
"sd": 0.01768384821517 
},
{
 "name": "grid",
"mean": 0.00656352195122,
"sd": 0.01719088703857 
},
{
 "name": "grid",
"mean": 0.006419720121951,
"sd": 0.01687190577946 
},
{
 "name": "grid",
"mean": 0.00643443902439,
"sd": 0.01633982485661 
},
{
 "name": "grid",
"mean": 0.006274501829268,
"sd": 0.01626254460899 
},
{
 "name": "grid",
"mean": 0.006305356097561,
"sd": 0.01564188642081 
},
{
 "name": "grid",
"mean": 0.006330039512195,
"sd": 0.01529296424954 
},
{
 "name": "grid",
"mean": 0.006350235033259,
"sd": 0.01511226776981 
},
{
 "name": "grid",
"mean": 0.006176273170732,
"sd": 0.01511829357246 
},
{
 "name": "grid",
"mean": 0.00624462172949,
"sd": 0.01445936738387 
},
{
 "name": "grid",
"mean": 0.006270252439024,
"sd": 0.01436283462389 
},
{
 "name": "grid",
"mean":       0.006363,
"sd": 0.01715658028367 
},
{
 "name": "grid",
"mean": 0.006384021138211,
"sd": 0.0166496151627 
},
{
 "name": "grid",
"mean": 0.006217781707317,
"sd": 0.01643236005898 
},
{
 "name": "grid",
"mean": 0.006254938211382,
"sd": 0.01586257559479 
},
{
 "name": "grid",
"mean": 0.006284663414634,
"sd": 0.01554299763117 
},
{
 "name": "grid",
"mean": 0.006308984035477,
"sd": 0.01537758722977 
},
{
 "name": "grid",
"mean": 0.006125855284553,
"sd": 0.01523969747794 
},
{
 "name": "grid",
"mean": 0.006168488780488,
"sd": 0.01485714047779 
},
{
 "name": "grid",
"mean": 0.006203370731707,
"sd": 0.0146620523804 
},
{
 "name": "grid",
"mean": 0.00623243902439,
"sd": 0.01458403329411 
},
{
 "name": "grid",
"mean": 0.006097757427938,
"sd": 0.01405852518315 
},
{
 "name": "grid",
"mean": 0.006135626829268,
"sd": 0.0139480086252 
},
{
 "name": "grid",
"mean": 0.006167670168856,
"sd": 0.01392968091151 
},
{
 "name": "grid",
"mean": 0.006161061585366,
"sd": 0.01674012684524 
},
{
 "name": "grid",
"mean": 0.006204520325203,
"sd": 0.01619425305676 
},
{
 "name": "grid",
"mean": 0.006239287317073,
"sd": 0.0158831816907 
},
{
 "name": "grid",
"mean": 0.006267733037694,
"sd": 0.01571696856055 
},
{
 "name": "grid",
"mean": 0.006075437398374,
"sd": 0.01547946354893 
},
{
 "name": "grid",
"mean": 0.006123112682927,
"sd": 0.01512544033295 
},
{
 "name": "grid",
"mean": 0.006162119733925,
"sd": 0.01494461825311 
},
{
 "name": "grid",
"mean": 0.006194625609756,
"sd": 0.0148716825667 
},
{
 "name": "grid",
"mean": 0.00600693804878,
"sd": 0.01450230839185 
},
{
 "name": "grid",
"mean": 0.006056506430155,
"sd": 0.01427658530785 
},
{
 "name": "grid",
"mean": 0.006097813414634,
"sd": 0.01418386786633 
},
{
 "name": "grid",
"mean": 0.006132765478424,
"sd": 0.01417460497155 
},
{
 "name": "grid",
"mean": 0.005950893126386,
"sd": 0.01372810695966 
},
{
 "name": "grid",
"mean": 0.006001001219512,
"sd": 0.01359019744271 
},
{
 "name": "grid",
"mean": 0.006043400375235,
"sd": 0.013557942806 
},
{
 "name": "grid",
"mean": 0.006079742508711,
"sd": 0.01359243207797 
},
{
 "name": "grid",
"mean": 0.006025019512195,
"sd": 0.01583221520108 
},
{
 "name": "grid",
"mean": 0.006120868736142,
"sd": 0.01530264059953 
},
{
 "name": "grid",
"mean": 0.006156812195122,
"sd": 0.01522201577176 
},
{
 "name": "grid",
"mean": 0.006015255432373,
"sd": 0.01457611078583 
},
{
 "name": "grid",
"mean":        0.00606,
"sd": 0.01448748346817 
},
{
 "name": "grid",
"mean": 0.006097860787992,
"sd": 0.01447641354961 
},
{
 "name": "grid",
"mean": 0.005909642128603,
"sd": 0.01396123065084 
},
{
 "name": "grid",
"mean": 0.005963187804878,
"sd": 0.01384054854125 
},
{
 "name": "grid",
"mean": 0.006008495684803,
"sd": 0.01381662190713 
},
{
 "name": "grid",
"mean": 0.006047331010453,
"sd": 0.01385353163728 
},
{
 "name": "grid",
"mean": 0.005866375609756,
"sd": 0.01329400557404 
},
{
 "name": "grid",
"mean": 0.005919130581614,
"sd": 0.01323671174369 
},
{
 "name": "grid",
"mean": 0.00596434912892,
"sd": 0.01325750097816 
},
{
 "name": "grid",
"mean": 0.006003538536585,
"sd": 0.01332751953136 
},
{
 "name": "grid",
"mean": 0.006366633536585,
"sd": 0.01764626847683 
},
{
 "name": "grid",
"mean": 0.006387250948509,
"sd": 0.01690827343781 
},
{
 "name": "grid",
"mean": 0.006221415243902,
"sd": 0.01711462218221 
},
{
 "name": "grid",
"mean": 0.00625816802168,
"sd": 0.01627635806239 
},
{
 "name": "grid",
"mean": 0.006287570243902,
"sd": 0.01576325394502 
},
{
 "name": "grid",
"mean": 0.006311626607539,
"sd": 0.01545850004896 
},
{
 "name": "grid",
"mean": 0.006129085094851,
"sd": 0.01581661810208 
},
{
 "name": "grid",
"mean": 0.006171395609756,
"sd": 0.01521088456076 
},
{
 "name": "grid",
"mean": 0.006206013303769,
"sd": 0.01485134805444 
},
{
 "name": "grid",
"mean": 0.006234861382114,
"sd": 0.0146525256157 
},
{
 "name": "grid",
"mean":      0.0061004,
"sd": 0.0143638648569 
},
{
 "name": "grid",
"mean": 0.006138049186992,
"sd": 0.0141119570438 
},
{
 "name": "grid",
"mean": 0.00616990619137,
"sd": 0.01398792816726 
},
{
 "name": "grid",
"mean": 0.006164695121951,
"sd": 0.01727123014032 
},
{
 "name": "grid",
"mean": 0.006207750135501,
"sd": 0.0164845556025 
},
{
 "name": "grid",
"mean": 0.006242194146341,
"sd": 0.01600260333789 
},
{
 "name": "grid",
"mean": 0.006270375609756,
"sd": 0.01571516605323 
},
{
 "name": "grid",
"mean": 0.006078667208672,
"sd": 0.0159285603493 
},
{
 "name": "grid",
"mean": 0.006126019512195,
"sd": 0.01537295627416 
},
{
 "name": "grid",
"mean": 0.006164762305987,
"sd": 0.01504581943107 
},
{
 "name": "grid",
"mean": 0.00619704796748,
"sd": 0.01486691940431 
},
{
 "name": "grid",
"mean": 0.006009844878049,
"sd": 0.01488646279023 
},
{
 "name": "grid",
"mean": 0.006059149002217,
"sd": 0.01448957142041 
},
{
 "name": "grid",
"mean": 0.006100235772358,
"sd": 0.0142701914865 
},
{
 "name": "grid",
"mean": 0.006135001500938,
"sd": 0.01416751917462 
},
{
 "name": "grid",
"mean": 0.005953535698448,
"sd": 0.01405985195032 
},
{
 "name": "grid",
"mean": 0.006003423577236,
"sd": 0.01377489132974 
},
{
 "name": "grid",
"mean": 0.006045636397749,
"sd": 0.01363196933672 
},
{
 "name": "grid",
"mean": 0.006081818815331,
"sd": 0.01358351779604 
},
{
 "name": "grid",
"mean": 0.006028249322493,
"sd": 0.0161540241204 
},
{
 "name": "grid",
"mean": 0.006080643414634,
"sd": 0.01562898670823 
},
{
 "name": "grid",
"mean": 0.006123511308204,
"sd": 0.01531842633995 
},
{
 "name": "grid",
"mean": 0.006159234552846,
"sd": 0.01514674832366 
},
{
 "name": "grid",
"mean": 0.005964468780488,
"sd": 0.0150631288966 
},
{
 "name": "grid",
"mean": 0.006017898004435,
"sd": 0.01469823479635 
},
{
 "name": "grid",
"mean": 0.006062422357724,
"sd": 0.01449825179348 
},
{
 "name": "grid",
"mean": 0.006100096810507,
"sd": 0.01440620602624 
},
{
 "name": "grid",
"mean": 0.005912284700665,
"sd": 0.01419798528129 
},
{
 "name": "grid",
"mean": 0.005965610162602,
"sd": 0.01394527945047 
},
{
 "name": "grid",
"mean": 0.006010731707317,
"sd": 0.01382334493308 
},
{
 "name": "grid",
"mean": 0.00586879796748,
"sd": 0.01349957504642 
},
{
 "name": "grid",
"mean": 0.005921366604128,
"sd": 0.01332703880848 
},
{
 "name": "grid",
"mean": 0.006005476422764,
"sd": 0.0132657196008 
},
{
 "name": "grid",
"mean": 0.005976647006652,
"sd": 0.01498639023152 
},
{
 "name": "grid",
"mean": 0.006024608943089,
"sd": 0.01479290882901 
},
{
 "name": "grid",
"mean": 0.006065192120075,
"sd": 0.01470111056805 
},
{
 "name": "grid",
"mean": 0.005871033702882,
"sd": 0.01442045405291 
},
{
 "name": "grid",
"mean": 0.005927796747967,
"sd": 0.0141867513303 
},
{
 "name": "grid",
"mean": 0.005975827016886,
"sd": 0.01407490140771 
},
{
 "name": "grid",
"mean": 0.006016995818815,
"sd": 0.01404330458577 
},
{
 "name": "grid",
"mean": 0.005830984552846,
"sd": 0.01368188103258 
},
{
 "name": "grid",
"mean": 0.005886461913696,
"sd": 0.01353004259176 
},
{
 "name": "grid",
"mean": 0.005975225691057,
"sd": 0.0134878409223 
},
{
 "name": "grid",
"mean": 0.005797096810507,
"sd": 0.01307670679601 
},
{
 "name": "grid",
"mean": 0.005851032055749,
"sd": 0.01298391254179 
},
{
 "name": "grid",
"mean": 0.005897775934959,
"sd": 0.01297216806927 
},
{
 "name": "grid",
"mean": 0.005938676829268,
"sd": 0.01301440148975 
},
{
 "name": "grid",
"mean": 0.006168328658537,
"sd": 0.01843484885151 
},
{
 "name": "grid",
"mean": 0.006210979945799,
"sd": 0.01731424676991 
},
{
 "name": "grid",
"mean": 0.006273018181818,
"sd": 0.01610377149537 
},
{
 "name": "grid",
"mean": 0.00608189701897,
"sd": 0.0169227688128 
},
{
 "name": "grid",
"mean": 0.006167404878049,
"sd": 0.01555099414574 
},
{
 "name": "grid",
"mean": 0.006199470325203,
"sd": 0.01520925031562 
},
{
 "name": "grid",
"mean": 0.006061791574279,
"sd": 0.01511608880595 
},
{
 "name": "grid",
"mean": 0.006102658130081,
"sd": 0.01471503567282 
},
{
 "name": "grid",
"mean": 0.006137237523452,
"sd": 0.01447105365856 
},
{
 "name": "grid",
"mean": 0.00595617827051,
"sd": 0.0148094436756 
},
{
 "name": "grid",
"mean": 0.006005845934959,
"sd": 0.01432618051469 
},
{
 "name": "grid",
"mean": 0.006047872420263,
"sd": 0.01402629615495 
},
{
 "name": "grid",
"mean": 0.006083895121951,
"sd": 0.01385417557761 
},
{
 "name": "grid",
"mean": 0.006031479132791,
"sd": 0.01702356539752 
},
{
 "name": "grid",
"mean": 0.006126153880266,
"sd": 0.01573401508495 
},
{
 "name": "grid",
"mean": 0.006161656910569,
"sd": 0.01541349596243 
},
{
 "name": "grid",
"mean": 0.006020540576497,
"sd": 0.01523269101517 
},
{
 "name": "grid",
"mean": 0.006064844715447,
"sd": 0.0148643562011 
},
{
 "name": "grid",
"mean": 0.006102332833021,
"sd": 0.01464256179238 
},
{
 "name": "grid",
"mean": 0.005914927272727,
"sd": 0.01485500857477 
},
{
 "name": "grid",
"mean": 0.005968032520325,
"sd": 0.01441591156882 
},
{
 "name": "grid",
"mean": 0.006012967729831,
"sd": 0.01414794367798 
},
{
 "name": "grid",
"mean": 0.006051483623693,
"sd": 0.0139983481957 
},
{
 "name": "grid",
"mean": 0.005871220325203,
"sd": 0.01407778821565 
},
{
 "name": "grid",
"mean": 0.005923602626642,
"sd": 0.01374419913936 
},
{
 "name": "grid",
"mean": 0.00596850174216,
"sd": 0.01355035344929 
},
{
 "name": "grid",
"mean": 0.006007414308943,
"sd": 0.01345421694927 
},
{
 "name": "grid",
"mean": 0.005979289578714,
"sd": 0.01542844678506 
},
{
 "name": "grid",
"mean": 0.006027031300813,
"sd": 0.01508097484396 
},
{
 "name": "grid",
"mean": 0.006067428142589,
"sd": 0.01487150110294 
},
{
 "name": "grid",
"mean": 0.005873676274945,
"sd": 0.01498286679489 
},
{
 "name": "grid",
"mean": 0.005930219105691,
"sd": 0.0145762639634 
},
{
 "name": "grid",
"mean": 0.0059780630394,
"sd": 0.01433022484683 
},
{
 "name": "grid",
"mean": 0.006019072125436,
"sd": 0.01419471983348 
},
{
 "name": "grid",
"mean": 0.005833406910569,
"sd": 0.0141772823071 
},
{
 "name": "grid",
"mean": 0.00588869793621,
"sd": 0.01387545128859 
},
{
 "name": "grid",
"mean": 0.005977163577236,
"sd": 0.01362300433097 
},
{
 "name": "grid",
"mean": 0.005799332833021,
"sd": 0.01351591498786 
},
{
 "name": "grid",
"mean": 0.005853108362369,
"sd": 0.01329191428602 
},
{
 "name": "grid",
"mean": 0.005899713821138,
"sd": 0.01317589462933 
},
{
 "name": "grid",
"mean": 0.005940493597561,
"sd": 0.01313553981627 
},
{
 "name": "grid",
"mean": 0.005832425277162,
"sd": 0.01519094055325 
},
{
 "name": "grid",
"mean": 0.005892405691057,
"sd": 0.01480494317624 
},
{
 "name": "grid",
"mean": 0.005943158348968,
"sd": 0.01457086426272 
},
{
 "name": "grid",
"mean": 0.005986660627178,
"sd": 0.0144411612332 
},
{
 "name": "grid",
"mean": 0.005795593495935,
"sd": 0.01434839547601 
},
{
 "name": "grid",
"mean": 0.005853793245779,
"sd": 0.0140682974911 
},
{
 "name": "grid",
"mean": 0.005903678745645,
"sd": 0.01391061082684 
},
{
 "name": "grid",
"mean": 0.005946912845528,
"sd": 0.01383767612273 
},
{
 "name": "grid",
"mean": 0.005764428142589,
"sd": 0.01365660676849 
},
{
 "name": "grid",
"mean": 0.005820696864111,
"sd": 0.01345480142623 
},
{
 "name": "grid",
"mean": 0.005869463089431,
"sd": 0.01335376618603 
},
{
 "name": "grid",
"mean": 0.005912133536585,
"sd": 0.01332297651321 
},
{
 "name": "grid",
"mean": 0.005737714982578,
"sd": 0.01308154812298 
},
{
 "name": "grid",
"mean": 0.005792013333333,
"sd": 0.01293840894882 
},
{
 "name": "grid",
"mean": 0.005839524390244,
"sd": 0.01287977498919 
},
{
 "name": "grid",
"mean": 0.005881445911047,
"sd": 0.01288042998203 
},
{
 "name": "grid",
"mean": 0.006972941463415,
"sd": 0.02416998659772 
},
{
 "name": "grid",
"mean": 0.006912834146341,
"sd": 0.02230958778257 
},
{
 "name": "grid",
"mean": 0.006867753658537,
"sd": 0.02107383659084 
},
{
 "name": "grid",
"mean": 0.006832691056911,
"sd": 0.02022550009023 
},
{
 "name": "grid",
"mean": 0.006746870383275,
"sd": 0.0214640060436 
},
{
 "name": "grid",
"mean": 0.006722535365854,
"sd": 0.02019178359174 
},
{
 "name": "grid",
"mean": 0.006703608130081,
"sd": 0.01933762059628 
},
{
 "name": "grid",
"mean": 0.006688466341463,
"sd": 0.01875174708647 
},
{
 "name": "grid",
"mean": 0.006577317073171,
"sd": 0.01947244453501 
},
{
 "name": "grid",
"mean": 0.006574525203252,
"sd": 0.01857503006826 
},
{
 "name": "grid",
"mean": 0.006570464301552,
"sd": 0.01756686479636 
},
{
 "name": "grid",
"mean": 0.006445442276423,
"sd": 0.01795370070577 
},
{
 "name": "grid",
"mean": 0.006456117073171,
"sd": 0.01730633439902 
},
{
 "name": "grid",
"mean": 0.006464850997783,
"sd": 0.01687825649916 
},
{
 "name": "grid",
"mean": 0.006472129268293,
"sd": 0.01659698426888 
},
{
 "name": "grid",
"mean": 0.00668204738676,
"sd": 0.02176545441266 
},
{
 "name": "grid",
"mean": 0.006665815243902,
"sd": 0.02051664523756 
},
{
 "name": "grid",
"mean": 0.006653190243902,
"sd": 0.01967126055866 
},
{
 "name": "grid",
"mean": 0.006643090243902,
"sd": 0.01908529390009 
},
{
 "name": "grid",
"mean": 0.00652059695122,
"sd": 0.01970446043175 
},
{
 "name": "grid",
"mean": 0.006524107317073,
"sd": 0.01883559260164 
},
{
 "name": "grid",
"mean": 0.006529213303769,
"sd": 0.01784812032918 
},
{
 "name": "grid",
"mean": 0.006395024390244,
"sd": 0.0181332859854 
},
{
 "name": "grid",
"mean":      0.0064236,
"sd": 0.01710699055837 
},
{
 "name": "grid",
"mean": 0.006434315853659,
"sd": 0.01683553226154 
},
{
 "name": "grid",
"mean": 0.006294566341463,
"sd": 0.01690215723577 
},
{
 "name": "grid",
"mean": 0.006317986696231,
"sd": 0.01645914979475 
},
{
 "name": "grid",
"mean": 0.006337503658537,
"sd": 0.01617346198158 
},
{
 "name": "grid",
"mean": 0.006354018011257,
"sd": 0.01599430699115 
},
{
 "name": "grid",
"mean": 0.006463876829268,
"sd": 0.02005025474607 
},
{
 "name": "grid",
"mean": 0.006473689430894,
"sd": 0.01918872557365 
},
{
 "name": "grid",
"mean": 0.006487962305987,
"sd": 0.01819283979092 
},
{
 "name": "grid",
"mean": 0.006344606504065,
"sd": 0.01841131679406 
},
{
 "name": "grid",
"mean": 0.006382349002217,
"sd": 0.0174036223051 
},
{
 "name": "grid",
"mean": 0.006396502439024,
"sd": 0.01713126710981 
},
{
 "name": "grid",
"mean": 0.006276735698448,
"sd": 0.01670190044102 
},
{
 "name": "grid",
"mean": 0.006299690243902,
"sd": 0.01642523322598 
},
{
 "name": "grid",
"mean": 0.006319113320826,
"sd": 0.01624933664604 
},
{
 "name": "grid",
"mean": 0.006171122394678,
"sd": 0.01609911943766 
},
{
 "name": "grid",
"mean": 0.00620287804878,
"sd": 0.01579849530385 
},
{
 "name": "grid",
"mean": 0.006229748217636,
"sd": 0.01561347296532 
},
{
 "name": "grid",
"mean": 0.006252779790941,
"sd": 0.01550736690675 
},
{
 "name": "grid",
"mean": 0.006294188617886,
"sd": 0.01878342209028 
},
{
 "name": "grid",
"mean": 0.006319988780488,
"sd": 0.0181778519356 
},
{
 "name": "grid",
"mean": 0.006341098004435,
"sd": 0.0177647508447 
},
{
 "name": "grid",
"mean": 0.00635868902439,
"sd": 0.01748128672599 
},
{
 "name": "grid",
"mean": 0.006203814146341,
"sd": 0.01743523975823 
},
{
 "name": "grid",
"mean": 0.006235484700665,
"sd": 0.01701372106799 
},
{
 "name": "grid",
"mean": 0.006261876829268,
"sd": 0.01673516080401 
},
{
 "name": "grid",
"mean": 0.006284208630394,
"sd": 0.01655378307524 
},
{
 "name": "grid",
"mean": 0.006129871396896,
"sd": 0.01635566555989 
},
{
 "name": "grid",
"mean": 0.006165064634146,
"sd": 0.01606337518312 
},
{
 "name": "grid",
"mean": 0.006194843527205,
"sd": 0.01588085121581 
},
{
 "name": "grid",
"mean": 0.006220368292683,
"sd": 0.01577338519811 
},
{
 "name": "grid",
"mean": 0.006068252439024,
"sd": 0.01547561407535 
},
{
 "name": "grid",
"mean": 0.006105478424015,
"sd": 0.01527593341734 
},
{
 "name": "grid",
"mean": 0.00613738641115,
"sd": 0.01516301497189 
},
{
 "name": "grid",
"mean":     0.00616504,
"sd": 0.01510965644637 
},
{
 "name": "grid",
"mean":      0.0066862,
"sd": 0.021709059355 
},
{
 "name": "grid",
"mean": 0.006669448780488,
"sd": 0.0203223700811 
},
{
 "name": "grid",
"mean": 0.006656420054201,
"sd": 0.01938824281239 
},
{
 "name": "grid",
"mean": 0.006645997073171,
"sd": 0.01874614265163 
},
{
 "name": "grid",
"mean": 0.006524230487805,
"sd": 0.01965138789045 
},
{
 "name": "grid",
"mean": 0.006527337127371,
"sd": 0.01866396307684 
},
{
 "name": "grid",
"mean": 0.006531855875831,
"sd": 0.01754536111725 
},
{
 "name": "grid",
"mean": 0.006398254200542,
"sd": 0.01808310071308 
},
{
 "name": "grid",
"mean": 0.006426242572062,
"sd": 0.01688268780993 
},
{
 "name": "grid",
"mean": 0.006436738211382,
"sd": 0.01656311979486 
},
{
 "name": "grid",
"mean": 0.006297473170732,
"sd": 0.01685453349693 
},
{
 "name": "grid",
"mean": 0.006320629268293,
"sd": 0.01632088716839 
},
{
 "name": "grid",
"mean": 0.00633992601626,
"sd": 0.01597124198306 
},
{
 "name": "grid",
"mean": 0.006356254033771,
"sd": 0.01574755234991 
},
{
 "name": "grid",
"mean": 0.006467510365854,
"sd": 0.01987711520644 
},
{
 "name": "grid",
"mean": 0.006476919241192,
"sd": 0.01891981427737 
},
{
 "name": "grid",
"mean": 0.006490604878049,
"sd": 0.01782447827288 
},
{
 "name": "grid",
"mean": 0.006347836314363,
"sd": 0.01825780027642 
},
{
 "name": "grid",
"mean": 0.006384991574279,
"sd": 0.01710878136089 
},
{
 "name": "grid",
"mean": 0.006398924796748,
"sd": 0.01679993970262 
},
{
 "name": "grid",
"mean": 0.00627937827051,
"sd": 0.01648846666406 
},
{
 "name": "grid",
"mean": 0.006302112601626,
"sd": 0.01615994600716 
},
{
 "name": "grid",
"mean": 0.00632134934334,
"sd": 0.01594934292282 
},
{
 "name": "grid",
"mean": 0.006173764966741,
"sd": 0.01597464946507 
},
{
 "name": "grid",
"mean": 0.006205300406504,
"sd": 0.01560595814087 
},
{
 "name": "grid",
"mean": 0.00623198424015,
"sd": 0.0153731824186 
},
{
 "name": "grid",
"mean": 0.006254856097561,
"sd": 0.01523419069289 
},
{
 "name": "grid",
"mean": 0.006297418428184,
"sd": 0.01853040324264 
},
{
 "name": "grid",
"mean": 0.006343740576497,
"sd": 0.017402844925 
},
{
 "name": "grid",
"mean": 0.006361111382114,
"sd": 0.01709411937583 
},
{
 "name": "grid",
"mean": 0.006238127272727,
"sd": 0.01672815195298 
},
{
 "name": "grid",
"mean": 0.006264299186992,
"sd": 0.01640966318903 
},
{
 "name": "grid",
"mean": 0.006286444652908,
"sd": 0.01620312876215 
},
{
 "name": "grid",
"mean": 0.006132513968958,
"sd": 0.0161543766009 
},
{
 "name": "grid",
"mean": 0.00616748699187,
"sd": 0.01580637113057 
},
{
 "name": "grid",
"mean": 0.006197079549719,
"sd": 0.01558616964885 
},
{
 "name": "grid",
"mean": 0.006222444599303,
"sd": 0.01545401369922 
},
{
 "name": "grid",
"mean": 0.006070674796748,
"sd": 0.01529385119109 
},
{
 "name": "grid",
"mean": 0.006107714446529,
"sd": 0.01504310447209 
},
{
 "name": "grid",
"mean": 0.00613946271777,
"sd": 0.01489471545084 
},
{
 "name": "grid",
"mean": 0.006166977886179,
"sd": 0.01481734850251 
},
{
 "name": "grid",
"mean": 0.006161344878049,
"sd": 0.01751401316221 
},
{
 "name": "grid",
"mean": 0.006196876274945,
"sd": 0.01703690002476 
},
{
 "name": "grid",
"mean": 0.006226485772358,
"sd": 0.01671765963139 
},
{
 "name": "grid",
"mean": 0.006251539962477,
"sd": 0.01650651177725 
},
{
 "name": "grid",
"mean": 0.006091262971175,
"sd": 0.01640736727987 
},
{
 "name": "grid",
"mean": 0.006129673577236,
"sd": 0.01606881183195 
},
{
 "name": "grid",
"mean": 0.006162174859287,
"sd": 0.01585201369942 
},
{
 "name": "grid",
"mean": 0.006190033101045,
"sd": 0.01571920037514 
},
{
 "name": "grid",
"mean": 0.006032861382114,
"sd": 0.01550578851359 
},
{
 "name": "grid",
"mean": 0.006072809756098,
"sd": 0.01526717887484 
},
{
 "name": "grid",
"mean": 0.006107051219512,
"sd": 0.01512511434964 
},
{
 "name": "grid",
"mean": 0.006136727154472,
"sd": 0.01505000310678 
},
{
 "name": "grid",
"mean": 0.005983444652908,
"sd": 0.01476029025668 
},
{
 "name": "grid",
"mean": 0.006024069337979,
"sd": 0.01459498337705 
},
{
 "name": "grid",
"mean": 0.006059277398374,
"sd": 0.01450811092451 
},
{
 "name": "grid",
"mean": 0.00609008445122,
"sd": 0.01447586715274 
},
{
 "name": "grid",
"mean": 0.006471143902439,
"sd": 0.02028968753656 
},
{
 "name": "grid",
"mean": 0.006480149051491,
"sd": 0.01913811333155 
},
{
 "name": "grid",
"mean": 0.006493247450111,
"sd": 0.01780073876809 
},
{
 "name": "grid",
"mean": 0.006351066124661,
"sd": 0.01860843309437 
},
{
 "name": "grid",
"mean": 0.006387634146341,
"sd": 0.01717429471385 
},
{
 "name": "grid",
"mean": 0.006401347154472,
"sd": 0.01677597352213 
},
{
 "name": "grid",
"mean": 0.006282020842572,
"sd": 0.01664954223002 
},
{
 "name": "grid",
"mean": 0.00630453495935,
"sd": 0.01621533458674 
},
{
 "name": "grid",
"mean": 0.006323585365854,
"sd": 0.01592531817155 
},
{
 "name": "grid",
"mean": 0.006176407538803,
"sd": 0.01623634421115 
},
{
 "name": "grid",
"mean": 0.006207722764228,
"sd": 0.01574601719491 
},
{
 "name": "grid",
"mean": 0.006234220262664,
"sd": 0.01542019968334 
},
{
 "name": "grid",
"mean": 0.006256932404181,
"sd": 0.0152102328402 
},
{
 "name": "grid",
"mean": 0.006300648238482,
"sd": 0.01877473221379 
},
{
 "name": "grid",
"mean": 0.006346383148559,
"sd": 0.01739405988663 
},
{
 "name": "grid",
"mean": 0.006363533739837,
"sd": 0.01700764831637 
},
{
 "name": "grid",
"mean": 0.006240769844789,
"sd": 0.01681121946475 
},
{
 "name": "grid",
"mean": 0.006266721544715,
"sd": 0.016398967565 
},
{
 "name": "grid",
"mean": 0.006288680675422,
"sd": 0.01612292312907 
},
{
 "name": "grid",
"mean": 0.00613515654102,
"sd": 0.01633528817522 
},
{
 "name": "grid",
"mean": 0.006169909349593,
"sd": 0.01587729091313 
},
{
 "name": "grid",
"mean": 0.006199315572233,
"sd": 0.01557400251281 
},
{
 "name": "grid",
"mean": 0.006224520905923,
"sd": 0.01537926778239 
},
{
 "name": "grid",
"mean": 0.006073097154472,
"sd": 0.01545143326298 
},
{
 "name": "grid",
"mean": 0.006109950469043,
"sd": 0.01510395522728 
},
{
 "name": "grid",
"mean": 0.00614153902439,
"sd": 0.01488141636352 
},
{
 "name": "grid",
"mean": 0.006168915772358,
"sd": 0.01474740698853 
},
{
 "name": "grid",
"mean": 0.006199518847007,
"sd": 0.01704377608327 
},
{
 "name": "grid",
"mean": 0.006228908130081,
"sd": 0.01664286949977 
},
{
 "name": "grid",
"mean": 0.006253775984991,
"sd": 0.01637208749497 
},
{
 "name": "grid",
"mean": 0.006093905543237,
"sd": 0.0165084150073 
},
{
 "name": "grid",
"mean": 0.006132095934959,
"sd": 0.01607201239821 
},
{
 "name": "grid",
"mean": 0.006164410881801,
"sd": 0.01578227669319 
},
{
 "name": "grid",
"mean": 0.006192109407666,
"sd": 0.01559529799849 
},
{
 "name": "grid",
"mean": 0.006035283739837,
"sd": 0.01559263459035 
},
{
 "name": "grid",
"mean": 0.006075045778612,
"sd": 0.01526742517562 
},
{
 "name": "grid",
"mean": 0.00613866504065,
"sd": 0.01493527859868 
},
{
 "name": "grid",
"mean": 0.005985680675422,
"sd": 0.01483533924659 
},
{
 "name": "grid",
"mean": 0.006026145644599,
"sd": 0.01459283838176 
},
{
 "name": "grid",
"mean": 0.006061215284553,
"sd": 0.01444672615208 
},
{
 "name": "grid",
"mean": 0.006091901219512,
"sd": 0.01436921544128 
},
{
 "name": "grid",
"mean": 0.006052654545455,
"sd": 0.01675342508864 
},
{
 "name": "grid",
"mean": 0.006094282520325,
"sd": 0.01632791183424 
},
{
 "name": "grid",
"mean": 0.00612950619137,
"sd": 0.01604290087011 
},
{
 "name": "grid",
"mean": 0.006159697909408,
"sd": 0.015856402791 
},
{
 "name": "grid",
"mean": 0.005997470325203,
"sd": 0.01579821583618 
},
{
 "name": "grid",
"mean": 0.00604014108818,
"sd": 0.01548621279833 
},
{
 "name": "grid",
"mean": 0.006076716027875,
"sd": 0.01528593518082 
},
{
 "name": "grid",
"mean": 0.006108414308943,
"sd": 0.01516455805314 
},
{
 "name": "grid",
"mean": 0.005950775984991,
"sd": 0.01500832803938 
},
{
 "name": "grid",
"mean": 0.005993734146341,
"sd": 0.01478048548814 
},
{
 "name": "grid",
"mean": 0.006030964552846,
"sd": 0.01464353131017 
},
{
 "name": "grid",
"mean": 0.006063541158537,
"sd": 0.01457113724121 
},
{
 "name": "grid",
"mean": 0.005910752264808,
"sd": 0.01434692720442 
},
{
 "name": "grid",
"mean": 0.005953514796748,
"sd": 0.01418237502745 
},
{
 "name": "grid",
"mean": 0.005990932012195,
"sd": 0.01409324823661 
},
{
 "name": "grid",
"mean": 0.006023947202296,
"sd": 0.014057971402 
},
{
 "name": "grid",
"mean": 0.00630387804878,
"sd": 0.01949772093133 
},
{
 "name": "grid",
"mean": 0.006328709268293,
"sd": 0.01846493598878 
},
{
 "name": "grid",
"mean": 0.006349025720621,
"sd": 0.01773892037704 
},
{
 "name": "grid",
"mean": 0.006365956097561,
"sd": 0.01722640234206 
},
{
 "name": "grid",
"mean": 0.006212534634146,
"sd": 0.01804799447634 
},
{
 "name": "grid",
"mean": 0.006243412416851,
"sd": 0.01725760125189 
},
{
 "name": "grid",
"mean": 0.006269143902439,
"sd": 0.01670367866063 
},
{
 "name": "grid",
"mean": 0.006290916697936,
"sd": 0.01631715476825 
},
{
 "name": "grid",
"mean": 0.006137799113082,
"sd": 0.0168861204875 
},
{
 "name": "grid",
"mean": 0.006172331707317,
"sd": 0.0162718473918 
},
{
 "name": "grid",
"mean": 0.006201551594747,
"sd": 0.01584500063671 
},
{
 "name": "grid",
"mean": 0.006226597212544,
"sd": 0.01555267484802 
},
{
 "name": "grid",
"mean": 0.006075519512195,
"sd": 0.01593829814894 
},
{
 "name": "grid",
"mean": 0.006112186491557,
"sd": 0.01545501717304 
},
{
 "name": "grid",
"mean": 0.00614361533101,
"sd": 0.01512379042787 
},
{
 "name": "grid",
"mean": 0.006170853658537,
"sd": 0.01490296301573 
},
{
 "name": "grid",
"mean": 0.006167158536585,
"sd": 0.01816840103203 
},
{
 "name": "grid",
"mean": 0.006202161419069,
"sd": 0.017411097405 
},
{
 "name": "grid",
"mean": 0.006231330487805,
"sd": 0.01687980210786 
},
{
 "name": "grid",
"mean": 0.006256012007505,
"sd": 0.01650815519056 
},
{
 "name": "grid",
"mean": 0.006096548115299,
"sd": 0.01697867780473 
},
{
 "name": "grid",
"mean": 0.006134518292683,
"sd": 0.0163966491621 
},
{
 "name": "grid",
"mean": 0.006166646904315,
"sd": 0.01599274203744 
},
{
 "name": "grid",
"mean": 0.006194185714286,
"sd": 0.01571629276224 
},
{
 "name": "grid",
"mean": 0.006037706097561,
"sd": 0.01600839410707 
},
{
 "name": "grid",
"mean": 0.006077281801126,
"sd": 0.01555620175853 
},
{
 "name": "grid",
"mean": 0.006111203832753,
"sd": 0.01524778721245 
},
{
 "name": "grid",
"mean": 0.006140602926829,
"sd": 0.01504335725989 
},
{
 "name": "grid",
"mean": 0.005987916697936,
"sd": 0.01520532911922 
},
{
 "name": "grid",
"mean": 0.00602822195122,
"sd": 0.01485115495829 
},
{
 "name": "grid",
"mean": 0.006063153170732,
"sd": 0.01461542363228 
},
{
 "name": "grid",
"mean": 0.006093717987805,
"sd": 0.01446613820048 
},
{
 "name": "grid",
"mean": 0.006055297117517,
"sd": 0.01714273300082 
},
{
 "name": "grid",
"mean": 0.006096704878049,
"sd": 0.01658303640768 
},
{
 "name": "grid",
"mean": 0.006131742213884,
"sd": 0.01619367956739 
},
{
 "name": "grid",
"mean": 0.006161774216028,
"sd": 0.01592604059766 
},
{
 "name": "grid",
"mean": 0.005999892682927,
"sd": 0.01614242616338 
},
{
 "name": "grid",
"mean": 0.006042377110694,
"sd": 0.01571295573218 
},
{
 "name": "grid",
"mean": 0.006078792334495,
"sd": 0.01542017316752 
},
{
 "name": "grid",
"mean": 0.006110352195122,
"sd": 0.0152260181545 
},
{
 "name": "grid",
"mean": 0.005953012007505,
"sd": 0.01531462382651 
},
{
 "name": "grid",
"mean": 0.005995810452962,
"sd": 0.01498310194669 
},
{
 "name": "grid",
"mean": 0.006032902439024,
"sd": 0.01476358620849 
},
{
 "name": "grid",
"mean": 0.006065357926829,
"sd": 0.01462555194335 
},
{
 "name": "grid",
"mean": 0.005912828571429,
"sd": 0.01462099789361 
},
{
 "name": "grid",
"mean": 0.005955452682927,
"sd": 0.01436429220436 
},
{
 "name": "grid",
"mean": 0.005992748780488,
"sd": 0.01420104565889 
},
{
 "name": "grid",
"mean": 0.006025657101865,
"sd": 0.01410627585785 
},
{
 "name": "grid",
"mean": 0.005962079268293,
"sd": 0.01633882093493 
},
{
 "name": "grid",
"mean": 0.006007472420263,
"sd": 0.0159236380858 
},
{
 "name": "grid",
"mean": 0.006046380836237,
"sd": 0.01563934825451 
},
{
 "name": "grid",
"mean": 0.006080101463415,
"sd": 0.01544944659934 
},
{
 "name": "grid",
"mean": 0.005918107317073,
"sd": 0.01548021574874 
},
{
 "name": "grid",
"mean": 0.005963398954704,
"sd": 0.01516412318483 
},
{
 "name": "grid",
"mean": 0.006002651707317,
"sd": 0.01495463432224 
},
{
 "name": "grid",
"mean": 0.006036997865854,
"sd": 0.01482257424656 
},
{
 "name": "grid",
"mean": 0.005880417073171,
"sd": 0.01476078189442 
},
{
 "name": "grid",
"mean": 0.00592520195122,
"sd": 0.01452013564333 
},
{
 "name": "grid",
"mean": 0.005964388719512,
"sd": 0.01436794695982 
},
{
 "name": "grid",
"mean": 0.00599896527977,
"sd": 0.0142804027502 
},
{
 "name": "grid",
"mean": 0.005847752195122,
"sd": 0.01415150714207 
},
{
 "name": "grid",
"mean": 0.005891779573171,
"sd": 0.01396902649738 
},
{
 "name": "grid",
"mean": 0.005930627259684,
"sd": 0.01386115764594 
},
{
 "name": "grid",
"mean": 0.005965158536585,
"sd": 0.0138081155123 
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

<div id = 'chart69459f50a5' class = 'rChart nvd3'></div>
<script type='text/javascript'>
 $(document).ready(function(){
      drawchart69459f50a5()
    });
    function drawchart69459f50a5(){  
      var opts = {
 "dom": "chart69459f50a5",
"width":    800,
"height":    400,
"x": "date",
"y": "weight",
"group": "stock",
"type": "stackedAreaChart",
"id": "chart69459f50a5" 
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
##    CA   EMN   FIA  CTAG    EM    GM 
## 0.062 0.382 0.344 0.094 0.050 0.072 
## 
## Objective Measures:
##       CRRA 
## -0.0005303 
## 
## 
##     mean 
## 0.005418 
## 
## 
##     ES 
## 0.0307 
## 
## 
##   StdDev 
## 0.009984
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
