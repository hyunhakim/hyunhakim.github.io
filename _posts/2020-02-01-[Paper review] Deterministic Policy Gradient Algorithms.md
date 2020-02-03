

Deterministic Policy Gradient Algorithms (D.silver et al, 2014)

Paper Link : [main text](http://proceedings.mlr.press/v32/silver14.pdf), [supplementary material](http://proceedings.mlr.press/v32/silver14-supp.pdf)

<br />

## Abstract

본 논문은 기존의 stochastic policy gradient 방법보다 훨씬 효율적으로 최적의 policy를 찾아낼 수 있는 방법인 deterministic policy gradient 방법에 대해 소개한다.

<br />

## Introduction

deterministic policy gradient는 action-value function의 expected gradient 형태다.

또한, deterministic policy gradient은 stochastic policy gradient의 policy variance가 제로인 제한된 경우라고 한다.

deterministic policy gradient은 state space에 대해서만 policy gradient를 구하기 때문에 action space와 state space 둘 다에 대해서 구하는 stochastic policy gradient에 비해 효율적이다. 특히 high dimensional action space를 가지는 tasks에서의 성능 향상이 크다.

본 논문에서는 deterministic policy gradient 알고리즘이 exploration을 하기 위해서 behavior policy는 stochastic으로 사용하고 target policy를 deterministic으로 사용하였다.

<br />

##  Background

일반적으로 action-value function을 추정하기 위해서 function approximator를 사용하는데, 이것의 문제는 bias가 생긴다는 것이다. 하지만, function approximator가 compatible condition에 부합하면 bias가 발생하지 않는다.

여기서 compatible condition은 다음 두가지를 의미한다.

- ![image](https://user-images.githubusercontent.com/59254578/73548670-87075d00-4484-11ea-93d2-a7756e3290f8.png)

  function approximator가 stochastic policy의 features에 linear하다.

- ![image](https://user-images.githubusercontent.com/59254578/73548889-e9f8f400-4484-11ea-878c-1eff0de94d59.png)

  w가 features로부터 function approximator를 추정하는 linear regression 문제에 대한 solution이다.

<br />

off_policy의 경우 performance objective가 target policy로 부터 얻은 value function과 behavior policy로부터 얻은 state distribution의 평균으로 바뀐다. 식은 아래와 같다.

![image](https://user-images.githubusercontent.com/59254578/73550583-2ed25a00-4488-11ea-8b26-1a9b1d1b1479.png)

off-policy actor-critic에서 behavior policy는 trajectories를 만들어내고, 이 trajectories로부터 critic은 value function을 추정하고 actor는 (target) policy parameter theta를 업데이트한다.

<br />

## Gradients of Deterministic Policies

이제부터 policy gradient framework가 어떻게 deterministic policy로 확장되는지 살펴본다.



### Action-Value Gradients

continuous action space에서 policy improvement를 할 때, greedy 방법을 사용할 때의 문제점은 매 step마다 global maximization을 필요로 한다는 것이다.

greedy 방법 대신 우리는 아래와 같이 gradient를 구해서 개선할 수 있다.

![image](https://user-images.githubusercontent.com/59254578/73586069-d16ff480-44eb-11ea-826f-1c34357a6ddf.png)

위 식에서 chain rule을 적용하면 아래와 같이 action에 대한 action-value의 gradient와 policy parameter에 대한 policy gradient로 쪼갤 수 있다.

![image](https://user-images.githubusercontent.com/59254578/73586142-84d8e900-44ec-11ea-9c56-106569741ec5.png)

policy가 바뀌게 되면 방문하게 되는 state가 달라질 것이고, 그럼 당연히 state distribution ρ 또한 바뀌게 된다. 하지만 stochastic policy gradient theorem에서 이것은 gradient를 구할 때 무시할 수 있다.

이처럼 deterministic policy gradient theorem에서도 state distribution의 gradient를 구할 필요가 없다.



### Deterministic Policy Gradient Theorem

- deterministic policy	

![image](https://user-images.githubusercontent.com/59254578/73602798-a0abc000-45bc-11ea-968d-446390c36360.png)

- performance objective

![image](https://user-images.githubusercontent.com/59254578/73602804-bde08e80-45bc-11ea-9ab5-0fc1b1a6a7e6.png)

- probability distribution

![image](https://user-images.githubusercontent.com/59254578/73602808-d2248b80-45bc-11ea-8aa9-efd95d15e5b6.png)

- discounted state distribution

  ![image](https://user-images.githubusercontent.com/59254578/73602814-e2d50180-45bc-11ea-928e-5100e28591a4.png)



우리가 알고 싶은 것은 deterministic policy의 특성을 사용해도 sutton의 policy gradient theorem을 만족하냐는 것이다.



![image](https://user-images.githubusercontent.com/59254578/73594815-2e53c500-4555-11ea-8813-0c4d444ea163.png)

여기서 중요한 것은 deterministic policy gradient는 action-value function의 expected gradient 형태라는 것이고 action은 deterministic하게 결정되므로 state space에 대해서만 computation을 하면 된다는 것이다.

위의 theorem 1의 증명은 아래와 같다. 더 자세한 증명은 appendix를 참고하자.

![image](https://user-images.githubusercontent.com/59254578/73595056-88558a00-4557-11ea-9833-6654439cdeb0.png)

<br />

### Limit of the Stochastic Policy Gradient

deterministic policy gradient는 언뜻 봐서는 아래의 stochastic 버전처럼 안보인다.

![image](https://user-images.githubusercontent.com/59254578/73603047-ff733880-45c0-11ea-91d7-e31436ff36e9.png)



하지만 stochastic policy의 variance가 0으로 수렴하면 stochastic policy gradient와 deterministic policy gradient는 동일해진다.

![image](https://user-images.githubusercontent.com/59254578/73603114-950ec800-45c1-11ea-8f75-a78a0c91317d.png)

위의 theorem 2의 증명은 appendix를 참고.

이 정리가 중요한 이유는 deterministic policy gradient를  compatible function approximation, actor-critic, episodic/batch methods 등 기존의 유명한 policy gradients 기법들에 stochastic 버전처럼 적용할 수 있다는 것이다.

<br />

## Deterministic Actor-Critic Algorithms

이제 deterministic policy gradient theorem을 이용하여 on-policy와 off-policy actor-critic algorithms를 유도할 것이다.

<br />

### On-Policy Deterministic Actor-Critic

이 방법의 문제는 deterministic은 적절하게 exploration을 하지 않기 때문에 sub-optimal에 빠질 수 있다.

아래의 수식들은 SARSA critic을 이용한 on-policy actor-critic algorithm이다.

![image](https://user-images.githubusercontent.com/59254578/73604629-115fd600-45d7-11ea-8e0a-669cfe560b46.png)

critic은 실제 $Q_\mu (s,a)$ 대신 미분 가능한 $Q_w (s,a)$로 대체하여 action-value function을 estimate하며, 이 둘 간 mean square error를 최소화하는 것이 목표다. actor는 보상이 최대화되는 방향, 즉, deterministic policy를 stochastic gradient ascent 방법으로 update 한다.

<br />

### Off-Policy Deterministic Actor-Critic



stochastic behavior policy $\beta(a\vert s)$에 의해 생성된 trajectories로부터 deterministic target policy $\mu_\theta(s)$를 학습하는 off-policy actor-critic 알고리즘이다. stochastic behavior policy에 의해 적절하게 탐험이 가능해진다.



![image](https://user-images.githubusercontent.com/59254578/73604692-330d8d00-45d8-11ea-8c12-8976426dcdd8.png)

on-policy 알고리즘과 다른 점은 update target 부분에서 다음 액션을 $\mu_\theta(s_{t+1})$로 사용한 것이다. $\mu_\theta(s_{t+1})$는 가장 높은 Q 값을 가지는 행동이 된다. 즉, Q-Learning인 것이다.

보통 Stochastic off-policy actor-critic은 대개 actor와 critic 모두 importance sampling을 필요로 하지만, deterministic policy gradient에선 importance sampling이 필요없다.

그 이유는 다음과 같다.

- Actor

  deterministic policy gradients는 action에 대한 integral(적분)이 사라지기 때문에, 즉, deterministic이므로 policy가 distribution을 갖지 않기 때문에 sampling 자체가 필요가 없다.

- Critic

  Critic이 사용하는 Q-learning은 importance sampling이 필요없는 off policy 알고리즘으로, Q-learning도 업데이트 목표를 특정 분포에서 샘플링을 통해 estimate 하는 것이 아니라 Q 함수를 최대화하는 action을 선택하는 것이기에 위 actor 에서의 deterministic 경우와 비슷하게 볼 수 있다.

<br />

### Compatible Function Approximation

function approximator는 bias가 발생하고, off-policy learning에 의한 instabilities 문제점이 있다.

그래서 stochastic처럼 $\nabla_a Q_\mu(s,a)$를 $\nabla_a Q_w(s,a)$로 대체해도 deterministic policy gradient에 영향을 미치지 않을 compatible function approximator $Q_w(s,a)$를 찾아야 한다.

![image](https://user-images.githubusercontent.com/59254578/73608605-5e0fd500-4608-11ea-80bf-6d1a811e0da1.png)

위의 1 / 2의 조건을 만족하면 $Q_w(s,a)$는 deterministic policy $\mu_\theta(s)$와 compatible 하다는 것이 theorem 3이다.



- COPDAC-Q algorithm (Compatible Off-Policy Deterministic Actor-Critic Q-learning critic)

  ![image](https://user-images.githubusercontent.com/59254578/73608673-366d3c80-4609-11ea-8443-c97e56b2dc7c.png)

<br />

## Experiments



### Continuous Bandit

![image](https://user-images.githubusercontent.com/59254578/73608717-9d8af100-4609-11ea-9433-f9510704a69d.png)

Stochastic Actor-Critic (SAC)과 COPDAC 간 성능 비교

- action dimension이 커질수록 성능 차이가 심해진다.
- 빠르게 수렴하는 것을 통해 DPG의 data efficiency가 SPG에 비해 좋다는 것을 확인할 수 있다.

<br />

### Continuous Reinforcement Learning

![image](https://user-images.githubusercontent.com/59254578/73608743-ddea6f00-4609-11ea-9268-1e0268769233.png)

COPDAC-Q, SAC, off-policy stochastic actor-critic(OffPAC-TD) 간 성능 비교

- COPDAC-Q의 성능이 약간 더 좋다.
- COPDAC-Q의 학습이 더 빨리 이루어진다.

<br />

### Octopus Arm

![image](https://user-images.githubusercontent.com/59254578/73608773-30c42680-460a-11ea-9b6e-3d65cdad243f.png)

COPDAC-Q 사용 시, action space dimension이 큰 octopus arm을 잘 control하여 target을 맞춘다.