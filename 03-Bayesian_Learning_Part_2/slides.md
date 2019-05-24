---
theme: boxes
theme: Madrid
mainfont: TeX Gyre Pagella
monofont: DejaVuSansMono
header-includes: \usepackage{tikz}
---

\title{Lecture 3}
\author{Cezar Ionescu}
\date{25/05/2019}
\titlegraphic{\includegraphics[scale=0.4]{oudce.jpg}}
\maketitle

Administrative
==============

**No lecture on the 1st of June!**

Homework from 2019-05-18 due **now**!

Questions?
==========

Joint probability distributions
===============================

Revisiting the diagnostic problem of the previous lecture:

- ```H =  {healthy, ill}```{.haskell}

- test results: ```R = {⊖, ⊕}```{.haskell}

- ```0.008```{.haskell} of the population have the disease.

- patient ill ```⇒```{.haskell} test positive in ```98%```{.haskell} of the cases  

- patient healthy ```⇒```{.haskell} test positive in ```3%```{.haskell} of the cases  

Joint probability distributions
===============================

> Ω = (H, R)
>   = {(healthy, ⊖), (healthy, ⊕), (ill, ⊖), (ill, ⊕)

. . .

> Healthy = {(healthy, ⊖), (healthy, ⊕)}

. . .

> pₕ : ℙ(H) → [0, 1]
> pₕ (healthy) = p({(healthy, ⊖), (healthy, ⊕)})
>              = p (healthy, ⊖) + p (healthy, ⊕)

Joint probability distributions
===============================

**Notation**:

- for ```x ∈ Ω```{.haskell} ```p(x)```{.haskell} is an abbreviation for ```p({x})```{.haskell}.  

- similarly ```p(healthy, ⊖)```{.haskell} denotes ```p({(healthy, ⊖)})```{.haskell}

- ```p(healthy)```{.haskell} denotes ```p({(healthy, r) | r ∈ R})```{.haskell}


**Remark**: in general, if ```Ω = (Ω₁, Ω₂, ..., Ωₙ)```{.haskell} and ```ωᵢ ∈ Ωᵢ```{.haskell}, we will use ```p(ω_i)```{.haskell} to denote

> p(ωᵢ) = p({(x₁, x₂, ..., xₙ) | x₁ ∈ Ω₁, ..., xₙ ∈ Ωₙ, xᵢ = ωᵢ})

Joint probability distributions
===============================

- the "complete" probability measure ```p```{.haskell} is called the **joint probability distribution**

- the probability measures over the individual components (or over tuples of individual components) are called **marginal probability distributions**

**Remark**: it is trivial to derive the marginal probability distributions, given the joint probability distribution.  However, even if we have all the marginal distributions, we cannot derive the joint distribution from them.

Joint probability distributions
===============================

If ```Ω```{.haskell} consists of a tuple of ```n```{.haskell} boolean components, then the joint probability distribution can be represented as a table with ```2ⁿ```{.haskell} rows.

Joint probability distributions
===============================

Let ```Ω = (Ω₁, Ω₂, ..., Ωₙ)```{.haskell} and ```(ω₁, ω₂, ..., ωₙ) ∈ Ω```{.haskell}.  Then

> p(ω₁, ω₂, ..., ωₙ)  =  p(ω₁ ∩  ω₂ ∩  ... ∩ ωₙ)
>                     =  p(ω₁ | ω₂ ∩  ... ∩ ωₙ)*p(ω₂ ∩  ... ∩ ωₙ)

It is easy to see that, therefore:

> p(ω₁, ω₂, ..., ωₙ)  =  p(ω₁ |  ω₂ ∩  ... ∩ ωₙ) * 
>                        p(ω₂ | ω₃ ∩ ... ∩ ωₙ) * ...
>                        p(ωₙ)

Independence
============

**Definition**: Two events ```X, Y ⊆ Ω```{.haskell} are called **independent** if 

> p(X ∩ Y) = p(X) * p(Y)

Examples
========

**Example**:  Consider a fair standard die, and the events ```even = {2, 4, 6}```{.haskell}, ```big = {5, 6}```{.haskell}.  Then ```even```{.haskell} and ```big```{.haskell} are independent.

**Exercise**: If ```X, Y```{.haskell} are independent, then so are ```X, ¬Y```{.haskell}.

**Example**: The events ```big```{.haskell} and ```divBy3 = {3, 6}```{.haskell} are **not** independent.

Conditional independence
========================

**Definition**: Consider events ```X, Y, Z ⊆ Ω```{.haskell} such that ```p(Z) ≠ 0```{.haskell}.  Events ```X, Y```{.haskell} are called **conditionally independent given** ```Z```{.haskell} if

> p(X ∩ Y | Z) = P(X | Z) * p(Y | Z)

Examples
========

**Example**: 

- The events ```even, big```{.haskell} are conditionally dependent given ```divBy3```{.haskell}.

- The events ```divBy3, big```{.haskell} are conditionally independent given ```{2, 3, 5, 6}```{.haskell}.

Bayesian networks
=================

![](forestfire.png)

Bayesian networks
=================

>   p(thunder, forestfire, campfire, lightning, storm, bustourgroup)
> =
>   p(thunder | forestfire ∩ campfire ∩ lightning ∩ storm ∩ bustourgroup) * 
>   p(forestfire ∩ campfire ∩ lightning ∩ storm ∩ bustourgroup)
> =
>   p(thunder | lightning) * 
>   p(forestfire ∩ campfire ∩ lightning ∩ storm ∩ bustourgroup)
> =
>   p(thunder | lightning) * 
>   p(forestfire | campfire ∩ lightning) *
>   p(campfire | storm ∩ bustourgroup) *
>   p(lightning | storm) *
>   p(storm) * p(bustourgroup)

Homework
========

(based on Nilsson, 19.4, page 340) Consider the following Bayesian network:

![](burglary.png){ height=55% }

Compute ```p(¬J, ¬M, A, B, E)```{.haskell}.  This is the probability that there is both an earthquake and a burglary, the alarm rings, but neither John nor Mary call.

Exercise
========

**Exercise**: Compute ```p(¬J, ¬M, B, E)```{.haskell} (this is exercise 19.4 in Nielsson).
