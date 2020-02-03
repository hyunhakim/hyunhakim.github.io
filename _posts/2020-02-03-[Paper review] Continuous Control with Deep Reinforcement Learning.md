

Deep Deterministic Policy Gradient (DDPG) (Timothy P. Lillicrap et al, 2016)

Paper Link : [https://arxiv.org/pdf/1509.02971.pdf](https://arxiv.org/pdf/1509.02971.pdf)

<br />

## Abstract

본 논문에서는 DQN의 좋은 점들과 deterministic policy gradient를 기반으로 한 아이디어를 알고리즘을 제시한다. 이 알고리즘은 continuous action space의 environment에서 효과적이고 planning algorithm에도 견줄만한 성능을 보인다.

<br />

## Introduction

DQN은 high dimensional observation space 문제를 풀 수 있지만, discrete & low dimensional action space만 다룰 수 있는 한계가 있다. continuous action space를 다루기 위해서는 매 스텝마다 iterative optimization process를 거쳐야 하기 때문이다.

continuous한 action space를 discretization을 하면 action space가 exponential하게 늘어나는 문제뿐만 아니라 discretization으로 인한 정보싀 손실이 생겨 섬세한 control을 할 수 없다.

그래서 본 논문에서 continuous control을 위한 접근법을 제시한다.

<br />

## Background

### Bellman Equation

$$Q^{\pi}(s_t, a_t)={\rm E}_{r_{i \geqq t},s_{i \geqq t} \backsim E, a_{i \geqq t} \backsim \pi } [R_{t} \vert s_t, a_t  ]$$

상태 $s_t$에서 행동 $a_t$를 취했을 때 Expected return은 위와 같다.

이를 벨만 방정식을 이용하여 변형하면 아래와 같다.

$$Q^{\pi}(s_t, a_t)={\rm E}_{r_{t},s_{t} \backsim E } [r(s_t,a_t)+\gamma {\rm E}_{a_{t+1} \backsim \pi } [ Q^{\pi}(s_{t+1}, a_{t+1}) ] ]$$

여기서 deterministic policy를 적용하면 아래와 같다.

$$Q^{\mu}(s_t, a_t)={\rm E}_{r_{t},s_{t} \backsim E } [r(s_t,a_t)+\gamma Q^{\mu}(s_{t+1}, \mu (s_{t+1})) ]$$

policy가 deterministic하기 때문에 두 번째 식에서 expectation이 빠진 것을 알 수 있다.



### Q learning

$L(\theta^{Q}) = {\rm E}_{s_t \backsim \rho^\beta , a_t \backsim \beta , r_t \backsim E} [(Q(s_t, a_t \vert \theta^Q)-y_t)^2]$

여기서 $y_t = r(s_t, a_t) + \gamma Q^{\mu}(s_{t+1},\mu(s_{t+1}))$ 이다.

또한, $\mu(s) = argmax_{a}Q(s,a)$ 로 Q learning은 argmax로 deterministic한 policy이므로 off-policy로 사용할 수 있다.



### DPG

$$\nabla_{\theta^\mu} J \approx  {\rm E}_{s_t \backsim \rho^\beta} [ \nabla_{\theta^\mu} Q(s, a \vert \theta ^ Q) \vert_{s=s_t, a=\mu(s_t)}]$$

$$\ \ \ \ \ \ \ \= {\rm E}_{s_t \backsim \rho^\beta} [ \nabla_{a} Q(s, a \vert \theta ^ Q) \vert_{s=s_t, a=\mu(s_t)} \nabla_{\theta^\mu} \mu(s \vert Q^{\mu})\vert_{s=s_t}]$$

위 수식에 대한 내용은 이전에 작성한 [DPG paper review](https://hyunhakim.github.io/Paper-review-Deterministic-Policy-Gradient-Algorithms/) 참조.
$$

$$


## Algorithm

### Replay Buffer

강화학습에 neural network를 사용할 때 sample들이 i.i.d.라는 가정을 한다. 하지만 실제로는 그렇지 않다.

이 문제를 해결하기 위해 사용하는 것이 바로 replay buffer다. exploration policy를 따라 얻은 sample들을 tuple형태로 저장한다.

DDPG 알고리즘은 off-policy 알고리즘이기 때문에 replay buffer의 사이즈가 커질 수 있다. 사이즈가 커진만큼 replay buffer에서 뽑은 uncorrelated transition set으로부터 학습할 수 있게 된다.

<br />

### Soft target update

![image](https://user-images.githubusercontent.com/59254578/73629365-3a936b80-4696-11ea-9aa8-a448eb201a45.png)

Q learning을 다시 살펴보면, network $Q(s,a\vert\theta^Q)$가 업데이트가 되면서 Loss를 줄여나가게 된다. 하지만 target value에서도 이 network가 계산에 사용되기 때문에 발산할 수 있고 불안정하다는 단점이 있다.

이 문제를 해결하기 위한 것이 target update이다.

DQN에서는 일정 주기마다 origin network의 weight를 target network로 직접 복사해서 사용한다.

DDPG에서는 exponential moving average(지수이동평균)을 사용하여 target network를 조금씩 변화시킴으로써 발산하지 않고 학습이 안정적으로 이루어지게 한다.

- critic network parameter update

  $\theta^{Q^{‘}} \leftarrow \tau \theta^{Q} + (1-\tau) \theta^{Q^{‘}}$ with $\tau\ \ll\ 1$

- actor network parameter update

  $\theta^{\mu^{‘}} \leftarrow \tau \theta^{\mu} + (1-\tau) \theta^{\mu^{‘}}$ with $\tau\ \ll\ 1$

<br />

### Batch normalization

서로 scale이 다른 feature를 state로 사용하면 network가 학습하는 데 어려움이 생기게 된다.

이 문제를 해결하기 위한 것이 batch normalization이다. 즉, feature들과 각 layer input의 scale을 맞춰준다.

![image](https://user-images.githubusercontent.com/59254578/73630263-feadd580-4698-11ea-98bc-427e3b0b369d.png)

<br />

### Noise Process

$\mu^\prime\ =\ \mu(s_t\vert\theta^\mu_t)\ +\ \mathcal{N}$

exploration을 위해서 output으로 나온 행동에 noise를 추가한다.

<br />

### Pseudo code of DDPG

![image](https://user-images.githubusercontent.com/59254578/73628157-c1dee000-4692-11ea-993f-7962e9bd2004.png)



