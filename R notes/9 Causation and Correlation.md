## Making causal statements from observed/experimental data
This requires three fundamental assumptions to be done: 
* **Conditional exchangability**: States that as long as we have accounted for all confounders, we would obtain the same outcomes if the groups swapped treatment statuses.
* **SUTVA**: Assumes that an individual’s treatment status does not influence other individuals in the sample (aka the individual’s outcome depends only on their own treatment status, not on the treatment of others.)
* **Overlap**: States that for the included covariates, all subgroups of individuals have some probability of getting either treatment assignment.

## Propensity scores
Propensity scores are the **probability of receiving a treatment** given a set of observed covariates. They are widely used in causal inference to reduce confounding bias in observational studies, allowing treated and untreated groups to be compared more fairly.  
Propensity scores summarize multiple covariates into a single score, which can then be used for:
- Matching (pairing treated and untreated individuals with similar scores)
- Stratification (dividing subjects into strata based on their score)
- **Weighting** (e.g., inverse probability of treatment weighting, IPTW) -> shows strong perf  
- Covariate adjustment in regression models
- Following this, it is crucial to assess **balance** between the treatment & control groups.

Because this probability (propensity score) corresponds to a binary outcome—either being in the treatment group or the control group—we can model the propensity scores using **logistic regression.**

### Assessing balance
**1. Visual assessments**  
Plotting a balance plot `bal.plot()`.  

**2. Numerical methods**  
We can also assess overlap and balance numerically with the **standardised mean difference (SMD)** and the **variance ratio** for each variable. Good balance can be defined as:
* SMD between -0.1 and 0.1
* Variance ratio between 0.5 and 2.0


### Creating a balance table
```r
bt <- bal.tab(low_ef ~ age + cholesterol + heart_attack,
  data = los_data,
  disp.v.ratio = TRUE, # show variance ratio
  binary = "std" # use SMD
)
```
- The first argument to `bal.tab()` should be a formula or a treatment variable, not x = .... 
- `binary = std` shows SMDs for all variables
- `disp.v.ratio = TRUE` shows variance ratios for all continuous variables

For the example above, the results are:
```r
Balance Measures
                Type Diff.Un V.Ratio.Un
age          Contin.  0.5575     1.1072
cholesterol   Binary  0.5238          .
heart_attack  Binary  0.6162          .

Sample sizes
    Control Treated
All     716     284
```

### Modelling propensity scores
The `glm()` function in R makes modeling propensity scores via logistic regression simple. By default, the `glm()` function fits a linear regression model, so we need to modify the family argument to specify that the treatment group variable is binary. To do this, we set the family argument to "binomial".

**Task**  
Since, in our heart health example, we are trying to estimate the causal effect of low ejection fraction on length of hospital stay, the propensity score model should model the probability of having low ejection fraction given other characteristics.

The dataset `los_data` has been loaded for you in notebook.Rmd. Use the `glm()` function to run a logistic regression that models the probability of having low ejection fraction given age and cholesterol level. Save the results as `ef_model`.

```r
# Predict ejection fraction from age and cholesterol
ef_model <- glm(
  formula = low_ef ~ age + cholesterol,
  data = los_data,
  family = "binomial"
)
```

### Weighting propensity scores
Propensity score weighting transforms estimated propensity scores into weights that emphasize or diminish certain observations in our dataset. A form of this is known as inverse probability of treatment weighting (IPTW).

IPTW weights are calculated differently depending on whether we want to estimate the average treatment effect (ATE) or the average treatment effect on the treated (ATT). 
* **ATE** looks at the effect across the entire population (both treated and control groups).
  > _"What would be the effect if everyone in the population got treated vs if everyone got control?"_ 
* **ATT** is just on the treated group.
  >_"For people who actually received treatment, what would their outcomes be if they hadn’t been treated?"_	
  > Therefore, the **treated** group remains as-is (weight = 1) while the **control group** is reshaped with weight, so their covariates match the treated group's.

```r
	                ATE	  ATT
Treatment Weight	1/p	   1
Control Weight	1/(1-p)	p/(1-p)
```
- By giving **MORE** weight to individuals who look like those in the opposite treatment assignment => low propensity score, 0.1, for treated individual (looks more like control) has HIGH weight of 10 => they are better counterfactuals for individuals in the other treatment group
- By giving **LESS** weight to individuals who look like those in their own treatment assignment => high propensity score, 0.9, for treated individual (looks like other treated individuals) has LOW weight of 1.1

### Re-checking overlap and balance
If propensity score weighting is successful, we expect the distribution of propensity scores in the treatment group to be similar to that of the control group - we can use the `bal.plot()` function -> shows distribution of scores.    

The `love.plot()` function in cobalt **visually** checks the standard mean differences (SMD) between treatment groups for all variables before and after weighting.  


**Task**  
Time to assess the quality of the propensity score weighting you completed in the previous exercises! The `los_data` dataset has been loaded for you, and code has been provided for the `weightit` model, which is saved as `iptw_ef`.

```r
# create IPTW weights
iptw_ef <- weightit(
  formula = low_ef ~ age + cholesterol,
  data = los_data, 
  estimand = "ATT",
  method = "ps"
```

Use the `bal.plot()` function to show the distribution of propensity scores before and after the IPTW procedure and save the plot as `ps_bal`.
```r
ps_bal <- bal.plot(
  x = iptw_ef, #weightit object
  var.name = "prop.score", # propensity score
  which = "both", #view p score before and after weighting
  colors = c("#E69F00", "#009E73") 
)
```
Use the `love.plot()` function to plot the balance of variables after the propensity score weighting/IPTW. Save this plot as `ps_love`. 
```r
ps_love <- love.plot(
  x = iptw_ef,
  binary = "std", #use SMD
  thresholds = c(m=0.1), #guidelines
  colors = c("#E69F00", "#009E73")
)
```

### Refining propensity score models
As you may have noticed, propensity score methods are an iterative process: we check variable balance, model propensity scores, perform weighting, then check balance again. If imbalance still exists, we can change the propensity score model. Note that it can take multiple tries to find a good propensity score model. 
- Recall that successful weighting occurs when both SMDs fall between -0.1 and 0.1 (adjusted), which is seen visually with `love.plot()`.
