---
layout: archive
title: "Projects"
permalink: /projects/
author_profile: true
---

## Horseshoe process regression

In healthy, premenopausal women, basal body temperature (BBT) follows a well-defined pattern. Each menstrual cycle starts with the onset of the period, at which time BBT is low. With ovulation (usually around day 14 of the menstrual cycle), a woman's BBT jumps and remains high until the onset of the next period, when it drops suddenly to the pre-ovulation temperature and the cycle repeats. For an individual woman, tracking BBT over several menstrual cycles may give insight into underlying health conditions or provide guidance on how best to time sexual intercourse to improve or reduce chances of pregnancy, especially when used in combination with other health and fertility indicators.

BBT is difficult to model. Although the pattern outlined above generally holds, the details may vary across and within women, with the date of ovulation sometimes very early or late, the full menstrual cycle length ranging from 20 to 40+ days, and differing degrees of sharpness in the temperature jump. Many conventional methods to analyze these data would oversmooth the temperature jump and introduce excess motion into the flat pre- and post-ovulation portions of the association, making it difficult to identify the true time of ovulation. This bias could have real ramifications for a couple trying to achieve or avoid pregnancy. The data below illustrate some of these features, showing observed BBT data from one woman's menstrual cycle and the results from three different fitted models. The observed data are given as dots, with an expert's best guess of the true ovulation date given as a vertical dashed line. Three different model fits and their 95% uncertainty intervals are shown. We see that using either a Gaussian process regression (GPR) or a penalized spline model (Pspline) would estimate the date of ovulation as three days earlier than reality. Horseshoe process regression (HPR; the method I propose) accurately identifies the date of ovulation.

![Basal Body Temperature Data](mens_plot.png)

How does HPR address this problem? The horseshoe distribution is a Bayesian shrinkage prior. Suppose we are fitting a linear regression model with $p$ predictors. If $p$ is large, we may want to shrink the estimates of the linear coefficients $\beta_j$, $j = 1, ..., p$. The horseshoe prior is one way to do so, in which we assume each $\beta_j$ is normally distributed with mean $0$ and variance $\tau^2 \lambda_j^2$, with $\tau, \lambda_j$ independently distributed as half-Cauchy with location $0$ and scale $1$. We call $\tau$ the global shrinkage parameter, as it provides overall shrinkage on the linear coefficients. If $\tau$ is large, the prior admits many large coefficients; if $\tau$ is small, the coefficients are pushed towards zero. However, the horseshoe prior also includes local shrinkage parameters, $\lambda_j$, one for each linear coefficient. The local shrinkage parameters allow individual coefficients to attain high values, even if $\tau$ is small.

To fit a BBT trajectory, we extend the horseshoe distribution as a stochastic process, assuming that incremental change in BBT over time is horseshoe distributed. Let $B_i$ be a measurement of BBT taken on day $t_i$, $i = 1,...,n.$ Then we assume that $Bi−Bi−1 ∼N(0,τ2λ2i(ti−ti−1))$, where $\tau, \lambda_i$ have the same priors as given above. This implies that on average we expect to see very little change in BBT over time, with the exception of a select number of large, sudden jumps, created by the local shrinkage parameters $\lambda_i$. That is the model fit we see from the HPR in the figure above--something that looks like a step function, although HPR is adept at fitting any association that exhibits abrupt changes. After exploring this simplest formulation of HPR, I subsequently extended it to allow for additional linear predictors, non-Gaussian outcomes, monotonicity constraints, and correlated data. All of these methods are implemented in the R package [HPR](https://github.com/elizabethchase/HPR) that I developed.

## Competing risks tradeoffs in prostate cancer care

## Flexible competing risks analyses

## Flint Community Engagement Project

## Gun violence

