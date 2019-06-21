---
theme: boxes
theme: Madrid
mainfont: TeX Gyre Pagella
monofont: DejaVuSansMono
header-includes: \usepackage{tikz}
---

\title{Lecture 6}
\author{Cezar Ionescu \\ 22/06/2019}
\titlegraphic{\includegraphics[scale=0.3]{oudce.jpg}}
\maketitle

Administrative
==============

- Homework from 15/06/2019 due now!

- Please complete and hand in the declarations of authorship.

Questions?
==========

Solution to homework from lecture 4
===================================

Bayesian learning and neural networks
=====================================
Given data of the form

> d = {(x₁, c(x₁)), ..., (xₘ, c(xₘ))}

where ```c```{.haskell} is an unknown function, Bayesian learning attempts to find the most probable hypothesis ```h ∈ H```{.haskell} that can be used to determine ```c```{.haskell}.

When using neural networks, ```h```{.haskell} represents an assignment of the weights and tresholds of the neurons.

Bayesian learning and neural networks
=====================================

It can be shown (see slides of Lecture 5) that the most probable hypothesis is, under common circumstances, the one that minimises the *error* (or *loss*) function

> E (w) = Σ (xᵢ - f w (xᵢ))²

where ```f : Weights -> X -> ℝ```{.haskell} is the function implemented by the network.

Hill climbing
=============

Consider the following problem: given a function 

> f : ([-5, 5], [-5, 5]) -> ℝ

find ```x, y ∈ [-5, 5]```{.haskell} such that ```f(x, y)```{.haskell} is minimal.

We could, e.g., try some values:

> f(1, 1) = 0.0
> f(2, 1) = 0.0
> f(3, 1) = 2.0
> f(3, 2) = -2.0

Hill climbing
=============

We need a way of exploring the domain of ```f```{.haskell} more systematically.  We can, for example, start in ```(0, 0)```{.haskell} and test the corners of the square of side ```1```{.haskell} centered in the origin:

> f(0, 0)       = 5
> f(-0.5, -0.5) = 8.25
> f(-0.5, 0.5)  = 4.75
> f(0.5, 0.5)   = 2.25
> f(0.5, -0.5)  = 6.75

We can now use ```(0.5, 0.5)```{.haskell} as a starting point and continue, perhaps making the side of the square smaller (e.g., ```0.9```{.haskell} instead of ```1```{.haskell}.

Hill climbing and neural networks
=================================

Attempts to use hill climbing to minimise ```E```{.haskell} are usually unsuccessful.  That is because the space to be explored is exponential in the number of parameters (for two dimensions we have ```4```{.haskell} points, for three dimensions ```8```{.haskell}, for ```n```{.haskell} dimensions ```2ⁿ```{.haskell} points).

We need a "guide" to point us in the right direction.

Gradient
========

For a function ```f : X -> ℝ```{.haskell} where ```X ⊆ ℝⁿ```{.haskell}, the gradient (if it exists) is the vector

> ∇ f (x) = [D₁ f (x), ..., Dₙ f (x)]

where we use ```Dᵢ f (x)```{.haskell} instead of the more frequent ```δ f (x) / δ xᵢ```{.haskell}

```∇ f (x)```{.haskell} points towards the direction of steepest increase in ```f```{.haskell} at ```x```{.haskell}.

Gradient descent
================

If we have information about the gradient of ```f```{.haskell}, then we can do **much** better than hill climbing:

> f(0, 0) = 5
> gradf(0, 0) = [-2, -4]

So if we move in the direction ```[-2, -4]```{.haskell} we should see an increase in ```f```{.haskell}; if we move in the opposite direction, a decrease:

> f(-2, -4) = 37
> f(2, 4)   = -3

We now move to ```(2, 4)```{.haskell} and start again, perhaps choosing a different step size (e.g., ```0.9```{.haskell}).

This procedure is a variant of *gradient descent*.  In "real" gradient descent, the step size ```η```{.haskell}is chosen to minimise

> f(x - η * D₁ f (x, y), y - η * D₂ f (x, y))

Revisiting perceptrons
======================

> perceptron : (ℝⁿ, ℝ) -> ℝⁿ -> {-1, 1}
> perceptron ([w₁,..., wₙ], θ) [x₁, ..., xₙ] = if s ≥ θ then 1 
>                                                      else -1
>  where s = w₁*x₁ + ... + wₙ * xₙ

We are given

> d = {(x₁, c(x₁)), ..., (xₘ, c(xₘ))}

Can we use gradient descent to determine ```wᵢ```{.haskell} and ```θ```{.haskell}?

Gradient descent and perceptrons
================================

We can compute ```E```{.haskell}, but the problem is that it is not differentiable w.r.t. ```wᵢ```{.haskell}.

Because ```perceptron ([w₁,..., wₙ], θ) : ℝⁿ -> {-1, 1}```{.haskell} is **discontinuous**.

We can approximate the step function required by ```perceptron```{.haskell} with a differentiable function.  For example:

> perceptron ([w₁,..., wₙ], θ) [x₁, ..., xₙ] = σ(s)
>  where s     =  w₁*x₁ + ... + wₙ * xₙ
>        σ(y)  =  1 / 1 + exp(-y)

Note that this scales the output to ```[0, 1]```{.haskell}.

> σ'(y) = σ(y) * (1 - σ(y))

Gradient descent and perceptrons
================================

More generally

> perceptron ([w₁,..., wₙ], θ) [x₁, ..., xₙ] = f(s)
>  where s     =  w₁*x₁ + ... + wₙ * xₙ
>        f(y)  =  ...

where ```f```{.haskell} is some nicely differentiable function.

Gradient descent and perceptrons
================================

In the following, we consider a perceptron with no ```θ```{.haskell}, since

> perceptron ([w₁,..., wₙ], θ) [x₁, ..., xₙ] = 
> perceptron ([w₁,..., wₙ, θ]) [x₁, ..., xₙ, 1]

and abbreviate

> ws = [w₁,..., wₙ]

Consider ```m = 1```{.haskell} (only one element in the data), so that

> E(ws) = (c(x) - perceptron ws (x))²
>       = (t - per ws (x))²

Gradient descent and perceptrons
================================

We can apply gradient descent if we can compute ```Dᵢ E (wᵢ)```{.haskell}.  This is easy:

> Dᵢ E (ws)  =  Dᵢ (t - per ws (x))²
>            =  Dᵢ (t - f(s))²
>            =  2 * (t - f(s)) * Dᵢ (t - f(s))
>            =  -2 * (t - f(s)) * f'(s) * Dᵢ s 
>            =  -2 * (t - f(s)) * f'(s) * xᵢ 

Notice the similarity with the learning rule from the last lecture.

Stochastic gradient descent
===========================

In practice, ```m > 1```{.haskell}.  

"Proper" gradient descent demands the computation of ```E```{.haskell}.

However, this would be too time-consuming, and usually an approximate ```E```{.haskell} is computed instead, using a "batch" size between ```1```{.haskell} and ```m```{.haskell}.  This approximate version is known as *stochastic gradient descent*.

More than one layer
===================

What if we have more than one layer?  Consider a simple example:

![Two layer network](two_layers.jpg){ height=70% }


More than one layer
===================

We can compute the error as before

> E(ws, vs) = (t - per ws [y₁, y₂])²
>   where y₁ = per vs₁ [x₁, x₂]
>         y₂ = per vs₂ [x₁, x₂]
>         vs₁ = [v₁₁, v₂₁]
>         vs₂ = [v₁₂, v₂₂]

> D₁ E(ws, vs) =  -2 * (t - f(s)) * f'(s) * y₁ 

But how can we compute, e.g., ```D₃ E(ws, vs)```{.haskell}?

Backpropagation
===============

> D₃ E(ws, vs) = D₃ (t - per ws [y₁, y₂])²
>              = D₁ E(ws, vs) * D₁ y₁ 
>              = D₁ E(ws, vs) * D₁ f(z)
>              = D₁ E(ws, vs) * f'(z) * D₁ z
>              = D₁ E(ws, vs) * f'(z) * x₁

Homework
========

Consider a two-layer perceptron as in Figure 1, with ```f(y) = y```{.haskell} (such a perceptron is called *linear*).  Suppose we start with

> v₁₁ = 1, v₁₂ = -1, v₂₁ = -1, v₂₂ = 1, w₁ = 1, w₂ = 2

Assume the data is ```([-1, 1], 0)```{.haskell}.  How does the backpropagation algorithm adjust ```v₁₁```{.haskell}?
