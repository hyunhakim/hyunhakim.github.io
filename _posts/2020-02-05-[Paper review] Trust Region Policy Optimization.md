

Trust Region Policy Optimization (John Schulman  et al. 2017)

Paper Link : [https://arxiv.org/pdf/1502.05477.pdf](https://arxiv.org/pdf/1502.05477.pdf)

<br />

## Abstract

본 논문에서는  monotonic improvement를 보장하는, 반복적으로 policy optimizing하는 알고리즘인 TRPO에 대해서 소개한다. 이 알고리즘은 이론적으로 검증된 몇몇의 approximation을 사용하여 실용적인 알고리즘이다. 이 알고리즘은 [natural policy gradient (Kakade, 2002)](http://papers.nips.cc/paper/2073-a-natural-policy-gradient.pdf) 와 유사하고 neural network와 같이 큰 nonlinear policies를 최적화하는 데 효과적이다. 다양한 task들에 대해서 robust performance를 낸다고 한다.

<br />

## Preliminaries

![image](https://user-images.githubusercontent.com/59254578/73845521-2f8f3580-4866-11ea-9337-fba9adf84f5e.png)

우리의 목적은 $\eta(\pi)$를 최대화하는 것이다. 그럼 도대체 이 $\eta(\pi)$를 어떻게 최대화할 수 있을까.



![image](https://user-images.githubusercontent.com/59254578/73845798-bba15d00-4866-11ea-8fbf-3743b6e6c52b.png)

어떤 새로운 $\tilde\pi$의 성능($\eta$)을 확인하는 방법은 위와 같이 기존의 policy, $\pi$로 유도할 수 있다. 이 식은  time step의 변화에 따른 값들을 expectation했기 때문에 timestep 관한 식이다. 이 식을 아래와 같은 과정으로 sum over states 식으로 변형할 수 있다.

![image](https://user-images.githubusercontent.com/59254578/73846313-aed13900-4867-11ea-971d-0f0be4b24559.png)

여기서 $\sum_a\tilde\pi(a\vert s) A_\pi(s,a) \geq 0$ 이라면 $\tilde\pi$는 항상 $\eta$보다 크다. 즉, policy가 나빠질 일이 없다. 하지만 $\sum_a\tilde\pi(a\vert s) A_\pi(s,a)$가 분명 어떤 state들에서는 0보다 작은 상황이 존재할 거고 이 문제를 해결하는 과정이 뒤에서 설명된다.

policy가 변하면 state distribution($\rho$)도 변한다. 따라서, 수식 (2)에서 $\rho_\tilde\pi(s)$가 존재하면 direct로 optimize하는 게 어려워진다. 그래서 아래와 같이 현재의 policy에 대한 state distribution으로 대체한다.

![image](https://user-images.githubusercontent.com/59254578/73847426-9e21c280-4869-11ea-83ed-64f3cc996dbc.png)

왜 이게 가능할까?

$\pi_\theta$가 $\theta$에 대하여 미분가능한 함수라 하자. $L_\pi\left(\tilde\pi\right)$가 $\eta(\pi)$에 대한 first order면 다음 식이 성립한다고 한다.

$$\begin{align} L_{\pi_{\theta_0} }\left(\pi_{\theta_0}\right) &= \eta\left(\pi_{\theta_0}\right) \\ \nabla_\theta \left.L_{\pi_{\theta_0} }\left(\pi_{\theta_0}\right)\right\vert _{\theta=\theta_0} &= \nabla_\theta\left.\eta(\pi_{\theta_0})\right\vert _{\theta=\theta_0} \end{align}$$

결국 이 식의 의미는 $\pi_{\theta_0}$가 매우 작게 변하면 $L_{\pi_{\theta_0} }$를 개선시키는 것이 $\eta$를 개선시키는 것이다. 따라서 (3) 식으로 대체가 가능한 것이다.

<br />

그렇다면 $\pi_{\theta_0}$를 얼마나 작게 변화시켜야 하는 것인가.

Kakade & Langford가 2002년에 발표한 논문에서도 이것에 대해서 고민했고, conservative policy iteration이라는 기법을 제공한다.

<br>

**conservative policy iteration**

기존의 policy를 $\pi_\mathrm{old}$라고 하고 $\pi’=\arg\max_{\pi’}L_{\pi_\mathrm{old} }\left(\pi’\right)$과 같이 정의하면, 새로운 mixture policy $\pi_\mathrm{new}$를 다음과 같이 제시하였다.

![image](https://user-images.githubusercontent.com/59254578/73849639-9c59fe00-486d-11ea-9d32-b330375351ba.png)

즉, 기존의 policy와 다음 policy를 가중해서 사용하겠다는 것이다.

<br>

그리고 Kakade & Langford는 아래와 같은 lower bound를 정의하였다.

![image](https://user-images.githubusercontent.com/59254578/73849697-b09dfb00-486d-11ea-93a9-a63181ac55ee.png)

이것의 의미는 lower bound 보다는 항상 큰 $\eta$가 있기 때문에 lower bound를 improve하면 $\eta$도 자연스럽게 improve가 된다는 것이다.

하지만 이 lower bound는 mixture policy (euation 5)에 대해서만 적용가능한데, mixture policy는 practical하지 않다는 문제가 있다.

<br />

## Monotonic Improvement Guarantee for General Stochastic Policies

Equation 6은 right-hand를 improve하면 left-hand가 improve됨을 보장한다는 의미인데, 이 논문에서는 conservative policy iteration에서만 적용가능한 이 수식을 general stochastic policies에도 적용가능하게 바꾼다.

![image](https://user-images.githubusercontent.com/59254578/73850600-5736cb80-486f-11ea-8434-6acefd040844.png)

$\alpha$는 $\pi$와 $\tilde\pi$ 사이의 distance를 total variation divergence로 측정하여 대체한다. total variation divergence는 아래와 같다.

$$D_\mathrm{TV}(p\parallel q) = \frac{1}{2}\sum_i\left\vert p_i - q_i\right\vert$$

즉, policy가 차이나는만큼 step size(update 정도)로 설정하겠다는 의미다.

<br>

또 다른 distance measure 방법에는 KL divergence가 있다. total variation divergence와 KL divergence 사이에는 다음과 같은 관계가 있다.

$$D_\mathrm{TV}(p\parallel q)^2 \leq D_\mathrm{KL}(p\parallel q)$$

따라서, 당연히 Equation 9가 가능하다.

![image](https://user-images.githubusercontent.com/59254578/73851758-63bc2380-4871-11ea-82b8-ce3251ab5838.png)

<br>

이로부터 $\eta$가 non-decreasing하는 Algorithm 1을 제시하였다.

![image](https://user-images.githubusercontent.com/59254578/73852104-fceb3a00-4871-11ea-9253-dca9ac17317a.png)

<br>

왜 monotonically improving ($\eta(\pi_0)\leq\eta(\pi_1)\leq\cdots$) 하는지 보자.

$M_i(\pi)=L_{\pi_i}(\pi) - CD_\mathrm{KL}^\max\left(\pi_i,\pi\right)$라고 할 때,

$$\begin{align} \eta \left(\pi_{i+1}\right) &\geq M_i\left(\pi_{i+1}\right)\\ \eta \left(\pi_{i}\right) &= M_i\left(\pi_{i}\right) \\ \eta \left(\pi_{i+1}\right) - \eta \left(\pi_{i}\right) &\geq M_i\left(\pi_{i+1}\right) - M_i\left(\pi_{i}\right) \end{align}$$

<br>

$M_i\left(\pi_{i+1}\right) - M_i\left(\pi_{i}\right)$가 매 itration마다 0보다 크거나 같기 때문에 $\eta$가 감소하지 않는 것이다.

![image](https://user-images.githubusercontent.com/59254578/73852822-2bb5e000-4873-11ea-8d60-81dff14f3b97.png)

minorization-maximization (MM) algorithm과 같이 $M_i$는 $\eta$를 minorize하는 surrogate function이다. 이 말은 $M_i$를 최대화하면 $\eta$도 자연스럽게 최대화가 된다는 것이다.

<br>

TRPO는 Algorithm 1에서 KL divergence를 penalty가 아닌 constraint로 사용하는 것이다.

<br />

## Optimization of Parameterized Policies

이제는 parametrized policies에 대한 optimization을 적용하기 위하여 어떻게 approximation을 하는지 살펴볼 것이다.

우선, 편의를 위해 notation을 다음과 같이 간략하게 바꾼다.

- $\eta(\theta):=\eta\left(\pi_\theta\right)$
- $L_\theta\left(\tilde\theta\right):=L_{\pi_\theta}\left(\pi_{\tilde\theta}\right)$
- $D_\mathrm{KL}\left(\theta\parallel\tilde\theta\right):=D_\mathrm{KL}\left(\pi_\theta\parallel\pi_{\tilde\theta}\right)$
- $\theta_\mathrm{old}$ : ***parameter of previous policy***

<br>

![image](https://user-images.githubusercontent.com/59254578/73856144-5d7d7580-4878-11ea-902f-22f6acebab9e.png)

바뀐 notation으로 algorithm 1의 핵심부분을 다시 표기하였다.

하지만 $C$는 매우 큰 값이다. 그래서 step size를 의미하는 divergence의 값이 매우 작게 되기 때문에 step size도 너무 작을 것이다.

<br >

![image](https://user-images.githubusercontent.com/59254578/73856312-99b0d600-4878-11ea-8ea7-c53ed9832d3f.png)

그래서 KL divergence를 penalty로 사용하는 게 아니라 constraint로 사용하면 좀 더 큰 step size를 가질 수 있게 된다. 이게 바로 TRPO의 핵심인 trust region constraint이다.

하지만 또 문제가 있다. 이 최적화문제의 constraint는 모든 state space에 대해서 성립해야 한다. 또한 maximum값을 매번 찾아야 하고, state가 많은 경우 constraint의 수가 매우 많아져서 문제를 풀기 어렵게 만든다. constraint의 수를 줄이기 위하여 다음과 같은 avergae KL divergence를 이용하는 heuristic approximation을 취한다.

![image](https://user-images.githubusercontent.com/59254578/73857221-f95bb100-4879-11ea-84e2-debaa1a85ae5.png)

<br>

따라서, 다음과 같이 최적화 문제를 풀 수 있다.

![image](https://user-images.githubusercontent.com/59254578/73857439-493a7800-487a-11ea-84c3-c8cc6975c811.png)

<br />

## Sample-Based Estimation of the Objective and Constraint

이제는 ample-based estimation 즉, Monte Carlo estimation을 할 수 있도록 알고리즘을 또 변형한다. 한마디로 practical 알고리즘을 만드려고 계속 바꾸는 것이다.

<br>

Equation 12로부터 아래와 같이 확장할 수 있다.

![image](https://user-images.githubusercontent.com/59254578/73860710-4db55f80-487f-11ea-9df0-6ddfc11e161b.png)

$L$에 대한 식을 보면 $\eta(\pi)$가 있는데 이것은 old policy의 expected discounted reward다. 이것은 상수나 마찬가지니까 결국 $sum_s \rho_{\theta_\mathrm{old} }(s)\sum_a\pi_{\theta_\mathrm{old} }(a\vert s)A_{\theta_\mathrm{old}(s,a) }$만 최대화하면 된다.

<br>

![image](https://user-images.githubusercontent.com/59254578/73858884-7a1bac80-487c-11ea-9dbf-6eba571c0b12.png)

Equation 14는 Equation 13에서 advantage function을 Q value로 대체하고(어차피 둘 다 상수취급이라서 상관없다고 한다), state distribution을 expectation취하고, *q*를 이용한 importance smapling을 추가한 것이다.

<br>

이 때 sampling하는 두 가지 방법이 있는데, single path와 vine이다.

<br>

### Single path

single path는 policy $\pi_{\theta_{old}}$를 simulation 돌리고 얻은 하나의 trajectory를 이용하는 방법이다.

<br>

### Vine

 vine은 해석하면 '덩굴식물'이라는 뜻이다. 덩굴식물처럼 여러가지가 있다고 생각하면 된다.

policy를 따라서 얻은 state들에 대해서 매 state마다 여러 action을 취하여 얻은 subset trajectories(여기선 rollout이라 표현)들을 다 이용하는 방법이다.

estimation의 variance를 낮출 수 있지만 계산량이 많고 한 state에서 여러 action을 수행할 수 있어야 하기 때문에 현실적인 문제에 적용하기에는 어려움이 있다고 한다.

<br>

## Conclusion

KL divergence penalty로 $\eta(\pi)$를 최적화하는 알고리즘인 TRPO가 monotonic improvement한다는 것을 증명하였다.

robotic locomotion 도메인에서도 TRPO는 성공적으로 controllers를 학습했고, raw images를 입력으로 받는 atari game에서도 만족할 만한 결과가 나왔다.

본 논문은 이론적 근간이 되는 방법을 제시하여 future work에 대한 출발점을 제공하였기에 의미가 큰 논문이다.



