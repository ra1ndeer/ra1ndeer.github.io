# Approximate Posteriors and L1 Convergence

---

## Bayes Decision Function and Bayes Risk

---

Consider a simple binary classification setting, where we have a pair of random variables $$(X, Y)$$, with $$X$$ taking its values from $$\mathbb{R}^D$$ and $$Y$$ taking its values from $$\{0, 1\}$$. We can describe by the pair $$(\mu, \eta)$$, where $$\mu$$ is the probability measure for $$X$$ and $$\eta$$ is the regression of $$Y$$ on $$X$$, sometimes called the *a posteriori* probability. More explicitly, for a Borel-measurable set $$A \subseteq \mathbb{R}^d$$,

* $$\mu(A) = \mathbb{P}[X \in A]$$,
* $$\eta(x) = \mathbb{P}[Y=1 \mid X=x] = \mathbb{E}[Y \mid X=x], \forall x \in \mathbb{R}^D$$.

Basically, $$\eta(x)$$ is the conditional probability that $$Y$$ is $$1$$ given $$X=x$$. This is more than enough to describe the joint distribution of $$X,Y$$, since, for any set $$C \subseteq \mathbb{R}^D \times \{0, 1\}$$, we can write $$C$$ as

$$C = \left( C \cap \left(\mathbb{R}^D \times \{0\} \right) \right) \cup \left( C \cap \left(\mathbb{R}^D \times \{1\} \right) \right) \equiv $$

$$\equiv C_0 \times \{0\} \cup C_1 \times \{1\}$$

which, in turn, allows us to write

$$\mathbb{P}[(X, Y) \in C] = \mathbb{P}[X \in C_0, Y=0] + \mathbb{P}[X \in C_1, Y=1] =$$

$$= \int_{C_0} (1 - \eta(x))\mu(dx) + \int_{C_1}\eta(x)\mu(dx)$$

Note that there is no Borel-measurable set $$C$$ for which the above is not valid, meaning that the distribution of $$(X, Y)$$ is determined by $$(\mu, \eta)$$. 

To define an actual classifier (or decision function), it suffices to consider some function $$g$$ such that $$g : \mathbb{R}^D \rightarrow \{0, 1\}$$. In a very obvious sense, the error probability of $$g$$ is given by $$L(g) = \mathbb{P}[g(X) \neq Y]$$. The lower the error probability, the better the classifier. The goal when designing a classifier is to minimize this error probability. The Bayes decision function, given by:

$$g^*(x) = \mathbb{I}_{\{\eta(x) > 1/2\}}$$

where $$\mathbb{I}_{\{A\}}(x)$$ is the indicator function (one when $$x \in A$$ and zero otherwise), is the Is the decision function that minimizes the error probability. To prove this, let us consider some other completely arbitrary decision function $$g : \mathbb{R}^D \rightarrow \{0, 1\}$$, and let us write the conditional error probability for this function $$g$$ given $$X=x$$:

$$\mathbb{P}[g(X) \neq Y \mid X=x] = $$

$$= 1 - \mathbb{P}[Y = g(X) \mid X=x] =$$

$$=1 - \left( \mathbb{P}\left[ Y =1, g(X) = 1 \mid X=x \right] + \mathbb{P}\left[ Y=0, g(X) = 0 \mid X=x \right] \right) =$$

$$= 1 - \left( \mathbb{I}_{\{g(x) = 1\}} \mathbb{P}[Y=1 \mid X=x] + \mathbb{I}_{\{g(x) = 0\}}  \mathbb{P}[Y=0 \mid X=x]\right) = $$

$$= \left( \mathbb{I}_{\{g(x) = 1\}} \eta(x) + \mathbb{I}_{\{g(x)=0\}} (1 - \eta(x)) \right)$$

Thus, for every $$x \in \mathbb{R}^D$$,

$$\mathbb{P} \left[ g(X) \neq Y \mid X =x \right] - \mathbb{P}\left[ g^*(x) \neq Y \mid X=x \right] = $$

$$= \eta(x) \left[ \mathbb{I}_{\{g^*(x)=1\}} -  \mathbb{I}_{\{g(x)=1\}} \right] + (1 - \eta(x))\left[ \mathbb{I}_{\{g^*(x)=0\}} -  \mathbb{I}_{\{g(x)=0\}} \right] =$$

$$=(2 \eta(x) - 1) \left( \mathbb{I}_{\{g^*(x) = 1\}} - \mathbb{I}_{\{g(x) = 1\}} \right) \geq 0$$

where the inequality arises from the definition of the Bayes decision function. Integrating both sides with respect to $\mu(dx)$ yields:

$$\mathbb{P} \left[ g(X) \neq Y \mid X =x \right] - \mathbb{P}\left[ g^*(x) \neq Y \mid X=x \right] \geq 0 \Leftrightarrow $$

$$\Leftrightarrow \mathbb{P} \left[ g^*(X) \neq Y \mid X =x \right] \leq \mathbb{P}\left[ g(x) \neq Y \mid X=x \right] \Leftrightarrow$$

$$\Leftrightarrow \int \mathbb{P}[g^*(X) \neq Y \mid X=x] \mu (dx) \leq \int \mathbb{P} [g(x) \neq Y \mid X=x] \mu(dx) \Leftrightarrow$$

$$\Leftrightarrow \mathbb{P}[g^*(X) \neq Y] \leq \mathbb{P}[g(X) \neq Y]$$

In other words, the Bayes decision function is the  optimal choice regarding minimization of the error probability. Coincidently, $$\mathbb{P}[g^*(X) \neq Y] = L(g^*)$$ is called the Bayes probability of error, Bayes error, or Bayes risk.

## *A posteriori* distributions, more often than not, have to be approximated

---

More often than not, posterior distributions have to be approximated, since the integral required to compute them may not have a closed form. This poses an issue with the Bayes decision function, since we may have to approximate $$\eta(x)$$ with some function $$\tilde{\eta}(x)$$. It seems natural to plug this approximate posterior in the Bayes decision function:

$$g(x) = \mathbb{I}_{\{\tilde{\eta}(x) > 1/2\}}$$

But now our optimality condition is gone. To solve this issue, consider the difference between the approximated and the optimal Bayes risk. If, for some $$x \in \mathbb{R}^D$$ we have $$g(x) = g^*(x)$$ then this difference is obviously zero. More interestingly, let us consider when $$g(x) \neq g^*(x)$$ and skip some steps using the previous proof:

$$\mathbb{P}[g(X) \neq Y \mid X=x] - \mathbb{P}[g^*(X) \neq Y \mid X=x] = $$

$$=[2 \eta(x) - 1] [\mathbb{I}_{\{g^*(x) = 1\}} - \mathbb{I}_{\{g(x)=1\}}]$$

Note that since we are considering the case when the decision functions are different, the term inside the second square brackets can only be $$1$$ or $$-1$$. When this term is $$1$$, the value inside the first square brackets in greater than zero, when it's $$-1$$, it is lower than zero, which means we can write the above as

$$\left| 2 \eta(x) - 1 \right| \mathbb{I}_{\{g(x) \neq g^*(x)\}}$$

To rid ourselves of conditional probability, we proceed as usual, we integrate with respect to the probability measure $$\mu(dx)$$, which just means taking the expectation with respect to $$X$$. 

$$ \int\mathbb{P}[g(X) \neq Y \mid X=x] \mu(dx) - \int \mathbb{P}[g^*(X) \neq Y \mid X=x] \mu(dx) = $$

$$=\mathbb{P}[g(X) \neq Y] - \mathbb{P}[g^*(X) \neq Y] = $$

$$= \int \left| 2 \eta(x) - 1 \right| \mathbb{I}_{\{g(x) \neq g^*(x)\}} \mu(dx)$$

Again focusing on the fact that $$g(x) \neq g^*(x)$$ means that we have the two following possibilities:

* $$g(x)=1$$; $$g^*(x)=0$$  $$\Rightarrow$$ $$\eta(x) \leq \frac{1}{2}$$; $$\tilde{\eta}(x) > \frac{1}{2}$$;
* $$g(x)=0$$; $$g^*(x)=1$$  $$\Rightarrow$$ $$\eta(x) > \frac{1}{2}$$; $$\tilde{\eta}(x) \leq \frac{1}{2}$$

In any of the two cases, it is immediate to notice that we have $$\mid \eta(x) - \tilde{\eta}(x) \mid \geq \mid \eta(x) - 1/2 \mid$$. And thus:

$$\int \left| 2 \eta(x) - 1 \right| \mathbb{I}_{\{g(x) \neq g^*(x)\}} \mu(dx) \leq$$

$$\leq \int 2 \mid \eta(x) - \tilde{\eta}(x) \mid \mu(dx) = 2\mathbb{E}[\mid \eta(x) - \tilde{\eta}(x) \mid]$$

The above inequality states that, if we have an approximate posterior distribution which can be arbitrarily close to to the true posterior, then the difference in Bayes risk of the plug-in decision function and the optimal decision function has an upper bound given by the $$\mathbb{L}_1$$ distance between the approximate posterior and the true posterior. On the other hand, if the $$\mathbb{L}_1$$ distance is zero, then there is convergence in $$\mathbb{L}_1$$ which in turn implies convergence in probability (which, in turn, also implies convergence in distribution). It's a neat result that incentivizes approximations to be as close as possible of the real posterior (in the $$\mathbb{L}_1$$ sense).

---

### References

* Luc Devroye, László Györfi, Gábor Lugosi, *A Probabilistic Theory of Patter Recognition*
* Kai Lai Chung, *A Course in Probability Theory*

