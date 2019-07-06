---
theme: boxes
theme: Madrid
mainfont: TeX Gyre Pagella
monofont: DejaVuSansMono
header-includes: \usepackage{tikz}
---

\title{Lecture 8}
\author{Cezar Ionescu \\ 06/07/2019}
\titlegraphic{\includegraphics[scale=0.3]{oudce.jpg}}
\maketitle

Administrative
==============

- Homework from 29/06/2019 due now!

- Please complete and hand in the declarations of authorship.

Questions?
==========

Solution to homework from lecture 7
===================================

> y₁(v₁₁, v₂₁) = f(x₁*v₁₁ + x₂*v₂₁)    = -2
> y₂(v₁₁, v₂₁) = f(x₁*v₁₂ + x₂*v₂₂)    =  2
> o(w₁, w₂, y₁, y₂) = f(y₁*w₁ + y₂*w₂) =  2
> E (w₁, w₂, v₁₁, ...) = (t - o)²

> ∂ E / ∂ v₁₁ = E'(o) * ∂ o / ∂ v₁₁
>             = 2*(t - o)*(-1) * ∂ o / ∂ v₁₁
>             = 2*(-2)*(-1)*f'(z)*∂ g / ∂ v₁₁
>             = 4*1*∂ (y₁*w₁ + y₂*w₂) / ∂ v₁₁
>             = 4 * ∂ (y₁*w₁ + y₂*w₂) / ∂ y₁ * ∂ y₁ / ∂ v₁₁
>             = 4 * w₁ * f'(z) * ∂ (x₁*v₁₁ + x₂*v₂₁) / ∂ v₁₁
>             = 4 * x₁ = -4



\      
===================================

  \begin{center}
    \begin{Large}
      {\bf Dynamic programming}
    \end{Large}
  \end{center}

A maze
======

  \begin{center}
    \begin{tikzpicture}
      \draw (0, 0) grid (6, 6);
      \fill (1, 4) rectangle (2, 5);
      \fill (1, 3) rectangle (2, 4);
      \fill (1, 2) rectangle (2, 3);
      \fill (2, 1) rectangle (3, 2);
      \fill (3, 1) rectangle (4, 2);
      \fill (3, 3) rectangle (4, 4);
      \fill (4, 3) rectangle (5, 4);

      \fill[orange] (4, 4) rectangle (5, 5);

      \draw (0.5, 0.5) circle [radius=0.5];

    \end{tikzpicture}
  \end{center}

Value map
=========

  \begin{center}
    \begin{tikzpicture}
      \draw (0, 0) grid (6, 6);
      \fill (1, 4) rectangle (2, 5);
      \fill (1, 3) rectangle (2, 4);
      \fill (1, 2) rectangle (2, 3);
      \fill (2, 1) rectangle (3, 2);
      \fill (3, 1) rectangle (4, 2);
      \fill (3, 3) rectangle (4, 4);
      \fill (4, 3) rectangle (5, 4);

      \fill[orange] (4, 4) rectangle (5, 5);
      \node at (4.5, 4.5) {1};
      \node at (3.5, 4.5) {0};
      \node at (5.5, 4.5) {0};
      \node at (2.5, 4.5) {-1};
      \node at (4.5, 5.5) {0};
      \node at (5.5, 5.5) {-1};
      \node at (3.5, 5.5) {-1};
      \node at (5.5, 3.5) {-1};
      \node at (2.5, 5.5) {-2};
      \node at (1.5, 5.5) {-3};
      \node at (0.5, 5.5) {-4};
      \node at (5.5, 2.5) {-2};
      \node at (5.5, 1.5) {-3};
      \node at (5.5, 0.5) {-4};
      \node at (2.5, 3.5) {-2};
      \node at (2.5, 2.5) {-3};
      \node at (4.5, 2.5) {-3};
      \node at (3.5, 2.5) {-4};
      \node at (4.5, 1.5) {-4};
      \node at (4.5, 0.5) {-5};
      \node at (0.5, 4.5) {-5};
      \node at (0.5, 3.5) {-6};
      \node at (0.5, 2.5) {-7};
      \node at (0.5, 1.5) {-8};
      \node at (0.5, 0.5) {-9};
      \node at (1.5, 0.5) {-8};
      \node at (2.5, 0.5) {-7};
      \node at (3.5, 0.5) {-6};
      \node at (1.5, 1.5) {-9};
    \end{tikzpicture}
  \end{center}

Dynamic programming
===================

  \begin{center}
    \begin{tikzpicture}
      \draw (0, 0) grid (6, 6);
      \fill (1, 4) rectangle (2, 5);
      \fill (1, 3) rectangle (2, 4);
      \fill (1, 2) rectangle (2, 3);
      \fill (2, 1) rectangle (3, 2);
      \fill (3, 1) rectangle (4, 2);
      \fill (3, 3) rectangle (4, 4);
      \fill (4, 3) rectangle (5, 4);

      \fill[orange] (4, 4) rectangle (5, 5);\pause
      \node at (4.5, 4.5) {1};\pause
      \node at (3.5, 4.5) {0};
      \node at (5.5, 4.5) {0};
      \node at (4.5, 5.5) {0};\pause
      \node at (2.5, 4.5) {-1};
      \node at (5.5, 5.5) {-1};
      \node at (3.5, 5.5) {-1};
      \node at (5.5, 3.5) {-1};\pause
      \node at (2.5, 5.5) {-2};
      \node at (2.5, 3.5) {-2};
      \node at (5.5, 2.5) {-2};\pause            
      \node at (1.5, 5.5) {-3};
      \node at (2.5, 2.5) {-3};
      \node at (4.5, 2.5) {-3};
      \node at (5.5, 1.5) {-3};\pause
      \node at (0.5, 5.5) {-4};
      \node at (5.5, 0.5) {-4};
      \node at (3.5, 2.5) {-4};
      \node at (4.5, 1.5) {-4};\pause
      \node at (4.5, 0.5) {-5};
      \node at (0.5, 4.5) {-5};\pause
      \node at (0.5, 3.5) {-6};
      \node at (3.5, 0.5) {-6};\pause
      \node at (0.5, 2.5) {-7};
      \node at (2.5, 0.5) {-7};\pause
      \node at (0.5, 1.5) {-8};
      \node at (1.5, 0.5) {-8};\pause
      \node at (0.5, 0.5) {-9};
      \node at (1.5, 1.5) {-9};
    \end{tikzpicture}
  \end{center}

Optimal policy
==============

If we have the optimal value map, defining the optimal policy is easy:


 > Choose the action that results in the greatest sum of reward now and
 > value in the next state.

Choose ```pol(s)```{.haskell} that maximises

> Reward(s, a) + Value(sys(s, a))

where ```sys : (State, Action) ~> State```{.haskell} is the function that tells us what happens depending on the current state and chosen action.

Review
======

Ingredients:

> sys : (State, Action) -> State
> rew : (State, Action) -> ℝ
> pol : State -> Action
> s₀ ∈ State -- given

Problem: Find ```pol```{.haskell} that maximises ```Σ₀ⁿ rew(sᵢ, pol(sᵢ))```{.haskell}, where

```s```{.haskell}$_{\tiny{\verb|i+1|}}$ ```= sys(sᵢ, pol(sᵢ))```{.haskell}

Value function
==============

> Val : ℕ -> State -> ℝ

> Valᵢ(s) = maxₚₒₗ Σᵢⁿ rew(sᵢ, pol(sᵢ))

Note:

```Val₀(s₀)```{.haskell} is the maximal value for the entire problem.

Bellman equation
================

If ```pol(s) = aᵒᵖᵗ```{.haskell} the optimal action in state ```s```{.haskell}, then \

```Valᵢ(s) = Rew(s, aᵒᵖᵗ) + Val```{.haskell}$_{\tiny{\verb|i+1|}}$```(sys(s, aᵒᵖᵗ))```{.haskell}

Therefore 

```Valᵢ(s) = maxₐ (Rew(s, a) + Val```{.haskell}$_{\tiny{\verb|i+1|}}$```(sys(s, a)))```{.haskell}

This is called *Bellman's equation*.


\ 
========

  \begin{center}
    \begin{Large}
      {\bf Direct adaptive optimal control}
    \end{Large}
  \end{center}

Model of the problem
====================

If we do not know the exact map of the maze, or the location of the
goal, we cannot apply dynamic programming. We need an *adaptive method*.
Alternatives:

-   explore the maze and make a map of it, find the goal, and then apply
    dynamic programming (*indirect* method);

-   learn the optimal value map directly! (*direct* method).

Reinforcement learning is *direct adaptive optimal control*.

\ 
=====

  \begin{center}
    \begin{Large}
      {\bf Value iteration}
    \end{Large}
  \end{center}

Start with a randomly generated value map
=========================================

  \begin{center}
    \begin{tikzpicture}
      \draw (0, 0) grid (6, 6);
      \fill (1, 4) rectangle (2, 5);
      \fill (1, 3) rectangle (2, 4);
      \fill (1, 2) rectangle (2, 3);
      \fill (2, 1) rectangle (3, 2);
      \fill (3, 1) rectangle (4, 2);
      \fill (3, 3) rectangle (4, 4);
      \fill (4, 3) rectangle (5, 4);

      \fill[orange] (4, 4) rectangle (5, 5);\pause
      \node at (1.5, 1.5) {0};
      \node at (1.5, 2.5) {0};
      \node at (1.5, 3.5) {0};

      \node at (4.5, 4.5) {0};
      \node at (3.5, 4.5) {0};
      \node at (5.5, 4.5) {0};
      \node at (4.5, 5.5) {0};
      \node at (2.5, 4.5) {0};
      \node at (5.5, 5.5) {0};
      \node at (3.5, 5.5) {0};
      \node at (5.5, 3.5) {0};
      \node at (2.5, 5.5) {0};
      \node at (2.5, 3.5) {0};
      \node at (5.5, 2.5) {0};
      \node at (1.5, 5.5) {0};
      \node at (2.5, 2.5) {0};
      \node at (4.5, 2.5) {0};
      \node at (5.5, 1.5) {0};
      \node at (0.5, 5.5) {0};
      \node at (5.5, 0.5) {0};
      \node at (3.5, 2.5) {0};
      \node at (4.5, 1.5) {0};
      \node at (4.5, 0.5) {0};
      \node at (0.5, 4.5) {0};
      \node at (0.5, 3.5) {0};
      \node at (3.5, 0.5) {0};
      \node at (0.5, 2.5) {0};
      \node at (2.5, 0.5) {0};
      \node at (0.5, 1.5) {0};
      \node at (1.5, 0.5) {0};
      \node at (0.5, 0.5) {0};
      \node at (1.5, 1.5) {0};
    \end{tikzpicture}
  \end{center}

Alternatives
============

  \begin{center}
    \begin{tikzpicture}
      \draw (0, 0) grid (6, 6);
      \fill (1, 4) rectangle (2, 5);
      \fill (1, 3) rectangle (2, 4);
      \fill (1, 2) rectangle (2, 3);
      \fill (2, 1) rectangle (3, 2);
      \fill (3, 1) rectangle (4, 2);
      \fill (3, 3) rectangle (4, 4);
      \fill (4, 3) rectangle (5, 4);

      \fill[orange] (4, 4) rectangle (5, 5);
      \node at (4.5, 4.5) {0};
      \node at (3.5, 4.5) {0};
      \node at (5.5, 4.5) {0};
      \node at (4.5, 5.5) {0};
      \node at (2.5, 4.5) {0};
      \node at (5.5, 5.5) {0};
      \node at (3.5, 5.5) {0};
      \node at (5.5, 3.5) {0};
      \node at (2.5, 5.5) {0};
      \node at (2.5, 3.5) {0};
      \node at (5.5, 2.5) {0};
      \node at (1.5, 5.5) {0};
      \node at (2.5, 2.5) {0};
      \node at (4.5, 2.5) {0};
      \node at (5.5, 1.5) {0};
      \node at (0.5, 5.5) {0};
      \node at (5.5, 0.5) {0};
      \node at (3.5, 2.5) {0};
      \node at (4.5, 1.5) {0};
      \node at (4.5, 0.5) {0};
      \node at (0.5, 4.5) {0};
      \node at (0.5, 3.5) {0};
      \node at (3.5, 0.5) {0};
      \node at (0.5, 2.5) {0};
      \node at (2.5, 0.5) {0};
      \node at (0.5, 1.5) {0};
      \node at (1.5, 0.5) {0};
      \node at (0.5, 0.5) {0};
      \node at (1.5, 1.5) {0};

      \draw[fill=blue,fill opacity=0.5] (2.5, 5.5) circle [radius=0.5];\pause
      \draw [->, red, ultra thick] (2.5, 5.5) -- (3.5, 5.5);
      \draw [->, red, ultra thick] (2.5, 5.5) -- (1.5, 5.5);
      \draw [->, red, ultra thick] (2.5, 5.5) -- (2.5, 4.5);
    \end{tikzpicture}
  \end{center}

Choose an action
================

  \begin{center}
    \begin{tikzpicture}
      \draw (0, 0) grid (6, 6);
      \fill (1, 4) rectangle (2, 5);
      \fill (1, 3) rectangle (2, 4);
      \fill (1, 2) rectangle (2, 3);
      \fill (2, 1) rectangle (3, 2);
      \fill (3, 1) rectangle (4, 2);
      \fill (3, 3) rectangle (4, 4);
      \fill (4, 3) rectangle (5, 4);

      \fill[orange] (4, 4) rectangle (5, 5);
      \node at (4.5, 4.5) {0};
      \node at (3.5, 4.5) {0};
      \node at (5.5, 4.5) {0};
      \node at (4.5, 5.5) {0};
      \node at (2.5, 4.5) {0};
      \node at (5.5, 5.5) {0};
      \node at (3.5, 5.5) {0};
      \node at (5.5, 3.5) {0};
      \node at (2.5, 5.5) {0};
      \node at (2.5, 3.5) {0};
      \node at (5.5, 2.5) {0};
      \node at (1.5, 5.5) {0};
      \node at (2.5, 2.5) {0};
      \node at (4.5, 2.5) {0};
      \node at (5.5, 1.5) {0};
      \node at (0.5, 5.5) {0};
      \node at (5.5, 0.5) {0};
      \node at (3.5, 2.5) {0};
      \node at (4.5, 1.5) {0};
      \node at (4.5, 0.5) {0};
      \node at (0.5, 4.5) {0};
      \node at (0.5, 3.5) {0};
      \node at (3.5, 0.5) {0};
      \node at (0.5, 2.5) {0};
      \node at (2.5, 0.5) {0};
      \node at (0.5, 1.5) {0};
      \node at (1.5, 0.5) {0};
      \node at (0.5, 0.5) {0};
      \node at (1.5, 1.5) {0};

      \draw[fill=blue,fill opacity=0.5] (2.5, 5.5) circle [radius=0.5];
      \draw [->, red, ultra thick] (2.5, 5.5) -- (2.5, 4.5);
    \end{tikzpicture}
  \end{center}

Make a move
===========

  \begin{center}
    \begin{tikzpicture}
      \draw (0, 0) grid (6, 6);
      \fill (1, 4) rectangle (2, 5);
      \fill (1, 3) rectangle (2, 4);
      \fill (1, 2) rectangle (2, 3);
      \fill (2, 1) rectangle (3, 2);
      \fill (3, 1) rectangle (4, 2);
      \fill (3, 3) rectangle (4, 4);
      \fill (4, 3) rectangle (5, 4);

      \fill[orange] (4, 4) rectangle (5, 5);
      \node at (4.5, 4.5) {0};
      \node at (3.5, 4.5) {0};
      \node at (5.5, 4.5) {0};
      \node at (4.5, 5.5) {0};
      \node at (2.5, 4.5) {0};
      \node at (5.5, 5.5) {0};
      \node at (3.5, 5.5) {0};
      \node at (5.5, 3.5) {0};
      \node at (2.5, 5.5) {0};
      \node at (2.5, 3.5) {0};
      \node at (5.5, 2.5) {0};
      \node at (1.5, 5.5) {0};
      \node at (2.5, 2.5) {0};
      \node at (4.5, 2.5) {0};
      \node at (5.5, 1.5) {0};
      \node at (0.5, 5.5) {0};
      \node at (5.5, 0.5) {0};
      \node at (3.5, 2.5) {0};
      \node at (4.5, 1.5) {0};
      \node at (4.5, 0.5) {0};
      \node at (0.5, 4.5) {0};
      \node at (0.5, 3.5) {0};
      \node at (3.5, 0.5) {0};
      \node at (0.5, 2.5) {0};
      \node at (2.5, 0.5) {0};
      \node at (0.5, 1.5) {0};
      \node at (1.5, 0.5) {0};
      \node at (0.5, 0.5) {0};
      \node at (1.5, 1.5) {0};

      \draw[fill=blue,fill opacity=0.5] (2.5, 4.5) circle [radius=0.5];
    \end{tikzpicture}
  \end{center}

Changing the current value function
===================================

If we had the optimal value function, then

> Val((3,6)) = Rew((3,6), ↓) + Val((3,5))

But we have

> Val((3, 6)) = 0,  Rew((3,6), ↓) = -1, Val((3,5)) = 0

Update rule
===========

Idea from *supervised learning*: 

> Val((3,6)) <- Val((3,6)) + 
>               δ * (Rew((3,6), ↓) + Val((3,5)) - Val((3,6))) 

Therefore (say ```δ = 0.5```{.haskell})

> Val(3,6) <- 0 + 0.5 * (-1 + 0 - 0) = -0.5

The new situation
=================

  \begin{center}
    \begin{tikzpicture}
      \draw (0, 0) grid (6, 6);
      \fill (1, 4) rectangle (2, 5);
      \fill (1, 3) rectangle (2, 4);
      \fill (1, 2) rectangle (2, 3);
      \fill (2, 1) rectangle (3, 2);
      \fill (3, 1) rectangle (4, 2);
      \fill (3, 3) rectangle (4, 4);
      \fill (4, 3) rectangle (5, 4);

      \fill[orange] (4, 4) rectangle (5, 5);
      \node at (4.5, 4.5) {0};
      \node at (3.5, 4.5) {0};
      \node at (5.5, 4.5) {0};
      \node at (4.5, 5.5) {0};
      \node at (2.5, 4.5) {0};
      \node at (5.5, 5.5) {0};
      \node at (3.5, 5.5) {0};
      \node at (5.5, 3.5) {0};
      \node at (2.5, 5.5) {-0.5};
      \node at (2.5, 3.5) {0};
      \node at (5.5, 2.5) {0};
      \node at (1.5, 5.5) {0};
      \node at (2.5, 2.5) {0};
      \node at (4.5, 2.5) {0};
      \node at (5.5, 1.5) {0};
      \node at (0.5, 5.5) {0};
      \node at (5.5, 0.5) {0};
      \node at (3.5, 2.5) {0};
      \node at (4.5, 1.5) {0};
      \node at (4.5, 0.5) {0};
      \node at (0.5, 4.5) {0};
      \node at (0.5, 3.5) {0};
      \node at (3.5, 0.5) {0};
      \node at (0.5, 2.5) {0};
      \node at (2.5, 0.5) {0};
      \node at (0.5, 1.5) {0};
      \node at (1.5, 0.5) {0};
      \node at (0.5, 0.5) {0};
      \node at (1.5, 1.5) {0};

      \draw[fill=blue,fill opacity=0.5] (2.5, 4.5) circle [radius=0.5];\pause
      \draw [->, red, ultra thick] (2.5, 4.5) -- (1.5, 4.5);
      \draw [->, red, ultra thick] (2.5, 4.5) -- (3.5, 4.5);
      \draw [->, red, ultra thick] (2.5, 4.5) -- (2.5, 5.5);
      \draw [->, red, ultra thick] (2.5, 5.5) -- (2.5, 3.5);
    \end{tikzpicture}
  \end{center}

Pick a new direction
====================

  \begin{center}
    \begin{tikzpicture}
      \draw (0, 0) grid (6, 6);
      \fill (1, 4) rectangle (2, 5);
      \fill (1, 3) rectangle (2, 4);
      \fill (1, 2) rectangle (2, 3);
      \fill (2, 1) rectangle (3, 2);
      \fill (3, 1) rectangle (4, 2);
      \fill (3, 3) rectangle (4, 4);
      \fill (4, 3) rectangle (5, 4);

      \fill[orange] (4, 4) rectangle (5, 5);
      \node at (4.5, 4.5) {0};
      \node at (3.5, 4.5) {0};
      \node at (5.5, 4.5) {0};
      \node at (4.5, 5.5) {0};
      \node at (2.5, 4.5) {0};
      \node at (5.5, 5.5) {0};
      \node at (3.5, 5.5) {0};
      \node at (5.5, 3.5) {0};
      \node at (2.5, 5.5) {-0.5};
      \node at (2.5, 3.5) {0};
      \node at (5.5, 2.5) {0};
      \node at (1.5, 5.5) {0};
      \node at (2.5, 2.5) {0};
      \node at (4.5, 2.5) {0};
      \node at (5.5, 1.5) {0};
      \node at (0.5, 5.5) {0};
      \node at (5.5, 0.5) {0};
      \node at (3.5, 2.5) {0};
      \node at (4.5, 1.5) {0};
      \node at (4.5, 0.5) {0};
      \node at (0.5, 4.5) {0};
      \node at (0.5, 3.5) {0};
      \node at (3.5, 0.5) {0};
      \node at (0.5, 2.5) {0};
      \node at (2.5, 0.5) {0};
      \node at (0.5, 1.5) {0};
      \node at (1.5, 0.5) {0};
      \node at (0.5, 0.5) {0};
      \node at (1.5, 1.5) {0};

      \draw[fill=blue,fill opacity=0.5] (2.5, 4.5) circle [radius=0.5];
      \draw [->, red, ultra thick] (2.5, 4.5) -- (3.5, 4.5);
    \end{tikzpicture}
  \end{center}

Move and update
===============

  \begin{center}
    \begin{tikzpicture}
      \draw (0, 0) grid (6, 6);
      \fill (1, 4) rectangle (2, 5);
      \fill (1, 3) rectangle (2, 4);
      \fill (1, 2) rectangle (2, 3);
      \fill (2, 1) rectangle (3, 2);
      \fill (3, 1) rectangle (4, 2);
      \fill (3, 3) rectangle (4, 4);
      \fill (4, 3) rectangle (5, 4);

      \fill[orange] (4, 4) rectangle (5, 5);
      \node at (4.5, 4.5) {0};
      \node at (3.5, 4.5) {0};
      \node at (5.5, 4.5) {0};
      \node at (4.5, 5.5) {0};
      \node at (2.5, 4.5) {-0.5};
      \node at (5.5, 5.5) {0};
      \node at (3.5, 5.5) {0};
      \node at (5.5, 3.5) {0};
      \node at (2.5, 5.5) {-0.5};
      \node at (2.5, 3.5) {0};
      \node at (5.5, 2.5) {0};
      \node at (1.5, 5.5) {0};
      \node at (2.5, 2.5) {0};
      \node at (4.5, 2.5) {0};
      \node at (5.5, 1.5) {0};
      \node at (0.5, 5.5) {0};
      \node at (5.5, 0.5) {0};
      \node at (3.5, 2.5) {0};
      \node at (4.5, 1.5) {0};
      \node at (4.5, 0.5) {0};
      \node at (0.5, 4.5) {0};
      \node at (0.5, 3.5) {0};
      \node at (3.5, 0.5) {0};
      \node at (0.5, 2.5) {0};
      \node at (2.5, 0.5) {0};
      \node at (0.5, 1.5) {0};
      \node at (1.5, 0.5) {0};
      \node at (0.5, 0.5) {0};
      \node at (1.5, 1.5) {0};

      \draw[fill=blue,fill opacity=0.5] (3.5, 4.5) circle [radius=0.5];
    \end{tikzpicture}
  \end{center}

And so on...
============

  \begin{center}
    \begin{tikzpicture}
      \draw (0, 0) grid (6, 6);
      \fill (1, 4) rectangle (2, 5);
      \fill (1, 3) rectangle (2, 4);
      \fill (1, 2) rectangle (2, 3);
      \fill (2, 1) rectangle (3, 2);
      \fill (3, 1) rectangle (4, 2);
      \fill (3, 3) rectangle (4, 4);
      \fill (4, 3) rectangle (5, 4);

      \fill[orange] (4, 4) rectangle (5, 5);
      \node at (4.5, 4.5) {0};
      \node at (3.5, 4.5) {0};
      \node at (5.5, 4.5) {0};
      \node at (4.5, 5.5) {0};
      \node at (2.5, 4.5) {-0.5};
      \node at (5.5, 5.5) {0};
      \node at (3.5, 5.5) {0};
      \node at (5.5, 3.5) {0};
      \node at (2.5, 5.5) {-0.5};
      \node at (2.5, 3.5) {0};
      \node at (5.5, 2.5) {0};
      \node at (1.5, 5.5) {0};
      \node at (2.5, 2.5) {0};
      \node at (4.5, 2.5) {0};
      \node at (5.5, 1.5) {0};
      \node at (0.5, 5.5) {0};
      \node at (5.5, 0.5) {0};
      \node at (3.5, 2.5) {0};
      \node at (4.5, 1.5) {0};
      \node at (4.5, 0.5) {0};
      \node at (0.5, 4.5) {0};
      \node at (0.5, 3.5) {0};
      \node at (3.5, 0.5) {0};
      \node at (0.5, 2.5) {0};
      \node at (2.5, 0.5) {0};
      \node at (0.5, 1.5) {0};
      \node at (1.5, 0.5) {0};
      \node at (0.5, 0.5) {0};
      \node at (1.5, 1.5) {0};

      \draw[fill=blue,fill opacity=0.5] (3.5, 4.5) circle [radius=0.5];
      \draw [->, red, ultra thick] (3.5, 4.5) -- (4.5, 4.5);
      \draw [->, red, ultra thick] (3.5, 4.5) -- (2.5, 4.5);
      \draw [->, red, ultra thick] (3.5, 4.5) -- (3.5, 3.5);
      \draw [->, red, ultra thick] (3.5, 4.5) -- (3.5, 5.5);
    \end{tikzpicture}
  \end{center}

A good choice
=============

  \begin{center}
    \begin{tikzpicture}
      \draw (0, 0) grid (6, 6);
      \fill (1, 4) rectangle (2, 5);
      \fill (1, 3) rectangle (2, 4);
      \fill (1, 2) rectangle (2, 3);
      \fill (2, 1) rectangle (3, 2);
      \fill (3, 1) rectangle (4, 2);
      \fill (3, 3) rectangle (4, 4);
      \fill (4, 3) rectangle (5, 4);

      \fill[orange] (4, 4) rectangle (5, 5);
      \node at (4.5, 4.5) {0};
      \node at (3.5, 4.5) {0};
      \node at (5.5, 4.5) {0};
      \node at (4.5, 5.5) {0};
      \node at (2.5, 4.5) {-0.5};
      \node at (5.5, 5.5) {0};
      \node at (3.5, 5.5) {0};
      \node at (5.5, 3.5) {0};
      \node at (2.5, 5.5) {-0.5};
      \node at (2.5, 3.5) {0};
      \node at (5.5, 2.5) {0};
      \node at (1.5, 5.5) {0};
      \node at (2.5, 2.5) {0};
      \node at (4.5, 2.5) {0};
      \node at (5.5, 1.5) {0};
      \node at (0.5, 5.5) {0};
      \node at (5.5, 0.5) {0};
      \node at (3.5, 2.5) {0};
      \node at (4.5, 1.5) {0};
      \node at (4.5, 0.5) {0};
      \node at (0.5, 4.5) {0};
      \node at (0.5, 3.5) {0};
      \node at (3.5, 0.5) {0};
      \node at (0.5, 2.5) {0};
      \node at (2.5, 0.5) {0};
      \node at (0.5, 1.5) {0};
      \node at (1.5, 0.5) {0};
      \node at (0.5, 0.5) {0};
      \node at (1.5, 1.5) {0};

      \draw[fill=blue,fill opacity=0.5] (3.5, 4.5) circle [radius=0.5];
      \draw [->, red, ultra thick] (3.5, 4.5) -- (4.5, 4.5);
    \end{tikzpicture}
  \end{center}

A good choice
=============

  \begin{center}
    \begin{tikzpicture}
      \draw (0, 0) grid (6, 6);
      \fill (1, 4) rectangle (2, 5);
      \fill (1, 3) rectangle (2, 4);
      \fill (1, 2) rectangle (2, 3);
      \fill (2, 1) rectangle (3, 2);
      \fill (3, 1) rectangle (4, 2);
      \fill (3, 3) rectangle (4, 4);
      \fill (4, 3) rectangle (5, 4);

      \fill[orange] (4, 4) rectangle (5, 5);
      \node at (4.5, 4.5) {0};
      \node at (3.5, 4.5) {0.5};
      \node at (5.5, 4.5) {0};
      \node at (4.5, 5.5) {0};
      \node at (2.5, 4.5) {-0.5};
      \node at (5.5, 5.5) {0};
      \node at (3.5, 5.5) {0};
      \node at (5.5, 3.5) {0};
      \node at (2.5, 5.5) {-0.5};
      \node at (2.5, 3.5) {0};
      \node at (5.5, 2.5) {0};
      \node at (1.5, 5.5) {0};
      \node at (2.5, 2.5) {0};
      \node at (4.5, 2.5) {0};
      \node at (5.5, 1.5) {0};
      \node at (0.5, 5.5) {0};
      \node at (5.5, 0.5) {0};
      \node at (3.5, 2.5) {0};
      \node at (4.5, 1.5) {0};
      \node at (4.5, 0.5) {0};
      \node at (0.5, 4.5) {0};
      \node at (0.5, 3.5) {0};
      \node at (3.5, 0.5) {0};
      \node at (0.5, 2.5) {0};
      \node at (2.5, 0.5) {0};
      \node at (0.5, 1.5) {0};
      \node at (1.5, 0.5) {0};
      \node at (0.5, 0.5) {0};
      \node at (1.5, 1.5) {0};

      \draw[fill=blue,fill opacity=0.5] (4.5, 4.5) circle [radius=0.5];
    \end{tikzpicture}
  \end{center}

The game goes on
================

  \begin{center}
    \begin{tikzpicture}
      \draw (0, 0) grid (6, 6);
      \fill (1, 4) rectangle (2, 5);
      \fill (1, 3) rectangle (2, 4);
      \fill (1, 2) rectangle (2, 3);
      \fill (2, 1) rectangle (3, 2);
      \fill (3, 1) rectangle (4, 2);
      \fill (3, 3) rectangle (4, 4);
      \fill (4, 3) rectangle (5, 4);

      \fill[orange] (4, 4) rectangle (5, 5);
      \node at (4.5, 4.5) {0};
      \node at (3.5, 4.5) {0.5};
      \node at (5.5, 4.5) {0};
      \node at (4.5, 5.5) {0};
      \node at (2.5, 4.5) {-0.5};
      \node at (5.5, 5.5) {0};
      \node at (3.5, 5.5) {0};
      \node at (5.5, 3.5) {0};
      \node at (2.5, 5.5) {-0.5};
      \node at (2.5, 3.5) {0};
      \node at (5.5, 2.5) {0};
      \node at (1.5, 5.5) {0};
      \node at (2.5, 2.5) {0};
      \node at (4.5, 2.5) {0};
      \node at (5.5, 1.5) {0};
      \node at (0.5, 5.5) {0};
      \node at (5.5, 0.5) {0};
      \node at (3.5, 2.5) {0};
      \node at (4.5, 1.5) {0};
      \node at (4.5, 0.5) {0};
      \node at (0.5, 4.5) {0};
      \node at (0.5, 3.5) {0};
      \node at (3.5, 0.5) {0};
      \node at (0.5, 2.5) {0};
      \node at (2.5, 0.5) {0};
      \node at (0.5, 1.5) {0};
      \node at (1.5, 0.5) {0};
      \node at (0.5, 0.5) {0};
      \node at (1.5, 1.5) {0};

      \draw[fill=blue,fill opacity=0.5] (0.5, 0.5) circle [radius=0.5];
    \end{tikzpicture}
  \end{center}

Discount factors
================

In this setting, we maximise the sum of rewards. Problem: if the number
of steps is large, the values become difficult to estimate (huge
absolute value).  Moreover, in many situations we need to operate with *infinite horizons*.

To deal with this problem, we introduce a *discount
factor*: ```0 < β < 1```{.haskell} 

Instead of maximising

> Rew(s₀, pol(s₀)) + Rew(s₁, pol(s₁)) + Rew(s₂, pol(s₂)) + ... 

we maximise

> Rew(s₀, pol(s₀)) + β*Rew(s₁, pol(s₁)) + β²*Rew(s₂, pol(s₂)) + ... 

where ```s```{.haskell}$_{\tiny{\verb|t+1|}}$ ```= sys(sₜ, pol(sₜ))```{.haskell}

Value function
==============

If we have an infinite horizon, the value function is *stationary*:

```Vᵢ(s) = maxₚₒₗ Σᵢ```$^{\tiny{\infty}}$ ```βᵗ*rew(sₜ, pol(sₜ))```{.haskell} where

> sᵢ = s

```s```{.haskell}$_{\tiny{\verb|t+1|}}$ ```= sys(sₜ, pol(sₜ))```{.haskell}

This is independent of ```i```{.haskell}!

Bellman's equation
==================

We choose ```pol(s)```{.haskell} that maximises

> Rew(s, a) + β * Val(sys(s, a))

Therefore

> Val(s) = maxₐ (Rew(s, a) + β * Val(sys(s, a)))

Value iteration and function approximation
==========================================

> Val(s) = maxₐ (Rew(s, a) + β * Val(sys(s, a)))

Bellman's equation allows us to iteratively approximate the optimal
value function.

> Val(s) <- Val(s) +
>           δ * (maxₐ (Rew(s, a) + β * Val(sys(s, a)))) - Val(s))

The optimal value function ```Val```{.haskell} is unique (but there
might be many policy functions that realise it). 

Value iteration
===============

> Val(s) <- Val(s) +
>           δ * (maxₐ (Rew(s, a) + β * Val(sys(s, a)))) - Val(s))

Problem: we require knowledge of the reward function.

We could use supervised learning to approximate the reward
function. 

But now we have two function approximations and an optimisation at
every step!

\ 
===========

  \begin{center}
    \begin{Large}
      {\bf $Q$-learning}
    \end{Large}
  \end{center}

Watkins' idea
=============

The reward function only enters the picture when combined with ```Val```{.haskell},
e.g.:

> Val(s) = maxₐ (Rew(s, a) + β * Val(sys(s, a)))

Chris Watkins (1989): maybe it's simpler to learn the combination of
```Rew```{.haskell} and ```Val```{.haskell}! 

> Q(s, a) = Rew(s, a) + β * Val(sys(s, a))

Q-learning and ```Val```{.haskell}
==================================

> Q(s, a) = Rew(s, a) + β * Val(sys(s, a))

We have ```Val(s) = maxₐ Q(s, a)```{.haskell} therefore, knowing ```Q```{.haskell} is
sufficient for determining ```Val```{.haskell}.

Q-learning and the optimal policy
=================================

> Q(s, a) = Rew(s, a) + β * Val(sys(s, a))

The optimal policy is the one that maximises

> Rew(s, a) + β * Val(sys(s, a))

Therefore, the optimal action in a state ```s```{.haskell} is the one
that maximises the ```Q```{.haskell} function:

> aᵒᵖᵗ = arg maxₐ Q(s, a)

Recursive equation for Q-learning
=================================

> Q(s, a) = Rew(s, a) + β * Val(sys(s, a))

> Val(s) = maxₐ Q(s, a)

Therefore

> Q(s, a) = maxₐ (Rew(s, a) + β * maxₓ Q(sys(s, a), x))

We have a recursive equation for the ```Q```{.haskell} function.

Q-learning in action
====================

  \begin{columns}
    \begin{column}{0.6\textwidth}
      \begin{center}
        \begin{tikzpicture}
          \draw (0, 0) grid (6, 6);
          \fill (1, 4) rectangle (2, 5);
          \fill (1, 3) rectangle (2, 4);
          \fill (1, 2) rectangle (2, 3);
          \fill (2, 1) rectangle (3, 2);
          \fill (3, 1) rectangle (4, 2);
          \fill (3, 3) rectangle (4, 4);
          \fill (4, 3) rectangle (5, 4);

          \fill[orange] (4, 4) rectangle (5, 5);

          \node at (2.5, 5.5) {s};

          \draw[fill=blue,fill opacity=0.5] (2.5, 5.5) circle [radius=0.5];
          \draw [->, red, ultra thick] (2.5, 5.5) -- (3.5, 5.5);
          \draw [->, red, ultra thick] (2.5, 5.5) -- (1.5, 5.5);
          \draw [->, red, ultra thick] (2.5, 5.5) -- (2.5, 4.5);
        \end{tikzpicture}
      \end{center}
    \end{column}
    \begin{column}{0.35\textwidth}
      $Q(s, \leftarrow) = -10$

      $Q(s, \downarrow) = -8$

      $Q(s, \rightarrow) = -9$
    \end{column}
  \end{columns}

Q-learning in action
====================

  \begin{columns}
    \begin{column}{0.6\textwidth}
      \begin{center}
        \begin{tikzpicture}
          \draw (0, 0) grid (6, 6);
          \fill (1, 4) rectangle (2, 5);
          \fill (1, 3) rectangle (2, 4);
          \fill (1, 2) rectangle (2, 3);
          \fill (2, 1) rectangle (3, 2);
          \fill (3, 1) rectangle (4, 2);
          \fill (3, 3) rectangle (4, 4);
          \fill (4, 3) rectangle (5, 4);

          \fill[orange] (4, 4) rectangle (5, 5);

          \node at (2.5, 5.5) {s};
          
          \draw[fill=blue,fill opacity=0.5] (2.5, 5.5) circle [radius=0.5];
          \draw [->, red, ultra thick] (2.5, 5.5) -- (2.5, 4.5);
        \end{tikzpicture}
      \end{center}
    \end{column}
    \begin{column}{0.35\textwidth}
      $Q(s, \downarrow) = -8$
    \end{column}
  \end{columns}

Q-learning in action
====================

  \begin{columns}
    \begin{column}{0.6\textwidth}
      \begin{center}
        \begin{tikzpicture}
          \draw (0, 0) grid (6, 6);
          \fill (1, 4) rectangle (2, 5);
          \fill (1, 3) rectangle (2, 4);
          \fill (1, 2) rectangle (2, 3);
          \fill (2, 1) rectangle (3, 2);
          \fill (3, 1) rectangle (4, 2);
          \fill (3, 3) rectangle (4, 4);
          \fill (4, 3) rectangle (5, 4);

          \fill[orange] (4, 4) rectangle (5, 5);
          \node at (2.5, 5.5) {s};
          \node at (2.5, 4.5) {s'};

          \draw[fill=blue,fill opacity=0.5] (2.5, 4.5) circle [radius=0.5];
          
        \end{tikzpicture}
      \end{center}
    \end{column}
    \begin{column}{0.35\textwidth}
      $Q(s, \downarrow) = -8$

      $Rew(s, \downarrow) = -1$
      
    \end{column}
  \end{columns}

Q-learning in action
====================

  \begin{columns}
    \begin{column}{0.6\textwidth}
      \begin{center}
        \begin{tikzpicture}
          \draw (0, 0) grid (6, 6);
          \fill (1, 4) rectangle (2, 5);
          \fill (1, 3) rectangle (2, 4);
          \fill (1, 2) rectangle (2, 3);
          \fill (2, 1) rectangle (3, 2);
          \fill (3, 1) rectangle (4, 2);
          \fill (3, 3) rectangle (4, 4);
          \fill (4, 3) rectangle (5, 4);

          \fill[orange] (4, 4) rectangle (5, 5);
          \node at (2.5, 5.5) {s};
          \node at (2.5, 4.5) {s'};

          \draw[fill=blue,fill opacity=0.5] (2.5, 4.5) circle [radius=0.5];
          \draw [->, red, ultra thick] (2.5, 4.5) -- (1.5, 4.5);
          \draw [->, red, ultra thick] (2.5, 4.5) -- (3.5, 4.5);
          \draw [->, red, ultra thick] (2.5, 4.5) -- (2.5, 5.5);
          \draw [->, red, ultra thick] (2.5, 5.5) -- (2.5, 3.5);

        \end{tikzpicture}
      \end{center}
    \end{column}
    \begin{column}{0.35\textwidth}
      $Q(s, \downarrow) = -8$

      $Rew(s, \downarrow) = -1$
      
      $Q(s', \leftarrow) = -\infty$

      $Q(s', \downarrow) = -8$

      $Q(s', \rightarrow) = -5$

      $Q(s', \uparrow) = -10$
      
    \end{column}
  \end{columns}


Q-learning in action
====================

  \begin{columns}
    \begin{column}{0.6\textwidth}
      \begin{center}
        \begin{tikzpicture}
          \draw (0, 0) grid (6, 6);
          \fill (1, 4) rectangle (2, 5);
          \fill (1, 3) rectangle (2, 4);
          \fill (1, 2) rectangle (2, 3);
          \fill (2, 1) rectangle (3, 2);
          \fill (3, 1) rectangle (4, 2);
          \fill (3, 3) rectangle (4, 4);
          \fill (4, 3) rectangle (5, 4);

          \fill[orange] (4, 4) rectangle (5, 5);

          \node at (2.5, 5.5) {s};
          \node at (2.5, 4.5) {s'};

          \draw[fill=blue,fill opacity=0.5] (2.5, 4.5) circle [radius=0.5];
        \end{tikzpicture}
      \end{center}
    \end{column}
    \begin{column}{0.35\textwidth}
      $Q(s, \downarrow) = -8$

      $Rew(s, \downarrow) = -1$

      $Val(s') = max (-\infty, -8, -5, -10)$

      $Rew(s, \downarrow) + \beta Val(s') = -1 + 1 * (-5) = -6$
    \end{column}
  \end{columns}


Q-learning in action
====================

  \begin{columns}
    \begin{column}{0.6\textwidth}
      \begin{center}
        \begin{tikzpicture}
          \draw (0, 0) grid (6, 6);
          \fill (1, 4) rectangle (2, 5);
          \fill (1, 3) rectangle (2, 4);
          \fill (1, 2) rectangle (2, 3);
          \fill (2, 1) rectangle (3, 2);
          \fill (3, 1) rectangle (4, 2);
          \fill (3, 3) rectangle (4, 4);
          \fill (4, 3) rectangle (5, 4);

          \fill[orange] (4, 4) rectangle (5, 5);

          \node at (2.5, 5.5) {s};
          \node at (2.5, 4.5) {s'};

          \draw[fill=blue,fill opacity=0.5] (2.5, 4.5) circle [radius=0.5];
        \end{tikzpicture}
      \end{center}
    \end{column}
    \begin{column}{0.35\textwidth}
      $Q(s, \downarrow) = -8$

      $Rew(s, \downarrow) + \beta Val(s') = -6$

      $Q(s, \downarrow) \leftarrow -8 + \delta (-6 - (-8)) = -6$
    \end{column}
  \end{columns}

Homework
========

Solve exercise ```13.2```{.haskell}, point ```a)```{.haskell}, from Mitchell (page 388).  See next slide.

This homework is optional, you only need to hand it in if you haven't already passed four assignments.

Mitchell uses slightly different notations:

```V```{.haskell}$^{{\tiny *}}$```(s)= Val(s)```{.haskell}

> γ = β

Homework
========

![Mitchell, 13.2 (a).](13.2.png){ height=90% }

Stochastic systems
==================

We have assumed a deterministic system, but in most cases this is
unrealistic. E.g., the maze environment could take us (with a small
probability) in a different direction than we chose. In that case, we
need to maximise the *expected value* of the sum of rewards.
$Q$-learning can be applied to these cases almost without any change!

Conclusions
===========

Reinforcement learning applications: games, RoboCup Soccer, self-driving
cars, etc. See also discussion in Week 1 (robot-catching arm,
self-driving car, barriers on emissions...) Reinforcement learning seems
to make the most out of very little: is it the road to Artificial
General Intelligence? Chris Watkins
(<http://www.cs.rhul.ac.uk/~chrisw/>):

 > I have long felt the standard model of RL is deceptively attractive,
 > but limited.


