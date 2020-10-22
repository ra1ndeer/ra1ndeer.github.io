# Legendre Transformation, Dual Coordinate Systems and Dual Divergence

---

## Dualistic Structure of Coupled Coordinate Systems on Manifolds

---

A $$n$$-dimensional manifold $$M$$ is a topological space with specific characteristics. Informally, it can be understood as a set of points such that each point has $$n$$-dimensional extensions in its neighborhood (as Prof. Amari states in his book, *Information Geometry and its Applications*) or, in an even more intuitive way, as a topological space that locally behaves as an $$n$$-dimensional Euclidean space, $$\mathbb{R}^n$$ (as explained by Prof. Nielsen in his article *An Elementary Introduction to Information Geometry*). Due to this local structure, we can introduce local coordinate systems (although we don't really have to, but it is, more often than not, convenient to do so):

$$\xi = (\xi_1, \dots, \xi_n)$$ 

We consider a convex function $$\psi$$ of $$\xi$$, $$\psi(\xi)$$. If we take the gradient of this function with respect to $$\xi$$, we obtain a new coordinate system

$$\xi^* = \nabla \psi(\xi)$$

By definition, these new coordinates are equal to the normal vector of the supporting tangent hyperplane at $$\xi$$. Additionally, each point of the manifold has its own normal vector, meaning that point can be either specified by its coordinates, or by its normal vector. This leads to the conclusion that the transformation between $$\xi$$ and $$\xi^*$$ is one-to-one differentiable and $$\xi^*$$ is just another coordinate system on $$M$$. This transformation is commonly known as the Legendre transformation and it has a dualistic structure concerning two coupled coordinate systems $$\xi$$ and $$\xi^*$$. The first step to showing this, is to define some new function of $$\xi^*$$, which we will denote by $$\psi^*(\xi^*)$$, which is given by:

$$\psi^*(\xi^*) = \xi \cdot \xi^* - \psi(\xi)$$

where we are not assuming that $$\xi$$ has no dependencies. In fact, we are assuming that $$\xi$$ is a function of $$\xi^*$$, $$\xi = f(\xi^*)$$, with $$f$$ being the inverse function of $$\xi^* = \nabla \psi(\xi)$$, which we have to keep in mind since we are going to take the derivative with respect to $$\xi^*$$ of the above equation:

$$\nabla \psi^*(\xi^*) = \nabla\left( \xi \cdot \xi^* - \psi(\xi) \right) =$$

$$=\nabla(\xi \cdot \xi^*) - \nabla \psi(\xi) =$$

$$= \nabla \left( \sum_i \xi_i \xi_i^* \right) - \nabla \psi(\xi) \frac{\partial \xi}{\partial \xi^*} =$$

$$=\xi + \xi^* \frac{\partial \xi}{\partial \xi^*} - \nabla \psi(\xi) \frac{\partial \xi}{\partial \xi^*}$$

Now note that we have $$\xi^* = \nabla \psi (\xi)$$, which means that the last two terms in the equation above cancel out and we are left with the missing piece of our dualistic structure:

$$\nabla \psi^*(\xi^*) = \xi \quad \text{and} \quad \nabla\psi(\xi) = \xi^*$$

where $$\psi^*$$ is called the Legendre dual of $$\psi$$. It remains to show that $$\psi^*$$ is a convex function. For that matter, we evaluate its Hessian, which we will denote by $$G^*$$:

$$G^*(\xi^*) = \nabla \left( \nabla \psi^*(\xi^*)\right) = \nabla( \xi) = \frac{\partial \xi}{\partial \xi^*}$$

which is the Jacobian (first order derivative) of the inverse transformation from $$\xi^*$$ to $$\xi$$, *i.e*, the Jacobian of $$\nabla \psi^*(\xi^*)$$. Note that the inverse of this derivative is given by

$$\left( \frac{\partial \xi}{\partial \xi^*} \right)^{-1} = \frac{\partial \xi^*}{\partial \xi} = \nabla \xi^* = \nabla \nabla \psi(\xi) = G(\xi)$$

In other words, $$G^*$$ is the inverse of the Hessian of $$\psi$$ (which we now denote by $$G$$). Since $$G$$ is a positive-definite matrix, then so is is its inverse and this directly implies that $$\psi^*$$ is a convex function of $$\xi^*$$.

## Dual Bregman Divergence

---

From the dual convex function, we can derive the dual Bregman divergence:

$$D_{\psi^*}[\xi^* : \xi^{*'}] = \psi^*(\xi^*) - \psi^*(\xi^{*'}) - \nabla \psi^*(\xi^{*'}) \cdot (\xi^* - \xi^{*'})$$

But note the following about this divergence. If we substitute $$\nabla \psi^*(\xi^{*'}) = \xi^{'}$$ and $$\psi^*(\xi^*) = \xi \cdot \xi^* - \psi(\xi)$$, we obtain the following:

$$\psi^*(\xi^*) - \psi^*(\xi^{*'}) - \nabla \psi^*(\xi^{*'}) \cdot (\xi^* - \xi^{*'}) =$$

$$=\psi^*(\xi^*) - \psi^*(\xi^{*'}) - \xi^{'} \cdot(\xi^* - \xi^{*'}) =$$

$$= \xi \cdot \xi^* - \psi(\xi) - \xi^{'} \cdot \xi^{*'} + \psi(\xi^{'}) - \xi^{'}\cdot\xi^{x} + \xi^{'} \cdot \xi^{*'} =$$

$$= \psi(\xi^{'}) - \psi(\xi) - \xi^{*'} \cdot(\xi - \xi^{'}) =$$

$$= \psi(\xi^{'}) - \psi(\xi) - \nabla \psi(\xi) \cdot (\xi^{'} - \xi) =$$

$$= D_{\psi}[\xi^{'} : \xi]$$

In other words, the primal divergence is equal to the dual divergence except for the order of the two points under consideration, which are exchanged. It would be fairly convenient if we could obtain a self-dual expression for the divergence, *i.e.*, an expression for the divergence which is the same as its dual divergence. That's what Theorem 1.1 from *Information Geometry and its Applications* grants us. It is stated as follows: The divergence from $$P$$ to $$Q$$ derived from a convex function $$\psi(\xi)$$ is written as:

$$D_{\psi}[P : Q] = \psi(\xi_P) + \psi^*(\xi_Q^*) - \xi_P \cdot \xi_Q^*$$

where $$\xi_P$$ is the coordinates of $$P$$ in the $$\xi$$ coordinate system and $$\xi_Q^*$$ is the coordinates of $$Q$$ in the $$\xi^*$$ coordinate system. The proof is fairly straightforward. We begin with the usual expression for the Bregman divergence:

$$D_{\psi}[P : Q] = \psi(\xi_P) - \psi(\xi_Q) - \nabla \psi(\xi_Q) \cdot (\xi_P - \xi_Q)$$

and substitute $$\xi_Q^* = \nabla \psi(\xi_Q)$$ while rearranging the expression a bit:

$$\psi(\xi_P) - \psi(\xi_Q) - \nabla \psi(\xi_Q) \cdot (\xi_P - \xi_Q) = $$

$$=\psi(\xi_P) - \psi(\xi_Q) - \xi_Q^* \cdot (\xi_P - \xi_Q) =$$

$$=\psi(\xi_P) - \psi(\xi_Q) - \xi_Q^* \cdot \xi_P + \xi_Q^* \cdot \xi_Q$$

The final step is simple substitution of $$\psi^*(\xi^*) = \xi \cdot \xi^* - \psi(\xi)$$, which immediately leads to the desired result.

### References

* Shun-ichi Amari, *Information Geometry and its Applications*, (2016)
* Frank Nielsen, *An Elementary Introduction to Information Geometry*, (2020)