
# Regularization and model selection #

## 1. Cross Vallidation ##

**Hold-out cross validation**

1. Random split S into $S_{train}$ and $S_{cv}$
2. Train each model on $S_{train}$ only
3. Select and output hypothesis with the smallest error $\hat\epsilon (h_{i})$ on the hold-out cross validation set

cons : waste data


**K-fold cross validation**

1. Random split S into k disjoint subset

2. For each model, we evalute the following
	
	For $j = 1,......k$
		Train the model on subsets without $S_{j}$, and test the hypothesis on $S_{j}$
3. Pickthe model with lowest estimated generalization error, and retrain that model on the entire data set S.


Commonly used choice : $k=10$

## 2. Feature Selection ##

**Forward Seach**

1. Initiliza $\textit{F} = \emptyset$
2. Repeat {
	(a) For i = 1,....n, if $i \notin \textit{F}, let F_{i} = F \cup \{i\} $ and use some version of cross validtion to evalute features $\textit{F}_{i}$
	
	(b) Set $\textit{F}$ to the best feature found on step(a)
3. Selection and output the best feature subset that was evaluated during the entire search procedure.

aka **Wrapper model feature selection** 

Another method, **backward search**

cons : computationally expensive ~ $O(n^2)$


**Filter feature selection** 

Compute some simple score $S_{i}$ that measure how informative each feature $x_{i}$ is about the class labels $y$.

**Correlations** between $x_{i}$ and $y$

**Mutual information**:
$$ MI(x_{i},y) = \sum_{x \in \{0, 1\}} \sum_{y \in \{ 0, 1\}} p(x_{i},y) log \frac{p(x_{i},y)}{p(x_{i})p(y)} $$

**Kullback-Leibler divergence** : a measure of how difference the probability distributions $p(x_{i})p(y)$ and $p(x_{i},y)$ are.

$$ MI(x_{i},y) = KL (p(x_{i},y) || p(x_{i})p(y) )$$


## 3. Bayesian statistics and regularization ##

Fitting using maximum likelihood (ML)

$$ \theta_{ML} = arg {max}_{\theta} \prod_{i=1}^{m} p (y^{(i)} | x^{(i)}; \theta   )  $$

The view of the $\theta$ as being *constant-valued but unknown* is **frequentist**

**Bayesian** view of the world : $\theta$ as being a *random variable* whose value is unknown. Specify a **prior distribution** $p(\theta)$ "prior beliefs" about the parameter.

Given a training set S, compute the **posterior** distribution on the parameters:

$$ p( \theta | S) = \frac{p(S | \theta) p (\theta)} {p(S)} $$

$$= \frac{(\prod_{i=1}^{m} P(y^{i}  | x^{i},\theta))p(\theta)}                  {\int_{\theta} (\prod_{i=1}^{m} P(y^{i}  | x^{i},\theta))p(\theta) } $$


$P(y^{i}  | x^{i},\theta)$ comes from whatever model used in the problem

When given a new example $x$ to make its predictions, compute the posterior distribution using :

$$P(y | x, S) = \int_{\theta} {P(y | x, \theta) p(\theta|S)}d\theta  $$

Then output



$$E(y | x, S) = \int_{\theta} y {P(y | x, S) }dy $$


The posterior distribution hard to compute, replace with a single point estimate **MAP(maximum a posteriori)**




$$ \theta_{MAP} = arg {max}_{\theta} \prod_{i=1}^{m} p (y^{(i)} | x^{(i)}; \theta   ) p(\theta) $$

$ \theta_{MAP}  vs \theta_{ML} $

$ \theta_{MAP} $ have less norm, to be less susceptible to overfitting
