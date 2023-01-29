# Probability
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
``` mermaid
graph LR
    A[Causal Estimand] --> |Identificaiton| B[Statistical Estimand]
    B[Statistical Estimand] --> |Estimation| C[Estimate]
```

- Causal Estimand: $E[Y(1) - Y(0)]$
- Statistical Estimand: $E_X[E[Y|T=1, X] - E[Y|T=0, X]]$
- Estimate: $Average(Average(Y_i|T_i=1)) - Average(Average(Y_i|T_i=0))$

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

* $Y_0, Y_1$ are both potential outcomes
* $\text{CATE}(X)$: Conditional Average Treatment Effect given X
    - $\text{CATE}(X)=E[Y_1-Y_0|X]$
    - We never directly observe $\text{CATE}$ sinc we only see one of the potential outcomes
    - Not that $\text{CATE}$ and **individual treatment effect** can be considered the same
* $\text{ATE}$: Average Treatment Effect
    - $\text{ATE}=E[Y|T=1]-E[Y|T=0]$
    - The average effect when we had a promotion minus the average effect when we don't have the promotion
    - This effect is **only valid under the assumption of ignorability** which is valid in a randomized control trial (RCT)

## Difference Between $\text{ATE}$ and $\text{CATE}$