# Basic Probability and Statistics
P(A): probability of A
P(A'): probability of not A (the complement)
P(A, B): probability of A and B occuring together

## Basic Principles
- P(A) <= 1
- P(A') = 1 - P(A)
- P(A or B) = P(A) + P(B) - P(A and B)

## Conditional Probability
P(A|B) = P(A and B) / P(B)

Example:
A: number of fatalities
B: number of people whose age is between 40-49
P(A and B): number of fatalities for people whose age is between 40-49

P(A|B) = P(A and B) / P(B)

*P(A|B) != P(A) as higher age groups tend to have higher fatality rate*

## Independence
If A and B are independent (not correlated) then
P(A and B) = P(A)P(B)
and
P(A|B) = P(A)

## Conditional Independence
[Typical Independence] Suppose Alice and Malice each toss *separate* die. A is Alice's outcome and B is Malice's outcome. Knowing Alice's outcome provides no information about Malice's outcome.

However if Alice and Malice toss the same dice, and the dice is biased toward showing some numbers, then knowing Alice's outcome provide some information about Malice's outcome can be inferred.

**Independece does not hold**
A⟂B | C if P(A|B, C) = P(A|C)

Given the information of C, B contributes no information about A.

If P(C) is the distribution for outcomes for the shared dice, then B⟂A | C - i.e. knowing Alice's outcome A gives no additional informaiton about Malice's outcome B. Therefore
P(B|(A, C)) = P(B|C).

Another Example:
``` mermaid
graph TD
    A --> B;
    A --> C;
    C --> E;
    D --> E;

  subgraph "Stats Knowledge";
    A[Statistics];
    B[Causal Inference];
    C[Machine Learning];
  end

  subgraph "Job Requirement";
    C[Machine Learning];
    D[SQL Skill];
    E[Job Offer];
  end
```

- knowledge of statistics means you know more about causal inference and machine learning
- to get a job offer you need to know *either* machine learning or SQL
- B is not independent of C - if you are good at causal inference, you are probably good at statistics, and likely good at machine learning
- B is independent of C given/conditioned on A - if you know someones level of statistics, you can infer machine learning skills and knowledge of causal inference will not give any further information
- C is independent of D - knowing how good you are at machine learning tells me nothing about how good you are at SQL
- C is not conditionally independent of D given E - given you get a job offer, knowing how good you are at ML gives me an idea about your SQL skills

## Simpson's Paradox
It can be that for all age groups, fatality rate in Italy is lower than those in China, *but the overall fatality rate in Italy is higher* than in China.

This is because Age is a confounding factor - causing the Simpson Paradox. Italian population is generally older than the Chinese population.

``` mermaid 
graph LR
    Country --> Age
    Country --> Fatality
    Age --> Fatality
```

# Potential Outcomes Framework
For a set of i.i.d. subjects $i = 1, ..., n$ we observe a tuple $(X_i, Y_i, W_i)$ comprised of:
- a *feature vector* $X_i$
- a *response* or *potential outcomes* $Y_i$
- a *treatment assignment* $T_i$

The goal is to estimate the Causal Estimand or Causal Effect of Treatment. However we only get to observe one outcome for each $i$ i.e. $Y_i = Y_i(T_i)$ - you are either treated or not treated.
- This is the "missingness" issue in causal inference

``` mermaid
graph LR
    A[Causal Estimand] --> |Identificaiton| B[Statistical Estimand]
    B[Statistical Estimand] --> |Estimation| C[Estimate]
```

- Causal Estimand or Causal Effect of Treatment: $E[Y(1) - Y(0)]$
- Statistical Estimand: $E_X[E[Y|T=1, X] - E[Y|T=0, X]]$
- Estimate: $Average(Average(Y_i|T_i=1)) - Average(Y_i|T_i=0))$

## Average Treatment Effect - ATE
The causal effect of treatment is the average treatment effect i.e. $E[Y(1) - Y(0)]$.

However we cannot observe treatment and control outcomes for all subjects $i$.

Approach 1: Pure Randomized Control Trial
- we assume that $(Y_0, Y_1) \bot T$
- the potential outcomes are independent of the treatment assignment
- in an RCT we get the following: $E[Y_i(1)] - E[Y_i(0)] = E[Y_i(1)|T_i=1] - E[Y_i(0)|T_i=0] = E[Y_i|T_i=1] - E[Y_i|T_i=0]$

Approach 2: Conditional Average Treatment Effect
- used when we expect that the *treatment assignment* is confounded by pre-treatment covariates $X$
- this is commonly seen in observed data
- we assume that $(Y_0, Y_1) \bot T | X$
- controlling for X is enough if treatment is *as good as random* conditional on $X$
- this is also known as *unconfoundedness*
- this means that the ATE is calculated as $E[Y_i(1) - Y_i(0)] =E[Y_i(1)-Y_i(0)|X] = E[E[Y_i|X_i, T_i=1] - E[Y_i|X_i, T_i=0]$
- note that $\text{CATE}$ and **individual treatment effect** can be considered the same

## Choice of Estimator
A good estimator is:
- Unbiased
- Consistent
- Efficient
- Robust

## Example

``` mermaid
graph LR
    cx[<b>Alice</b><br>Age: 25<br>Cohort Age: 5<br>Region: City<br>Device: iOS] --> t0[T=0<br>Promotion]
    cx --> t1[T=1<br>No promotion]
    t0 --> o1["LTV=?<br>Y(0)"]
    t1 --> o2["LTV=?<br>Y(1)"]
    style cx text-align: left
    style t0 text-align: left
    style t1 text-align: left
    style o1 text-align: left
```

| X (age, cohort_age, region, device) | Promo (Y1) | No Promo (Y0) | Observed Y | CATE |
|:-----------------------------------:|------------|---------------|------------|:----:|
| (25, 5, city, iOS)                  |     **60** |            55 |         60 |    5 |
| (35, 1, suburb, iOS)                |         70 |        **65** |         65 |    5 |
| (25, 5, city, android)              |     **70** |            60 |         70 |   10 |
| (27, 0, city, iOS)                  |         90 |        **80** |         80 |   10 |
| (36, 2, city, android)              |         85 |        **80** |         80 |    5 |
| (17, 2, suburb, iOS)                |     **75** |            70 |         75 |    5 |
| (21, 7, suburb, iOS)                |        100 |        **90** |         90 |   10 |
| (36, 2, city, android)              |     **80** |            70 |         80 |   10 |
| E(Y)                                |      71.25 |         78.75 |            |  7.5 |
|                                     | ATE        | 7.5           |            |      |

The bold values represent actual observed outcomes.

If we assumed RCT then ATE = -7.5 = E[Y|T=1] - E[Y|T=0].

However in this case we can see that the actual treatment effect is 7.5 - and a perfect CATE strategy would estimate that.

## Assumptions
### Ignorability
Basically we are saying that we control for everything possible to make sure our
treatment assignment is not biased.

1. The causal structure when treatment assignment mechanism is ignorable - i.e. not
   arrow from X to T - no confounding
``` mermaid
graph LR
  T ---> Y
  X ---> Y
```
2. Causal structure of X confounding the effect of T on Y. The confounding is depicted
   as the red dashed line.
``` mermaid
graph LR
  T --> Y
  X --> T
  X --> Y
```

### Positivity (a.k.a Overlap)
Two ways to look at positivity a.k.a overlap:

1. For all values of covariates $x$ present in the population of subjects, $0 < P(T=1|X=x) < 1$. That is the probability of treatment assignment for all values of covariate $x$ should be non-zero.
2. As overlap between the conditional distributions $P(X|T=0)$ and $P(X|T=1)$ is present. 

An example is:

 - Data on previous promos were only applied to Cx that are more than 2 years since activation. 
 - This means you can’t estimate reliably the outcome under T = 1 for Cx with < 2 cohort age.

#### The Positivity - Unconfoundedness Tradeoff
Similar to curse of dimensionality, as you increase the number of covariates you condition on the degree of overlap decreases. 

Therefore even if you can guarantee **ignorability** by controling for many confounders, you may still have a problem with overlap or positivity.