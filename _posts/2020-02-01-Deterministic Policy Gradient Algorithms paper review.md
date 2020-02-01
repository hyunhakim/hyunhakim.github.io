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

이처럼 stochastic policy gradient theorem에서도 state distribution의 gradient를 구할 필요가 없다.

