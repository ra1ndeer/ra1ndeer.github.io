# The Evidence Lower Bound (ELBO)

In a variational setting, it is common to assume that the observations, $$\mathbf{x}$$, are generated according to some random, latent, unobserved process, $$\mathbf{z}$$, and that said generative process can be parametrized by $$\theta$$, $$p_{\theta}(\mathbf{x}, \mathbf{z}).$$ However, since there is no way of observing the latent process, it is necessary to integrate this quantity out:

$$\log p_{\theta}(\mathbf{x}) = \log \int p_{\theta}(\mathbf{x}, \mathbf{z}) d \mathbf{z}$$

Variational inference concerns itself with this type of problems where there is some integral that cannot be computed analytically and requires some kind of approximation. By the definition of conditional probability, we can write $$p_{\theta}(\mathbf{x}, \mathbf{z})$$ as $$p_{\theta}(\mathbf{x} \mid \mathbf{z}) p(\mathbf{z})$$, where $$p_{\theta}(\mathbf{x}\mid\mathbf{z})$$ is the probability of a given observation $$\mathbf{x}$$ after observing $$\mathbf{z}$$ and $$p(\mathbf{z})$$ is the prior distribution of $$\mathbf{z}$$, over which some *a priori* assumption on its form is made:

$$\log \int p_{\theta}(\mathbf{x},\mathbf{z}) d\mathbf{z} = \log \int p_{\theta}(\mathbf{x}|\mathbf{z}) p(\mathbf{z}) d\mathbf{z}$$

Currently in our derivation we have a prior distribution for $$\mathbf{z}$$, which is basically an educated guess as to what the latent distribution might look like. One of the quantities of interest is the posterior distribution of $$\mathbf{z}$$, which we will approximate by some density function $$q$$ parametrized by $$\phi$$, $$q_{\phi}(\mathbf{z} \mid \mathbf{x})$$. This function can be interpreted as the updated belief of how $$\mathbf{z}$$ behaves after observing $$\mathbf{x}$$. For the sake of mathematical convenience, let us multiply and divide the previous result by this probability density:

$$\log \int p_{\theta}(\mathbf{x}|\mathbf{z})p(\mathbf{z}) d\mathbf{z} = \log \int \frac{q_{\phi}(\mathbf{z}|\mathbf{x})}{q_{\phi}(\mathbf{z}|\mathbf{x})} p_{\theta}(\mathbf{x}|\mathbf{z}) p(\mathbf{z}) d\mathbf{z}$$

We didn't produce the $$ q_{\phi}(\mathbf{z} \mid \mathbf{x}) $$ term just for kicks, we did it so we can write the integral as an expectation over $$ q_{\phi}(\mathbf{z} \mid \mathbf{x}) $$:

$$\log \int \frac{q_{\phi}(\mathbf{z}|\mathbf{x})}{q_{\phi}(\mathbf{z}|\mathbf{x})} p_{\theta}(\mathbf{x}|\mathbf{z}) p(\mathbf{z}) d\mathbf{z} = \log \mathbb{E}_{q_{\phi}(\mathbf{z}|\mathbf{x})} \left[ \frac{p_{\theta}(\mathbf{x}|\mathbf{z}) p(\mathbf{z})}{q_{\phi}(\mathbf{z}|\mathbf{x})} \right]$$

And now for the most important step in deriving the evidence lower bound, applying Jensen's inequality. The most common formulation of Jensen's inequality states that for a convex function $$f$$ and a random variable $$X$$, we have that $$f(\mathbb{E}[X]) \leq \mathbb{E}[f(X)]$$. However, the logarithm is a concave function. Fortunately, Jensen's inequality can also be stated for concave functions: if $$f$$ is a concave function and $$X$$ a random variable, we have that $$f(\mathbb{E}[X]) \geq \mathbb{E}[f(X)]$$. Applying this to the equation above yields:

$$\log \mathbb{E}_{q_{\phi}(\mathbf{z}|\mathbf{x})} \left[ \frac{p_{\theta}(\mathbf{x}|\mathbf{z}) p(\mathbf{z})}{q_{\phi}(\mathbf{z}|\mathbf{x})} \right] \geq \mathbb{E}_{q_{\phi}(\mathbf{z}|\mathbf{x})} \left[ \log \frac{p_{\theta}(\mathbf{x}|\mathbf{z}) p(\mathbf{z})}{ q_{\phi}(\mathbf{z}|\mathbf{x})} \right] =$$

$$= \mathbb{E}_{q_{\phi}(\mathbf{z}|\mathbf{x})}\left[ \log p_{\theta}(\mathbf{x}|\mathbf{z}) \right] - \mathbb{E}_{q_{\phi}(\mathbf{z}|\mathbf{x})}\left[ \log \frac{q_{\phi}(\mathbf{z}|\mathbf{x})}{p(\mathbf{z})} \right]$$

For those familiar with information theory, the rightmost term in the equation above is the Kullback-Leibler divergence of $$q_{\phi}(\mathbf{z} \mid \mathbf{x})$$ relative to $$p(\mathbf{z})$$, which means that we can summarize all the calculations above as:

$$\log p_{\theta}(\mathbf{x}) \geq \mathbb{E}_{q_{\phi}(\mathbf{z}|\mathbf{x})}\left[ \log p_{\theta}(\mathbf{x}|\mathbf{z}) \right] - \mathcal{D}_{KL} \left[ q_{\phi}(\mathbf{z}|\mathbf{x}) || p(\mathbf{z}) \right] \equiv $$

$$\equiv \mathcal{L}(\theta, \phi)$$

In other words, we have successfully derived the evidence lower bound (ELBO), $$\mathcal{L}(\theta, \phi)$$.

### References

* Diederik P. Kingma and Max Welling, *Auto-Encoding Variational Bayes*, arXiv:1312.6114