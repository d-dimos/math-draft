---
layout: default
title: 
permalink: /topics/frames/
exclude: true
---

# Changing Coordinate Frames with SE(3) transformations

We follow the convention that active transformations (rotations and translations) act by first rotating and then translating, i.e., $p' = R\,p + t$.

Consider a coordinate frame $A$. Apply a rotation $R^A$ and then a translation $t^A$ (both expressed in $A$) to obtain a new frame $B$.

Now, say we have a point $p^A$ expressed w.r.t. frame $A$. To express it w.r.t. frame $B$, we apply the the transform that maps $B$ to $A$, **and we must express that w.r.t. $B$**. Since we use the “first rotate, then translate” convention, we first rotate by $(R^A)^{-1}$. The orientations now match. We cannot simply subtract $t^A$ because it is expressed in $A$. We must rotate it into $B$ before subtracting. Thus:

$$
p^B = (R^A)^{-1} \cdot p^A - (R^A)^{-1} \cdot t^A \qquad (1)
$$

Following our convention, the general conversion formula from $A$ to $B$ is

$$
p^B = {^{B}\!R}_{A} \cdot p^A + {^{B}\!t}_{A},
$$

which, by comparison with (1), gives
$$
{^{B}\!R}_{A} = (R^A)^{-1}, \qquad {^{B}\!t}_{A} = -\, (R^A)^{-1} \cdot t^A .
$$

At the same time we also have the relation
$$
p^A = {^{A}\!R}_{B} \cdot p^B + {^{A}\!t}_{B},
$$
and from (1) (by inversion) we obtain
$$
p^A = R^A \cdot p^B + t^A,
$$
so that
$$
{^{A}\!R}_{B} = R^A, \qquad {^{A}\!t}_{B} = t^A.
$$

This matches the intuition: to convert a point expressed w.r.t. frame $B$ into a representation w.r.t. frame $A$, we apply the the transform that maps $A$ to $B$, **(expressing that w.r.t. $A$)**.

Notice that both sides of the conversion formula from $X$ to $Y$ are vectors expressed w.r.t. frame $Y$.

