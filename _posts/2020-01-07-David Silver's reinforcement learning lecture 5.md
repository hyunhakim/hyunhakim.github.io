

## **Model-Free Control**

<br />

## **1. On and Off-Policy Learning**

- **On-policy learning**

    현재 움직이고 있는(action을 취하는) policy와 improve하는 policy가 같다.

    <br />

- **Off-policy learning**

    action을 취하는 policy(behavior policy)와 improve하는 policy(target policy)를 다르게 취한다.

    <br />

## **2. On-policy Monte-Carlo Control**

Policy Iteration을 다시 보면, policy evaluation과 policy improvement를 반복해서 최적의 policy를 찾아낸다(단, model-baesd인 상황에서).

그렇다면 policy evaluation을 mc를 적용하고 policy improvement를 기존처럼 greedy policy improvement를 사용하면 model-free에서 control 문제를 풀 수 있지 않을까 생각할 수 있다. 하지만, 기존의 greedy policy improvement는 MDP를 알아야(model-based)하므로 불가능하다.

![https://user-images.githubusercontent.com/18068210/71872854-f9746e00-3160-11ea-936d-bd7c85247a21.png](https://user-images.githubusercontent.com/18068210/71872854-f9746e00-3160-11ea-936d-bd7c85247a21.png)

value function은 MDP model을 알아야 greedy policy improvement가 가능(기존 방식).

그치만 q-function은 model-free에 대해서도 greedy policy improvement가 가능하다. model은 몰라도 action을 해보고 거기서 나오는 state 중에서 최댓값을 얻을 수 있으니까.

<br />

![https://user-images.githubusercontent.com/18068210/71874620-ba94e700-3165-11ea-88f0-b2e1cd38c8b8.png](https://user-images.githubusercontent.com/18068210/71874620-ba94e700-3165-11ea-88f0-b2e1cd38c8b8.png)

위의 예시는 action-value function기반의 MC로 policy iteration을 풀 때의 문제점을 보여준다.

처음에 오른쪽 문을 열었을 때가 reward를 주기 때문에 agent는 계속해서 오른쪽이 더 좋은줄 알고 오른쪽만 연다.

처음 단 한번의 선택으로 이것이 최적으로 향하는 것일까? 그렇지 않다는 것이다. 모든 state들에 대해서 고려하지 않을 수 있기 때문이다.

이 문제를 보완한 것이 ε-greedy exploration이다.

<br />

- **ε-Greedy Exploration**

    ![https://user-images.githubusercontent.com/18068210/71874912-6e967200-3166-11ea-886b-7a7c25bd8350.png](https://user-images.githubusercontent.com/18068210/71874912-6e967200-3166-11ea-886b-7a7c25bd8350.png)

    낮은 확률(ε/m)로 임의의 다른 action을 취하도록 함으로써 모든 state들을 고려할 수 있게 하고, 높은 확률로 최대의 q function을 갖도록 하는 action을 함으로써 최적으로 수렴하게 한다.

    <br />

    ![https://user-images.githubusercontent.com/18068210/71875953-f2515e00-3168-11ea-9841-cb2d4e208699.png](https://user-images.githubusercontent.com/18068210/71875953-f2515e00-3168-11ea-9841-cb2d4e208699.png)

    ε-greedy exploration을 쓰면 왜 최적으로 수혐하는지, 왜 개선이 되는지를 수식으로 나타낸 것이다.

<br />

- **Monte-Carlo Policy Iteration**

    ![https://user-images.githubusercontent.com/18068210/71878159-51fe3800-316e-11ea-975f-581635f50e0f.png](https://user-images.githubusercontent.com/18068210/71878159-51fe3800-316e-11ea-975f-581635f50e0f.png)

    정리하면 다음과 같이 policy evaluation은 MC, policy improvement는 ε-greedy policy improvement를 사용.

<br />

- **Monte-Carlo Control**

    ![https://user-images.githubusercontent.com/18068210/71878394-dfda2300-316e-11ea-80cf-b59ea423097a.png](https://user-images.githubusercontent.com/18068210/71878394-dfda2300-316e-11ea-80cf-b59ea423097a.png)

    좀 더 효율적으로 하기 위해서 sampling한 episode들 전체가 끝날 때까지 기다리는 게 아니라, 매 episode가 끝날 때마다 평가하고 개선하는 방법이다.

    <br />

    - **Greedy in the Limit with Infinite Exploration (GLIE)**

        ![https://user-images.githubusercontent.com/59254578/71896413-162b9880-3197-11ea-818a-7c7fad23536c.png](https://user-images.githubusercontent.com/59254578/71896413-162b9880-3197-11ea-818a-7c7fad23536c.png)

        GLIE는 두 가지 조건을 의미한다.

        첫 번째로, atate-action 쌍이 무한히 많이 탐험되어야 한다는 것.

        두 번째로, policy가 greedy policy 즉, optimal policy로 수렴해야 한다는 것.

        하지만 epsilon greedy는 greedy하게 하나의 action만 선택하지 않는데 이럴 경우는 GLIE하지않다.

        그치만 만약 epsilon이 시간에 따라서 0으로 수렴(1/k)한다면 epsilon greedy 또한 GLIE가 될 수 있다.

        <br />

        - **GLIE Monte-Carlo Contorl**

            ![https://user-images.githubusercontent.com/59254578/71896868-3871e600-3198-11ea-958b-f54558461b78.png](https://user-images.githubusercontent.com/59254578/71896868-3871e600-3198-11ea-958b-f54558461b78.png)

            이렇게 GLIE MC control이 완성된다.

            <br />

    ## **3. On-policy Temporal-Difference Learning**

    이제는 TD를 이용한 policy control 문제를 다룬다. 

    <br />

    - **Sarsa**

        ![https://user-images.githubusercontent.com/59254578/71897378-c3071500-3199-11ea-9b97-3cf2b0466bb6.png](https://user-images.githubusercontent.com/59254578/71897378-c3071500-3199-11ea-9b97-3cf2b0466bb6.png)

        그림과 같이 현재의 **S**tate에서 **A**ction을 취했을 때 얻는 **R**eward, 그리고 다음 **S**tate에서 취할 **A**ction을 고려하는 게 한 step이고 매 step마다 update가 된다. TD(0)에서 state-value function 대신 action-value function을 사용한다.

        <br />

        ![https://user-images.githubusercontent.com/59254578/71897700-a1f2f400-319a-11ea-8e1c-365fbbbd31f4.png](https://user-images.githubusercontent.com/59254578/71897700-a1f2f400-319a-11ea-8e1c-365fbbbd31f4.png)

        TD이기 때문에 매 step마다 update가 되는 것이고, 이때 policy evaluation으로 **Sarsa**를 사용한다.

        <br />

        ![https://user-images.githubusercontent.com/59254578/71897826-044bf480-319b-11ea-90cd-76eac68edaaa.png](https://user-images.githubusercontent.com/59254578/71897826-044bf480-319b-11ea-90cd-76eac68edaaa.png)

        Sarsa의 algorithm을 보면 위와 같다. on-policy TD control algorithm으로서 매 time-step마다 현재의 Q value를 imediate reward와 다음 action의 Q value를 가지고 update한다. policy는 따로 정의되지는 않고 이 Q value를 보고 epsilon-greedy하게 움직이는 것 자체가 policy이다.

        <br />

        ![https://user-images.githubusercontent.com/59254578/71898070-b1bf0800-319b-11ea-8947-0ee2ee7f74e1.png](https://user-images.githubusercontent.com/59254578/71898070-b1bf0800-319b-11ea-8947-0ee2ee7f74e1.png)

        Sarsa는 optimal action-value function에 수렴한다. 단, 조건이 있다.

        위에서 언급한 GLIE를 만족해야 한다.

        step-size(얼마나 update할 지 정하는 매개변수)도 위와 같은 조건을 만족해야 한다.

        - q value가 매우 큰 경우를 위해서 이 값에 도달하기 위해 step size가 어느 정도 커야 한다.
        - 뒤로 갈수록(시간이 지날수록) update 정도가 줄어들어야 한다. 즉, 수렴이 되어야 한다.

        <br />

        - ***n*-Step Sarsa**

            ![https://user-images.githubusercontent.com/59254578/71898921-e0d67900-319d-11ea-98cc-6909a5f37a53.png](https://user-images.githubusercontent.com/59254578/71898921-e0d67900-319d-11ea-98cc-6909a5f37a53.png)

            n-step TD랑 똑같지만 return 대신 Q-function이 사용되었다.

            <br />

        - **Forward View Sarsa(λ)**

            ![https://user-images.githubusercontent.com/59254578/71899039-3c086b80-319e-11ea-80b5-700b36e9f031.png](https://user-images.githubusercontent.com/59254578/71899039-3c086b80-319e-11ea-80b5-700b36e9f031.png)

            forward도 마찬가지다.

            <br />

        - **Backward View Sarsa(λ)**

            ![https://user-images.githubusercontent.com/59254578/71899206-8e498c80-319e-11ea-8470-725b86b42563.png](https://user-images.githubusercontent.com/59254578/71899206-8e498c80-319e-11ea-8470-725b86b42563.png)

            backward도 마찬가지로 return 대신 q-function을 사용.

            알고리즘은 아래와 같다.

            ![img](https://user-images.githubusercontent.com/59254578/71912873-ac70b600-31b9-11ea-84c0-a5965f4296ff.png)

            한 번의 action으로 모든 state와 action을 update 해준다. 연산량이 많지만, 정보 전달 속도가 빠르다. 이것은 아래의 예제에서 확인할 수 있다.

            ![https://user-images.githubusercontent.com/59254578/71899806-d917d400-319f-11ea-8099-0dfa9ed1492e.png](https://user-images.githubusercontent.com/59254578/71899806-d917d400-319f-11ea-8099-0dfa9ed1492e.png)

            one-step Sarsa는 목적지에 도착하면 바로 이전 state만 크게 update 되지만, Sarsa(λ)는 한참 전의 state들도 한 번에 update 되는 것을 알 수 있다. 이것은 eligibility trace 때문이라고 생각한다.

            <br />

    ## **4. Off-Policy Learning**

    ![https://user-images.githubusercontent.com/59254578/71903469-817d6680-31a7-11ea-9def-1f7b1e3b9b23.png](https://user-images.githubusercontent.com/59254578/71903469-817d6680-31a7-11ea-9def-1f7b1e3b9b23.png)

    on-policy는 한계가 있다. 바로 탐험의 문제다. 현재알고있는 정보에 대해 greedy로 policy를 정해버리면 optimal에 가지 못 할 확률이 커지기 때문에 에이전트는 항상 탐험이 필요하다. 따라서 on-policy처럼 움직이는 policy(**behaviour policy**)와 학습하는 policy(**target policy**)가 같은 것이 아니고 이 두개의 policy를 분리시킨 것이 off-policy이다.

    Off-policy는 다음과 같은 장점이 있다.

    - 다른 agent나 사람을 관찰하고 그로부터 학습할 수 있다
    - 이전의 policy들을 재활용하여 학습할 수 있다.
    - 탐험을 계속 하면서도 optimal한 policy를 학습할 수 있다.(Q-learning)
    - 하나의 policy를 따르면서 여러개의 policy를 학습할 수 있다.

    <br />

    <br />

    - **Importance Sampling**

        ![https://user-images.githubusercontent.com/59254578/71908579-a5de4080-31b1-11ea-8a1b-61a9e4af6e72.png](https://user-images.githubusercontent.com/59254578/71908579-a5de4080-31b1-11ea-8a1b-61a9e4af6e72.png)

        어떤 값을 추정하는데 가장 기본적인 방법은 그냥 random하게 찍어보는 것이다. 하지만 너무 광범위하게 탐색하기도 하고 어떠한 중요한 부분을 알아서 그 위주로 탐색을 하면 더 빠르고 효율적으로 값을 추정할 수 있고 그러한 아이디어가 바로 "Importance Sampling"이다.

        P와 q라는 다른 distribution이 있을 때 q라는 distribution에서 실재로 진행을 함에도 불구하고 p로 추정하는 것처럼 할 수 있다는 것이다. 강화학습에서도 policy가 다르면 state의 distribution은 달라지게 되어 있다. 따라서 다른 distribution을 통해 추정할 수 있다는 개념을 그대로 가져와서 다른 policy를 통해서 얻어진 sample을 이용하여 Q 값을 추정할 수 있다는 것이다. 일종의 trick이라고 할 수 있을 것 같다.

        f(X)라는 함수를 value function이라고 생각하고 강화학습에서는 이 value function = expected future reward를 계속 추정해나가는데 P(X)라는 현재 policy로 형성된 distribution으로부터 학습을 하고 있었습니다. 하지만 다른 Q라는 distribution을 따르면서도 똑같이 학습할 수 있는데 단, 위와 같이 간단히 식을 변형시켜주면 된다.

    <br />

    - **Importance Sampling for Off-Policy Monte-Carlo**

        ![https://user-images.githubusercontent.com/59254578/71909079-94496880-31b2-11ea-811e-095670c2ea2d.png](https://user-images.githubusercontent.com/59254578/71909079-94496880-31b2-11ea-811e-095670c2ea2d.png)

        각 스텝에 reward를 받게 된 것은 μ-policy를 따라서 얻었던 것이므로 매 step마다 π/μ를 해줘야 한다.

        episode가 끝날 때까지 곱해주기 때문에 값이 매우 작아지거나 매우 커지게 되는 문제가 발생하기 때문에 Monte-Carlo에 off-policy를 적용시키는 것은 좋지 않다.

        <br />

    - **Importance Sampling for Off-Policy TD**

        ![https://user-images.githubusercontent.com/59254578/71909417-5a2c9680-31b3-11ea-8df0-72f40d9afdf3.png](https://user-images.githubusercontent.com/59254578/71909417-5a2c9680-31b3-11ea-8df0-72f40d9afdf3.png)

        Off-Police TD에서는 MC 때와는 달리 Importance Sampling을 1-step만 진행하면 된다.

        MC때와 비교하면 Variance가 낮아지기는 했지만 여전히 원래 TD에 비하면 Importance sampling때문에 높은 variance를 가지고 있다는 문제점이 있다. 그래서 나오게 된 것이 Q Learning이다.

        <br />

    - **Q-Learning**

        ![https://user-images.githubusercontent.com/59254578/71909997-6ebd5e80-31b4-11ea-8e69-65d82c793e6a.png](https://user-images.githubusercontent.com/59254578/71909997-6ebd5e80-31b4-11ea-8e69-65d82c793e6a.png)

        현재 state S에서 action을 선택하는 것은 behaviour policy를 따라서 선택을 한다. TD에서 udpate할 때는 one-step을 bootstrap하는데 이 때 다음 state의 action을 선택하는 데는 behaviour policy와는 다른 policy(alternative policy)를 사용하면 Importance Sampling이 필요하지 않다. 이전의 Off-Policy에서는 Value function을 사용했었는데 여기서는 action-value function을 사용함으로써 다음 action까지 선택을 해야하는데 그 때 다른 policy를 사용한다는 것이다.

        <br />

    - **Off-Policy Control with Q-Learning**

        ![https://user-images.githubusercontent.com/59254578/71910757-dc1dbf00-31b5-11ea-93c9-f7985bac12b7.png](https://user-images.githubusercontent.com/59254578/71910757-dc1dbf00-31b5-11ea-93c9-f7985bac12b7.png)

        이 Q-learning 알고리즘이 가장 유명하고 많이 쓰인다.

        greedy한 policy로 학습을 진행하면 수렴을 빨리 하는데 충분히 탐험을 하지 않았기 때문에 local에 빠지기 쉽다. 그래서 탐험을 위해서 ε-greedy policy를 사용하면 탐험을 계속하는데 이렇게 학습하면 수렴속도가 느려져서 학습속도가 느려지게 된다. 그래서 target policy는 greedy로 빠르게 최적으로 향하게 한다.

        ![https://user-images.githubusercontent.com/59254578/71911546-3b300380-31b7-11ea-861d-8aeeb894f55a.png](https://user-images.githubusercontent.com/59254578/71911546-3b300380-31b7-11ea-861d-8aeeb894f55a.png)

        - Behaviour policy로는 g-greedy w.r.t. Q(s,a)

        - Target policy(alternative policy)로는 greedy w.r.t. Q(s,a)

        위와 같이 최적의 action-value function으로 수렴하게 된다.

        <br />

        - **Algorithm**

            ![https://user-images.githubusercontent.com/59254578/71911608-5ef34980-31b7-11ea-845d-e27eada48d54.png](https://user-images.githubusercontent.com/59254578/71911608-5ef34980-31b7-11ea-845d-e27eada48d54.png)

            <br />

    - **Relationship Between DP and TD**

        ![https://user-images.githubusercontent.com/59254578/71911813-c3160d80-31b7-11ea-954b-d31074b5dc1b.png](https://user-images.githubusercontent.com/59254578/71911813-c3160d80-31b7-11ea-954b-d31074b5dc1b.png)

<br />

<br />

<br />

<br />

**※ 참고문헌 및 자료**

- [[David Silver's Lecture]](http://www0.cs.ucl.ac.uk/staff/d.silver/web/Teaching.html)
- [[팡요랩]](https://www.youtube.com/channel/UCwkGvF7xKz2E0Lv-fZ9wv2g)
- [[Fundamental of Reinforcement Learning](https://dnddnjs.gitbook.io/rl/)]