Trust Region Policy Optimization (John Schulman  et al. 2017)

Paper Link : [https://arxiv.org/pdf/1502.05477.pdf](https://arxiv.org/pdf/1502.05477.pdf)

<br />

## Abstract

본 논문에서는  monotonic improvement를 보장하는, 반복적으로 policy optimizing하는 알고리즘인 TRPO에 대해서 소개한다. 이 알고리즘은 이론적으로 검증된 몇몇의 approximation을 사용하여 실용적인 알고리즘이다. 이 알고리즘은 [natural policy gradient (Kakade, 2002)](http://papers.nips.cc/paper/2073-a-natural-policy-gradient.pdf) 와 유사하고 neural network와 같이 큰 nonlinear policies를 최적화하는 데 효과적이다. 다양한 task들에 대해서 robust performance를 낸다고 한다.

<br />

## Preliminaries

![image](https://user-images.githubusercontent.com/59254578/73845521-2f8f3580-4866-11ea-9337-fba9adf84f5e.png)

우리의 목적은 $\eta(\pi)$를 최대화하는 것이다.

<br />

## Conclusion

KL divergence penalty로 $\eta(\pi)$를 최적화하는 알고리즘인 TRPO가 monotonic improvement한다는 것을 증명하였다.

robotic locomotion 도메인에서도 TRPO는 성공적으로 controllers를 학습했고, raw images를 입력으로 받는 atari game에서도 만족할 만한 결과가 나왔다.

본 논문은 이론적 근간이 되는 방법을 제시하여 future work에 대한 출발점을 제공하였기에 의미가 큰 논문이다.



