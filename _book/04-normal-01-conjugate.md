## Inference for Normal Mean with Unknown Variance {#sec:normal-gamma}


In \@ref(sec:normal-normal) we described the normal-normal conjugate  family for inference about an unknown mean $\mu$ with a known standard deviation $\sigma$ when the data were assumed to be a random sample from a normal population. In this section we will introduce the normal-gamma conjugate family for the common situation when $\sigma$ is unknown.

### Sampling Model

Recall that a conjugate pair is a sampling model for the data and prior distribution for the unknown parameters such that the posterior distribution is in the same family of distributions as the prior distribution. 
We will assume that the data are a random sample of size $n$ from a normal population with mean $\mu$ and variance $\sigma^2$; the following is a mathematical shorthand to represent this distribution assumption 

$$\begin{equation}
Y_1, \ldots Y_n  \iid
\textsf{N}(\mu, \sigma^2) 
\end{equation}$$
where  iid above the distributed as symbol '$\sim$'  indicates that each of the observations are **i**ndependent of the others (given $\mu$ and $\sigma^2$) and are **i**dentically **d**istributed.  As both $\mu$ and $\sigma^2$ unknown, we will need to specify a **joint** prior distribution to describe our prior uncertainty about them.

### Conjugate prior
If you recall from \@ref(sec:normal-normal) the conjugate prior for $\mu$ with a known standard deviation $\sigma$ was a normal distribution.  We will build on this  to specify the conditional prior distribution for $\mu$ as 
\begin{equation}
\mu \mid \sigma^2   \sim  \textsf{N}(m_0, \sigma^2/n_0)
(\#eq:04-conjugate-normal)
\end{equation}
with hyper-parameters $m_0$, the prior mean for $\mu$, and $\sigma^2/n_0$ the prior variance.  While previously the variance was a known constant $\tau^2$, replacing $\tau^2$ with a multiple of $\sigma^2$ is necessary for the joint conjugate prior.   Because $\sigma$ has the same units as the data, the hyper-parameter $n_0$ is unitless, but is used to express our prior precision about $\mu$ with larger values of $n_0$ indicating more precision and smaller values less precision.  We will see later how the hyper-parameter $n_0$ may be interpreted as a prior sample size. 

As $\sigma^2$ is unknown, a Bayesian would use a
prior distribution to describe the uncertainty about the variance before seeing data.  Since the variance is non-negative, continuous, and with no upper limit, you might think that a gamma distribution would be a candidate for a  conjugate prior for the variance, based on the distributions that we have seen so far. That is close, but it turns out that it is actually the inverse of the variance, which is known as the precision, that has a conjugate gamma prior distribution.
Letting $\phi = 1/\sigma^2$ denote the precision or inverse variance,  the conjugate prior for $\phi$,
\begin{equation}
\phi \sim \textsf{Gamma}\left(\frac{v_0}{2}, \frac{v_0 s^2_0}{2} \right)
(\#eq:04-conjugate-gamma)
\end{equation}
is a gamma distribution with prior degrees of freedom $v_0$ and prior variance $s^2_0$.  Equivalently we may say that the inverse of the variance has a 
$$1/\sigma^2 \sim \textsf{Gamma}(v_0/2, s^2_0 v_0/2)$$
gamma distribution.  Together the Normal conditional distribution for $\mu$ given $\sigma^2$ in \@ref(eq:04-conjugate-normal)  and the marginal Gamma distribution for $\phi$ in \@ref(eq:04-conjugate-gamma) leads to a joint distribution for the pair $(\mu, \phi)$ that we will call the Normal-Gamma family of distributions
\begin{equation}(\mu, \phi) \sim \textsf{NormalGamma}(m_0, n_0, s^2_0, v_0)
(\#eq:04-cojugate-normal-gamma)
\end{equation}
with four hyperparameters $m_0$, $n_0$, $s^2_0$, and $v_0$.

### Posterior Distribution

As a conjugate family the posterior
distribution of the parameters is in the same family as the prior distribution.  As a conjugate prior for a sample from a  normal distribution, the posterior is also Normal-Gamma 
\begin{equation}
(\mu, \phi) \mid \text{data} \sim \textsf{NormalGamma}(m_n, n_n, s^2_n, v_n)
\end{equation}
where the subscript $n$ on the
hyper-parameters indicates the updated values after seeing the $n$ observations from the data:
\begin{eqnarray*}
m_n & = & \frac{n \bar{Y} + n_0 m_0} {n + n_0}  \\
& \\
n_n & = & n_0 + n  \\
v_n & = & v_0 + n  \\
s^2_n & =  & \frac{1}{v_n}\left[s^2_0 v_0 + s^2 (n-1) + \frac{n_0 n}{n_n} (\bar{Y} - m_0)^2 \right] 
\end{eqnarray*}
The updated hyperparameter $m_n$ in the posterior distribution of $\mu$ is the posterior mean, which is a weighted average of the sample mean $\bar{Y}$ and prior mean $m_0$ with weights $n/(n + n_0$ and $n_0/(n + n_0)$ respectively.
The posterior sample size $n_n$ is the sum of the prior sample
size $n_n$ and the sample size $n$, representing the combined precision of the estimate for $\mu$.  

The posterior degrees of freedom $v_n$ are also increased by adding the  sample size $n$ to the prior degrees of freedom $v_0$.

The posterior variance hyper-parameter $s^2_n$ combines three sources of information about $\sigma$ in terms of sums of squared deviations.  The first term in
the square brackets is the sample variance times the sample degrees of
freedom which is the sample sum of squares. The second term represents
the prior sum of squares, while the third term is based on the squared
difference of the sample mean and prior mean.  We then divide by the
posterior degrees of freedom to get the new variance.



The joint  Normal-Gamma distribution for $\mu$ and $\phi$, $(\mu, \phi) \mid \data \sim \NoGa(m_n, n_n, s^2_n, v_n)$
is equivalent to a hierarchical model with $\mu$ given $\sigma$ having a conditional normal distribution 
$$\begin{aligned}
\mu \mid \data, \sigma^2  &\sim  \No(m_n, \sigma^2/n_n)  \\
1/\sigma^2 \mid \data  & \sim   \Ga(v_n/2, s^2_n v_n/2) 
\end{aligned}
$$
and the inverse variance having a marginal  gamma distribution.

### Marginal Distribution for $\mu$
Since we are interested in inference about mu unconditionally, we need
the marginal distribution of mu that averages over the uncertainty in
$\sigma$.  One can show by integration that the marginal distribution for
$\mu$ is a Student t distribution
$$\begin{aligned} \mu \mid \data \sim \St(v_n, m_n, s^2_n/n_n) \pause
 \Leftrightarrow  t = \frac{\mu - m_n}{s_n/\sqrt{n_n}} \sim \St(v_n, 0 , 1)
 \end{aligned}
$$  with density
$$
\begin{aligned}
p(t) =\frac{\Gamma\left(\frac{v_n + 1}{2} \right)}
{\sqrt{\pi v_n} \Gamma\left(\frac{v_n}{2} \right)}
\left(1 + \frac{1}{v_n}\frac{(\mu - m_n)^2} {s^2_n/n_n} \right)^{-\frac{v_n+1}{2}}
\end{aligned}$$
with the degrees of freedom $v_n$, a
location parameter $m_n$ and squared scale parameter that is the
posterior variance parameter divided by the posterior sample size. 

standardizing mu by subtracting the location and dividing by the scale
yields a standard t distribution with degrees of freedom v_n, location
0 and scale 1.  The latter representation allows us to use standard
statistical functions for posterior inference.

Let's look at an example based on a sample of total trihalomethanes or
TTHM in tapwater from a city in NC, that can be loaded from the `statsr` package


```r
library(statsr)
data(tapwater)
```




Using prior information about TTHM
from the city, I will use a Normal-Gamma prior distribution with
prior mean of 35 parts per billion based on a prior sample size of
twenty-five and an estimate of the variance of one hundred fifty-six
point two five with degrees of freedom twenty-four.  In section **ADD REFERENCE**,
we will describe how we arrived at these values.

Using the sample mean of 55.5 and the sample variance five hundred
forty point seven from the sample of size 28 we obtain the following
update the hyper-parameters




```r
m_0 = 35; n_0 = 25; s2_0 = 156.25; v_0 = n_0 - 1

Y = tapwater$tthm
ybar = mean(Y)
s2 = var(Y)
n = length(Y)
n_n = n_0 + n
m_n = (n*ybar + n_0*m_0)/n_n
v_n = v_0 + n
s2_n = ((n-1)*s2 + v_0*s2_0 + n_0*n*(m_0 - ybar)^2/n_n)/v_n
```
Prior $\textsf{NormalGamma}(35, 25, 156.25, 24)$ 

Data  $\bar{Y} = 55.5239286$, $s^2 = 540.7478692$, $n = 28$

\begin{eqnarray*}
n_n & = &  25 +  28 = 53\\
m_n  & = & \frac{28 \times55.5239286 + 25 \times35}{53} = 45.8428302  \\
v_n & = & 24 + 28 = 52  \\
s^2_n & = & \frac{(n-1) s^2 + v_0 s^2_0 + n_0 n (m_0 - \bar{Y})^2 /n_n }{v_n}  \\
  & = & \frac{1}{52}
     \left[27 \times 540.7478692 +
          24 \times 156.25  +
          \frac{25 \times 28}{53} \times (35 - 55.5239286)^2
\right] = 459.9  \\
\end{eqnarray*}
Posterior $\textsf{NormalGamma}(45.8428302, 53, 459.8774861, 52)$
providing the normal-gamma posterior that summarizes our posterior
uncertainty after seeing the data.

To find a credible interval for the mean, we use the Student t
distribution.  Since the distribution of mu is unimodal and symmetric,
the shortest 95 percent credible interval or the Highest Posterior
Density interval, HPD for short,


<img src="04-normal-01-conjugate_files/figure-html/tapwater-post-mu-1.png" width="384" />

is the orange interval given by the
Lower endpoint L and upper endpoint U where the probability that mu is
in the interval (L, U) is the shaded area which is equal to zero point
nine five.

using the standardized t distribution and some algebra, these values are
based on the quantiles of the standard t distribution times the scale
plus the posterior mean and should look familiar to expressions
for confidence intervals from frequentist statistics.

The following code illustrates using R to calculate the 95 percent
credible interval using quantiles from the standard t distribution.
The tap water data are available from the statsr package.


Based on the updated posterior, we find that there is a 95 chance that
the mean TTHM concentration is between 39.9 parts per billion and 51.8
parts per billion.


To recap, we introduced the normal -gamma conjugate prior for
inference about an unknown mean and variance for samples from a normal
distribution. 

We described how the joint normal-gamma distribution leads to the
student t distribution for inference about mu when sigma is unknown,

and showed how to obtain credible intervals for mu  using R.


next, we will introduce Monte Carlo sampling to explore prior and posterior
distributions in more detail. 
