# Week2_Task_우림

---

<aside>

<aside>

## **📘 Title**

> ℹ️ *APA. 인용 방식 권고*
> 
</aside>

- Hasselt, H. V., Guez, A., & Silver, D. (2015). Deep reinforcement learning with double Q-learning. *Proceedings of the 2015 AAAI Conference on Artificial Intelligence*, 2094-2100. [https://arxiv.org/abs/1509.06461](https://arxiv.org/abs/1509.06461)
</aside>

---

<aside>

<aside>

## **📖 Abstract**

> ℹ️ *본인의 방식으로 재해석 해주세요. 그대로 가져오는 것은 금합니다.*
> 
</aside>

- DDQN(Deep Double Q-learning): DQN(Double Q-learning) 알고리즘 + 심층 강화학습 결합한 알고리즘을 제안
    - Q-learning: 인기 있는 강화학습 알고리즘이지만 action values를 overestimate 하는 경향 존재
    - DQN(Double Q-learning): 마찬가지로 학습 과정에서 overestimate 할 경향이 크고 제한된 환경에서만 사용 가능
- ❓ Overestimation 발생 이유: target Q-value 계산시 max 값을 사용하기 때문
- 이전 연구에서의 문제점(overestimation; 과대평가)을 해결하기 위해 등장한 알고리즘. 완전히 새로운 방법론이라기보다는 Q-learning, DQN을 조금씩 변형하여 과대평가 문제를 줄이고, Atari 2600 게임 환경에서의 실험을 통해 DQN보다 좋아진 성능을 보임.
</aside>

---

<aside>

<aside>

## **💡 Introduction**

> **📍 논문 선택의 동기**
> 
> 
> **✅ 논문에서 다루고 있는 주요 연구 문제나 질문**
> 
</aside>

**📍 논문 선택의 동기**

- 강화학습 발전 과정에 따라 논문을 리뷰하고 싶었으나… DDQN 전에 DQN이 있었다 (week3 리뷰 예정)
- 인기 있는 Q-learning 알고리즘 이후 발전, 변형된 알고리즘을 공부하고자 선택

**✅ 논문에서 다루고 있는 주요 연구 문제나 질문**

- Q-learning의 overestimation(과대평가) 이슈를 어떻게 해결?
- 해당 논문에서 제시하는 DDQN은 Atari 2600에 대해 기존 DQN 대비 성능이 향상되는지 보이고자 함
</aside>

---

<aside>

<aside>

## **📚 Background**

> ℹ️ *논문의 주제와 관련된 기존 연구들 및 배경 지식을 소개*
> 
> 
> **📍 Related Work 1**
> 
> **📍 Related Work 2**
> 
</aside>

**📍 Q-learning**

- 주어진 policy $\pi$에 대해 state s에서 action a의 true value
- Optimal policy: 각 state에서 가장 높은 Q-value 선택
    
    ![image.png](Week2_Task_%E1%84%8B%E1%85%AE%E1%84%85%E1%85%B5%E1%86%B7%201b204fc3e65180e59512fe7ef28fc26a/image.png)
    
- 단점: 모든 states에서 모든 action values를 학습하기에는 연산량이 너무 많음
    
    → 대신 Q-value를 $\theta$로 파라미터화하여 학습!
    
    ![image.png](Week2_Task_%E1%84%8B%E1%85%AE%E1%84%85%E1%85%B5%E1%86%B7%201b204fc3e65180e59512fe7ef28fc26a/image%201.png)
    
- target을 다음과 같의 정의:
    
    ![image.png](Week2_Task_%E1%84%8B%E1%85%AE%E1%84%85%E1%85%B5%E1%86%B7%201b204fc3e65180e59512fe7ef28fc26a/2fe0e357-396f-43d9-a466-8e123db46462.png)
    

**📍 Deep Q Network**

- Q-learning + multi-layered neural network
- target network를 사용하여 $\theta$ 대신 $\theta^-$ 사용
    
    ![image.png](Week2_Task_%E1%84%8B%E1%85%AE%E1%84%85%E1%85%B5%E1%86%B7%201b204fc3e65180e59512fe7ef28fc26a/image%202.png)
    

**📍 Double Q-learning**

- Q-learning, DQN에서의 max operator는 action을 선택, 평가하는 데 같은 값을 사용하며 이는 overestimate 야기 가능 → 두 개의 Q-함수 사용
- $\theta$, $\theta'$ 둘 중 하나를 랜덤하게 선택하여 업데이트
    
    ![image.png](Week2_Task_%E1%84%8B%E1%85%AE%E1%84%85%E1%85%B5%E1%86%B7%201b204fc3e65180e59512fe7ef28fc26a/image%203.png)
    
- 하나의 Q-함수 사용: action 선택
- 다른 Q-함수 사용: 해당 action value를 평가
</aside>

---

<aside>

<aside>

## **🔍 Methods**

> **✅ 사용된 연구 방법**
> 
> 
> **✅ 실험 설계**
> 
> **📍 모델 비교** 
> 
</aside>

- Double DQN: Double Q-learning + DQN 이라고 보면 될 것 같음
    - 업데이트는 DQN과 동일한 방식이나 target을 다음으로 대체:
        
        ![image.png](Week2_Task_%E1%84%8B%E1%85%AE%E1%84%85%E1%85%B5%E1%86%B7%201b204fc3e65180e59512fe7ef28fc26a/image%204.png)
        
    - 두 번째 network로 target network를 사용하는 방식 사용
    - DQN 형태를 유지하면서 Double Q-learning의 장점을 최대한 활용
</aside>

---

<aside>

<aside>

## **🔍 Experiments**

> **✅ 데이터셋**
> 
> 
> **✅ Models**
> 
> **✅ Evaluation Metrics**
> 
> **✅ Implementation Details**
> 
</aside>

- DQN의 overestimation 분석 + Double DQN이 value accuracy와 policy quality 측면에서 DQN보다 좋은 성능을 보여 줌
- Datasets: Atari 2600 games
- Experimental setting, network architecture: DQN paper*에서의 아웃라인을 따름
- 네트워크는 마지막 네 프레임을 인풋으로 받아서 각 액션의 액션 값을 출력
- 각 게임에서 200만 프레임(약 1주일 동안) 단일 GPU로 네트워크 훈련
- 평가 지표: value estimate
- Results on overoptimism
    - 6개의 Atari 게임에 대한 실험 결과
    - Learning curve of DQN: true value보다 높은 위치
    - Learning curve of DDQN: true value와 훨씬 가까움
        - DDQN이 더 정확한 value estimate, 더 나은 policy를 만들어냄

* Mnih, V., Kavukcuoglu, K., Silver, D., Rusu, A. A., Veness, J., Bellemare, M. G., Graves, A., Riedmiller, M., Fidjeland, A. K., Ostrovski, G., Petersen, S., Beattie, C., Sadik, A., Antonoglou, I., King, H., Kumaran, D., Wierstra, D., Legg, S., & Hassabis, D. (2015). Human-level control through deep reinforcement learning. *Nature, 518*(7540), 529-533. https://doi.org/10.1038/nature14236

</aside>

---

<aside>

<aside>

## **📖 Conclusion**

> **✅ Limitation**
> 
> 
> **✅ Contribution**
> 
</aside>

- Q-learning이 large-scale 문제에서도 지나치게 낙관적일 수 있는 이유를 보여 줌
- Atari 게임에서의 value 과대평가 문제가 이전에 인정된 것보다 실제로 더 흔하고 심각하다는 것을 보여 줌
- DDQN이 과대평가 문제를 성공적으로 줄이고 안정적이고 신뢰할 수 있는 학습이 가능하다는 것을 보여 줌
- 추가적인 네트워크나 모수를 필요로 하지 않고 기존 아키텍처+심층신경망을 사용하는 것으로 DDQN이라는 특정 방법론 제안
- DDQN이 더 나은 policy를 찾는다는 것을 보여 줌
</aside>

---

<aside>

<aside>

## **📚 하향식 접근**

> ℹ️ *공통 논문 학습 중 몰랐던 개념, 용어에 대한 논문을 추가로 찾아봅시다!*
> 
> 
> ℹ️ *한 도메인의 모르는 내용에 대해 **하향식**으로 접근하다보면 관련 논문들이 점차 쉽게 읽히게 됩니다.* 
> 
> **📍 e.g.** `transformer`논문을 보다가 `sequence to sequence`가 뭔지 몰라 찾아서 공부
> 
> **📍 e.g.**`sequence to sequence` 논문을 보다가 `LSTM`이 뭔지 몰라 찾아서 공부
> 
</aside>

- 강화학습에서 사용되는 용어 및 전반적인 개념 익힘
</aside>

---

<aside>

<aside>

## **🤔 Question**

> ℹ️ *본인이 수행한 학습에 대해 스스로 질문하고 답해보세요.*
> 
> 
> **📍 이 논문이 등장하게 된 이유 + 이 논문이 관련 Task에 기여한 내용**
> 
> **📍 배울 수 있었던 내용과 추가로 궁금한 점**
> 
> **📍 Git-Hub에 공개된 코드를 보고 이해한 바 `(Opt.)`**
> 
</aside>

- 본 연구는 이전 연구의 한계점을 보완하고자 등장하였으며 이미 개발된 방법론(Q-learning 및 DQN)을 활용/변형하여 추가적인 네트워크나 모수를 요구하지 않으면서도 과대평가 문제를 해결할 수 있는 방법론을 제시하였다.
- 강화학습의 초기 발전 과정에 따른 방법론을 간단하게나마 배울 수 있었음.
- 사실 강화학습에 대해 깊이 공부해 본 적이 없고 백그라운드 지식이 없어서 이번 논문을 읽는 데 많이 애먹었음…… *~~(background에서 언급된 Q-learning, DQN 모두 첨 봤어요 ㅎㅎ…)~~* 레이블이 있는 데이터에서 패턴을 학습하는 지도학습과는 달리 강화학습은 **정답이 없고**, 보상을 기반으로 최적의 행동을 학습하는 방식이라는 점에서 같은 머신러닝 분야임에도 불구하고 결이 많이 다르다고 느꼈다. 그러나 앞서 언급한 논문들을 먼저 읽어 보고 본 논문을 다시 리딩한다면 처음보다 이해하기 수월할 것 같다. 빠른 시일 내에 모든 공통 논문을 읽어야겠다고 다짐하는 계기가 되기도…. 그래도 내가 잘 알지 못했던 새로운 분야를 공부하고 알아간다는 건 즐겁다. ❤️
</aside>

---

<aside>

<aside>

## **🤔 Next Task**

> ℹ️ *차주에 리뷰 예정인 논문 기재(APA. 인용 방식 권고)*
> 
</aside>

- Mnih, V., Kavukcuoglu, K., Silver, D., Rusu, A. A., Veness, J., Bellemare, M. G., Graves, A., Riedmiller, M., Fidjeland, A. K., Ostrovski, G., Petersen, S., Beattie, C., Sadik, A., Antonoglou, I., King, H., Kumaran, D., Wierstra, D., Legg, S., & Hassabis, D. (2015). Human-level control through deep reinforcement learning. *Nature, 518*(7540), 529-533. https://doi.org/10.1038/nature14236

</aside>

---
