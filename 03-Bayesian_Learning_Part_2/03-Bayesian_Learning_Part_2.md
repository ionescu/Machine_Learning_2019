---
title: Lecture 3
monofont: DejaVuSansMono
author: Cezar Ionescu
geometry: "left=2.5cm,right=2.5cm,top=2.5cm,bottom=2.5cm"
mainfont: TeX Gyre Pagella
fontsize: 12pt
monofont: DejaVuSansMono
header-includes: \usepackage{tikz}
...

Bayesian networks
-----------------

=== Joint probability distributions

The mathematical model of probability consists of a triple ```(Ω, Event, p)```{.haskell} as in the previous lecture.  Very frequently, however, the set of possible outcomes ```Ω```{.haskell} has the structure of a *cartesian product*, i.e., a tuple of possible results.  For instance, the diagnostic problem we discussed last time could also have been modelled by taking ```Omega = (H, R)```{.haskell}, i.e., the set of tuples ```{(healthy, ⊖), (healthy, ⊕), (ill, ⊖), (ill, ⊕)```{.haskell}.
The event "healthy person" would then be described by ```Healthy = {(healthy, ⊖), (healthy, ⊕)}```{.haskell}, etc.

**Exercise**: Define the other events of interest: "ill", "tested positive", "tested negative".

We can now construct new probability spaces on each component of ```Ω```{.haskell}.  For example:

> pₕ : ℙ(H) → [0, 1]
> pₕ (healthy) = p({(healthy, ⊖), (healthy, ⊕)})
>              = p (healthy, ⊖) + p (healthy, ⊕)

**Remark**: in general, if ```Ω = (Ω₁, Ω₂, ..., Ωₙ)```{.haskell} and ```ωᵢ ∈ Ωᵢ```{.haskell}, we will use ```p(ω_i)```{.haskell} to denote

> p(ωᵢ) = p({(x₁, x₂, ..., xₙ) | x₁ ∈ Ω₁, ..., xₙ ∈ Ωₙ, xᵢ = ωᵢ})

**Remark**: recall our convention that for ```x ∈ Ω```{.haskell} ```p(x)```{.haskell} is an abbreviation for ```p({x})```{.haskell}.  Above, we have taken this convention one step further, by using ```p(x, y)```{.haskell} to denote ```p({(x, y)})```{.haskell}, and ```p(x)```{.haskell} to denote ```p({(x, y) | y ∈ Y})```{.haskell}.  We shall continue using this form of abbreviation throughout, as is standard usage.

The "complete" probability measure ```p```{.haskell} is called the **joint probability distribution**, the probability measures over the individual components (or over tuples of individual components) are called **marginal probability distributions**.

**Remark**: it is trivial to derive the marginal probability distributions, given the joint probability distribution.  However, even if we have all the marginal distributions, we cannot derive the joint distribution from them.

By definition, the joint distribution is sufficient to answer any query about the probabilities of events in ```Ω```{.haskell}.  The problem is that it requires exponential storage space with respect to the number of components.  For example, if ```Ω```{.haskell} consists of a tuple of ```n```{.haskell} boolean components, then the joint probability distribution can be represented as a table with ```2ⁿ```{.haskell} rows.

Let ```Ω = (Ω₁, Ω₂, ..., Ωₙ)```{.haskell} and ```(ω₁, ω₂, ..., ωₙ) ∈ Ω```{.haskell}.  Then

> p(ω₁, ω₂, ..., ωₙ)  =  p(ω₁ ∩  ω₂ ∩  ... ∩ ωₙ)                   -- recall convention above!
>                     =  p(ω₁ | ω₂ ∩  ... ∩ ωₙ)*p(ω₂ ∩  ... ∩ ωₙ)  -- Bayes' theorem

It is easy to see that, therefore:

> p(ω₁, ω₂, ..., ωₙ)  =  p(ω₁ |  ω₂ ∩  ... ∩ ωₙ) * 
>                        p(ω₂ | ω₃ ∩ ... ∩ ωₙ) * ...
>                        p(ωₙ)

=== Independence, conditional independence

**Definition**: Two events ```X, Y ⊆ Ω```{.haskell} are called **independent** if 

> p(X ∩ Y) = p(X) * p(Y)

**Example**:  Consider a fair standard die, and the events ```even = {2, 4, 6}```{.haskell}, ```big = {5, 6}```{.haskell}.  Then ```even```{.haskell} and ```big```{.haskell} are independent.

**Exercise**: If ```X, Y```{.haskell} are independent, then so are ```X, ¬Y```{.haskell}.

**Example**: The events ```big```{.haskell} and ```divBy3 = {3, 6}```{.haskell} are **not** independent.

**Definition**: Consider events ```X, Y, Z ⊆ Ω```{.haskell} such that ```p(Z) ≠ 0```{.haskell}.  Events ```X, Y```{.haskell} are called **conditionally independent given** ```Z```{.haskell} if

> p(X ∩ Y | Z) = P(X | Z) * p(Y | Z)

**Example**: 

- The events ```even, big```{.haskell} are conditionally dependent given ```divBy3```{.haskell}.  Intuition: knowing the result is ```divBy3```{.haskell} means that knowledge of it being ```big```{.haskell} informs knowledge of it being ```even```{.haskell}.

- The events ```divBy3, big```{.haskell} are conditionally independent given ```{2, 3, 5, 6}```{.haskell}.  Intuition: knowing that the result is in ```{2, 3, 5, 6}```{.haskell} means that knowledge of it being ```big```{.haskell} doesn't tell me anything more about it being ```divBy3```{.haskell}

=== Bayesian networks

A Bayesian network is a graphical device for recording conditional independence information for a finite ```Ω = (Ω₁, Ω₂, ..., Ωₙ)```{.haskell}, in order to make the computation of entries of the joint probability distribution more efficient.  The network is a *directed acyclic graph (DAG)*, with one node for each component ```Ωᵢ```{.haskell}.  For example:

![Mitchell, page 186](forestfire.png)

The network is assumed to be constructed in such a way that the probability of an ```ω_i```{.haskell} is conditionally independent of any non-descendants given its parents.

**Example*: In the network above, we have

> p(thunder | lightning ∩ bustourgroup) = p(thunder | lightning)

but in general

> p(lightning | thunder ∩ storm) ≠ p(lightning | storm)

In this way, the Bayesian network allows us to compute any entry in the joint distribution table.  For example:

>   p(thunder, forestfire, campfire, lightning, storm, bustourgroup)
> =
>   p(thunder | forestfire ∩ campfire ∩ lightning ∩ storm ∩ bustourgroup) * 
>   p(forestfire ∩ campfire ∩ lightning ∩ storm ∩ bustourgroup)
> =
>   p(thunder | lightning) * 
>   p(forestfire ∩ campfire ∩ lightning ∩ storm ∩ bustourgroup)
> =
>   p(thunder | lightning) * 
>   p(forestfire | campfire ∩ lightning ∩ storm ∩ bustourgroup) *
>   p(campfire ∩ lightning ∩ storm ∩ bustourgroup)
> =
>   p(thunder | lightning) * 
>   p(forestfire | campfire ∩ lightning) *
>   p(campfire ∩ lightning ∩ storm ∩ bustourgroup)
> =
>   p(thunder | lightning) * 
>   p(forestfire | campfire ∩ lightning) *
>   p(campfire | lightning ∩ storm ∩ bustourgroup) *
>   p(lightning ∩ storm ∩ bustourgroup)
> =
>   p(thunder | lightning) * 
>   p(forestfire | campfire ∩ lightning) *
>   p(campfire | storm ∩ bustourgroup) *
>   p(lightning | storm) *
>   p(storm) * p(bustourgroup)

Assuming all the ```Ωᵢ```{.haskell} are boolean, we need two tables 1x2 tables to store the probability values for ```Storm, BusTourGroup```{.haskell}, two 2x2 tables to store the conditional table for ```Lightning | Storm```{.haskell} and ```Thunder | Lightning```{.haskell}, and two 4x2 tables to store the conditional table for ```ForestFire | Campfire ∩ Lightning```{.haskell} and ```Campfire | Storm ∩ BusTourGroup```{.haskell}, altogether 28 values.  If we stored the full joint distribution table, we would need ```2⁶ = 64```{.haskell} values.

**Homework**: (based on Nilsson, 19.4, page 340) Consider the following Bayesian network:

![Nielsson, page 341](burglary.png)

Compute ```p(¬J, ¬M, A, B, E)```{.haskell}.  This is the probability that there is both an earthquake and a burglary, the alarm rings, but neither John nor Mary call.

**Exercise**: Compute ```p(¬J, ¬M, B, E)```{.haskell} (this is exercise 19.4 in Nielsson).

Minimum description length (MDL) principle
------------------------------------------

The ASCII code used to represent alphabetic characters of the English language (and largely replaced by Unicode nowadays) used one byte (eight bits) per character.  Thus, to encode a string of 100 characters, 800 bits were needed.  Quite often, this encoding was very wasteful, and substantial memory savings could be achieved by *compressing* the string (for example by using *zip* or *rar*).  Compression uses a different, string-specific encoding, with the property that more frequent characters receive shorter codes.  For example, to encode ```nine```{.haskell} we could use

> 'n' → 0
> 'i' → 10
> 'e' → 11

so that 

> 'nine' → 010011

It turns out that an optimal encoding scheme requires, for every character ```c```{.haskell}, about ```-log₂ f(c)```{.haskell} bits, where ```f(c)```{.haskell} is the frequency with which the character ```c```{.haskell} appears in the string.

More generally, in order to encode messages drawn at random from a finite set, the optimal encoding scheme will associate to message ```mᵢ```{.haskell}, having probability ```pᵢ```{.haskell}, about ```-log₂ pᵢ```{.haskell} bits.

We these preliminaries, we return to the definition of the MAP hypothesis.  We have

> hₘₐₚ = argmaxₕ p(d | h) * p(h)
> iff
> hₘₐₚ = argmaxₕ log₂ (p(d | h) * p(h))
> iff
> hₘₐₚ = argmaxₕ log₂ p(d | h) + log₂ p(h)
> iff 
> hₘₐₚ = argminₕ -log₂ p(d | h) - log₂ p(h)

Here, the expression ```-log₂ p(h)```{.haskell} represents the length that we associate to hypothesis ```h```{.haskell} under the optimal encoding.  Thus, ```-log₂ p(h) = length(optimal_encode(h))```{.haskell}.  

Similarly, the expression ```-log₂ p(d | h)```{.haskell} represents the length associated to the data ```d```{.haskell} under the assumption that hypothesis ```h```{.haskell} is the correct one.  We write this as ```length(optimal_encode(d | h))```{.haskell}.  Therefore

> hₘₐₚ = argminₕ length(optimal_encode(d | h)) + length(optimal_encode(h))

In other words, the MAP hypothesis is the one that minimises the sum of two description lengths: that of the hypothesis itself, and that of the given data, assuming that hypothesis to be the correct one.  This can be seen as a mathematical interpretation of Occam's razor.




