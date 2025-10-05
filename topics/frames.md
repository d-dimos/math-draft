---
layout: default
title: 
permalink: /topics/frames/
exclude: true
---

# Coordinate Changes Between Frames in SE(3)

$\textbf{Notation}$

- $p^X$: the coordinates of a point expressed w.r.t. frame $X$.

- ${^{Y}\square}_{X}$ (equivalently $$\square^Y_X$$): $X$ is the source frame, and $Y$ is the expressed-in/target frame. For example, $${^{Y}T}_{X}$$ maps coordinates initially expressed in $X$ into coordinates expressed in $Y$.

$\textbf{Change of Coordinates}$

Let $p^A$ be the coordinates of a point w.r.t. $A$ and $p^B$ the coordinates of the same point w.r.t. $B$.

To convert $p^A$ into $p^B$, we use

$$
p^B = {^{B}R}_{A}\,p^A + {^{B}t}_{A} \qquad (1)
$$

and conversely

$$
p^A = {^{A}R}_{B}\,p^B + {^{A}t}_{B} \qquad (2)
$$

- ${^{A}R}_{B}$ is the orientation of frame $B$ expressed in frame $A$,

- ${^{A}t}_{B}$ gives the coordinates of the origin of $B$ expressed in frame $A$,

- The pair $$({^{A}R}_{B},{^{A}t}_{B})$$ is the **pose of frame $B$ w.r.t. frame $A$** and it maps points expressed in $B$ to their representations expressed in $A$ via $(2)$.

- the converse holds for $${^{B}R}_{A}$$ and $${^{B}t}_{A}$$.

From $(1)$ and $(2)$ we obtain the inverse relations:

$$
{^{B}R}_{A} = ({^{A}R}_{B})^{-1}, \qquad
{^{B}t}_{A} = -({^{A}R}_{B})^{-1}\,{^{A}t}_{B}.
$$

$\textbf{Chaining Consecutive Transformations}$

In homogeneous form:

$$
\begin{bmatrix}p^A\\[2pt]1\end{bmatrix}
= \underbrace{\begin{bmatrix}
{^{A}R}_{B} & {^{A}t}_{B}\\[2pt]
0 & 1
\end{bmatrix}}_{ {^{A}T}_{B} }\,\begin{bmatrix}p^B\\[2pt]1\end{bmatrix}
$$

To map point representations along consecutive frames, we post-multiply homogeneous points by consecutive transforms to their left. For instance, to re-express a point from frame $C$ to $B$ to $A$, we write:

$$
\begin{bmatrix}p^{A}\\[2pt]1\end{bmatrix}
= {^{A}T}_{B}\,{^{B}T}_{C}\,\begin{bmatrix}p^{C}\\[2pt]1\end{bmatrix}
$$

The matrix closest to the vector acts first (here $${^{B}T}_{C}$$ maps $C$ to $B$, and the left factor then maps the result to $A$).

---
---
$\textbf{Example}$

Consider a (world) frame $W$ and a camera frame that are initially aligned. Suppose the camera is rotated about $z^W$ by $10^\circ$ and then translated so that its origin lies at the point $$\begin{bmatrix}7\\2\\9\end{bmatrix}$$ w.r.t. $W$. The final camera frame is $C$. Find the mapping $p^C \to p^W$.

The camera pose w.r.t. $W$ is

$$
{^{W}R}_{C} = R_z(10^\circ), \qquad
{^{W}t}_{C} =
\begin{bmatrix}7\\2\\9\end{bmatrix}.
$$

Then

$$
p^W = {^{W}R}_{C}\,p^C + {^{W}t}_{C},
\qquad
p^C = {^{C}R}_{W}\,p^W + {^{C}t}_{W},
$$

with

$$
{^{C}R}_{W} = ({^{W}R}_{C})^{-1} = R_z(-10^\circ), \qquad
{^{C}t}_{W} = -({^{W}R}_{C})^{-1}\,{^{W}t}_{C}.
$$

---
---
$\textbf{Note}$

Rotation matrices $R \in SO(3)$ are orthogonal $$(R^\top R=I)$$ with $\det(R)=1$, hence

$$
({^{A}R}_{B})^{-1} = ({^{A}R}_{B})^\top = {^{B}R}_{A}.
$$
