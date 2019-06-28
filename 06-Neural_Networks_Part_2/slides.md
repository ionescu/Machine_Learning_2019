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

```Ω, Event, p```{.haskell} and ```X```{.haskell} as above.  Consider the following experiment: ```n```{.haskell}  elements ```e₁, ..., eₙ```{.haskell} are drawn independently from ```Ω```{.haskell} and we compute the mean value of ```X```{.haskell} for this sample ```μₛ = (X(e₁) + ... + X(eₙ))/n```{.haskell}.  Then ```μₛ```{.haskell} is a random variable whose probability distribution approaches with increasing ```n```{.haskell} the normal distribution with mean ```μ```{.haskell} and standard deviation ```σ/√(n)```{.haskell}.

The theorem states that ```μₛ```{.haskell} is a random variable.  But a random variable is a function defined in the context of a probability space.  What is that probability space here?  You will need to specify ```Ω', Event', p' : Event' → [0, 1]```{.haskell} and define ```μₛ : Ω' → ℝ```{.haskell} as a function in terms of in terms of the given ```Omega, Event, p```{.haskell}, and ```X```{.haskell}.

Solution to homework from lecture 4
===================================

> Ω' = (Ω, ..., Ω) = Ωⁿ

> Event' = ℙ (Ω')

> p'(e₁, ..., eₙ) = p(e₁) * ... * p(eₙ)

> μₛ(e₁, ..., eₙ) = (X(e₁) + ... + X(eₙ))/n

Bayesian learning and neural networks
=====================================
Given data of the form

> d = {(x₁, c(x₁)), ..., (xₘ, c(xₘ))}

where ```c```{.haskell} is an unknown function, Bayesian learning attempts to find the most probable hypothesis ```h ∈ H```{.haskell} that can be used to determine ```c```{.haskell}.

When using neural networks, ```h```{.haskell} represents an assignment of the weights and tresholds of the neurons.

Bayesian learning and neural networks
=====================================

It can be shown (see slides of Lecture 5) that the most probable hypothesis is, under common circumstances, the one that minimises the *error* (or *loss*) function

> E (w) = Σ (c(xᵢ) - f w (xᵢ))²

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

where we use ```Dᵢ f (x)```{.haskell} instead of the more frequent ```∂ f (x) / ∂ xᵢ```{.haskell}

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

This procedure is a variant of *gradient descent*.  In "real" gradient descent, the step size ```η```{.haskell} is chosen to minimise

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
>  where s     =  w₁*x₁ + ... + wₙ * xₙ - θ
>        σ(y)  =  1 / 1 + exp(-y)

Note that this scales the output to ```[0, 1]```{.haskell}.

> σ'(y) = σ(y) * (1 - σ(y))

Gradient descent and perceptrons
================================

More generally

> perceptron ([w₁,..., wₙ], θ) [x₁, ..., xₙ] = f(s)
>  where s     =  w₁*x₁ + ... + wₙ * xₙ - θ
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

> D₃ E(ws, vs) = D₃ (t - per ws [y₁(v₁₁, v₂₁), y₂(v₁₂, v₂₂)])²
>              = D₃ E(ws, y₁, y₂) * D₁ y₁ 
>              = D₃ E(ws, y₁, y₂) * D₁ f(z)
>              = D₃ E(ws, y₁, y₂) * f'(z) * D₁ z
>              = D₃ E(ws, y₁, y₂) * f'(z) * x₁

Homework
========

Consider a two-layer perceptron as in Figure 1, with ```f(y) = y, f'(y) = 1```{.haskell} (such a perceptron is called *linear*).  Suppose we start with

> v₁₁ = 1, v₁₂ = -1, v₂₁ = -1, v₂₂ = 1, w₁ = 1, w₂ = 2

Assume the data is ```([-1, 1], 0)```{.haskell}.  How does the backpropagation algorithm adjust ```v₁₁```{.haskell}?

Other architectures
===================

- recurrent neural networks (Hopfield 1982, Schmidhubber 1992, others in between)

- convolutional neural networks (partly already Rumelhart et al. 1986)

- capsule networks (Hinton 2017)


Recurrent neural networks
=========================

-  Neurons have a state which is fed back to the input.

-  This is an implementation of memory.

-  RNNs could, in theory, implement any computable function, but in practice are limited.

-  Gradient descent problematic (``vanishing gradient''), leading to LSTM (``Long Short-Term Memory'').

Convolutional neural networks
=============================

-  Architecture addresses two important problems with traditional feed-forward networks:

  - loss of local or temporal order in inputs

  - combinatorial explosion of weights

Loss of spatial information in perceptrons
==========================================

!["Linearisation" of an image](conv1.png)

Filters
=======

![Decomposing an image](conv2.png){ width=80% }

Overlapping filters: stride
===========================

![An overlapping decomposition](conv3.png)

Convolutional neural networks
=============================

![Convolutional neural network [^1]](cnn.png)


[^1]: Aphex34 \url{https://commons.wikimedia.org/wiki/File:Typical_cnn.png}, \url{https://creativecommons.org/licenses/by-sa/4.0/legalcode}}


The surprising success of "deep" networks
=========================================

-  Two layers of weights are sufficient, but not efficient

-  Surprisingly, *many* layers turn out to be more efficient

-  Pseudo-explanation (Hinton 2005): layers select increasingly  high-level functions

-  A different pseudo-explanation: neurons similar to logic gates.

Training deep networks
======================

-  The problem with many layers is that training is hard (backprop is NP-complete).

  - CNNs used to decrease the numbers of weights

  - denoising used to extract ``features'' (compress input space)

Pros and cons of deep neural networks
=====================================

- Pros
 
  - very successful (handwriting recognition, speech recognition, automatic translation, policy functions for reinforcement learning, etc.)

- Cons
 
  - Very opaque

  - Beyond metaphors, not clear why they work (two layers should, e.g., generalise better)

Acknowledgements
================

Figures on slides *Loss of spatial information*, *Filters*, and *Overlapping filters* by Farah Shamout (Balliol College, Oxford).
