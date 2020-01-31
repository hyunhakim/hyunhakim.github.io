# Deterministic Policy Gradient Algorithms paper review

<br />

## Abstract

본 논문은 기존의 stochastic policy gradient 방법보다 훨씬 효율적으로 최적의 policy를 찾아낼 수 있는 방법인 deterministic policy gradient 방법에 대해 소개한다.

<br />

## Introduction

deterministic policy gradient action-value function의 gradient를 따르는 단순한 형태라고 한다.

또한, deterministic policy gradient은 stochastic policy gradient의 policy variance가 제로인 제한된 경우라고 한다.

deterministic policy gradient은 state space에 대해서만 policy gradient를 구하기 때문에 action space와 state space 둘 다에 대해서 구하는 stochastic policy gradient에 비해 효율적이다.

본 논문에서는 deterministic policy gradient 알고리즘이 exploration을 하기 위해서 behavior policy는 stochastic으로 사용하고 target policy를 deterministic으로 사용하였다.

<br />

##  Background

일반적으로 action-value function을 추정하기 위해서 function approximator를 사용하는데, 이것의 문제는 bias가 생긴다는 것이다. 하지만, function approximator가 compatible하면 bias가 없다.

여기서 compatible 하다는 것은 다음 두가지를 의미한다.

- ![image](https://user-images.githubusercontent.com/59254578/73548670-87075d00-4484-11ea-93d2-a7756e3290f8.png)

  function approximator가 stochastic policy의 features에 linear하다.

- ![image](https://user-images.githubusercontent.com/59254578/73548889-e9f8f400-4484-11ea-878c-1eff0de94d59.png)

  w가 features로부터 function approximator를 추정하는 linear regression 문제에 대한 solution이다.

<br />

off_policy의 경우 performance objective가 target policy로 부터 얻은 value function과 behavior policy로부터 얻은 state distribution의 평균으로 바뀐다. 식은 아래와 같다.

![image](https://user-images.githubusercontent.com/59254578/73550583-2ed25a00-4488-11ea-8b26-1a9b1d1b1479.png)

off-policy actor-critic에서 behavior policy는 trajectories를 만들어내고, 이 trajectories로부터 critic은 value function을 추정하고 actor는 (target) policy parameter theta를 업데이트한다.

