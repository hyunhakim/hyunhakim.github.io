

Proximal Policy Optimization Algorithms (John Schulman  et al. 2017)

Paper Link : [https://arxiv.org/pdf/1707.06347.pdf](https://arxiv.org/pdf/1707.06347.pdf)

<br />

## Abstract

본 논문은 강화학습을 위한 policy gradient의 새로운 방법인 PPO에 대해 소개한다. 이 방법은 environment로부터 얻은 sampling data 들이 서로 교류(?)하는 방식이다. 또한, stochastic gradient ascent를 사용하는 "surrogate" objective function을 최적화하는 방법이다.

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



<br>

### Trust Region Methods



<br />

## Conclusion

매 policy update마다 multiple epochs of stochastic gradient ascent를 사용하는 policy optimization 방법인 PPO에 대해 소개하였다. PPO는 훨씬 간단하게 TRPO의 stability와 reliability를 지닌 알고리즘이다. vanilla policy gradient implementation 에서 단 몇 줄의 코드만 바꿔서 사용가능하기 때문에 general하다. 또한, performance도 전반적으로 좋은 것을 확인하였다.



