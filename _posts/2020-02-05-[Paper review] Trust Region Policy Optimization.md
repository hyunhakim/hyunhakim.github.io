

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





그렇다면 $\pi_{\theta_0}$를 얼마나 작게 변화시켜야 하는 것인가.

<br />

## Conclusion

KL divergence penalty로 $\eta(\pi)$를 최적화하는 알고리즘인 TRPO가 monotonic improvement한다는 것을 증명하였다.

robotic locomotion 도메인에서도 TRPO는 성공적으로 controllers를 학습했고, raw images를 입력으로 받는 atari game에서도 만족할 만한 결과가 나왔다.

본 논문은 이론적 근간이 되는 방법을 제시하여 future work에 대한 출발점을 제공하였기에 의미가 큰 논문이다.



