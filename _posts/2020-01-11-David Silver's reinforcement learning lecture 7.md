

## Policy Gradient

![https://user-images.githubusercontent.com/59254578/72204240-27d3af80-34b9-11ea-99e4-53e33c7c8aec.png](https://user-images.githubusercontent.com/59254578/72204240-27d3af80-34b9-11ea-99e4-53e33c7c8aec.png)

지금까지는 value function을 기반으로 policy를 정해왔다. 즉, value가 최대가 되도록 하는 action을 정해왔다.

![https://user-images.githubusercontent.com/59254578/72204284-800ab180-34b9-11ea-9b8a-d4fe69e82d49.png](https://user-images.githubusercontent.com/59254578/72204284-800ab180-34b9-11ea-9b8a-d4fe69e82d49.png)

이제는 value based가 아닌 바로 policy를 정하는 방법에 대해 배운다.

이 방법의 장점은 다음과 같다.

![https://user-images.githubusercontent.com/59254578/72204349-1b9c2200-34ba-11ea-97a5-f3ceabf9f818.png](https://user-images.githubusercontent.com/59254578/72204349-1b9c2200-34ba-11ea-97a5-f3ceabf9f818.png)

continuous action space에서는 value based로는 풀 수가 없지만 이 방법은 효과적이다.

또한, value based는 value가 최대인 action을 하도록 deteministic하게 정하였지만, 이 방법은 action별로 확률이 존재하도록 학습할 수 있다. 대신, 그만큼 policy가 확확 바뀌는게 아니라서 local에 수렴할 수 있지만 stable하다.

**Policy Objective Function**

![https://user-images.githubusercontent.com/59254578/72204694-0fb25f00-34be-11ea-8906-ba3c68e51e5a.png](https://user-images.githubusercontent.com/59254578/72204694-0fb25f00-34be-11ea-8906-ba3c68e51e5a.png)

그렇다면 value function을 안쓰는데 policy를 어떻게 평가할 것인지 의문이 들텐데, 바로 더 근본적인 목적인 reward이다. value function도 결국 총 얻는 reward에 대한 기댓값이므로 reward를 최대화 시키도록 목적함수를 정하면 좋은 policy를 찾을 수 있을 것이다.

보통 위에서 보이는 3가지 목적함수를 이용한다고 한다.

stationary distribution은 각 state에 머무르는 비율로 이해하면 될 것 같다. time-step마다 받은 reward들을 discount시키지 않고 stationary distribution을 사용해서 각 state에서 우리의 목적(maximize objective function)을 위해 어떤 행동이 좋았나 판단하는 거라고 생각한다.

**Policy Gradient**

![https://user-images.githubusercontent.com/59254578/72204813-6b311c80-34bf-11ea-9912-df0a7edbf35e.png](https://user-images.githubusercontent.com/59254578/72204813-6b311c80-34bf-11ea-9912-df0a7edbf35e.png)

똑같이 목적함수에 대한 gradient를 구하고 이를 통해 J를 최대화 하는 것이다. 그래서 gradient ascent인 문제이다.

## **1. Finite Difference Policy Gradient**

![https://user-images.githubusercontent.com/59254578/72204917-6325ac80-34c0-11ea-8b81-2994ee60b82f.png](https://user-images.githubusercontent.com/59254578/72204917-6325ac80-34c0-11ea-8b81-2994ee60b82f.png)

이 방법은 수치적인 방법으로서 가장 간단하게 objective function의 gradient를 구할 수 있는 방법이다. 만약 Parameter vector가 5개의 dimention로 이루어져 있다고 한다면 각 parameter를 조금씩 변화시켜보고 5개의 parameter에 대한 Gradient를 각각 구한다. parameter space가 작을 때는 간단하지만 늘어날수록 비효율적이고 노이지한 방법. policy가 미분 가능하지 않더라도 작동한다는 장점이 있어서 초기 policy gradient에서 사용되던 방법이다.

## **2. Monte-Carlo Policy Gradient**

앞에서 살펴봤던 Finite Difference Policy gradient는 numerical한 방법이고 앞으로 살펴볼 Monte-Carlo Policy Gradient와 Actor-Critic은 analytical하게 gradient를 계산하는 방법이다. analytical하게 gradient를 계산한다는 것은 objective function에 직접 gradient를 취해준다는 것이다.

**Score Function**

![https://user-images.githubusercontent.com/59254578/72205494-f95cd100-34c6-11ea-93a7-99597c0360e6.png](https://user-images.githubusercontent.com/59254578/72205494-f95cd100-34c6-11ea-93a7-99597c0360e6.png)

목적함수를 θ에 대해서 미분했는데 갑자기 왜 log가 나왔는지는 아래에서 설명하겠다.

중요한 것은 π가 미분을 해도 살아남아 있기 때문에 expectation을 취할 수 있게 된 것이다.

expectation을 취할 수 있게 만드려고 미분하는 것을 조금 변형시켜서 log가 나온 것이다.

이렇게 하면 좋은 점은 직접 얻은 reward를 통해 구하기 때문에 unbiased하다는 것이다.

![https://user-images.githubusercontent.com/59254578/72205464-9ff4a200-34c6-11ea-9450-97273365b79e.png](https://user-images.githubusercontent.com/59254578/72205464-9ff4a200-34c6-11ea-9450-97273365b79e.png)

log가 어떻게 나오게 됐는지 위의 식을 보면 알 수 있다.

즉, score function과 sample을 통해 얻은 reward r(on-tep해서 얻은 r)을 통해서 objective function의 gradient를 구할 수 있다.

**Softmax policy**

![https://user-images.githubusercontent.com/59254578/72205711-b2bca600-34c9-11ea-99cf-0d3f4ef0541a.png](https://user-images.githubusercontent.com/59254578/72205711-b2bca600-34c9-11ea-99cf-0d3f4ef0541a.png)

softmax를 사용해서 score function을 위와 같이 구할 수 있다.

**Gaussian Policy**

![https://user-images.githubusercontent.com/59254578/72205751-f2838d80-34c9-11ea-81f2-907b1135eaf9.png](https://user-images.githubusercontent.com/59254578/72205751-f2838d80-34c9-11ea-81f2-907b1135eaf9.png)

반대로 continuous action space는 gaussian을 사용해서 위와 같이 쉽게 구할 수 있다.

**Policy Gradient Theorem**

![https://user-images.githubusercontent.com/59254578/72205692-80ab4400-34c9-11ea-9dfc-8f2e681df505.png](https://user-images.githubusercontent.com/59254578/72205692-80ab4400-34c9-11ea-9dfc-8f2e681df505.png)

지금까지 one-step에 대해서 이야기 했는데 multi-step의 경우엔 그냥 r 대신 q-function을 사용하면 된다.

**Monte-Carlo Policy Gradient (REINFORCE)**

![https://user-images.githubusercontent.com/59254578/72205788-5312ca80-34ca-11ea-9f8a-1aa338d2e135.png](https://user-images.githubusercontent.com/59254578/72205788-5312ca80-34ca-11ea-9f8a-1aa338d2e135.png)

policy gradient theorem에서 q function을 사용한다고 했는데 action value function의 값을 어떻게 알 수 있을까? 이전에 모든 state에 대해 action value function을 알기 어려워서 approximation을 했었는데 policy자체를 update하려니 기준이 필요하고 그러다보니 action value function을 사용해야하는데 사실 이 값을 알 방법이 애매하다.

하지만 알 수 있는 방법이 있는데 바로 Monte-Carlo방법이다. episode를 가보고 받았던 reward들을 기억해놓고 episode가 끝난 다음에 각 state에 대한 return을 계산하면 된다. return자체가 action value function의 unbiased estimation이다. 이 알고리즘은 REIFORCE라고도 한다.

여기서 v는 return을 의미한다.

## **3. Actor-Critic Policy Gradient**

MC 방법은 이전 강의에서 unbiased이지만 variance가 크다는 단점이 있다고 언급한 적이 있다. 이 variance를 줄이기 위한 방법이 actor-critic 방법이다.

**Actor & Critic**

![https://user-images.githubusercontent.com/59254578/72206368-37122780-34d0-11ea-80d1-7aee2e87b9c7.png](https://user-images.githubusercontent.com/59254578/72206368-37122780-34d0-11ea-80d1-7aee2e87b9c7.png)

critic과 actor 두 개의 parameter를 둘 다 학습하는 방법이다.

q function도 학습을 해서 policy iteration처럼 평가하고 개선하고를 반복한다.

이 Critic은 action value function을 통해 현재의 Policy를 평가하는 역할을 한다. action을 해보고 그 action의 action value function이 높았으면 그 action을 할 확률을 높이도록 policy의 parameter를 update하는데 그 판단척도가 되는 action value function또한 처음에는 잘 모르기 때문에 학습을 해줘야하고 그래서 critic이 필요하다.

![https://user-images.githubusercontent.com/59254578/72206485-d1bf3600-34d1-11ea-9579-da6495d5afae.png](https://user-images.githubusercontent.com/59254578/72206485-d1bf3600-34d1-11ea-9579-da6495d5afae.png)

여기서 TD(0)를 사용하여 action-value function을 linear하게 approximation한다.

하지만 여전히 variance가 크다는 문제가 있다고 한다. 예를 들어, q 값이 모두 엄청 크게 나왔을 경우에는 θ가 제대로 update 되지 않을 거다. 즉, 우리는 어떤 기준을 통해서 상대적인 기여도를 알고 update를 제대로 하고 싶은데 그러지 못한다는 거다. 이 문제를 해결할 수 있는 개념이 baseline이다.

**Baseline**

![https://user-images.githubusercontent.com/59254578/72206676-16e46780-34d4-11ea-82d7-f4d2c991ec69.png](https://user-images.githubusercontent.com/59254578/72206676-16e46780-34d4-11ea-82d7-f4d2c991ec69.png)

Q function 이후로 사용하고 있지 않던 State value function을 일종의 평균으로 사용해서 현재의 행동이 평균적으로 얻을 수 있는 value보다 얼마나 더 좋나라는 것을 계산하도록 해서 variance를 줄이는 것이다. 즉, 지금까지 해왔던 것보다 좋으면 그 방향으로 update를 하고, 아니면 그 반대방향으로 가겠다는 것이다.

B는 state에 대한 임의의 함수이기 때문에 state value function을 사용해도 되고, B(V)를 Q에서 빼줘도 expectation은 변하지 않는다. 왜냐하면 위의 증명처럼 state에 대한 함수는 0이기 때문이다. 0인 이유는 π를 모든 action에 대해 더하면 1이고 이걸 미분하면 0이니까.

![https://user-images.githubusercontent.com/59254578/72206758-f10b9280-34d4-11ea-93cb-cf767e0c23df.png](https://user-images.githubusercontent.com/59254578/72206758-f10b9280-34d4-11ea-93cb-cf767e0c23df.png)

그치만 V를 추가하면 V도 학습을 해줘야해서 연산량이 많아진다는 단점이 있다.

![https://user-images.githubusercontent.com/59254578/72206938-fff34480-34d6-11ea-8a2a-c799d35e0629.png](https://user-images.githubusercontent.com/59254578/72206938-fff34480-34d6-11ea-8a2a-c799d35e0629.png)

TD error는 advantage function의 unbiased estimate이므로 policy gradient 계산할 때 A 대신 TD error를 써도 된다는 말이다. 그래서 이 방법으로 q, v 둘 다 학습하는 게 아니라 v만 학습을 하면 되기 때문에 효율적이다.

![https://user-images.githubusercontent.com/59254578/72207065-5319c700-34d8-11ea-90bb-089a3b13b0a6.png](https://user-images.githubusercontent.com/59254578/72207065-5319c700-34d8-11ea-90bb-089a3b13b0a6.png)

TD(λ)와 eligibility trace도 사용해서 쓸 수 있다.

![https://user-images.githubusercontent.com/59254578/72207045-236abf00-34d8-11ea-882e-5a3068c02fd9.png](https://user-images.githubusercontent.com/59254578/72207045-236abf00-34d8-11ea-882e-5a3068c02fd9.png)

<br />

<br />

<br />

<br />

**※ 참고문헌 및 자료**

- [[David Silver's Lecture]](http://www0.cs.ucl.ac.uk/staff/d.silver/web/Teaching.html)
- [[팡요랩]](https://www.youtube.com/channel/UCwkGvF7xKz2E0Lv-fZ9wv2g)
- [[Fundamental of Reinforcement Learning](https://dnddnjs.gitbook.io/rl/)]