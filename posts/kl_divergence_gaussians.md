# The Kullback-Leibler Divergence between Multivariate Normal Distributions

---

We say that a random vector $$\vec X = (X_1, \dots, X_D)$$ follows a multivariate Normal distribution with parameters $$\vec\mu \in \mathbb{R}^D$$ and $$\Sigma \in \mathbb{R}^{D \times D}$$ if it has a probability density given by:

$$f(\vec x; \vec \mu, \Sigma) = \frac{1}{\sqrt{(2 \pi)^D \mid \Sigma \mid}} \exp \left[ -\frac{1}{2} (\vec x - \vec \mu)^T \Sigma^{-1}(\vec x - \vec \mu) \right]$$

Where $$\mid \Sigma \mid$$ is the determinant of $$\Sigma$$. Since we are going to need the log-density function, let us preemptively calculate it:

$$\log f(\vec x; \vec \mu, \Sigma) = -\frac{1}{2} \log \left[ (2 \pi)^D \mid \Sigma \mid \right] - \frac{1}{2}(\vec x - \vec \mu)^T \Sigma^{-1}(\vec x - \vec \mu) =$$

$$ = -\frac{1}{2} D \log(2 \pi) - \frac{1}{2} \log \mid \Sigma \mid - \frac{1}{2}(\vec x - \vec \mu)^T \Sigma^{-1}(\vec x - \vec \mu)$$

So now we are all set to start calculating the Kullback-Leibler divergence (see [this post](./primer/info_theory.html) if you do not know/remember what this divergence is) between two such distributions. Let $$ p(\vec x) $$ be a multivariate Normal density parametrized by $\vec \mu_0$ and $$\Sigma_0$$, and let $$q(\vec x)$$ be of the same kind but parametrized by $$\vec \mu_1$$ and $$\Sigma_1$$ instead. We begin by splitting the logarithm:

$$\mathcal{D}_{KL} \left[ p \parallel q \right] = \mathbb{E}_{p(x)} \left[ \log \frac{p(x)}{q(x)} \right] = $$

$$= \mathbb{E}_{p(x)} \left[ \log p(x) - \log q(x) \right]$$

Plugging in the log-density functions yields:

$$ \mathbb{E}_{p(x)} \left[ - \frac{1}{2} D \log (2 \pi) - \frac{1}{2} \log \mid \Sigma_0\mid - \frac{1}{2}(\vec x - \vec \mu_0)^T \Sigma_0^{-1} (\vec x - \vec \mu_0) \right] + $$

$$+ \mathbb{E}_{p(x)} \left[ \frac{1}{2} D \log (2 \pi) + \frac{1}{2} \log \mid \Sigma_1 \mid + \frac{1}{2} (\vec x - \vec \mu_1)^T \Sigma_1^{-1} (\vec x - \vec \mu_1) \right] = $$

$$= \frac{1}{2} \log \frac{\mid \Sigma_1 \mid}{\mid \Sigma_0 \mid} + \frac{1}{2} \mathbb{E}_{p(x)} \left[ (\vec x - \vec \mu_1)^T \Sigma_1^{-1} (\vec x - \vec \mu_1) \right] - $$

$$ - \frac{1}{2} \mathbb{E}_{p(x)} \left[ (\vec x - \vec \mu_0)^T \Sigma_0^{-1} (\vec x - \vec \mu_0) \right]  $$

And now for a painfully obvious statement: the trace of a number is the number itself, *i.e*, if $$x \in \mathbb{R}$$ then $$tr \{ x\} = x$$. Since the values inside the expectations are quadratic forms, we can simply take their trace without changing anything else in the equation. But wait, there's one more painfully obvious statement. The trace can also be taken with respect to the expectation, since it is also a single number. So why do we need the trace? Because it is cyclic, *i.e.*, $$tr\{ABC\} = tr\{ CAB \} = tr\{ BCA \}$$. This means that we can cycle the trace factors freely. And that's exactly what we'll do:

$$ \mathbb{E}_{p(\vec x)} \left[ (\vec x - \vec \mu_0)^T \Sigma_0^{-1} (\vec x - \vec \mu_0) \right] = $$

$$= \mathbb{E}_{p(\vec x)} \left[ tr\{(\vec x - \vec \mu_0)^T \Sigma_0^{-1} (\vec x - \vec \mu_0)\} \right] = $$

$$= \mathbb{E}_{p(\vec x)} \left[ tr\{(\vec x - \vec \mu_0) (\vec x - \vec \mu_0)^T \Sigma_0^{-1}\} \right] = $$

$$= tr\{\mathbb{E}_{p(\vec x)} \left[(\vec x - \vec \mu_0) (\vec x - \vec \mu_0)^T \Sigma_0^{-1} \right]\}$$

Note that an expectation is simply an integral in $$\vec x$$ weighted by $$p(\vec x)$$. So, naturally, we can place $$\Sigma_0^{-1}$$ outside of the expectation:

$$tr\{\mathbb{E}_{p(\vec x)} \left[(\vec x - \vec \mu_0) (\vec x - \vec \mu_0)^T \Sigma_0^{-1} \right]\} = $$

$$= tr\{ \mathbb{E}_{p(\vec x)} \left[ (\vec x - \vec \mu_0) (\vec x - \vec \mu_0)^T \right] \Sigma_0^{-1}\}$$

All this had one simple objective: now the expectation is, by definition, $$\Sigma_1$$, and thus:

$$tr\{ \mathbb{E}_{p(\vec x)} \left[ (\vec x - \vec \mu_0) (\vec x - \vec \mu_0)^T \right] \Sigma_0^{-1}\} = tr\{ \Sigma_0 \Sigma_0^{-1} \} = $$

$$= tr \{ I \} = D $$

Now all that is left is to somehow rid ourselves of the other expectation. Unfortunately, we can use the same reasoning we just did since the expectation is being taken with respect to a different density than that being integrated. However, there is one neat trick pertaining to this exact situation:

$$\mathbb{E}_{p(\vec x )} \left[ (\vec x - \vec \mu_1) \Sigma_1^{-1} (\vec x - \vec \mu_1) \right] = $$

$$= (\vec \mu_0 - \vec \mu_1)^T \Sigma_1^{-1} (\vec \mu_0 - \vec \mu_1) + tr \{ \Sigma_1^{-1} \Sigma_0 \}$$

Finally, the Kullback-Leibler divergence is given by:

$$\mathcal{D}_{KL}\left[ p \parallel q \right] = \frac{1}{2} \log \frac{\mid \Sigma_1 \mid}{\mid \Sigma_0 \mid } - \frac{D}{2} + $$

$$+ \frac{1}{2} (\vec \mu_0 - \vec \mu_1)^T \Sigma_1^{-1} (\vec \mu_0 - \vec \mu_1) + \frac{1}{2} tr \{ \Sigma_1^{-1} \Sigma_0 \}$$

## An Application in the Context of Variational Autoencoders

---

A common application of the Kullback-Leibler divergence between multivariate Normal distributions is the Variational Autoencoder, where this divergence, an integral part of the evidence lower bound, is calculated between an approximate posterior distribution, $$q_{\phi}(\vec z \mid \vec x)$$ and a prior distribution $$p(\vec z)$$. The most common choice for the prior is to take a multivariate standard Normal distribution, $$\mathcal{N}(\vec 0, I)$$, and, for the approximate posterior, it is usual to take a multivariate Normal distribution with diagonal covariance structure. Enforcing the diagonal covariance condition ensures latent feature disentanglement, *i.e*, each of the latent features models a specific part of the input without affecting the rest. In other words, the latent space dimensions are independent between themselves.

It may seem odd as one of the main things that is exhaustively repeated is that if two variables are uncorrelated, it does not mean that they are independent. However, in the case of the multivariate Normal distribution, this is true. To see this, let us assume $$\vec X = (X_1, \dots, X_D) \sim \mathcal{N}(\mu, \sigma^2 I)$$, where $$\sigma^2 = diag \{ \sigma_1^2, \dots, \sigma_D^2 \}$$. Then, we can write the density as

$$f(\vec x) = \frac{1}{\sqrt{(2 \pi)^D \mid \sigma^2 I \mid }} \exp \left[- \frac{1}{2} (\vec x - \vec \mu)^T (\sigma^2 I)^{-1} (\vec x - \vec \mu) \right]$$

Note that the matrix $$\sigma^2 I$$ is a diagonal matrix, meaning that its inverse is simply $$(1 / \sigma^2 )I$$, where our notation abuse represents $$(1 / \sigma^2 ) = diag \{ 1/\sigma_1^2 , \dots, 1 / \sigma_D^2 \}$$. This means that the quadratic product inside the exponential function can be written in much simpler terms. The same is true for the determinant of this matrix, since the determinant of a diagonal matrix is the product of its diagonal entries:

$$f(\vec x) = \frac{1}{ \sqrt{(2 \pi)^D \prod_{i=1}^D \sigma_i^2}} \exp \left[- \frac{1}{2} \sum_{i=1}^D \frac{(x_i - \mu_i)^2}{\sigma_i^2} \right]$$

But now we can factorize the entire expression as:

$$f(\vec x) = \prod_{i = 1}^D \frac{1}{\sqrt{2 \pi \sigma_i^2}} \exp \left[ - \frac{1}{2 \sigma_i^2} (x_i - \mu_i) \right]$$

Which is just a product of univariate Normal densities, thus proving that diagonal covariance structure for multivariate Normal distributions implies that each dimension is independent. Now back to the Kullback-Leibler divergence in the Variational Autoencoder context. Applying the expression we derived, the divergence between the approximate posterior and the prior is given by:

$$\mathcal{D}\left[ q_{\phi}(\vec z \mid \vec x) \parallel  p(\vec z)\right] = \frac{1}{2} \sum_{i=1}^D \left[ \mu_i^2 + \sigma_i^2 - \log \sigma_i^2 - 1 \right]$$

This is a very important constituent of the variational loss, since it acts as a regularizer, encouraging samples to be modelled after a standard normal distribution (encouraging is the key word, since it is not a restriction, simply a gentle nudge in the right direction).