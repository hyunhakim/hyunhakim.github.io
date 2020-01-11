

## **Value Function Approximation**

이제 우리는 실생활에 적용가능한(large scale) 문제를 풀기 위한 방법을 배우기 시작할 거다.

## **1. Value Function Approximation**

![https://user-images.githubusercontent.com/59254578/72077396-8c1c3500-333a-11ea-953b-d6053ff1045f.png](https://user-images.githubusercontent.com/59254578/72077396-8c1c3500-333a-11ea-953b-d6053ff1045f.png)

지금까지 배운 value function은 lookup table을 사용하여 계산하였는데, state의 갯수가 무수히 많은 경우에는 적용하기 힘들다. 그래서 function approximation으로 처음 접하는 state들에 대해서도 잘 처리할 수 있도록(generalize) w를 update하며 학습할 것이다.

![https://user-images.githubusercontent.com/59254578/72077768-29776900-333b-11ea-850a-7c27edd77689.png](https://user-images.githubusercontent.com/59254578/72077768-29776900-333b-11ea-850a-7c27edd77689.png)

Value function approximation에는 다음과 같은 종류들이 있다.

w가 업데이트 되면서 blackbox 처럼 approximation을 만들어준다. 

<br />

## **2. Incremental Methods**

- **Gradient Descent**

![https://user-images.githubusercontent.com/59254578/72085776-41ee8000-3349-11ea-86f1-568006997bc0.png](https://user-images.githubusercontent.com/59254578/72085776-41ee8000-3349-11ea-86f1-568006997bc0.png)

머신러닝의 경사하강법과 똑같다. 여기서 1/2은 편의상 추가한 것. 목적함수에 제곱을 사용하기 때문에 미분하면 서로 상쇄되게 하려고.

- **Linear Value Function Approximation**

    ![https://user-images.githubusercontent.com/59254578/72086028-9eea3600-3349-11ea-8c7f-e62edf8494c2.png](https://user-images.githubusercontent.com/59254578/72086028-9eea3600-3349-11ea-8c7f-e62edf8494c2.png)

    목적함수는 value function과 approx의 차이이고 이 차이를 최소로 하려는 게 목적이다.

    stochastic gradient descent는 policy에 따라 한 sample을 구하고 이로부터 계산하는 것이기 때문에 기댓값을 취하지 않는다.

    ![https://user-images.githubusercontent.com/59254578/72086770-d9080780-334a-11ea-8d75-b6fb512f7b6c.png](https://user-images.githubusercontent.com/59254578/72086770-d9080780-334a-11ea-8d75-b6fb512f7b6c.png)

    여기서 x는 feature vector이다. 각 state를 잘 표현하는 새로운 feature로 나타내는 것이라고 생각하면 된다.

    linear model은 approx function을 feature와 weight의 linear combination으로 나타낸다. 단, 이것은 value function이 선형조합으로 표현될 것라는 가정이 필요한데 아무래도 실생활에 적용하기는 부족해 보인다.

    또한, 실제 value function은 사실 모르니까 이 경우 안다고 가정한 것이다.

    그래서 MC와 TD를 이용해서 이 value function을 대체한다.

    **Monte-Carlo with Value Function Approximation**

    ![https://user-images.githubusercontent.com/59254578/72089807-102ce780-3350-11ea-977d-7cc85ad264f7.png](https://user-images.githubusercontent.com/59254578/72089807-102ce780-3350-11ea-977d-7cc85ad264f7.png)

    value function 대신 return을 넣어 계산한다. local optimum으로 수렴한다고 한다.

    MC를 통해 얻은 return은 unbiased sample이므로 supervised learning을 적용할 수도 있다.

    **TD Learning with Value Function Approximation**

    ![https://user-images.githubusercontent.com/59254578/72090018-87fb1200-3350-11ea-860e-a3d80c6c33a9.png](https://user-images.githubusercontent.com/59254578/72090018-87fb1200-3350-11ea-860e-a3d80c6c33a9.png)

    TD(0)는 global optimum에 가깝게 수렴한다고 한다.

    **TD(λ) with Value Function Approximation**

    ![https://user-images.githubusercontent.com/59254578/72090352-3dc66080-3351-11ea-873a-fcd4b9e342df.png](https://user-images.githubusercontent.com/59254578/72090352-3dc66080-3351-11ea-873a-fcd4b9e342df.png)

    TD(λ)로 대체할 수도 있다.

    **Control with Value Function Approximation**

    ![https://user-images.githubusercontent.com/59254578/72116729-82242180-338e-11ea-8d94-03cb82044f5b.png](https://user-images.githubusercontent.com/59254578/72116729-82242180-338e-11ea-8d94-03cb82044f5b.png)

    Policy evaluation에 approx를 적용하면 control도 가능하다.

**Action-Value Function Approximation**

![https://user-images.githubusercontent.com/59254578/72116811-c6172680-338e-11ea-9c29-cb6c5aeda237.png](https://user-images.githubusercontent.com/59254578/72116811-c6172680-338e-11ea-9c29-cb6c5aeda237.png)

![https://user-images.githubusercontent.com/59254578/72116989-5d7c7980-338f-11ea-83cc-d4d3fd42a01b.png](https://user-images.githubusercontent.com/59254578/72116989-5d7c7980-338f-11ea-83cc-d4d3fd42a01b.png)

![https://user-images.githubusercontent.com/59254578/72117011-6ff6b300-338f-11ea-946c-62f9e9693723.png](https://user-images.githubusercontent.com/59254578/72117011-6ff6b300-338f-11ea-946c-62f9e9693723.png)

action-value function도 마찬가지로 value function과 똑같은 방식으로 approx를 구할 수 있다.

## **3. Batch Method**

![https://user-images.githubusercontent.com/59254578/72132053-58d0b900-33c1-11ea-9c37-69ad92bd1f02.png](https://user-images.githubusercontent.com/59254578/72132053-58d0b900-33c1-11ea-9c37-69ad92bd1f02.png)

지금까지는 SGD를 통해서 parameter를 update하는 방법을 사용했다. 하지만 SGD는 experience data를 한 번만 사용하는 것이 비효율적이라는 문제점이 있다고 한다.

Batch method는 training data(agent가 경험한 것)들을 모아서 한꺼번에 update하는 방법이다. 하지만 Batch방법은 한 번에 업데이트하는 만큼 그 많은 데이터들에 가장 잘 맞는 value function을 찾기가 어렵기 때문에 SGD와 Batch방법의 중간을 사용하는 경우도 많다. 예를 들면, step-by-step으로 업데이트하는 것이 아니고 100개의 데이터가 모일 때까지 기다렸다가 100번에 한 번씩 업데이트하는 "mini-batch"방법도 있다.

위에서 말하는 SGD의 문제점인 experience data를 한 번만 사용하는 것이 비효율적이다라고 말하는 점에 대해서는 한 번만 사용하지 않고 여러번 사용하는 것으로 문제를 해결할 수 있다.

**Experience Replay**

![https://user-images.githubusercontent.com/59254578/72132295-ead8c180-33c1-11ea-88d6-d76d872ec5fb.png](https://user-images.githubusercontent.com/59254578/72132295-ead8c180-33c1-11ea-88d6-d76d872ec5fb.png)

Replay memory라는 것을 만들어 놓고서 agent가 경험했던 것들을 time-step마다 끊어서 저장해놓는다. action-value function의 parameter를 update하는 것은 time-step마다 하지만 하나의 transition에 대해서만 하는 것이 아니고 모아놓았던 transition을 repaly memory에서 100개면 100개 200개면 200개씩 꺼내서 그 mini-batch에 대해서 update를 진행한다.

이렇게 할 경우에 sample efficient할 수도 있지만 또한 episode내에서 스텝 바이 스텝으로 업데이트를 하면 그 데이터들 사이의 correlation 때문에 학습이 잘 안되는 문제도 해결할 수 있다는 장점이 있다.

<br />

<br />

<br />

<br />

**※ 참고문헌 및 자료**

- [[David Silver's Lecture]](http://www0.cs.ucl.ac.uk/staff/d.silver/web/Teaching.html)
- [[팡요랩]](https://www.youtube.com/channel/UCwkGvF7xKz2E0Lv-fZ9wv2g)
- [[Fundamental of Reinforcement Learning](https://dnddnjs.gitbook.io/rl/)]