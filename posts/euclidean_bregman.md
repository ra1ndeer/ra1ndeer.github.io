# Euclidean Distance and the Bregman Divergence

---

## A very brief introduction of manifolds and coordinate systems

Manifolds are locally equivalent to $$n$$-dimensional Euclidean spaces, meaning that we can introduce a local coordinate system for a manifold $$M$$ such that each point is uniquely specified by its coordinates in a neighborhood:

$$\xi = (\xi_1, \dots, \xi_n)$$

This can be used, for example, to specify a Gaussian distribution in terms of its parameters ($$\xi = (\mu, \sigma), \sigma > 0$$). It should be noted that this coordinate system **is not** unique, we could equivalently define a coordinate system based on the first and second moments of the gaussian distribution, $$\zeta = (\mathbb{E}[X]=\mu, \mathbb{E}[X^2]=\mu^2 + \sigma^2)$$

## Divergences and Manifolds

---

Now lets take two points $$P$$ and $$Q$$ whose coordinates in some manifold $$M$$ are given by $$\xi_P$$ and $$\xi_Q$$. T divergence between $$P$$ and $$Q$$, $$D[P : Q]$$, is a function of the coordinates of those points such that:

* $$D[P : Q] \geq 0$$;
* $$D[P:Q] = 0$$ if and only if $$P = Q$$;
* For $$P$$ and $$Q$$ sufficiently close, letting $$\xi_Q = \xi_P + d\xi$$, the Taylor expansion of the divergence is given by $$D[\xi_P : \xi_P + d\xi] = \frac{1}{2} \sum g_{ij}(\xi_P) d\xi_i d\xi_j + O(\mid d\xi\mid^3)$$, where the matrix $$G=(g_{ij})$$ depends on $$\xi_P$$ and is positive-definite.

There is one very important detail about divergences. While they are commonly used as a representation of how much the two points $$P$$ and $$Q$$ differ, it is not a distance, since, in general, a divergence is not symmetric and does not satisfy the triangle inequality.

Taking $$P$$ and $$Q$$ to be sufficiently close, we can define the square of an infinitesimal distance, $$ds$$, using the divergence Taylor expansion:

$$ds^2 = 2D[\xi : \xi + d\xi] = \sum g_{ij} d\xi_i d\xi_j$$

Since $$G=(g_{ij})$$ is positive definite and the square of the local distance between two nearby points is given by the equation above, we say that the divergence $$D$$ provides the manifold $$M$$ with a Riemannian structure.

## Convex Functions

---

A convex function is a function $$\psi(\xi)$$ such that the following inequality holds (for $$\lambda \in [0; 1]$$):

$$\lambda \psi(\xi_1) + (1 - \lambda) \psi(\xi_2) \geq \psi(\lambda \xi_1 + (1- \lambda) \xi_2)$$

Additionally, convexity can be defined in terms of the Hessian (second order derivative) of a function. If the function $$\psi(\xi)$$ is differentiable, then it is convex if and only if its Hessian is positive-definite.

## Bregman Divergence

---

Let us now consider two points, $$\xi$$ and $$\xi_0$$, of our manifold $$M$$, and some convex function $$\psi$$. The tangent hyperplane at the point $$\xi_0$$ is given by

$$z = \psi(\xi_0) + \nabla\psi(\xi_0) \cdot (\xi - \xi_0)$$

where $$z$$ is the "vertical" axis and $$\nabla$$ is the gradient operator. Since the function $$\psi$$ is convex, the hyperplane given by the equation above is always below the function $$\psi$$, meaning that it is a supporting hyperplane. Now take the point $$\xi$$ and the value of $$\psi$$ at $$\xi$$, $$\psi(\xi)$$. To evaluate how high $$\psi(\xi)$$ is relative to the supporting hyperplane of $$\psi$$ at $$\xi_0$$, we simply write out their difference:

$$D_{\psi}[\xi : \xi_0] = \psi(\xi) - \psi(\xi_0) - \nabla \psi(\xi_0) \cdot(\xi - \xi_0)$$

The quantity above immediately satisfies the criteria for divergences (due to the definition of convex function) and is named the Bregman divergence. Since it is a function, not only of the coordinates of the two points, but also of the chosen convex function there are many Bregman divergences as there are convex functions.

## Obtaining the Euclidean Distance from a Bregman Divergence

---

Consider a point $$\xi$$ and the origin. The Euclidean distance between $$\xi$$ and the origin is simply given by:

$$\psi(\xi) = \frac{1}{2} \sum \xi_i^2$$

Easily enough, its gradient is $$\nabla \psi(\xi_0) = (\xi_{0,1}, \dots, \xi_{0, n}) = \xi_0$$. Substituting these expressions in the Bregman divergence yields the following:

$$D_{\psi}[\xi : \xi_0] = \frac{1}{2} \sum \xi_i^2 - \frac{1}{2} \sum \xi_{0, i}^s - \xi_0 \cdot (\xi - \xi_0) =$$

$$=\frac{1}{2} \sum \xi_i^2 - \frac{1}{2}  \sum \xi_{0,i}^2 - \sum (\xi_i \xi_{0,i} - \xi_{0,i}^2) =$$

$$= \frac{1}{2} \sum \xi_i^2 + \frac{1}{2} \sum \xi_{0,i}^2 - \sum \xi_i \xi_{0,i} = $$

$$= \frac{1}{2} \sum (\xi_i - \xi_{0,i})^2 = \frac{1}{2} \mid \xi - \xi_0 \mid^2$$

And thus, we readily conclude that the Euclidean distance can be written as

$$\mid \xi - \xi_0 \mid =  \sqrt{2 D_{\psi}[\xi : \xi_0]}$$

Knowledge is knowing tomato is a fruit, and wisdom is knowing not to use it on a fruit salad, so we'll probably just keep using the usual form of the Euclidean distance whenever it is necessary.

### References

---

* Shun-ichi Amari, *Information Geometry and Its Applications*, (2016)