

Proximal Policy Optimization Algorithms (John Schulman  et al. 2017)

Paper Link : [https://arxiv.org/pdf/1707.06347.pdf](https://arxiv.org/pdf/1707.06347.pdf)

<br />

## Abstract

본 논문은 강화학습을 위한 policy gradient의 새로운 방법인 PPO에 대해 소개한다. stochastic gradient ascent를 사용하는 "surrogate" objective function을 최적화하는 방법이다.

standard policy gradient 방법은 data sample마다 한 번의 gradient update가 이루어지지만, 본 논문에서는 여러 epochs를 걸친 minibatch updates가 가능한 objective function을 제시한다.

PPO는 TRPO의 좋은 점들은 유지하되 훨씬 더 간단하고, general하고, 더 좋은 sample complexity를 갖는 방법이다. PPO는 a collection of benchmark tasks, including simulated robotic locomotion and Atari game playing에서 기존의 다른 online policy gradient methods보다 우수한 성능을 보여준다. 무엇보다 sample complexity, simplicity, wall-time 사이의 밸런스를 맞춘다는 장점이 있다.

<br />

## Introduction

최근 몇년간, 강화학습에 neural network function approximator를 사용하는 다양한 접근들이 제시되었다. 이에 대한 예시로는 Q learning, "vanilla" policy gradient, trust region / natural policy gradient 등이 있다. 하지만 이 방법들에는 몇 가지 문제점들이 있다.

- Q learning
  - 단순한 문제도 못푼다.
  - 이해가 잘 되지 않는다.

<br>

- vanilla policy gradient
  - poor data efficiency
  - robustness

<br>

- TRPO
  - complicate
  - dropout과 같은 noise를 포함한 architectures와 호환 불가
  - parameter sharing (between the policy and value function) 불가

<br>

PPO는 first-order optimization만 사용하면서 위의 문제들을 개선시킨다. clipped probability ratios라는 performance of the policy의 lower bound 즉, surrogate function을 제시하는데 이것을 사용하여 실험한 결과 기존의 여러 방법들에 비해 결과가 좋게 나온 것을 확인하였다.

<br />

## Background: Policy Optimization

<br>

### Policy Gradient Methods

![image](https://user-images.githubusercontent.com/59254578/74003510-2d3bf100-49b6-11ea-82c4-39a6a08e22a3.png)

일반적으로 사용하는 policy gradient estimator는 위와 같은 형태다.

우리가 이 방법을 implementation을 하고하 할 때, 자동으로 differentiation을 구해주는 software work(ex. SGD, adam optimizer etc.)를 사용하면 우리는 그냥 아래의 objective function을 구해서 넣어주면 된다.

![image](https://user-images.githubusercontent.com/59254578/74003625-a1769480-49b6-11ea-9ffd-825779d63013.png)

<br>

### Trust Region Methods

- **constraint version**

  ![image](https://user-images.githubusercontent.com/59254578/74003802-3b3e4180-49b7-11ea-8645-e942f92465e6.png)

  이전에 다룬 [TRPO 논문 리뷰](https://hyunhakim.github.io/Paper-review-Trust-Region-Policy-Optimization/)에서 importance sampling을 적용하여 위와 같이 surrogate objective를 도출하였다. 이 식의 의미가 뭔지 다시 살펴보자. advantage function $A$에 따라 이전의 policy $\pi_{\theta_{old}}$에 대해서 현재의 policy $\pi_\theta$를 얼마만큼(step size) update할 것인가를 의미한다. 이때 step size는 constraint $\delta$로 얼만큼 update 할지 정해준다.

<br>

- **penalty version**

  ![image](https://user-images.githubusercontent.com/59254578/74004482-38dce700-49b9-11ea-96ec-9c49b3458b1f.png)

  마찬가지로 $\beta$를 penalty로 사용하여 step size를 조절한다. 하지만 TRPO에서는 이 $\beta$의 적절한 값을 찾기가 어려웠다.

<br />

## Clipped Surrogate Objective

<br>

### Constraint version

![image](https://user-images.githubusercontent.com/59254578/74004748-0bdd0400-49ba-11ea-8949-3023a0db8541.png)

TRPO의 surrogate objective는 constraint가 없으면 policy update가 과도하게 발생할 수 있다. TRPO 논문에서 위의 surrogate objective를 도출했을 때, 현재 policy의 parameter가 old policy parameter의 근처에 있어야만 성립한다는 가정을 하기 때문에 update가 너무 과도하게 발생하면 학습이 제대로 이루어지지 않을 거다.

그래서 PPO에서는 cplipped surrogate objective를 사용한다.

![image](https://user-images.githubusercontent.com/59254578/74029437-0ac6c980-49f0-11ea-8691-91abc4667f3f.png)

$clip(r_t(\theta),1-\epsilon,1+\epsilon)\tilde{A}_t$은 policy update가 과도하게 발생하지 않도록 probability ratio $r_t(\theta)$를 clipping한 것이다. 여기서 $\epsilon$은 hyperparameter이기 때문에 문제에 따라 적절하게 정해줘야 한다. 여기서 min을 취하는 이유는 unclipped objective $r_t(\theta)\tilde{A}_t$의 lower bound가 되기 때문에 여전히 surrogate objective의 역할(새로운 policy의 expected discounted reward $\eta$를 최대화하기 위해 minorized된 objective)을 할 수 있는 것이다.

<br>

그림을 통해 더 자세히 살펴보자.

![image](https://user-images.githubusercontent.com/59254578/74005542-8e66c300-49bc-11ea-9ae1-d07297c5629a.png)

여기서 빨간 점은 starting point를 의미한다. 즉 아직 policy가 변하지 않은 지점이기 때문에 $r = 1$이다.

$A > 0$일 경우에는 취한 action이 좋다는 거니까 결국 objective를 최대화하기 위해서 $r > 1$인 방향으로 update를 할 것이다(단, $\epsilon$보다는 작게). 한마디로 그 action에 대한 확률을 좀 높인다고 생각하면 될 것 같다.

$A < 0$일 경우에는 취한 action이 좋지 않다는 거니까 결국 objective를 최대화하기 위해서 $r < 1$인 방향으로 update를 할 것이다(단, $\epsilon$보다는 작게). 한마디로 그 action에 대한 확률을 좀 낮춘다고 생각하면 될 것 같다.

<br>

### Penalty version

![image](https://user-images.githubusercontent.com/59254578/74005903-852a2600-49bd-11ea-8ae6-efb3b6d38241.png)

TRPO에서 $\beta$를 정하기 어렵다고 했다. PPO에서는 위와 같이 $\beta$를 매번 변경해준다.

$d < d_{targ}$이면, 즉, $d$가 너무 작으면 update가 너무 작게 일어났다는 의미니까 $\beta$를 작게 해줌으로써 다음 번엔 좀 더 update가 크게 일어날 수 있다. 

$d > d_{targ}$이면, 즉, $d$가 크면 update가 과하게 일어났다는 의미니까 $\beta$를 크게 해줌으로써 다음 번엔 좀 더 update가 작게 일어날 수 있다. 

<br>

실험을 통해 확인한 결과, clipping constraint 방법이 더 성능이 좋다고 한다.

<br />

## Algorithm

![image](https://user-images.githubusercontent.com/59254578/74006193-4183ec00-49be-11ea-893d-e0244d64b33f.png)

*N*개의 actor들이 병렬적으로 *T* timesteps 동안 sampling을 하면서 advantage estimates를 계산한다. 그럼 총 *NT*개의 data에 대해서 minibatch size *M*으로 나눠서 *K*번의 epoch 동안 surrogate *L*을 최적화한다.

<br>

만약 policy와 value function의 parameter를 공유하는 neural network 구조를 사용한다면, policy surrogate와 value function error term을 아래와 같이 반드시 결합해야한다.

![image](https://user-images.githubusercontent.com/59254578/74006634-a2f88a80-49bf-11ea-9fd9-f1a19ab735a9.png)

여기서 *S*는 entropy bonus라고 한다. exploration을 위해 추가한 term이다.

<br>

추가적으로 A3C와 같은 요즘 인기있는 PG의 style에서는 T time steps(T는 episode length 보다 훨씬 작은 크기) 동안 policy에 따라서 sample들을 얻고, 이 sample들을 업데이트에 사용한다. 이러한 방식은 time step T 까지만 고려하는 advantage estimator가 필요하며, A3C에서 다음과 같이 사용한다.

![image](https://user-images.githubusercontent.com/59254578/74029977-431ad780-49f1-11ea-99e2-1b0d9bf3c508.png)

여기서 $t$는 주어진 length T trajectory segment 내에서 $[0, T]$에 있는 time index다.

<br>

위의 수식에 더하여 generalized version인 GAE의 truncated version(generalized advantage estimation)을 사용한다.($\lambda$가 1이면 위 식과 같아짐) 수식은 아래와 같다.

![image](https://user-images.githubusercontent.com/59254578/74030053-72c9df80-49f1-11ea-8fb3-c8d6a5cfbcb9.png)

<br />

## Conclusion

매 policy update마다 multiple epochs of stochastic gradient ascent를 사용하는 policy optimization 방법인 PPO에 대해 소개하였다. PPO는 훨씬 간단하게 TRPO의 stability와 reliability를 지닌 알고리즘이다. 즉, TRPO의 개념을 practical하고 general하게 변형한 것이 PPO라고 생각하면 된다. PPO는 vanilla policy gradient implementation 에서 단 몇 줄의 코드만 바꿔서 사용가능하기 때문에 general하다. 또한, performance도 전반적으로 좋다.



