---
layout: post
title:  Random Variables
date:   2023-03-18
categories: probability
permalink: /probability/random-variables
---

### Table of Content
1. [Random Variables](#1-random-variables)
	- [What is a Random Variable?](#11-what-is-a-random-variable)
	- [Distributions](#12-distributions)
2. [Discrete Random Variables](#discrete-random-variables)
	- [Probability Mass Function (PMF)](#probability-mass-function-pmf)
3. [Continuous Random Variables](#continuous-random-variables)
	- [Probability Density Function (PDF)](#probability-density-function-pdf)
4. [Cumulative Distribution Function (CDF)](#cumulative-distribution-function-cdf)

---

<br/>

## 1. Random Variables

#### 1.1. What is a random variable?

To define rigorously, we may need knowledge from Real Analysis and Measure Theory. However, in this course, we want to have an intuition of what a random variable is. There are 2 questions can be asked:

- **What is a Variable?** When we talk about variables, there should exist a function or a mapping. 
- **what does it mean by "random"?** "Random" here refers to a random process in which we will obtain a sample space $$\Omega$$

Together, we have an intuition that <span style="color:red;">a random variable is a mapping from sample space $$\Omega$$ to a measurable space, denote $$E$$, such as $$\mathbb{N}$$ or $$\mathbb{R}$$</span>, which means each possible outcome in $$\Omega$$ is mapped with a number in real life. Clearly, there is chance that we will not use all elements in $$E$$. We need a new definition of a set containing all the result of a random variables $$X$$ perform on $$\Omega$$. We have the set $$\mathbf{S} = \{ X( \omega) \| \omega \in \Omega \}$$. This set $$\mathbf{S}$$ is called **the state** of the random variable $$X$$ on $$\Omega$$.

<details>
<summary><b>But why do we want to do so?</b></summary>
One of intuitive answer is because the sample space \(\Omega\) can contain <b>infinite</b> number of possible outcomes, which means there could be a chance we run out of notations. Thus, we want to find a better way to enumerate all possible outcomes. The question here is what kind of set that we know it has infinite elements? Here it is, the set of \(\mathbb{N}\), \(\mathbb{Z}\), \(\mathbb{Q}\) and \(\mathbb{R}\). Besides, numbers are way easier to deal with than abstract elements in the sample space. 

<p></p>

</details>


<details>
<summary><b>Further Discussion</b></summary>
<ul> 
<li>
	The set \(\mathbf{S}\) is an <a href="https://en.wikipedia.org/wiki/Image_(mathematics)">image</a> of a random variable \(X\)
</li>
<li>
	Why a random variable \(X\) is a mapping from the sample space \(\Omega\) but not from \(\sigma\) \(\mathcal{F}\)?
</li>
https://en.wikipedia.org/wiki/Image_(mathematics)
<li>
	Based on the definition above, can we say that a random variable \(X\) is a bijective function? (The answer should be No. Why? <a href="https://math.stackexchange.com/questions/202540/is-a-random-variable-bijective">Read this thread</a>)
</li>
<li>
	Interestingly enough (for me), the idea behind the action of mapping from the sample space \(\Omega\) to a measurable space does align with the definition of probability measure we learned in the first lecture. Why does it have to be a measurable space? Note that we are talking about probability space which are defined by \( (\Omega, \mathcal{F}, \mathbb{P})\), and the measurable space contains a set and a \(\sigma\) -algebra. It means a probability space is a measure space! 
</li>
<li>
	The countability of a set: We might need some knowledge from Real Analysis to discuss this topic. However, in this note, briefly speaking, it is worth noting that \( \mathbb{N}\), \(\mathbb{Z}\) and \(\mathbb{Q}\) are <b>countable</b> sets while \(\mathbb{R}\) is <b>uncountable</b>. Why is it important to know this? We will be learning about <a href="#discrete-random-variables">Discrete</a> and <a href="#continuous-random-variables">Continuous</a> Random Variables in the next few minutes, and this information should help us get some intuition that makes it easier to understand the idea.
</li>
</ul>
</details>

<br/>

#### 1.2. Distributions

**What does it mean by distribution?** 

**Answer:** In probability theory and statistics, a probability distribution is the mathematical function that gives the probabilities of occurrence of different possible outcomes for an experiment. 

**Can we initialize a probability distribution, or probability function, on random variables?**

**Answer:** Yes. Why can we do that? Note that $$X=a_i$$ is an event. Because for any $$a_i \in \mathbf{S}$$, we have $$\{X=a_i\}=\{X \in \{a_i\}\} =\{ \omega \in \Omega: X(\omega)=a_i\}$$. Thus, it is valid to define a probability function on the set $$\mathbf{S}$$ or $$P(X=a_i) = p_i$$.

<details>
	<summary>Side notes</summary>
	\(X \leq x\) is also an event. (\(P(X \leq x)\) is a <a href="#cumulative-distribution-function-cdf"><b>Cumulative Distribution Function (CDF)</b></a> of \(X\). We will talk about CDF later after getting to know Discrete and Continuous Random Variables.)
</details>


<br/>

## Discrete Random Variables

Discrete Random Variables occur when there is a mapping from the sample space $$\Omega$$ to a finite or **countably** infinite set such as $$\mathbb{N}$$. Put it in simple words, we can list or enumerate all possible outcomes in $$\Omega$$ such as $$\omega_1, \omega_2, \omega_3, \ldots$$, and we can also enumerate all possible values in $$S$$ in such a style of $$a_1, a_2, a_3, \ldots$$. So, $$S$$ should be countable.


#### Probability Mass Function (PMF)

Probability Mass Function (PMF) is the **probability function** for discrete random variables. <span style="color:red;">Probability Mass Function (PMF) is available for only Discrete Random Variables!</span> 

Let $$f$$ be the PMF, then $$p_i = f(a_i) = P(X=a_i) \text{ } \forall \text{ } i \text{ such that } a_i \in \mathbb{S}$$. We have some properties:

- <p>\(p_i \geq 0 \text{ } \forall \text{ } i\)</p>
- <p>\( \sum_i p_i = 1\)</p>
- For all event $$\varepsilon \subset \mathbf{S}$$, we have: $$P(\varepsilon) = \sum_{k \in \varepsilon} P(X=k)$$ 

This part is important (for me). We now can see the reason **why PMF is only applicable for Discrete Random Variables.** It is clear that we consider the probability of each event $$X=x$$ **separately**, and we wish to have an exact result in the range [0,1]. And in properties 2 and 3, we want to sum all/some probability values, and to do so we must be able to enumerate all possible values.

Some common distributions:

<details>
<summary>Bernoulli</summary>

Definition: A random variable \(X\) is said to have a Bernoulli distribution if \(X\) has only 2 possible values such as 0 and 1, then \(P(X=1) = p\) and \(P(X=0)=1-p\)
</details>

<details> 
<summary>Binomial</summary>
</details>

<details>
<summary>Poisson</summary>
</details>

<details>
<summary>Uniform</summary>
</details>

<br/>


## Continuous Random Variables

#### Probability Density Function (PDF)

<details> 
<summary>Uniform</summary>
</details>

<details>
<summary>Exponential</summary>
</details>

<br/>

## Cumulative Distribution Function (CDF)

---
<br/>

References:
- Fulbright University Vietnam - MATH 205 Probability
- [Harvard University - Statistics 110](https://www.youtube.com/playlist?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo)
- [Khan Academy - Random Variables](https://www.khanacademy.org/math/statistics-probability/random-variables-stats-library/random-variables-discrete/v/random-variables)
- [Libretexts - Events and Random Variables](https://stats.libretexts.org/Bookshelves/Probability_Theory/Probability_Mathematical_Statistics_and_Stochastic_Processes_(Siegrist)/02%3A_Probability_Spaces/2.02%3A_Events_and_Random_Variables)