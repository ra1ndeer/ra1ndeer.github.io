# A Primer on Entropy, Kullback-Leibler Divergence and Mutual Information

---

## Entropy of a Random Variable

---

Given a random variable $$X$$, with sample space $$\mathcal{X}$$ and probability density function $$p$$, we define entropy as

$$H\left[X\right] = \mathbb{E}_{p(x)}\left[ \log \frac{1}{p(X)} \right],$$

which, depending on if $$X$$ is a discrete or continuous random variable, can be written as

* Discrete: $$H\left[ X \right] = \sum_{x \in \mathcal{X}} p(x) \log \frac{1}{p(x)} = - \sum_{x \in \mathcal{X}} p(x) \log p(x)$$

* Continuous: $$H \left[X \right] = \int_{\mathcal{X}} p(x) \log \frac{1}{p(x)} dx = -\int_{\mathcal{X}} p(x) \log p(x) dx$$

We can interpret the entropy as the average uncertainty of a random variable and its units depend on the base of the logarithm used. If the logarithm is base 2, then the entropy is measured in *bits*. On the other hand, if we are using the natural logarithm (base $$e$$), then the entropy is measured in *nats*. We can immediately derive a fundamental property of the entropy, as a consequence of its definition. Note that, since $$p$$ is a probability density (or mass, if $$X$$ is discrete), we have that $$\log p(x) \leq 0 \Rightarrow - \log p(x) = \log \frac{1}{p(x)} \geq 0$$. Multiplying this by $$p(x)$$ obviously does not change the sign and neither does summing or integrating this quantity over $$\mathcal{X}$$, meaning that the **entropy of a random variable is non-negative**. To investigate more properties of the entropy, we need to introduce some more concepts.

## Joint Entropy and Conditional Entropy

---

Let us now focus on pairs of random variables, $$(X, Y)$$. For such pair, with joint probability density $$p(x, y)$$, we can define the joint entropy as

$$H \left[ X, Y \right] = - \mathbb{E}_{p(x, y)} \left[ \log p(X, Y) \right]$$

We can similarly define the conditional entropy of $$Y$$ given $$X$$, which is interpreted as the average uncertainty of $$Y$$ given knowledge on the behavior of $$X$$:

$$H \left[Y \mid X \right] = - \mathbb{E}_{p(x, y)} \left[ \log p(Y \mid X) \right]$$

It is possible to move this formula around a bit so as to explicit what it actually means. First, we unfold the expectation and apply the definition of conditional probability $$p(x, y) = p(x)p(y \mid x)$$:

$$- \mathbb{E}_{p(x, y)} \left[ \log p(Y \mid X) \right] = - \int_{\mathcal{X}} \int_{\mathcal{Y}} p(x,y) \log p(y \mid x) dy dx = $$

$$= -\int_{\mathcal{X}} \int_{\mathcal{Y}} p(x) p(y \mid x) \log p(y \mid x) dy dx $$

Since $$p(x)$$ has, obviously, no dependence on $$y$$, we can isolate it from the integration over $$\mathcal{Y}$$:

$$-\int_{\mathcal{X}} \int_{\mathcal{Y}} p(x) p(y \mid x) \log p(y \mid x) dy dx = $$

$$= -\int_{\mathcal{X}} p(x) \int_{\mathcal{Y}} p(y \mid x) \log p(y \mid x) dy dx $$

Now note the integrand of the innermost integral: the distribution $$p(y \mid x)$$ featured is the distribution of $$y$$ given **one** specific value of $$X$$, $$x$$. In other words, it's the entropy of $$Y$$ given $$X = x$$:

$$ -\int_{\mathcal{X}} p(x) \int_{\mathcal{Y}} p(y \mid x) \log p(y \mid x) dy dx = \int p(x) H(Y \mid X = x) dx$$

Meaning we can interpret the conditional entropy of $$Y$$ given $$X$$ as a weighted sum (with weights given by the probabilities of each possible value of $$X$$) of the entropy of $$Y$$ given each individual value of $$X$$, $$x$$. Additionally, we may ask ourselves if the entropy of $$Y$$ given $$X$$ is equal to the entropy of $$X$$ given $$Y$$. Let us investigate this statement by manipulating the equation for the entropy of $$Y$$ given $$X$$.

$$H\left[ Y \mid X \right] = - \int_{\mathcal{X}} \int_{\mathcal{Y}} p(x, y) \log p(y \mid x) dy dx =$$

$$= - \int_{\mathcal{X}} \int_{\mathcal{Y}} p(x, y) \log \frac{p(x)p(x \mid y)}{p(y)} dy dx $$

We can now split the above integral into the sum of three easily identifiable integrals:

* $$-\int_{\mathcal{X}} \int_{\mathcal{Y}} p(x, y) \log p(x) dy dx$$
* $$-\int_{\mathcal{X}} \int_{\mathcal{Y}} p(x, y) \log p(x \mid y) dy dx$$
* $$\int_{\mathcal{X}} \int_{\mathcal{Y}} p(x, y) p(y) dy dx$$

In the first integral, we simply integrate out the $$y$$ and we are left with $$H\left[X\right]$$.  The second integral is the definition of conditional entropy of $$X$$ given $$Y$$, $$H\left[X \mid Y \right]$$. Finally, for the third integral, we can swap the integrals to integrate out the $$x$$, revealing the negative entropy of $$Y$$, $$-H\left[  Y\right]$$. Putting it all together:

$$H\left[ Y |X \right] = H\left[ X \right] + H\left[ X | Y \right] - H\left[Y \right]$$

Which means that the conditional entropies are only equal if the entropies themselves are equal.

There is a rather natural connection between the three types of entropy we have just seen, which can be easily derived from the joint entropy. We begin by applying the definition of conditional probability:

$$H \left[ X, Y \right] = - \int_{\mathcal{X}} \int_{\mathcal{Y}} p(x, y) \log p(x, y) dy dx = $$

$$ = - \int_{\mathcal{X}} \int_{\mathcal{Y}} p(x, y) \log \left[ p(x) p(y \mid x) \right] dy dx$$

Since the logarithm of the product is the sum of each individual logarithm, we can split the above result into two integrals:

$$- \int_{\mathcal{X}} \int_{\mathcal{Y}} p(x, y) \log p(x) dy dx - \int_{\mathcal{X}} \int_{\mathcal{Y}} p(x, y) \log p(y \mid x) dy dx$$

The rightmost integral is simply the conditional entropy we have just seen, so we can focus on the leftmost integral. Notice that the only dependence on $$y$$ is through $$p(x, y)$$, meaning we can write

$$- \int_{\mathcal{X}} \int_{\mathcal{Y}} p(x, y) \log p(x) dy dx = -\int_{\mathcal{X}} \log p(x) \int_{\mathcal{Y}} p(x, y) dy dx$$

Integrating $$p(x, y)$$ with respect to $$y$$ is what is known as marginalizing, *i.e*, we integrate out the random variable $$Y$$ and are left only with $$p(x)$$:

$$-\int_{\mathcal{X}} \log p(x) \int_{\mathcal{Y}} p(x, y) dy dx = -\int_{\mathcal{X}} p(x) \log p(x) dx$$

which is the entropy of the random variable $$X$$. Meaning that the relation between entropy, conditional entropy and joint entropy of a pair of random variables is given by:

$$H \left[X, Y\right] = H\left[X \right] + H\left[ Y \mid X \right]$$

## Kullback-Leibler Divergence (or Relative Entropy)

---

The relative entropy is mainly used to quantify how much two probability distributions differ. Alternatively, it can also be used to define a geometry in the parameter space for probability distributions, allowing for optimization schemes such as variational inference to thrive. For two probability densities $$p$$ and $$q$$, defined on the same sample space $$\mathcal{X}$$, the Kullback-Leibler is given by:

$$\mathcal{D}_{KL} \left[p \parallel q \right] = \mathbb{E}_{p(x)} \left[ \log \frac{p(X)}{q(X)} \right]$$

Similarly to entropy, the Kullback-Leibler divergence is non-negative, being zero only when $$p(x) = q(x), \forall x \in \mathcal{X}$$. To prove this, we start by taking the negative of the KL divergence over the support of $$p(x)$$, $$A$$, and apply Jensen's inequality:

$$- \mathcal{D}_{KL} \left[p \parallel q \right] =  \int_A p(x) \log \frac{q(x)}{p(x)}dx =$$

$$ = \mathbb{E}_{p(x)} \left[ \log \frac{q(X)}{p(X)} \right] \leq \log \mathbb{E}_{p(x)} \left[ \frac{q(X)}{p(X)} \right]$$

Now we make a simple statement: the support of $$p(x)$$, $$A$$, is smaller or equal to the sample space $$\mathcal{X}$$. As a consequence of this, we have that $$\int_A q(x) dx \leq \int_{\mathcal{X}} q(x) dx = 1$$. Which means we can write:

$$\log \mathbb{E}_{p(x)} \left[ \frac{q(X)}{p(X)} \right] = \log \int_A q(x) dx \leq \log \int_{\mathcal{X}} q(x) dx = \log 1 = 0$$ 

Intuitively, the Kullback-Leibler divergence is zero if and only if $$p(x) = q(x), \forall x \in \mathcal{X}$$, but we still have to prove this. For that matter, consider Jensen's inequality above. Since the logarithm is a strictly concave function, we have equality if and only if the ratio of $$q(x)$$ and $$p(x)$$ is constant for all $$x$$, *i.e.*, equal to $$c$$. This directly implies that $$\int_{A} q(x) dx= c \int_{A} p(x) dx = c$$, since $$A$$ is the support of $$p(x)$$. On the other hand, in the second inequality we used, we have equality only if $$\int_{A} q(x) dx = \int_{\mathcal{X}} q(x) dx = 1$$, which, in turn, implies that $$c$$ must be equal to one if those inequalities are to be equalities. In other words, for equality to arise, we must have $$p(x) = q(x), \forall x \in \mathcal{X}$$.

We can also define the conditional Kullback-Leibler divergence, in a completely analogous way to what we've already done:

$$\mathcal{D}_{KL} \left[ p(y \mid x) \parallel q(y \mid x) \right] = \int_{\mathcal{X}} \int_{\mathcal{Y}} p(x, y) \log \frac{p(y \mid x)}{q(y \mid x)} dy dx=$$
$$ = \mathbb{E}_{p(x, y)} \left[ \log \frac{p(Y \mid X)}{ q(Y \mid X)} \right]$$

After defining the conditional relative entropy, it seems natural to ask ourselves about the joint relative entropy:

$$\mathcal{D}_{KL} \left[ p(x, y) \parallel q(x, y) \right] = \int_{\mathcal{X}} \int_{\mathcal{Y}} p(x, y) \log \frac{p(x, y)}{q(x, y)} dy dx =$$

$$= \mathbb{E}_{p(x, y)} \left[ \log \frac{p(X, Y)}{q(X, Y)} \right]$$

By the definition of conditional probability, we can relate the above relative entropies through one simple expression, which is sometimes called the chain rule for relative entropy:

$$\mathcal{D}_{KL} \left[ p(x, y) || q(x, y) \right] = \int_{\mathcal{X}} \int_{\mathcal{Y}} p(x, y) \log \frac{p(x)p(y \mid x)}{q(x)q(y \mid x)} dy dx =$$

$$= \int_{\mathcal{X}}\int_{\mathcal{Y}} p(x, y)\log \frac{p(x)}{q(x)} dy dx +  \int_{\mathcal{X}}\int_{\mathcal{Y}} p(x, y) \log \frac{p(y \mid x)}{q(y \mid x)} dy dx =$$

$$= \mathcal{D}_{KL} \left[ p(x) \parallel q(x) \right] + \mathcal{D}_{KL} \left[ p(y \mid x) \parallel q(y \mid x) \right]$$

Finally, while the Kullback-Leibler divergence is sometimes used to measure the distance between probability densities it should be noted that it **is not a metric**, since it fails to be symmetric and to satisfy the triangle inequality.

## Mutual Information

---

Another useful concept is the mutual information, which measures how much information one random variable contains about another random variable:

$$I\left[ X, Y \right] = \mathbb{E}_{p(x, y)} \left[ \log \frac{p(X, Y)}{p(X)p(Y)} \right] = \mathcal{D}_{KL} \left[p(x,y) \parallel p(x)p(y) \right]$$

It is immediate to see that the mutual information is symmetric, *i.e.*, $$I[X, Y] = I[Y, X]$$. The mutual information is fundamental to fill in the gap between entropy concepts. It relates to entropy and entropy through the following expression:

$$I[X, Y] = \int_{\mathcal{X}} \int_{\mathcal{Y}} p(x, y) \log \frac{p(x,y)}{p(x)p(y)} dy dx = $$

$$=\int_{\mathcal{X}} \int_{\mathcal{Y}} p(x, y) \log \frac{p(x \mid y)}{p(x)} dy dx=$$

$$=- \int_{\mathcal{X}} \int_{\mathcal{Y}} p(x, y) \log p(x) dy dx + \int_{\mathcal{X}} \int_{\mathcal{Y}} p(x, y) \log p(x \mid y) dy dx =$$

$$= H[X] - H[X \mid Y]$$

This means that the mutual information is the reduction in the uncertainty of $$X$$ due to knowledge of $$Y$$. Note that if instead of taking $$p(x, y) / p(x)(y) = p(x \mid y) / p(x)$$ we take it to be equal to $$p(y\midx)/p(y)$$, we can repeat the calculations above to obtain:

$$I[X, Y] = H[Y] - H[Y \mid X]$$

We can further relate the mutual information with the joint entropy, by using the relation $$H[X, Y] = H[X] + H[Y \mid X]$$ that we previously derived:

$$I[X, Y] = H[X] + H[Y] - H[X, Y]$$

Since the mutual information can be written as the Kullback-Leibler divergence of $$p(x,y)$$ relative to $$p(x)p(y)$$, it is immediate to see that it is also non-negative and only equal to zero if $$X$$ and $$Y$$ are independent. Perhaps the most curious result is the following:

$$I[X, Y] \geq 0 \Leftrightarrow H[X] - H[X \mid Y] \geq 0 \Leftrightarrow H[X \mid Y] \leq H[X]$$

This means that, on average, knowledge of other random variable $$Y$$ can only reduce the uncertainty $$X$$. In other words, *information can't hurt*. Cool stuff.

### References

---

* Thomas M. Cover and Joy A. Thomas, *Elements of Information Theory*, 2nd Edition