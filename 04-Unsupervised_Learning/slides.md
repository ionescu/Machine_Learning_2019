---
theme: boxes
theme: Madrid
mainfont: TeX Gyre Pagella
monofont: DejaVuSansMono
header-includes: \usepackage{tikz}
---

\title{Lecture 4}
\author{Cezar Ionescu}
\date{08/06/2019}
\titlegraphic{\includegraphics[scale=0.4]{oudce.jpg}}
\maketitle

Administrative
==============

- Homework from 25/05/2019 due now!

- Please complete and hand in the declarations of authorship.

Questions?
==========

Unsupervised learning
=====================

- we have a sequence of data points that fall into ```k```{.haskell} classes

- example:

  - images of cats and dogs
  - images of possibly malfunctioning engines
  - images of handwritten digits 
  
- we want the learning program to determine the ```k```{.haskell} classes by itself

Formalisation of unsupervised learning
======================================

- ```Task = Image -> {1, 2, ..., k}```{.haskell}

. . .

- ```Experience = List(Image)```{.haskell}

. . .

- ```perf : (Task, List(Image)) → ℝ```{.haskell}

. . .

- ```perf(t, [img₁, ..., imgₙ]) = sum [correct(t, img₁), ..., correct(t, img₂)] / n```{.haskell}
  
  - ```correct : (Task, Image) → {1, 2, ..., k}```{.haskell} is "hypothetical"

Exemplars
=========

The data usually comes in the form of tuples of fixed length (the **features**):

> x = (x₁, ..., xₘ) ∈ (Ω₁, ..., Ωₘ), Ωᵢ ⊆ ℝ

We usually assume the classification is based on "ideal" elements:

> ξ = (ξ₁, ..., ξₘ) ∈ (Ω₁, ..., Ωₘ), Ωᵢ ⊆ ℝ

The data is a distortion of the ideal exemplars, e.g.

> x = (x₁, ..., xₘ) = (ξ₁ + ε₁, ..., ξₘ + εₘ)

where ```εⱼ```{.haskell} is a random variable (see below) with mean 0.

Thus, if we only had one exemplar, we could reconstruct it by estimating the mean value of the data.

Expectations
============

Consider a coin toss experiment:

> Ω = {H, T}
> p : Event → [0, 1]
> p(H) = α

What is the expected value of the result?

Expectations
============

Now consider a bet: ```h```{.haskell} $ for heads, ```t```{.haskell} $ for tails.  The expected value is

. . .

> h * α + t * (1 - α)


Remarks
=======

- ```h```{.haskell} and ```t```{.haskell} are *not* possible results of the random experiment, which consists of flipping a coin.

- We can construct a "derived" probability space as follows:

. . .

- ```Ω' = {h, t}```{.haskell}
- ```p' : Event' → [0, 1]```{.haskell}
  - ```p'(h) = p(H)```{.haskell}
  - ```p'(t) = p(T)```{.haskell}

. . .

- Even if we consider ```Ω'```{.haskell}, it is still the case that the expected value is *not* a possible result of the experiment.

Random variable
===============

**Definition**:  Given ```Ω, Event, p```{.haskell}.  A **random variable** is a function ```X : Ω → ℝ```{.haskell}.

Probability theory and statistics are largely the study of random variables.

Expected value
==============

**Definition**:  Given ```Ω, Event, p```{.haskell} and a random variable ```X```{.haskell}.  Assume that ```Ω```{.haskell} is finite: ```Ω = {ω₁, ω₂, ..., ωₙ}```{.haskell}.  The **mean** or **expected value** of ```X```{.haskell} is

> E(X) = X(ω₁) * p(ω₁) + X(ω₂) * p(ω₂) + ... + X(ωₙ) * p(ωₙ)

Intuition: center of mass.

Classical statistics versus machine learning
============================================

The expected value minimises the guessing error.

This is not always what we want: cf., penalty shots.

Exercise
========

Consider a card game played with a full deck, in which aces have a value of ```11```{.haskell}, jacks ```12```{.haskell}, queens ```13```{.haskell}, kings ```14```{.haskell}, and all other cards a value of ```0```{.haskell}.  We draw a card at random from the deck.  What is its expected value?  Define ```Ω, Event, p```{.haskell} and the random variable whose expected value you are computing.

Standard deviation
==================

**Definition**: Consider ```Ω = {ω₁, ω₂, ..., ωₙ}, Event, p, X : Ω → ℝ```{.haskell}.  Let ```E(X) = μ```{.haskell} The **standard deviation** of ```X```{.haskell} is

> std_dev(X) = √ ((X(ω₁) - μ)² * p(ω₁) + ... (X(ωₙ) - μ)² * p(ωₙ))

The **variance** of ```X```{.haskell} is ```std_dev(X)²```{.haskell}

Exercise
========

Compute the standard deviation of the random variable defined for the card game exercise.

Notation
========

In the following, when no ambiguities arise, we use ```μ```{.haskell} to denote ```E(X)```{.haskell} and ```σ```{.haskell} for ```std_dev(X)```{.haskell}

Chebyshev's theorem
===================

Let ```Ω, Event, p```{.haskell} and ```X```{.haskell} as above.  Let

> Farₖ = {ω ∈ Ω | ||X(ω) - μ|| > k * σ}

Then ```p(Farₖ) ≤ 1/k²```{.haskell}.

Interpretation of Chebyshev's theorem
=====================================

```Ω, Event, p```{.haskell} and ```X```{.haskell} as above.  If we draw a random element from ```Ω```{.haskell}, then

- it will fall in the interval ```[μ - 2 * σ, μ + 2 * σ]```{.haskell} with probability at least ```0.75```{.haskell}

- it will fall in the interval ```[μ - 3 * σ, μ + 3 * σ]```{.haskell} with probability at least ```0.889```{.haskell}

- it will fall in the interval ```[μ - 4 * σ, μ + 4 * σ]```{.haskell} with probability at least ```0.938```{.haskell}

Example
=======

The mean price of houses in a certain neighbourhood is ```400000```{.haskell} $, and the standard deviation is ```80000```{.haskell} $.  Find the price range in which ```75%```{.haskell} of the houses will sell. (Bluman, Chapter 3)

*Solution*: From Chebyshev's theorem, we know that at least ```75%```{.haskell} of the houses are within the interval ```[μ - 2 * σ, μ + 2 * σ]```{.haskell}.  Therefore, the interval is ```[240000, 560000]```{.haskell}.

The normal distribution
=======================

> pdf(x) = (1/√(2 * π * σ²)) * exp(- (x - μ)²/(2 * σ²))

![Normal distribution[^1]](normal.png){ height=50% }

[^1]: Figure by Dan Kernler - Own work, CC BY-SA 4.0, https://commons.wikimedia.org/w/index.php?curid=36506025

Example
=======

Consider the neighbourhood from the previous example, where the mean price of houses is ```400000```{.haskell} $, and the standard deviation is ```80000```{.haskell} $.  Find the price range in which ```75%```{.haskell} of the houses will sell, but now assuming that *the prices are normally distributed*.

*Solution*: We use the Python function ```scipy.stats.normal.interval```{.haskell}:

> import scipy.stats
> scipy.stats.norm.interval(0.75, 400000, 80000)
        
We obtain ```[307972, 492028]```{.haskell}.  From Chebyshev's theorem, we had the much larger interval ```[240000, 560000]```{.haskell}.

The central limit theorem
=========================

```Ω, Event, p```{.haskell} and ```X```{.haskell} as above.  Consider the following experiment: ```n```{.haskell}  elements ```e₁, ..., eₙ```{.haskell} are drawn independently from ```Ω```{.haskell} and we compute the mean value of ```X```{.haskell} for this sample ```μₛ = (X(e₁) + ... + X(eₙ))/n```{.haskell}.  Then ```μₛ```{.haskell} is a random variable whose probability distribution approaches with increasing ```n```{.haskell} the normal distribution with mean ```μ```{.haskell} and standard deviation ```σ/√(n)```{.haskell}.

Homework
========

The theorem states that ```μₛ```{.haskell} is a random variable.  But a random variable is a function defined in the context of a probability space.  What is that probability space here?  You will need to specify ```Ω', Event', p' : Event' → [0, 1]```{.haskell} and define ```μₛ : Ω' → ℝ```{.haskell} as a function in terms of in terms of the given ```Omega, Event, p```{.haskell}, and ```X```{.haskell}.

Quality of approximation
========================

- If the random variable ```X```{.haskell} is itself normally distributed, then the approximation in the central limit theorem is good even for small sample sizes ```n```{.haskell}.

- If the random variable ```X```{.haskell} is not normally distributed, the approximation requires larger ```n```{.haskell}.  In many practical applications, a value ```n ≥ 30```{.haskell} will be adequate.

Example
=======

The Nielsen agency reported that children between ```2```{.haskell} and ```5```{.haskell} watch an average of ```25```{.haskell} hours of TV per week.  Assuming a normal distribution with ```σ = 3```{.haskell} hours.  If ```20```{.haskell} children are randomly selected, find the probability that the average TV watching time in a week will be ```μₛ > 26.3```{.haskell} hours. (Bluman, Chapter 6)

*Solution*:  The sample average ```μₛ```{.haskell} is a random variable that is approximately normally distributed, with mean ```25```{.haskell} and standard deviation ```3/√(20)```{.haskell}.  We need to find the probability that the value of ```μₛ```{.haskell} is bigger than ```26.3```{.haskell}.  We use the Python function ```scipy.stats.norm.cdf```{.haskell} to find the probability that ```μₛ```{.haskell} is smaller or equal to ```26.3```{.haskell}, and subtract from unity:

> import scipy.stats
> 1 - scipy.stats.norm.cdf(26.3, 25, 3/scipy.sqrt(20))

We obtain ```0.026316151282870237```{.haskell}, or approximately ```2.6%```{.haskell}.

EM algorithm
============

**Example**: Data generated from ```normal(3, 0.8)```{.haskell} (red) and ```normal(7, 2)```{.haskell} (blue).

![](data_colour.png)

- ```20```{.haskell} dots of each colour

- the **sample** means and standard deviations are ```2.8, 0.8```{.haskell}  red, ```6.97, 2.0```{.haskell} for blue.

EM algorithm
============

What if we do not know which points came from which distribution?

![](data_no_colour.png)

EM algorithm
============

We are looking for ```μ₁, σ₁, μ₂, σ₂```{.haskell}.

- hypothesis space: ```H = (ℝ, ℝ, ℝ, ℝ)```{.haskell}

- data: ```d = (x₁, ..., xₙ)```{.haskell} drawn from the two probability distributions.  

We want ```hₘₐₚ ∈ H```{.haskell}  

> hₘₐₚ = argmax p(h | d)


If we have a uniform prior on ```H```{.haskell}, then this is equivalent to ```hₘₗ```{.haskell}:

> hₘₗ = argmax p(d | h)

EM algorithm
============

>    p(d | h)
> =
>    p(x₁, ..., xₙ | h)
> =
>    p(x₁ | h) * ... * p(xₙ | h)

EM algorithm
============

>    ln p(d | h)
> =
>    ln (p(x₁ | h) * ... * p(xₙ | h))
> =
>    ln p(x₁ | h) + ... + ln p(xₙ | h)

EM algorithm
============

If we know that ```xᵢ```{.haskell} has been drawn from distribution ```j```{.haskell}, we can compute

>    p(xᵢ | h) = pdf(xᵢ, μⱼ, σⱼ) = (1/√(2 * π * σⱼ²)) * 
>                             exp(- (x - μⱼ)²/(2 * σⱼ²))

EM algorithm
============

Introduce the "hidden data":

- data: ```d = ((x₁, z₁₁, z₁₂) ..., (xₙ, zₙ₁, zₙ₂))```{.haskell}, where ```zᵢⱼ ∈ {0, 1}```{.haskell} is ```1```{.haskell}, if ```xᵢ```{.haskell} has been drawn from distribution ```j```{.haskell}, and ```0```{.haskell} otherwise.

>    p(xᵢ, zᵢ₁, zᵢ₂ | h) = (1/√(2 * π * σ₁²)) * 
>                    exp(- zᵢ₁ * (x - μ₁)²/(2 * σ₁²)) + 
>                    (1/√(2 * π * σ₂²)) * 
>                    exp(- zᵢ₂ * (x - μ₂)²/(2 * σ₂²))

EM algorithm
============

The data has become a random variable, hence we cannot maximise ```ln p(d | h)```{.haskell}.  Instead, we maximise

> E (ln p(d | h))


EM algorithm
============

>    E (ln p(d | h))
> =
>    E (ln p(x₁, z₁₁, z₁₂ | h) + ... + ln p(xₙ, zₙ₁, zₙ₂ | h))
> = 
>    E (ln p(x₁, z₁₁, z₁₂ | h) + ... + E (ln p(xₙ, zₙ₁, zₙ₂ | h)

EM algorithm
============

>    E (ln p(xᵢ, zᵢ₁, zᵢ₂ | h)
> =
>    E (ln ((1/√(2 * π * σ₁²)) * exp(- zᵢ₁ * (xᵢ - μ₁)²/(2 * σ₁²)) + 
>           (1/√(2 * π * σ₂²)) * exp(- zᵢ₂ * (xᵢ - μ₂)²/(2 * σ₂²))))
> =
>    E (ln 1/√(2 * π * σ₁²) - zᵢ₁ * (xᵢ - μ₁)²/(2 * σ₁²)) + 
>       ln 1/√(2 * π * σ₂²) - zᵢ₂ * (xᵢ - μ₂)²/(2 * σ₂²))
> =
>    (ln 1/√(2 * π * σ₁²) + ln 1/√(2 * π * σ₂²) - E(zᵢ₁ * (xᵢ - μ₁)²/(2 * σ₁²)) + 
>     ln 1/√(2 * π * σ₂²) - E(zᵢ₂ * (xᵢ - μ₂)²/(2 * σ₂²))
> =
>    (ln 1/√(2 * π * σ₁²) + ln 1/√(2 * π * σ₂²) - E(zᵢ₁) * (xᵢ - μ₁)²/(2 * σ₁²) + 
>     ln 1/√(2 * π * σ₂²) - E(zᵢ₂) * (xᵢ - μ₂)²/(2 * σ₂²)

EM algorithm
============

Computing ```E(zᵢⱼ)```{.haskell} given ```h```{.haskell} is easy:

>    E(zᵢ₁) = p(xᵢ | μ₁, σ₁) / (p(xᵢ | μ₁, σ₁) + p(xᵢ | μ₂, σ₂)

EM algorithm
============

We now assume that ```zᵢⱼ = E(zᵢ₁)```{.haskell}.  Therefore, we now have "complete" data and can compute ```hₘₗ```{.haskell}.

**EM algorithm**

0. Pick arbitrary ```h ∈ H```{.haskell}
1. Iterate until convergence:
1.1  Compute ```E(z)```{.haskell}
1.2  Use ```z := E(z)```{.haskell} in order to calculate ```hₘₗ```{.haskell}
1.3. replace ```h```{.haskell} with new ```hₘₗ```{.haskell}

 algorithm is to maximise ```E (ln p(d | h))```{.haskell} *iteratively*.  We consider an initial ```h```{.haskell}, and then compute the corresponding value of ```E (ln p(d | h))```{.haskell}.  This will involve finding out ```E(zᵢⱼ)```{.haskell}.  We then assume that the values of the hidden variables are equal to the expected values.  This gives us "complete" data, for which we can maximise the log-likelihood (not just the expected log-likelihood).  We thus obtain a new ```h```{.haskell}, and we iterate.
