---
theme: boxes
theme: Madrid
mainfont: TeX Gyre Pagella
monofont: DejaVuSansMono
header-includes: \usepackage{tikz}
---

\title{Lecture 7}
\author{Cezar Ionescu \\ 29/06/2019}
\titlegraphic{\includegraphics[scale=0.3]{oudce.jpg}}
\maketitle

Administrative
==============

- Homework from 22/06/2019 due now!

- Please complete and hand in the declarations of authorship.

Questions?
==========

Solution to homework from lecture 6
===================================

Games and AI
============

Progress in AI has often been connected to game-playing systems.

-   1956: Checkers -- Arthur Samuel

-   1992: TD-Gammon -- Gerald Tesauro

-   1997: *Deep Blue* (IBM) defeats Kasparov

-   2016: AlphaGo (DeepMind, Google) defeats Lee Sedol

MENACE
======

In 1960, Donald Michie invented MENACE (*Matchbox Educable Noughts And
Crosses Engine*). MENACE is an automated system that learns to play
optimal tic-tac-toe (noughts and crosses) by playing against itself or against human opponents. It
is built out of matchboxes, bits of paper, and sugar drops (or coloured beads).

:::::: {.columns}
::: {.column width="49%"}
![Donald Michie](michie.jpg)
:::
:::{.column width="49%"}
![MENACE](o.png)
:::
::::::

Noughts and crosses
===================

  \begin{center}
    
    \begin{tikzpicture}
      \draw (0, 0) grid (3, 3);
      \pause
      \node at (1.5, 1.5) {X};
      \pause
      \node at (0.5, 2.5) {O};
      \pause
      \node at (2.5, 2.5) {X};
      \pause
      \node at (0.5, 0.5) {O};
      \pause
      \node at (2.5, 1.5) {X};
      \pause
      \node at (0.5, 1.5) {O};
      \pause
      \draw[red, thick] (0.5, 2.75) -- (0.5, 0.25);
      \pause
      \node at (1, -1) {Winner: O};
    \end{tikzpicture}
    \vfill
    \vfill
  \end{center}


Building a version of MENACE
============================

Our version looked similar to the one built by James Bridle:

![MENACE](menace.jpg){ width=70% }

Building MENACE
===============

![Close-up](menace-closeup.jpg){ width=60% }

...but we had much smaller sugar drops and were colour-coding the empty
slots.

MENACE in action
================

  \begin{tikzpicture}[scale=0.5]
    \def\o{0};
    \fill[green] (\o, 0) rectangle (\o+1, 1);
    \fill[blue] (\o+1, 0) rectangle (\o+2, 1);
    \fill[red] (\o+2, 0) rectangle (\o+3, 1);
    \fill[yellow] (\o+0, 1) rectangle (\o+1, 2);
    \fill[olive] (\o+1, 1) rectangle (\o+2, 2);
    \fill[magenta] (\o+2, 1) rectangle (\o+3, 2);
    \fill[pink] (\o+0, 2) rectangle (\o+1, 3);
    \fill[orange] (\o+1, 2) rectangle (\o+2, 3);
    \fill[brown] (\o+2, 2) rectangle (\o+3, 3);
    \draw (\o, 0) grid (\o+3, 3);
    \pause
    \fill[olive] (\o+1.5, -1) circle [radius=0.5];

    \pause
    \def\o{5};
    \fill[green] (\o, 0) rectangle (\o+1, 1);
    \fill[blue] (\o+1, 0) rectangle (\o+2, 1);
    \fill[red] (\o+2, 0) rectangle (\o+3, 1);
    \fill[yellow] (\o+0, 1) rectangle (\o+1, 2);
    \node at (\o+1.5, 1.5) {X};
    \fill[magenta] (\o+2, 1) rectangle (\o+3, 2);
    \fill[pink] (\o+0, 2) rectangle (\o+1, 3);
    \fill[orange] (\o+1, 2) rectangle (\o+2, 3);
    \fill[brown] (\o+2, 2) rectangle (\o+3, 3);
    \draw (\o, 0) grid (\o+3, 3);
    \pause
    \fill[pink] (\o+1.5, -1) circle [radius=0.5];

    \pause
    \def\o{9};
    \fill[green] (\o, 0) rectangle (\o+1, 1);
    \fill[blue] (\o+1, 0) rectangle (\o+2, 1);
    \fill[red] (\o+2, 0) rectangle (\o+3, 1);
    \fill[yellow] (\o+0, 1) rectangle (\o+1, 2);
    \node at (\o+1.5, 1.5) {X};
    \fill[magenta] (\o+2, 1) rectangle (\o+3, 2);
    \node at (\o+0.5, 2.5) {O};
    \fill[orange] (\o+1, 2) rectangle (\o+2, 3);
    \fill[brown] (\o+2, 2) rectangle (\o+3, 3);
    \draw (\o, 0) grid (\o+3, 3);
    \pause
    \fill[brown] (\o+1.5, -1) circle [radius=0.5];

    \pause
    \def\o{13};
    \fill[green] (\o, 0) rectangle (\o+1, 1);
    \fill[blue] (\o+1, 0) rectangle (\o+2, 1);
    \fill[red] (\o+2, 0) rectangle (\o+3, 1);
    \fill[yellow] (\o+0, 1) rectangle (\o+1, 2);
    \node at (\o+1.5, 1.5) {X};
    \fill[magenta] (\o+2, 1) rectangle (\o+3, 2);
    \node at (\o+0.5, 2.5) {O};
    \fill[orange] (\o+1, 2) rectangle (\o+2, 3);
    \node at (\o+2.5, 2.5) {X};
    \draw (\o, 0) grid (\o+3, 3);
    \pause
    \fill[red] (\o+1.5, -1) circle [radius=0.5];

    \pause
    \def\o{17};
    \fill[green] (\o, 0) rectangle (\o+1, 1);
    \fill[blue] (\o+1, 0) rectangle (\o+2, 1);
    \node at (\o+2.5, 0.5) {O};
    \fill[yellow] (\o+0, 1) rectangle (\o+1, 2);
    \node at (\o+1.5, 1.5) {X};
    \fill[magenta] (\o+2, 1) rectangle (\o+3, 2);
    \node at (\o+0.5, 2.5) {O};
    \fill[orange] (\o+1, 2) rectangle (\o+2, 3);
    \node at (\o+2.5, 2.5) {X};
    \draw (\o, 0) grid (\o+3, 3);
    \pause
    \fill[green] (\o+1.5, -1) circle [radius=0.5];

    \pause
    \def\o{21};
    \node at (\o+0.5, 0.5) {X};
    \fill[blue] (\o+1, 0) rectangle (\o+2, 1);
    \node at (\o+2.5, 0.5) {O};
    \fill[yellow] (\o+0, 1) rectangle (\o+1, 2);
    \node at (\o+1.5, 1.5) {X};
    \fill[magenta] (\o+2, 1) rectangle (\o+3, 2);
    \node at (\o+0.5, 2.5) {O};
    \fill[orange] (\o+1, 2) rectangle (\o+2, 3);
    \node at (\o+2.5, 2.5) {X};
    \draw (\o, 0) grid (\o+3, 3);

  \end{tikzpicture}


MENACE in action
================

  \begin{tikzpicture}[scale=0.5]
    \def\o{0};
    \fill[green] (\o, 0) rectangle (\o+1, 1);
    \fill[blue] (\o+1, 0) rectangle (\o+2, 1);
    \fill[red] (\o+2, 0) rectangle (\o+3, 1);
    \fill[yellow] (\o+0, 1) rectangle (\o+1, 2);
    \fill[olive] (\o+1, 1) rectangle (\o+2, 2);
    \fill[magenta] (\o+2, 1) rectangle (\o+3, 2);
    \fill[pink] (\o+0, 2) rectangle (\o+1, 3);
    \fill[orange] (\o+1, 2) rectangle (\o+2, 3);
    \fill[brown] (\o+2, 2) rectangle (\o+3, 3);
    \draw (\o, 0) grid (\o+3, 3);
    
    \fill[olive] (\o+1.5, -1) circle [radius=0.5];

    \def\o{5};
    \fill[green] (\o, 0) rectangle (\o+1, 1);
    \fill[blue] (\o+1, 0) rectangle (\o+2, 1);
    \fill[red] (\o+2, 0) rectangle (\o+3, 1);
    \fill[yellow] (\o+0, 1) rectangle (\o+1, 2);
    \node at (\o+1.5, 1.5) {X};
    \fill[magenta] (\o+2, 1) rectangle (\o+3, 2);
    \fill[pink] (\o+0, 2) rectangle (\o+1, 3);
    \fill[orange] (\o+1, 2) rectangle (\o+2, 3);
    \fill[brown] (\o+2, 2) rectangle (\o+3, 3);
    \draw (\o, 0) grid (\o+3, 3);
    
    % \fill[pink] (\o+1.5, -1) circle [radius=0.5];

    \def\o{9};
    \fill[green] (\o, 0) rectangle (\o+1, 1);
    \fill[blue] (\o+1, 0) rectangle (\o+2, 1);
    \fill[red] (\o+2, 0) rectangle (\o+3, 1);
    \fill[yellow] (\o+0, 1) rectangle (\o+1, 2);
    \node at (\o+1.5, 1.5) {X};
    \fill[magenta] (\o+2, 1) rectangle (\o+3, 2);
    \node at (\o+0.5, 2.5) {O};
    \fill[orange] (\o+1, 2) rectangle (\o+2, 3);
    \fill[brown] (\o+2, 2) rectangle (\o+3, 3);
    \draw (\o, 0) grid (\o+3, 3);
    
    \fill[brown] (\o+1.5, -1) circle [radius=0.5];

    \def\o{13};
    \fill[green] (\o, 0) rectangle (\o+1, 1);
    \fill[blue] (\o+1, 0) rectangle (\o+2, 1);
    \fill[red] (\o+2, 0) rectangle (\o+3, 1);
    \fill[yellow] (\o+0, 1) rectangle (\o+1, 2);
    \node at (\o+1.5, 1.5) {X};
    \fill[magenta] (\o+2, 1) rectangle (\o+3, 2);
    \node at (\o+0.5, 2.5) {O};
    \fill[orange] (\o+1, 2) rectangle (\o+2, 3);
    \node at (\o+2.5, 2.5) {X};
    \draw (\o, 0) grid (\o+3, 3);
    
    % \fill[red] (\o+1.5, -1) circle [radius=0.5];

    \def\o{17};
    \fill[green] (\o, 0) rectangle (\o+1, 1);
    \fill[blue] (\o+1, 0) rectangle (\o+2, 1);
    \node at (\o+2.5, 0.5) {O};
    \fill[yellow] (\o+0, 1) rectangle (\o+1, 2);
    \node at (\o+1.5, 1.5) {X};
    \fill[magenta] (\o+2, 1) rectangle (\o+3, 2);
    \node at (\o+0.5, 2.5) {O};
    \fill[orange] (\o+1, 2) rectangle (\o+2, 3);
    \node at (\o+2.5, 2.5) {X};
    \draw (\o, 0) grid (\o+3, 3);
    
    \fill[green] (\o+1.5, -1) circle [radius=0.5];

    \def\o{21};
    \node at (\o+0.5, 0.5) {X};
    \fill[blue] (\o+1, 0) rectangle (\o+2, 1);
    \node at (\o+2.5, 0.5) {O};
    \fill[yellow] (\o+0, 1) rectangle (\o+1, 2);
    \node at (\o+1.5, 1.5) {X};
    \fill[magenta] (\o+2, 1) rectangle (\o+3, 2);
    \node at (\o+0.5, 2.5) {O};
    \fill[orange] (\o+1, 2) rectangle (\o+2, 3);
    \node at (\o+2.5, 2.5) {X};
    \draw (\o, 0) grid (\o+3, 3);

  \end{tikzpicture}

Building MENACE
===============

There are about ```3000```{.haskell} possible boards. However, due to the symmetry
of the board, it is possible to reduce this number by a factor of ten.
This is a form of *knowledge engineering*: using available information
to reduce the complexity of the learning system.

Policies
========

What does the matchbox system represent? What is the input to the
matchbox system? What is the output? 

. . .

> matchboxSystem : Board ~> Move

The matchbox system tells us what action to take in a given state. Such
a function is called a *policy*.

Policies
========

Examples of policies:

-   deciding how to turn the steering wheel and act on the pedals in a
    self-driving car

-   deciding how to control a robot arm in order to catch a ball

-   deciding how to set emissions barriers in order to reduce greenhouse
    gases without damaging the economy

-   ...

In all these examples, actions *now* have consequences *in the future*.
A policy must have "foresight".

Policies
========

> policy : State ~> Action

-   tic-tac-toe: ```State = Board```{.haskell}, ```Action = Move```{.haskell}

-   self-driving car: \pause ```State = Sensors```{.haskell}, ```Action = (WheelTurn,
    BrakePress)```{.haskell}

-   robot arm: \pause ```State = (BallPos, HandPos)```{.haskell}, ```Action = (ArmTurn,
    FingerMove)```{.haskell}

-   emissions barriers: \pause ```State = (EconomyVars, ClimateVars)```{.haskell}, ```Action
    = EmissionLevel```{.haskell}

-   ...

Policies
========

A "good" policy attains a long-term goal, not just an immediate one.

- Tic-tac-toe:

  - Example of a long-term goal in tic-tac-toe: maximise the number of
    wins in the long run (or minimise the number of losses).

  - Example of a short-term goal in tic-tac-toe: occupy the centre.

Policies
========

- Self-driving cars:

  - Example of a long-term goal in self-driving cars: \pause smooth driving,
    minimise consumption, reach destination quickly.

  - Example of a short-term goal in self-driving cars: \pause turn left to
    avoid pedestrian.

Policies
========

- Robot arm:

  - Example of a long-term goal in controlling a robot arm: \pause catch the ball.

  - Example of a short-term goal in controlling a robot arm: \pause bend the arm.

Policies
========

- Setting emissions:

  - Example of a long-term goal in setting emissions: \pause keep global warming
    below 2°C. 

  - Example of a short-term goal in setting emissions: \pause eliminate
    diesel cars.

Matchboxes
==========

If the matchbox system represents a policy, what does a single matchbox
represent? 

> matchbox : ? -> ?

. . .

> matchbox : (Board, Move) -> Number

A matchbox represents the system's current valuation for the various
possible moves. Bigger values correspond to better moves. The
matchboxes implement a value function:

> value : (Board, Move) -> Number

Matchboxes
==========

The equivalent of matchboxes in self-driving cars: \pause a function that
gives the value of various possible actions depending on the sensor
readings.

The equivalent of matchboxes in controlling a robot arm: \pause a function
that gives the value of various possible arm movements depending on
the position of the ball and of the hand. 

The equivalent of matchboxes in setting emissions: \pause a function that
gives the value of possible emission barriers depending on the state
of the climate and the state of the economy.

State-action value function
===========================

> value : (State, Action) -> Number

In self-driving cars: 

> value : (Sensors, (WheelTurn, BrakePress)) -> Number

For robot arm: 

> value : ((BallPos, HandPos), (ArmTurn, FingerMove)) -> Number

For emissions barriers: 

> value : ((EconomyVars, ClimateVars), EmissionLevel) -> Number

Function approximation
======================

In each of these cases, knowledge of this function would imply the
solution to the problem. If we already know the values of many ```(State,
Action)```{.haskell} pairs, we can use a neural network to approximate the
state-action value function. Neural networks are good *function
approximators*.

Reinforcement learning
======================

In many cases, however, we do not have known values for ```(State,
Action)```{.haskell} pairs. The state-action value function must be approximated by
*trial and error*. The intuition is that of *behavioural conditioning*.
*Reinforcement learning* is a way of implementing efficient trial and
error behaviour. The algorithm implemented by MENACE is the simplest
example of reinforcement learning.

Choosing a move
===============

In MENACE, we choose a move stochastically. Moves with better values
(more representatives in the matchbox) have better chances of being
chosen. 

> choose : Matchbox ~> Move

In general:

> choose : State ~> Action

Why not just choose the action that has the maximal value?

Credit assignment
=================

When ```X```{.haskell} or ```O```{.haskell} lose a game, all moves
that lead to that loss are penalised.  

It is not always easy to assign credit or blame to the individual moves.  

Good moves lead to good results *on average*, not necessarily every
time.

Choosing stochastically reduces the risks of throwing away good moves.

Exploration versus exploitation
===============================

Donald Michie's original version added sugar drops to winning moves.

Realistic examples follow this pattern and "reward" winning moves. 

This raises a problem similar to that of credit assignment: we might
reward average, or even bad moves.

We want to get the rewards promised by well-valued moves, but we risk
missing even greater rewards if we do not try out new moves from time
to time.

This is known as the problem of "exploration versus exploitation".

Summary
=======

MENACE illustrates the most important problems and methods of AI:

-   knowledge engineering

-   credit assignment

-   exploration vs exploitation (cf. if we had only one sugar drop per
    move)

-   randomisation (cf. outputing typewritten vs probability of
    typewritten)

-   function approximation

-   reinforcement learning

Homework
========

A smart traffic control system can adjust the delays of traffic lights
in order to alleviate bottlenecks in the flow of vehicles and ensure a
smooth driving experience. In order to achieve this, the system must
anticipate how the delays will influence the evolution of the traffic
pattern.  We want to train this system using an idealised version of
the matchbox system.
                            
-   What are the states? What are the actions (sugar drops)?
                                
-   What is the equivalent of a matchbox for this system?
                                    
-   What could be a long-term goal for this system? What could be a
    short-term goal of the system?
                                                
-   How would the updating process work (how can bad actions be
    eliminated)?
                                                            
