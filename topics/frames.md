---
layout: default
title: 
permalink: /topics/frames/
exclude: true
---

# Relating Active Frame Motion to Passive Coordinate Mapping in SE(3)

We follow the convention that rigid transformations act by first rotating and then translating, i.e., $p' = R\,p + t$. We use the following notations:

- $p^X$: the coordinates of a point expressed w.r.t. frame $X$.

- $^Y\square_X$(equivalently $$\square^Y_X$$): $X$ is the source frame, and $Y$ is the expressed-in/target frame. For example, $${^{Y}\!R}_{X}$$ is a rotation expressed w.r.t. frame $Y$ applied to coordinates initially expressed in $X$.

Consider a coordinate frame $A$. Apply a rotation $R^A$ and then a translation $t^A$ (both expressed in $A$) to frame $A$ to obtain a new frame $B$ (this is an **active** motion of the frame).

Let $p^A$ be the coordinates of a point w.r.t. $A$ and $p^B$ the coordinates of the same point w.r.t. $B$.

To convert $p^A$ into $p^B$ (a **passive** change of coordinates), we use

$$
p^B = {^{B}\!R}_{A} \cdot p^A + {^{B}\!t}_{A} \qquad (1)
$$

and conversely

$$
p^A = {^{A}\!R}_{B} \cdot p^B + {^{A}\!t}_{B}. \qquad (2)
$$

From now on we focus on the $A \to B$ mapping. The reverse is analogous.

The pair $$({^{B}R}_{A},{^{B}t}_{A})$$ maps coordinates expressed in $A$ to coordinates expressed in $B$ via $(1)$. Note that this pair is **expressed in $B$**. By contrast, the active motion that *carried* frame $A$ to frame $B$ used $(R^A,t^A)$ **expressed in $A$**. In general, $t^A \neq {^{B}t}_{A}$ because they live in different frames.

What is the relation between the active $t^A$ and the passive $${^{B}\!t}_{A}$$?  
To map points from $A$ to $B$ we apply the inverse of the frame motion that carried $A$ to $B$: first rotate by $(R^A)^{-1}$ so the axes align, then express $t^A$ in frame $B$ (i.e., multiply by $(R^A)^{-1}$) and subtract:

$$
p^B = (R^A)^{-1} \cdot p^A - (R^A)^{-1} \cdot t^A. \qquad (3)
$$

Multiplying $(3)$ on the left by $R^A$ gives:

$$
p^A = R^A \cdot p^B + t^A. \qquad (4)
$$

Comparing $(1)$ with $(3)$ yields:

$$
{^{B}\!R}_{A} = (R^A)^{-1}, \qquad {^{B}\!t}_{A} = - (R^A)^{-1} \cdot t^A.
$$

Comparing $(2)$ with $(4)$ yields:

$$
{^{A}\!R}_{B} = R^A, \qquad {^{A}\!t}_{B} = t^A.
$$

Equivalently,

$$
{^{B}\!R}_{A} = ({^{A}\!R}_{B})^{-1}, \qquad
{^{B}\!t}_{A} = -({^{A}\!R}_{B})^{-1} \cdot {^{A}\!t}_{B}.
$$

The pair $$({^{A}\!R}_{B},{^{A}\!t}_{B})$$ is the **pose of frame $B$ w.r.t. frame $A$** and it maps points expressed in $B$ to their representations expressed in $A$.

---
---
$\textbf{Example}$

Consider a (world) frame $W$ and a camera with initial frame $C_{\text{old}}$ that is aligned with $W$. We actively rotate the camera by $10^\circ$ about $z^W$. Then we translate its center to
$$\begin{bmatrix}7\\2\\9\end{bmatrix}$$
in $W$. The final camera frame is $C$.

Based on our previous analysis:

- the camera pose w.r.t. $W$ is $$\big({^W\!R_C}, {^W\!t_C}\big)$$ and it maps $$p^C \mapsto p^W$$:

$$
{^{W}\!R}_{C} = R_z(10^\circ),
\qquad
{^{W}\!t}_{C} = \begin{bmatrix}7\\2\\9\end{bmatrix},
$$

$$
p^W = {^{W}\!R}_{C}\cdot p^C + {^{W}\!t}_{C}
$$

- to map $p^W \mapsto p^C$:

$$
p^C = {^{C}\!R}_{W}\cdot p^W + {^{C}\!t}_{W},
$$
 
$$
{^{C}\!R}_{W} = ({^{W}\!R}_{C})^{-1} = R_z(-10^\circ),
\qquad
{^{C}\!t}_{W} = -({^{W}\!R}_{C})^{-1} \cdot {^{W}\!t}_{C}.
$$

---
---
$\textbf{Note}$

Rotation matrices are orthogonal with $det(R)=1$, hence: $$({^{A}R}_{B})^{-1} = ({^{A}R}_{B})^\top = {^{B}R}_{A}$$.
