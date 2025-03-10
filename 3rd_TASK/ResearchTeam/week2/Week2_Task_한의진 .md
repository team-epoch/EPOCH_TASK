# Week2_Task_한의진

---

<aside>

<aside>

## **📘 Title**

> ℹ️ *APA. 인용 방식 권고*
> 
</aside>

- Hu, M., Cao, Y., Li, A., Li, Z., Liu, C., Li, T., Chen, M., & Liu, Y. (2024). FedMut: Generalized Federated Learning via Stochastic Mutation. Proceedings of the AAAI Conference on Artificial Intelligence, 38(11), 12528-12537.

</aside>

---

<aside>

<aside>

## **📖 Abstract**

> ℹ️ *본인의 방식으로 재해석 해주세요. 그대로 가져오는 것은 금합니다.*
> 
</aside>

- 기존 Federated Learning의 문제점
    - Federated Learning이란
        - 데이터의 프라이버시를 보호하면서 여러 개의 클라이언트에서 모델 학습
    - 데이터 불균형 문제
        - 클라이언트마다 데이터 분포가 다름
    - Sharp Solution
        - 특정 클라이언트에 overfit되어 local minima로 빠지는 문제점
- FedMut의 Idea
    - Model Mutation 적용(모델을 약간 변형해서 배포)
    - Flat Minimum으로 수렴하도록 함
        - 여러 변형 모델 학습 후 글로벌 모델 업데이트

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

- 기존 FedAvg 방식의 Overfit 및 비효율성 해결
    - 클라이언트가 업데이트한 로컬 모델을 중앙 서버로 전송해 **평균(Federated Averaging, FedAvg)하여 글로벌 모델을 업데이트**함.
    - 개인 데이터가 중앙 서버에 직접 공유되지 않아 **프라이버시 보호 가능**.
    - 보안 문제를 해결하기 위해 **Secure Aggregation 사용**
    - 클라이언트마다 **최적화 방향이 달라**서 **gradient divergence(기울기 분산 문제)** 발생.
        - FedAvg는 단순한 평균 집계를 사용하기 때문에, 서로 Conflict 발생 시 해결 불가
    - 이로 인해 모델이 **Sharp Minimum 발생**

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

- **Global Variable Methods** (Karimireddy et al. 2020, Li et al. 2020)
    - 최적화 문제의 해결을 위해 Global Parameter 사용
- **Client Grouping** (Fraboni et al. 2021, Chen et al. 2020)
    - 유사한 데이터 분포 Group
- **Knowledge Distillation** (Lin et al. 2020, Zhu et al. 2021)
    - 지식 증류 기법 → Small Sub-model로 압축

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

- Hochreiter & Schmidhuber, 1997; Stutz et al., 2021; Foret et al., 2020)
    - **Flat Minimum에서 훈련된 모델이 Generalization 성능이 더 높음**.
- **Mutation**
    - Global Model을 Mutation(변형)하여 생성
    - 글로벌 모델의 Neighborhood로 유사한 모델
- Local Training
    - Local Training을 통해 Local 안에서의 최적으로 Update
- **Model Aggregation**
    - 학습된 변이 모델을 Aggregation하여 Update
    - **Flat Minima로 수렴하도록 업데이트**
- FedMut의 장점
    - 추가 데이터 없이도 성능 개선 가능
    - Secure Aggregation과 호환 가능

![Screenshot 2025-03-10 at 20.05.34.png](Week2_Task_%E1%84%92%E1%85%A1%E1%86%AB%E1%84%8B%E1%85%B4%E1%84%8C%E1%85%B5%E1%86%AB%201b2138dcada680d3b3b9e9389c8b0331/Screenshot_2025-03-10_at_20.05.34.png)

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

- CIFAR-10, CIFAR-100 Dataset
- CNN, ResNet-18, VGG-16
- FedAvg, FedProx(Prox Term 추가), FedGen, CluSamp와 비교

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

![Screenshot 2025-03-10 at 20.19.57.png](Week2_Task_%E1%84%92%E1%85%A1%E1%86%AB%E1%84%8B%E1%85%B4%E1%84%8C%E1%85%B5%E1%86%AB%201b2138dcada680d3b3b9e9389c8b0331/Screenshot_2025-03-10_at_20.19.57.png)

- **추론 정확도(Accuracy) 및 수렴 속도(Convergence Rate) 향상**
- FedMut이 **전통적인 FedAvg보다 우수한 성능**을 보임
- 일반화 성능 향상

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

- **CluSamp** (Fraboni et al. 2021)의 개념에 대해 공부(처음 들어본 개념)

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

- 이 논문의 경우 기존 Federated Learning의 Parameter 학습의 클라이언트간 불일치 문제가 기존에 제기된 부분이 많았고, 이 부분의 문제를 해결하기 위해 시행된 실험 및 작성된 논문
- Gradient가 사라지거나 관련 문제가 발생하지 않는지 의문
- 향후 이 방법보다 개선된 방법이 있는지 조사 예정

</aside>

---

<aside>

<aside>

## **🤔 Next Task**

> ℹ️ *차주에 리뷰 예정인 논문 기재(APA. 인용 방식 권고)*
> 
</aside>

- Hao Guan, Pew-Thian Yap, Andrea Bozoki, Mingxia Liu, Federated learning for medical image analysis: A survey, Pattern Recognition, Volume 151, 2024, 110424, ISSN 0031-3203, [https://doi.org/10.1016/j.patcog.2024.110424](https://doi.org/10.1016/j.patcog.2024.110424).

</aside>

---