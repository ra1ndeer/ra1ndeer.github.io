# The Evidence Lower Bound (ELBO)

In the variational setting, it is usual to assume that the observations, $$\mathbf{x}$$, are generated according to some random, latent, unobserved process, $$\mathbf{z}$$, and that said generative process can be parametrized by $$\theta$$, $$p_{\theta}(\mathbf{x}, \mathbf{z}).$$ However, since there is no way of observing the latent process, it is necessary to integrate this quantity out:

$$\log p_{\theta}(\mathbf{x}) = \log \int p_{\theta}(\mathbf{x}, \mathbf{z}) d \mathbf{z}$$

Variational inference concerns itself with this type of problems where there is some integral that cannot be computed analytically and requires some kind of approximation. By the definition of conditional probability, we can write $$p_{\theta}(\mathbf{x}, \mathbf{z})$$ as $$p_{\theta}(\mathbf{x} \mathbg{z}) p(\mathbf{z})$$