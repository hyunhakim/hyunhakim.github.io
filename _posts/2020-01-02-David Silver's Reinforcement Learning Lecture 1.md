# David Silver's Reinforcement Learning Lecture 1

----------------------------------------------------

## **1. Characteristics of Reinforcement Learning**

![image](https://user-images.githubusercontent.com/59254578/71666650-29102880-2da5-11ea-9fc3-35e4b6f4ca5c.png)

기존의 머신러닝 방법론들과는 달리 supervisor가 없고 reward를 토대로 목표를 이루어 나간다.

이전의 action들이 독립적이지 않고 서로 영향을 주고 받는다.

## **2. Rewards**

- reward는 스칼라 신호로,  agent가 t 단계에서 얼마나 잘하고 있는지를 나타내는 지표
- agent의 임무는 축적된 reward를 최대화하는 것

## **3. Sequential Decision Making**

- Goal : total future reward를 최대화 하기 위한 action을 취하는 것

## **4. Agent and Environment**

![image](https://user-images.githubusercontent.com/59254578/71667339-03385300-2da8-11ea-9ddf-c356526f1a75.png)

여기서는 뇌가 agent, 지구가 environment다. 서로 상호작용을 하면서 observation, reward, action을 주고 받는다.

## **5. History and State**

- History : observations, actions, rewards의 모든 sequence를 가지고 있다.따라서, history를 바탕으로 agent는 action을 정하고, environment는 observations/rewards를 정한다.

- State : 다음에 무슨 일이 일어날지를 정하기 위해 사용되는 정보로, state는 history의 함수이다.

  - Markov state

    ![image](https://user-images.githubusercontent.com/59254578/71668170-2d3f4480-2dab-11ea-95d2-3b71c659792c.png)

    현재 정보가 이전 정보들을 다 포함해서 과거의 정보들은 미래와는 무관할 경우 markov하다고 말한다.

    environment state는 markov하다.

  - Fully Observable Environments

    agnet가 environment를 직접적으로 관측하는 상황.

    Full observability: agent state = environment state = information state

    이것을 Markov Decision Process(MDP)라고 한다.

  - Partially Observavble Environments

    agnet가 environment를 간접적으로 관측하는 상황

    (ex. 로봇이 카메라 센서 기반으로 이동 시 현재 보는 것만이 state이고 정확한 위치 정보는 모르는 상황)

    이것을 Partially Observable Markov Decision Process(POMDP)라고 한다.

## **6. Policy**

![image](https://user-images.githubusercontent.com/59254578/71668900-1b12d580-2dae-11ea-8d5a-32c8a5ae1311.png)

state를 기반으로 어떤 action을 할 지 정해주는 것을 policy라고 한다. 이때, 결정적인 방법과 확률적인 방법이 있다.

## **7. Value Function**

![image](https://user-images.githubusercontent.com/59254578/71668953-62996180-2dae-11ea-8aa1-65c4efe0c6e2.png)

policy를 기반으로 미래의 reward에 대한 기대값을 얻어서 state를 평가한다.

## **8. Model**

![image-20200102222846054](C:\Users\gusgk\AppData\Roaming\Typora\typora-user-images\image-20200102222846054.png)

environment가 해야하는 일을 모델링한다고 보면 된다.



## **9. Learning and Planning**

![image](https://user-images.githubusercontent.com/59254578/71669902-94f88e00-2db1-11ea-845a-7af94ff74cfb.png)

Learning과 planning의 차이는 environment를 아냐 모르냐의 차이이다. 즉, planning은 transition model과 reward를 예측가능하다.



## **10. Exploration and Exploitation**

Exploration : environment에 대한 정보들 더 얻기 위해 탐험하는 것.

Exploitation : 지금까지 얻어진 정보를 토대로 reward를 최대화하는 것.



## **11. Prediction and Control**

Prediction : evaluate the future (value function을 최적화)

Control : optimize the future (policy를 최적화)





**※ 참고문헌 및 자료**

- [David Silver Lecture]: http://www0.cs.ucl.ac.uk/staff/d.silver/web/Teaching.html

- [팡요랩]: https://www.youtube.com/channel/UCwkGvF7xKz2E0Lv-fZ9wv2g

  

