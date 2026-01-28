# Logit Model and its Application in IO

Date: 2025/11/15  
Author: Yanjun Gao

---

## Overview

This note summarizes the logit family of discrete choice models and their microeconomic random-utility foundations. It covers the binary (simple) logit, the multinomial logit (MNL), the Independence of Irrelevant Alternatives (IIA) property and its implications, and common solutions to IIA including nested logit and random-coefficient (mixed) logit models. The presentation emphasizes intuition, key formulas, and practical estimation remarks.

---

## 1. Simple Logit model with dummy outcome

Setup
- Binary outcome y ∈ {0,1}. Let X denote a vector of observed covariates and β the corresponding parameter vector.
- The model specifies the probability of y = 1 conditional on X as the logistic function:
  $P(y = 1 \mid X) = \dfrac{\exp(X\beta)}{1 + \exp(X\beta)}$.

Random-utility foundation
- A convenient microeconomic foundation is the random utility framework. Suppose the decision maker compares utilities
  $U_1 = X\beta + \varepsilon_1$ for choosing $y = 1$ and $U_0 = 0 + \varepsilon_0$ for choosing $y = 0$ (normalization of deterministic utility for baseline).
- If the unobserved shocks $\varepsilon_1$ and $\varepsilon_0$ are iid Gumbel (Type I extreme value), then the choice probability above follows exactly.

Latent-variable representation
- Equivalently, define a latent variable $y^* = X\beta + u$ where $u$ has a Logistic distribution. Observe $y = 1$ if $y^* > 0$. This yields the same logistic probability.

Estimation and interpretation
- Parameters are typically estimated by maximum likelihood.
- Coefficients $\beta_k$ are log-odds effects: a one-unit increase in $X_k$ multiplies the odds $P/(1-P)$ by $\exp(\beta_k)$.
- Marginal effects on the probability depend on X: $\partial P/\partial X_k = P(1 - P)\beta_k$ (for continuous covariates when $\beta_k$ multiplies $X_k$ linearly).
- Standard errors follow from the Hessian of the log-likelihood; robust (sandwich) SEs are used when heteroskedasticity or clustering is a concern.

Remarks
- The logistic model is computationally convenient and yields closed-form choice probabilities because of the Gumbel error assumption.

---

## 2. Multinomial Logit model

Setup
- Let there be $J$ discrete alternatives $j = 1,\dots,J$. For individual $i$, utility for alternative $j$ is
  $U_{ij} = X_{ij}'\beta_j + \varepsilon_{ij}$,
  where $X_{ij}$ can include alternative-specific attributes and individual covariates (interacted with alternative dummies).
- For identification one typically normalizes one alternative (say $j = J$) by setting $\beta_J = 0$.

Choice probabilities
- With $\varepsilon_{ij}$ iid Gumbel, the multinomial logit (MNL) probabilities are
  $P_{ij} = \frac{\exp(X_{ij}'\beta_j)}{\sum_{k=1}^J \exp(X_{ik}'\beta_k)}$
- The MNL can be viewed as an extension of the binary logistic model to multiple choices.

Interpretation and estimation
- Coefficients for an alternative are interpreted relative to the normalized base alternative: they represent effects on the log-odds of choosing $j$ versus the base.
- Estimation is by maximizing the multinomial log-likelihood.
- Even without thinking of micro-foundation, this method is still a very straightforward expansion to dummy variable logit model: for each option, estimate a different set of parameters. 

Observables, alternatives, and heterogeneity
- Alternative-specific variables (attributes that vary across $j$) can be included directly in $X_{ij}$.
- Individual-specific variables that do not vary with $j$ are typically interacted with alternative indicators to allow alternative-specific effects.

---

## 3. Independence of Irrelevant Alternatives (IIA)

Definition
- IIA implies that the odds ratio between any two alternatives $j$ and $k$ depends only on those two alternatives and not on other available alternatives:
  $\frac{P_{ij}}{P_{ik}} = \frac{\exp(X_{ij}'\beta_j)}{\exp(X_{ik}'\beta_k)}$
- In words, adding or changing a third alternative $m$ does not alter the relative odds of $j$ versus $k$.

Intuition and the "red bus / blue bus" example
- Suppose a commuter chooses between car and bus, and then a very similar bus alternative is introduced. Under IIA, the original bus share is split proportionally between the two buses; the car/bus odds are unaffected. This proportional split is often unrealistic when alternatives are close substitutes, leading to mistaken substitution patterns.

Consequences
- When alternatives share unobserved attributes (correlated unobserved utility components), the MNL's IIA assumption can produce biased or implausible substitution patterns.
- Empirical tests (e.g., Hausman–McFadden) are used to detect IIA violations; structural model choice should consider likely correlation patterns among alternatives.

---

## 4. Solutions to IIA

To relax IIA one must allow correlation in unobserved utility across alternatives. Two widely used approaches are nested logit and the random-coefficient (mixed) logit. They are actually based on different assumptions about the source of violation of IIA.

### 4.1 Nested logit

Source of Violation in IIA
- correlation in the **unobserved** utility shocks ($\varepsilon_{ij}$) across alternatives.
  
Structure
- Alternatives are grouped into nests where unobserved utility components are correlated within nests but independent across nests.
- The random component has a generalized extreme value (GEV) structure that yields closed-form (but more complex) choice probabilities.

Probability decomposition
- The probability of choosing alternative $j$ in nest $g$ decomposes into $P_{ij} = P(i\ \text{chooses}\ g)\times P(i\ \text{chooses}\ j\mid g)$
- The conditional (within-nest) probabilities have the logit form, while the upper-level nest choice uses the inclusive value (log-sum) of the nest.

Inclusive value
- The inclusive value for nest $g$ is the log-sum of utilities in the nest:
  $IV_{ig} = \ln\!\left(\sum_{m\in g} \exp\!\left(\frac{V_{im}}{\lambda_g}\right)\right)$
  where $0 < \lambda_g \le 1$ is the dissimilarity parameter controlling within-nest correlation; $\lambda_g = 1$ reduces the nest to independent extremes (i.e., standard MNL behavior).

Remarks
- Nested logit relaxes IIA across nests (substitution is richer across nests) but retains a form of IIA within each nest. Further nesting (multi-level nested logit) can model hierarchical correlation structures.
- Estimation uses maximum likelihood and requires correct specification of nests; misspecified nests can still lead to poor substitution predictions.

### 4.2 Random coefficient model (Mixed Logit)

Source of violation for IIA
- **Unobserved** heterogeneity in tastes ($\beta_i$) across individuals.

Structure
- Allow taste parameters to vary randomly across individuals: $\beta_i = \beta(d_i) + \eta_i$, with $\eta_i$ drawn from some distribution $f(\eta\mid \theta)$ (e.g., Normal). Conditional on $\beta_i$, the choice probability is the standard logit:
  $P_{ij\mid\beta_i} = \frac{\exp(X_{ij}'\beta_i)}{\sum_k \exp(X_{ik}'\beta_i)}$
- The unconditional choice probability integrates over the distribution of $\eta_i$:
  $P_{ij} = \int P_{ij\mid\beta} f(\eta\mid d_i,\theta)d\eta$
  Therefore, this method assumes the source of IIA is from unobserved heterogeneous taste across individuals.

Flexibility and properties
- Mixed logit (a.k.a. random-parameters logit) can approximate any random-utility model arbitrarily well under mild conditions and thus can generate very flexible substitution patterns that do not satisfy IIA.
- It can capture unobserved taste heterogeneity and correlation in unobserved utility across alternatives through shared random coefficients.

Estimation
- The integral above generally has no closed form and is approximated via simulation (simulated maximum likelihood or method of simulated moments).
- Typical practice uses Halton draws or scrambled quasi-random draws to improve simulation efficiency.
- Identification requires sufficient variation and careful choice of which coefficients are random versus fixed.

Computational notes
- Mixed logit is computationally heavier than nested logit or MNL, but modern computing makes it feasible for many applied problems.
- Choice between nested and mixed logit often depends on the nature of the substitution patterns you expect (structured across nests vs. continuous heterogeneity across individuals).

### 4.3 Fixed effects Mixed Logit by Dubois et al (2020)
Structure
- Still Allow taste parameters to vary randomly across individuals, but not imposing distributional assumption on $\beta_i$. Instead, estimate $\beta_i$ using individual's choice across many choice occassions.
- This allows flexible relation between  $\beta_i$ and $d(i)$, instead of arbitrarily assuming a functional form, like $\beta(d_i) = \gamma d_i$
- $beta_i$ is estimated as if it is fixed effect in panel model.
- But requires large number of choice occasions for each $i$. Incidental Parameters Problem.
- The assumption on the source of IIA is the same as in Mixed Logit model
---

## Practical guidance and model selection

- Start with simple MNL as a baseline and test for IIA violations when realistic substitution is a concern.
- If alternatives naturally cluster (clear nests), nested logit is an interpretable and parsimonious extension.
- When heterogeneity in tastes is suspected and substitution patterns are complex, mixed logit provides the greatest flexibility.
- Consider computational cost, identification (which parameters can be estimated), and the substantive context when selecting an extension.

---

## References
- Train, K. (2009). Discrete Choice Methods with Simulation. Cambridge University Press.
- McFadden, D. (1974). Conditional Logit Analysis of Qualitative Choice Behavior. In P. Zarembka (Ed.), Frontiers in Econometrics.
- Dubois, P., Griffith, R., & O’Connell, M. (2020). How well targeted are soda taxes?. American Economic Review, 110(11), 3661-3704.
