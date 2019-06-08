---
title: Lecture 4
monofont: DejaVuSansMono
author: Cezar Ionescu
geometry: "left=2.5cm,right=2.5cm,top=2.5cm,bottom=2.5cm"
mainfont: TeX Gyre Pagella
fontsize: 12pt
monofont: DejaVuSansMono
...

Unsupervised learning
---------------------

Unsupervised learning is a form of "learning from experience".  It is related to data mining: identifying patterns in data, and to classification.

**Example**: Assume we have a sequence of images that fall into ```k```{.haskell} classes.  They could be images of cats and dogs, or of possibly malfunctioning engines (in both cases we would have ```k = 2```{.haskell}), or handwritten digits (in which case ```k = 10```{.haskell}).  We want the learning program to determine the ```k```{.haskell} classes by itself.

- Task: ```Task = Image -> {1, 2, ..., k}```{.haskell}

- Experience: ```Experience = List(Image)```{.haskell}

- Performance: ```perf : (Task, List(Image)) → ℝ```{.haskell}

  - ```perf(t, [img₁, ..., imgₙ]) = sum [correct(t, img₁), ..., correct(t, img₂)] / n```{.haskell}
  
  - the function ```correct : (Task, List(Image)) → {1, 2, ..., k}```{.haskell} is a "hypothetical" function, since we might not know the correct classification (as in data mining).
  
A typical situation in which this type of learning can be achieved is when the data can be seen as having been generated from ```k```{.haskell} "ideal" exemplars, distorted in some way.  Unsupervised learning can then be seen as an attempt to reconstruct these exemplars from the data.

The data, and hence the ideal exemplars, usually comes in the form of tuples of fixed length (the **features**):

> ξ = (ξ₁, ..., ξₘ) ∈ (Ω₁, ..., Ωₘ), Ωᵢ ⊆ ℝ

We can model the "distortion" of the data by assuming that the data is obtained by perturbing the various features with a "noise":

> x = (x₁, ..., xₘ) = (ξ₁ + ε₁, ..., ξₘ + εₘ)

where ```εⱼ```{.haskell} is a random variable with mean 0.

Thus, if we only had one exemplar, we could reconstruct it by estimating the mean value of the data.

Statistics
----------

=== Random variables

Consider the typical experiment of flipping a (potentially biased) coin:

> Ω = {H, T}
> p : Event → [0, 1]
> p(H) = α

What is the expected value of a coin toss?

The answer is, of course, there isn't one: we expect the coin toss to result either in heads, or tails.

Now consider the situation in which the coin toss is the starting point of a bet: if the coin lands heads, you get ```h```{.haskell} $, if tails, you get ```t```{.haskell} $.  What is the expected value of a coin toss in this situation?  Intuitively, this would be the value we get if we take the bet a large number of times, i.e.:

> h * α + t * (1 - α)

**Remarks**:

- ```h```{.haskell} and ```t```{.haskell} are *not* possible results of the random experiment, which consists of flipping a coin.

- We can construct a "derived" probability space as follows:

  - ```Ω' = {h, t}```{.haskell}
  - ```p' : Event' → [0, 1]```{.haskell}
    - ```p'(h) = p(H)```{.haskell}
    - ```p'(t) = p(T)```{.haskell}
    
- Even if we consider ```Ω'```{.haskell}, it is still the case that the expected value is *not* a possible result of the experiment.

**Definition**:  Given ```Ω, Event, p```{.haskell}.  A **random variable** is a function ```X : Ω → ℝ```{.haskell}.

Probability theory and statistics are largely the study of random variables.

=== Expected values, standard deviations

**Definition**:  Given ```Ω, Event, p```{.haskell} and a random variable ```X```{.haskell}.  Assume that ```Ω```{.haskell} is finite: ```Ω = {ω₁, ω₂, ..., ωₙ}```{.haskell}.  The **mean** or **expected value** of ```X```{.haskell} is

> E(X) = X(ω₁) * p(ω₁) + X(ω₂) * p(ω₂) + ... + X(ωₙ) * p(ωₙ)

**Remark**: Consider the following game: a random experiment defined by ```Ω, Event, p```{.haskell} is performed many times.  We are given a random variable ```X : Ω → ℝ```{.haskell}.  We attempt, each time, to guess the value of the random variable, and we are rewarded proportionally to the distance between our guess and the actual result.  Then the optimal strategy consists in always naming the expected value of the variable.  Note that this **only the case if the reward is proportional** to the distance between guess and result!  This is typically the case in traditional statistics applications, but **not** in machine learning.

**Example**: A football player always shoots penalties either to the right of the goalkeeper, or to his left.  The goalkeeper can minimise the distance to the actual shots by standing still in the middle.  Nevertheless, it might actually be better to dive randomly left or right, and actually catch a ball from time to time.

**Exercise**: Consider a card game played with a full deck, in which aces have a value of ```11```{.haskell}, jacks ```12```{.haskell}, queens ```13```{.haskell}, kings ```14```{.haskell}, and all other cards a value of ```0```{.haskell}.  We draw a card at random from the deck.  What is its expected value?  Define ```Ω, Event, p```{.haskell} and the random variable whose expected value you are computing.

**Notation**:  In many situations, the derived probability space is silently substituted for the initial one.  That is, we consider

> Ω = {x(ω₁), ..., x(ωₙ)}

and even bypass ```ω```{.haskell}s directly, defining ```xᵢ = x(ωᵢ)```{.haskell}:

> Ω = {x₁, ..., xₙ}
> p : Event → [0, 1]
> p(A) = sum [p(xᵢ) | xᵢ ∈ A]

**Definition**: Consider ```Ω = {ω₁, ω₂, ..., ωₙ}, Event, p, X : Ω → ℝ```{.haskell}.  Let ```E(X) = μ```{.haskell} The **standard deviation** of ```X```{.haskell} is

> std_dev(X) = √ ((X(ω₁) - μ)² * p(ω₁) + ... (X(ωₙ) - μ)² * p(ωₙ))

The **variance** of ```X```{.haskell} is ```std_dev(X)²```{.haskell}

**Exercise**: Compute the standard deviation of the random variable defined for the card game exercise.

**Notation**: In the following, when no ambiguities arise, we use ```μ```{.haskell} to denote ```E(X)```{.haskell} and ```σ```{.haskell} for ```std_dev(X)```{.haskell}

The reason the standard deviation is an interesting measure, is that, no matter what the probability distribution has generated the data, samples are "unlikely" to be "many" standard deviations away from the mean.  The following remarkable result formalises this intuition:

**Theorem (Chebyshev)**:  Let ```Ω, Event, p```{.haskell} and ```X```{.haskell} as above.  Let

> Farₖ = {ω ∈ Ω | ||X(ω) - μ|| > k * σ}

Then ```p(Farₖ) ≤ 1/k²```{.haskell}.

**Interpretation of Chebyshev's theorem**:

```Ω, Event, p```{.haskell} and ```X```{.haskell} as above.  If we draw a random element from ```Ω```{.haskell}, then

- it will fall in the interval ```[μ - 2 * σ, μ + 2 * σ]```{.haskell} with probability at least ```0.75```{.haskell}

- it will fall in the interval ```[μ - 3 * σ, μ + 3 * σ]```{.haskell} with probability at least ```0.889```{.haskell}

- it will fall in the interval ```[μ - 4 * σ, μ + 4 * σ]```{.haskell} with probability at least ```0.938```{.haskell}

**Example**:

The mean price of houses in a certain neighbourhood is ```400000```{.haskell} $, and the standard deviation is ```80000```{.haskell} $.  Find the price range in which ```75%```{.haskell} of the houses will sell. (Bluman, Chapter 3)

*Solution*: From Chebyshev's theorem, we know that at least ```75%```{.haskell} of the houses are within the interval ```[μ - 2 * σ, μ + 2 * σ]```{.haskell}.  Therefore, the interval is ```[240000, 560000]```{.haskell}.

=== The normal distribution

> pdf(x) = (1/√(2 * π * σ²)) * exp(- (x - μ)²/(2 * σ²))

It is well-known that the normal distribution is ubiquitous.  If the data is normally distributed, then we can "tighten" the Chebyshev bounds considerably.

![Normal distribution[^1]](normal.png){ height=30% }

[^1]: Figure by Dan Kernler - Own work, CC BY-SA 4.0, https://commons.wikimedia.org/w/index.php?curid=36506025


**Example**:

Consider the neighbourhood from the previous example, where the mean price of houses is ```400000```{.haskell} $, and the standard deviation is ```80000```{.haskell} $.  Find the price range in which ```75%```{.haskell} of the houses will sell, but now assuming that *the prices are normally distributed*.

*Solution*: We use the Python function ```scipy.stats.normal.interval```{.haskell}:

> import scipy.stats
> scipy.stats.norm.interval(0.75, 400000, 80000)
        
We obtain ```[307972, 492028]```{.haskell}.  From Chebyshev's theorem, we had the much larger interval ```[240000, 560000]```{.haskell}.

=== The central limit theorem

The central limit theorem shows that means of samples are normally distributed around the real mean.  Hence, we can use the tighter bounds for estimates.

**Central limit theorem**:  ```Ω, Event, p```{.haskell} and ```X```{.haskell} as above.  Consider the following experiment: ```n```{.haskell}  elements ```e₁, ..., eₙ```{.haskell} are drawn independently from ```Ω```{.haskell} and we compute the mean value of ```X```{.haskell} for this sample ```μₛ = (X(e₁) + ... + X(eₙ))/n```{.haskell}.  Then ```μₛ```{.haskell} is a random variable whose probability distribution approaches with increasing ```n```{.haskell} the normal distribution with mean ```μ```{.haskell} and standard deviation ```σ/√(n)```{.haskell}.

**Homework**:  The theorem states that ```μₛ```{.haskell} is a random variable.  But a random variable is a function defined in the context of a probability space.  What is that probability space here?  You will need to specify ```Ω', Event', p' : Event' → [0, 1]```{.haskell} and define ```μₛ : Ω' → ℝ```{.haskell} as a function in terms of in terms of the given ```Omega, Event, p```{.haskell}, and ```X```{.haskell}.

**Remarks (quality of approximation)**: 

- If the random variable ```X```{.haskell} is itself normally distributed, then the approximation in the central limit theorem is good even for small sample sizes ```n```{.haskell}.

- If the random variable ```X```{.haskell} is not normally distributed, the approximation requires larger ```n```{.haskell}.  In many practical applications, a value ```n ≥ 30```{.haskell} will be adequate.

**Example**:

The Nielsen agency reported that children between ```2```{.haskell} and ```5```{.haskell} watch an average of ```25```{.haskell} hours of TV per week.  Assuming a normal distribution with ```σ = 3```{.haskell} hours.  If ```20```{.haskell} children are randomly selected, find the probability that the average TV watching time in a week will be ```μₛ > 26.3```{.haskell} hours. (Bluman, Chapter 6)

*Solution*:  The sample average ```μₛ```{.haskell} is a random variable that is approximately normally distributed, with mean ```25```{.haskell} and standard deviation ```3/√(20)```{.haskell}.  We need to find the probability that the value of ```μₛ```{.haskell} is bigger than ```26.3```{.haskell}.  We use the Python function ```scipy.stats.norm.cdf```{.haskell} to find the probability that ```μₛ```{.haskell} is smaller or equal to ```26.3```{.haskell}, and subtract from unity:

> import scipy.stats
> 1 - scipy.stats.norm.cdf(26.3, 25, 3/scipy.sqrt(20))

We obtain ```0.026316151282870237```{.haskell}, or approximately ```2.6%```{.haskell}.

EM algorithm
-------------

The central limit theorem explains why, when we have a single distorted exemplar, we can recover it as the mean of the data.  We now turn to the problem of multiple exemplars.


**Example**: Consider the following one-dimensional data generated from two normal distributions.  The red dots have been generated from ```normal(3, 0.8)```{.haskell}, the blue dots from ```normal(7, 2)```{.haskell}.

![](data_colour.png)

There are ```20```{.haskell} dots of each colour.  Since they come from normal distributions, the sample means and standard deviations are already very good approximations of the initial ones: ```2.8, 0.8```{.haskell} for the red ones, ```6.97, 2.0```{.haskell} for the blue ones.

However, if we do not know which points come from which distribution, the situation is rather bleak:

![](data_no_colour.png)

We are looking for ```μ₁, σ₁, μ₂, σ₂```{.haskell}.  Bayesian learning suggests trying to find the MAP hypothesis.

- hypothesis space: ```H = (ℝ, ℝ, ℝ, ℝ)```{.haskell}

- data: ```d = (x₁, ..., xₙ)```{.haskell} drawn from the two probability distributions.  

We want ```h ∈ H```{.haskell} which is ```argmax p(h | d)```{.haskell}.

If we have a uniform prior on ```H```{.haskell}, then this is equivalent to ```argmax p(d | h)```{.haskell}, i.e., we are looking for the ML hypothesis.

How can we compute ```p(d | h)```{.haskell}?  Since the data was generated from independent draws, we have

>    p(d | h)
> =
>    p(x₁, ..., xₙ | h)
> =
>    p(x₁ | h) * ... * p(xₙ | h)

We can see that, for large ```n```{.haskell}, this quantity will be vanishingly small.  In order to help with the computations, it is customary to maximise the *log-likelihood* instead:

>    ln p(d | h)
> =
>    ln (p(x₁ | h) * ... * p(xₙ | h))
> =
>    ln p(x₁ | h) + ... + ln p(xₙ | h)

If we know that ```xᵢ```{.haskell} has been drawn from distribution ```j```{.haskell}, we can compute

>    p(xᵢ | h) = pdf(xᵢ, μⱼ, σⱼ) = (1/√(2 * π * σⱼ²)) * exp(- (x - μⱼ)²/(2 * σⱼ²))

However, we do not know which distribution ```xᵢ```{.haskell} was chosen from.

The EM algorithm deals with this situation by considering the missing information as "hidden data".  That is, we take

- data: ```d = ((x₁, z₁₁, z₁₂) ..., (xₙ, zₙ₁, zₙ₂))```{.haskell}, where ```zᵢⱼ ∈ {0, 1}```{.haskell} is ```1```{.haskell}, if ```xᵢ```{.haskell} has been drawn from distribution ```j```{.haskell}, and ```0```{.haskell} otherwise.

**Remark**: This encoding of the choice of distribution is called "one-hot encoding".  It has the advantage that the likelihood can be expressed as

>    p(xᵢ, zᵢ₁, zᵢ₂ | h) = (1/√(2 * π * σ₁²)) * exp(- zᵢ₁ * (x - μ₁)²/(2 * σ₁²)) + 
>                          (1/√(2 * π * σ₂²)) * exp(- zᵢ₂ * (x - μ₂)²/(2 * σ₂²))

Since the data itself now has an unknown part, it becomes a random variable.  We can no longer reasonably want to maximise ```ln p(d | h)```{.haskell}, however, we can ask for maximising

> E (ln p(d | h))

This turns out to be relatively easy:

>    E (ln p(d | h))
> =
>    E (ln p(x₁, z₁₁, z₁₂ | h) + ... + ln p(xₙ, zₙ₁, zₙ₂ | h))
> = 
>    E (ln p(x₁, z₁₁, z₁₂ | h) + ... + E (ln p(xₙ, zₙ₁, zₙ₂ | h)

But

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

We have reduced the computation of ```E (ln p(d | h))```{.haskell} to that of ```E(zᵢ₁)```{.haskell} and ```E(zᵢ₂)```{.haskell}.  But these are easy: they are just the probability that ```xᵢ```{.haskell} comes from the red (respectively blue) distribution, given the hypothesis ```h```{.haskell}:

>    E(zᵢ₁) = p(xᵢ | μ₁, σ₁) / (p(xᵢ | μ₁, σ₁) + p(xᵢ | μ₂, σ₂)

The second crucial idea of the EM algorithm is to maximise ```E (ln p(d | h))```{.haskell} *iteratively*.  We consider an initial ```h```{.haskell}, and then compute the corresponding value of ```E (ln p(d | h))```{.haskell}.  This will involve finding out ```E(zᵢⱼ)```{.haskell}.  We then assume that the values of the hidden variables are equal to the expected values.  This gives us "complete" data, for which we can maximise the log-likelihood (not just the expected log-likelihood).  We thus obtain a new ```h```{.haskell}, and we iterate.



