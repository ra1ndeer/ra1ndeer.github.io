# A Bit Theory for Normalizing Flows
---
## Motivation

Consider a case similar to that of variational auto-encoders, where we have observations $$\mathbf{x}$$ governed by some unobserved latent random variable $$\mathbf{z}$$ through the model's parameters $$\theta$$. The existence of said latent variable mean that if we want to calculate the probability of some observation in the context of our model, we have to, somehow, take into account the effect of $$\mathbf{z}$$, because:

$$ \log p_{\theta}(\mathbf{x}) =  \log \int p_{\theta}(\mathbf{x}, \mathbf{z}) d\mathbf{z} = \log \int p_{\theta}(\mathbf{x}\mid\mathbf{z})p(\mathbf{z})d\mathbf{z}$$

In other words, we have to integrate out the effects of $$\mathbf{z}$$. For this matter, we can derive the so called evidence lower bound (ELBO), by introducing an approximate posterior distribution over the latent variables parametrized by $$\phi$$, $$q_{\phi}(\mathbf{z}\mid\mathbf{x})$$:

$$\log \int p_{\theta}(\mathbf{x}\mid\mathbf{z})p(\mathbf{z}) d\mathbf{z} = \log \int \frac{q_{\phi}(\mathbf{z}\mid\mathbf{x})}{q_{\phi}(\mathbf{z}\mid\mathbf{x})} p_{\theta}(\mathbf{x}\mid\mathbf{z})p(\mathbf{z}) d\mathbf{z} = $$

$$=\log \int \frac{p_{\theta}(\mathbf{x}\mid\mathbf{z})}{q_{\phi}(\mathbf{z}\mid\mathbf{x})} p(\mathbf{z}) q_{\phi}(\mathbf{z}\mid\mathbf{x}) d\mathbf{z} = \log \mathbb{E}_{q_{\phi}(\mathbf{z}\mid\mathbf{x})}\left[ \frac{p_{\theta}(\mathbf{x}\mid\mathbf{z})}{q_{\phi}(\mathbf{z}\mid\mathbf{x})} p(\mathbf{z}) \right]$$

Now, making use of Jensen's inequality for concave functions, we obtain:

$$\log \mathbb{E}_{q_{\phi}(\mathbf{z}\mid\mathbf{x})}\left[ \frac{p_{\theta}(\mathbf{x}\mid\mathbf{z})}{q_{\phi}(\mathbf{z}\mid\mathbf{x})} p(\mathbf{z}) \right] \geq \mathbb{E}_{q_{\phi}(\mathbf{z}\mid\mathbf{x})}\left[\log \frac{p(\mathbf{z})}{q_{\phi}(\mathbf{z}\mid\mathbf{x})}  + \log p_{\theta}(\mathbf{x}\mid\mathbf{z})\right]$$

$$ = - \mathcal{D}_{KL} \left[ q_{\phi}(\mathbf{z}\mid\mathbf{x}) \parallel p(\mathbf{z}) \right] + \mathbb{E}_{q_{\phi}(\mathbf{z}\mid\mathbf{x})}\left[ \log p_{\theta}(\mathbf{x}\mid\mathbf{z}) \right]$$

The Kullback-Leibler divergence term between the prior and the posterior acts as a regularizer while the other term can be interpreted as a reconstruction error. However good the results from VAEs are, we are still constraining our model to the choice of prior and posterior distribution (see [this post](./vanilla_vae.html) for more details on VAEs). Ideally, we want to be able to specify posterior distributions that are complex enough to adequately explain our observations. That's where Normalizing Flows come in.

## Normalizing Flows
---
In short, **normalizing flows are a set of diffeomorphisms applied successively to some probability density function that, in the end, produce a valid probability density**. Let's break this definition down so that we can better understand why this is a useful tool. At the core of normalizing flows is the change of variable rule for probability density functions. Consider the random variable, $$\mathbf{z}_0 \in \mathbb{R}^D$$, with density function $$q_0(\mathbf{z}_0)$$ and the diffeomorphism (differentiable invertible mapping whose inverse is also differentiable) $$f : \mathbb{R}^D \rightarrow \mathbb{R}^D$$. The probability density function of $$\mathbf{z} = f(\mathbf{z}_0)$$ is given by the change of variable rule for probability density functions:

$$ q(\mathbf{z'})  = q_0(\mathbf{z}_0) \left| \det \frac{\partial f^{-1}}{\partial \mathbf{z}} \right| = q_0(\mathbf{z}_0) \left| \det \frac{\partial f}{\partial \mathbf{z}_0} \right|^{-1}$$

Where the last step was possible since:

$$1 = \frac{\partial \mathbf{z}_0}{\partial \mathbf{z}_0} = \frac{\partial f^{-1}}{\partial (f(\mathbf{z}_0))} \frac{\partial f}{\partial \mathbf{z}_0} = \frac{\partial f^{-1}}{\partial \mathbf{z}} \frac{\partial f}{\partial \mathbf{z}_0} \Leftrightarrow \frac{\partial f}{\partial \mathbf{z}_0} = \left[ \frac{\partial f^{-1}}{\partial \mathbf{z}} \right]^{-1}$$

This can be easily extended for a collection of diffeomorphisms, $$\{f_i \}_{i=1}^K$$, and the random variable that is obtained through their composition, $$\mathbf{z}_K = f_K \circ ... \circ f_1(\mathbf{z}_0)$$:

$$ q_K(\mathbf{z}_K) = q_0 (\mathbf{z}_0) \prod_{i=1}^K \det \left| \frac{\partial f_k}{\partial \mathbf{z}_i} \right|^{-1}  \Leftrightarrow$$

$$\Leftrightarrow \log q_K(\mathbf{z}_K) = \log q_0(\mathbf{z}_0) - \sum_{i=1}^K \log \det \left| \frac{\partial f_i}{\partial \mathbf{z}_i} \right|$$

An interesting property of these transformations is that expectation of the form $$\mathbb{E}_{q_K(\mathbf{z}_K)}\left[h(\mathbf{z}_K)\right]$$ can be taken with respect to the base distribution, $$q_0(\mathbf{z}_0)$$, due to the law of the unconscious statistician and the fact that $$h(\mathbf{z}_K) = h(f_K \circ ... \circ f_1 (\mathbf{z}_0))$$ and thus, we can easily form Monte Carlo estimates for such expectations:

$$ \mathbf{E}_{q_K(\mathbf{z}_K)} \left[ h(\mathbf{z}_K) \right] \approx \frac{1}{L} \sum_{l=1}^L h(f_K \circ ... \circ f_1(\mathbf{z}_0^l))$$

However, one might notice that these transformations require computing the determinant of a matrix, an operation which is typically slow. That is the main trick behind most normalizing flows: swift computation of the matrix determinant.

## ELBO with Normalizing Flows
---
First, notice that the ELBO can be written in a more useful format as:

$$ELBO(\phi, \theta) = \mathbb{E}_{q_{\phi}(\mathbf{z}\mid\mathbf{x})} \left[ \log q_{\phi}(\mathbf{z}\mid\mathbf{x}) - \log p_{\theta}(\mathbf{x}, \mathbf{z}) \right] =$$

$$= \mathbb{E}_{q_{\phi}(\mathbf{z}\mid\mathbf{x})} \left[ \log q_{\phi}(\mathbf{z}\mid\mathbf{x}) \right] - \mathbb{E}_{q_{\phi}(\mathbf{z}\mid\mathbf{x})} \left[ \log p_{\theta}(\mathbf{x}\mid\mathbf{z}) \right] - \mathbb{E}_{q_{\phi}(\mathbf{z}\mid\mathbf{x})} \left[ \log p(\mathbf{z}) \right]$$

In the context of variational inference, $$p(\mathbf{z})$$ is our prior distribution, an assumption about the model that, when using VAEs, its common to take as a unit Gaussian distribution. The $$\mathbb{E}_{q_{\phi}(\mathbf{z}\mid\mathbf{x})} \left[ \log p_{\theta}(\mathbf{x}\mid\mathbf{z}) \right]$$ is the reconstruction loss and depends on the type of data we are considering, while the remaining term is the posterior latent distribution. Using normalizing flows, we approximate the posterior distribution by $$q_{\phi}(\mathbf{z}\mid\mathbf{x}) \approx q_K(\mathbf{z}_K)$$, and thus all expectations can be taken with respect to the base distribution, which is assumed to be Gaussian with mean $$\mu \in \mathbb{R}^D$$ and diagonal covariance structure, $$\sigma^2 I$$, with $$\sigma^2 \in \mathbb{R}^D$$ (similar to a VAE). Switching $$q_{\phi}(\mathbf{z}\mid \mathbf{x})$$ by $$q_k(\mathbf{z}_k)$$ yields the following ELBO:

$$ELBO(\phi, \theta) = \mathbb{E}_{q_0(\mathbf{z}_0)} \left[ \log q_0(z_0) \right] - \sum_{i=1}^k \mathbb{E}_{q_0(\mathbf{z}_0)}\left[ \log \det \left| \frac{\partial f_i}{\partial z_i} \right| \right] -$$

$$- \mathbb{E}_{q_{0}(\mathbf{z}_0)} \left[ \log p_{\theta}(\mathbf{x}\mid\mathbf{z}_k) \right] - \mathbb{E}_{q_{0}(\mathbf{z}_0)} \left[ \log p(\mathbf{z}_k) \right]$$

## References
---
Danilo Jimenez Rezende, Shakir Mohamed, *Variational Inference with Normalizing Flows*, (2016), arXiv:1505.05770v6