# Week3_Task_의진.md

---

<aside>

<aside>

## **📘 Title**

> ℹ️ *APA. 인용 방식 권고*
> 
</aside>

- Hao Guan, Pew-Thian Yap, Andrea Bozoki, Mingxia Liu, Federated learning for medical image analysis: A survey, Pattern Recognition, Volume 151, 2024, 110424, ISSN 0031-3203, [https://doi.org/10.1016/j.patcog.2024.110424](https://doi.org/10.1016/j.patcog.2024.110424).

</aside>

---

<aside>

<aside>

## **📖 Abstract**

> ℹ️ *본인의 방식으로 재해석 해주세요. 그대로 가져오는 것은 금합니다.*
> 
</aside>

- 의료 데이터셋에서 각 의료기관마다 데이터셋이 적은 단점
- 통합 학습이 최선
- 그러나 개인정보보호 문제가 발생(개인정보법 및 프라이버시로 직접 공유 불가)
- 연합 학습 방법별로 분류 및 현재 연구 동향

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

- Federated Learning(의료 인공지능에서의)에 대한 배경 지식을 스터디원과 공유
- Federated Learning의 필요성
    - 의료 영상 데이터는 여러 병원, 연구기관에서 수집되지만, **개인정보 보호법**에 따라 데이터 공유가 어려움
    - 데이터를 한 곳에 모아 학습하는 것이 **현실적으로 불가능**
    - 연합 학습(FL)은 **데이터를 공유하지 않고 협력 학습이 가능**한 기법으로, 모델을 개별 기관에서 학습한 후 **모델 가중치만 공유하여 Global Model을 구축**하는 방식임.
- 이 논문의 의의
    
    **더 광범위한 논문 수집 기간**
    
- 기존 서베이는 **2022년 이전 논문**만 다뤘지만,
- 본 논문은 **2017년 1월~2023년 10월까지** 연구를 포함하여 최신 트렌드를 반영.
    
    **연합 학습 소프트웨어 플랫폼 소개**
    
- 본 논문에서는 **FL 연구에 사용되는 플랫폼과 벤치마크 데이터셋**을 정리하여 실질적인 연구에 도움을 줌.
    
     **실험적 평가 포함**
    
    **새로운 연구 문제와 미래 방향 제시**
    
- 본 논문은 최신 연구를 반영하여,
    - **일반화 성능 문제** (학습에 포함되지 않은 기관에 대한 성능)
    - **의료 영상이 아닌 의료 비디오 데이터에 대한 연합 학습** 등의 새로운 연구 문제를 제시

- 연합 학습 기법을 크게 3가지로 분류
    1. **클라이언트 측 학습 기법** (Client-end Learning Methods)
    2. **서버 측 학습 기법** (Server-end Learning Methods)
    3. **서버-클라이언트 간 통신 기법** (Server-Client Communication Methods)

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

### **연합 학습의 일반적인 과정**

FL은 **클라이언트-서버(client-server)**

- **1) 클라이언트 선택 및 초기화**
    - 초기 모델 설정
- **2) 로컬 학습**:
    - 각 기관에서 개별적으로 로컬 데이터셋을 활용해 모델(U-Net, ViT 등)을 학습함.
- **3) 모델 업데이트 전송**:
    - 기관들은 데이터가 아닌 모델 업데이트
- **4) 모델 집계(Aggregation)**:
    - 서버는 모든 클라이언트의 업데이트를 수집하고, **평균화(FedAvg) 또는 가중 집계** 방법을 사용하여 모델을 갱신함.
- **5) 모델 전파(Broadcasting)**:
    - 업데이트된 모델
- **6) 반복 및 수렴(Iteration & Convergence)**:
    - 여러 반복을 거쳐 모델이 점점 더 정교해짐
- **7) 모델 배포(Deployment)**:
    - 최종 모델이 실제 환경에서 사용됨.
    - 병원 시스템과의 통합, 사용자 채택 등 실질적인 문제 해결 필요.

### **연합 학습의 유형**

### **수평 연합 학습 (Horizontal Federated Learning, HFL)**

- **동일한 종류의 데이터**를 보유한 여러 기관이 참여.
- 예) 여러 병원이 환자의 **흉부 X-ray 데이터**를 학습하는 경우.
    - 서로 다른 환자의 X-ray 데이터지만, 데이터의 특징(폐 크기, 형태 등)은 비슷함.
    - 데이터를 공유하지 않고도 FL을 활용해 협력 가능.

### **수직 연합 학습 (Vertical Federated Learning, VFL)**

- **동일한 환자에 대해 서로 다른 종류의 데이터**를 보유한 기관들이 협력.
- 예) 병원에서는 **영상 데이터**, 연구소에서는 **유전체(Genomic) 데이터**를 보유.
    - 동일 환자의 다양한 데이터 유형을 결합해 보다 정밀한 의료 분석 가능.

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

- 

### **클라이언트 간 도메인 편차 문제(Client End: Domain Shift Among Clients)**

### **(1) 도메인별 학습(Domain-Specific Learning)**

- 각 클라이언트의 데이터 특성을 반영해 **글로벌 모델을 개별적으로 미세 조정**하는 방법
- 예시 연구:
    - Feng et al. (2021): FL을 위한 인코더-디코더 구조를 설계하여 **공유된 인코더(서버) + 클라이언트별 디코더**를 활용
    - Chakravarty et al. (2021): **CNN + GNN** 조합을 사용하여 클라이언트별 데이터를 반영하는 FL 모델 구축

### **(2) 도메인 적응(Domain Adaptation)**

- 클라이언트 간 데이터 분포 차이를 줄이기 위해 **도메인 적응 기법** 사용
- 예시 연구:
    - Li et al. (2020): **fMRI 데이터에 노이즈 추가 후 도메인 분류기를 학습**하여 도메인 차이를 줄임
    - Dinsdale et al. (2020): **도메인 불변 특징을 학습하기 위해 Gaussian 분포를 가정**

### **(3) 의료 영상 조화(Image Harmonization)**

- GAN을 활용하여 의료 영상을 균일한 스타일로 변환 (예: **색상 보정**)
- 예시 연구:
    - Ke et al. (2021): GAN 기반 FL 모델을 사용해 병리 영상의 스타일 차이를 최소화

---

### **데이터 및 레이블 부족 문제(Client End: Limited Data and Labels)**

### **(1) 대조 학습(Contrastive Learning)**

- 자가 지도 학습(self-supervised learning) 기법을 활용하여 **유사한 데이터 간 특징을 학습**
- 예시 연구:
    - U-Net의 인코더를 대조 학습으로 사전 훈련(pretrain)한 후, **소량의 레이블 데이터로 미세 조정**

### **(2) 다중 작업 학습(Multi-Task Learning)**

- 여러 관련된 과제를 동시에 학습하는 방법
- 예시 연구:
    - Huang et al. (2021): ADHD, 자폐 스펙트럼 장애(ASD), 조현병(SCZ) 데이터를 연합 학습으로 통합

### **(3) 약한 지도 학습(Weakly-Supervised Learning)**

- 레이블이 부족한 데이터를 활용하는 방법
- 예시 연구:
    - Yang et al. (2021): **FL 환경에서 반지도 학습(Semi-Supervised Learning)**을 활용하여 CT 영상을 분할

### **(4) 지식 증류(Knowledge Distillation)**

- 큰 모델(teacher)의 출력을 작은 모델(student)에 전달하는 방식으로 학습
- 예시 연구:
    - Kumar et al. (2021): COVID-19 검출을 위해 기존 학습된 네트워크를 **teacher**, 클라이언트 모델을 **student**로 설정하여 지식 전달

### **(5) 데이터 합성(Data Synthesis)**

- GAN과 같은 생성 모델을 활용해 **가상의 의료 영상을 생성하여 데이터 부족 문제 해결**
- 예시 연구:
    - Zhu et al. (2021): FL에서 **가상 샘플 합성(Virtual Sample Synthesis)**을 적용하여 데이터 증강

### **서버 엔드 학습 (Server-End Learning)**

### **1. 서버 엔드: 가중치 집계 (Weight Aggregation)**

- **문제점**: 연합 학습(Federated Learning, FL)에서 클라이언트 모델의 가중치를 효과적으로 집계하는 방법이 필요함. 성능 일관성을 유지하고 각 통신 후 모델 성능 저하를 방지하는 것이 중요함.
- **연구 사례**:
    - **Progressive Fourier Aggregation (Chen et al.)**: 신경망의 성능에 중요한 저주파(low-frequency) 성분만 집계하여 학습된 지식을 공유하는 방법.
    - **Loss-based Aggregation (Li et al.)**: 클라이언트의 학습 손실(loss)을 기반으로 가중치를 부여하여 성능이 낮은 클라이언트의 기여도를 줄이는 방법.

### **2. 서버 엔드: 클라이언트 간 도메인 차이 (Domain Shift Among Clients)**

- **문제점**: 각 클라이언트의 데이터 분포 차이로 인해 글로벌 모델이 수렴하지 않거나 일부 클라이언트에서 성능이 떨어지는 문제가 발생.
- **연구 사례**:
    - **Bias Reduction (Hosseini et al.)**: 의료 기관 간 이미지 이질성 문제를 해결하기 위해 리소스 할당 기법을 활용하여 공정한 성능을 보장하는 최적화 목표를 제안.
    - **Guided-gradient Aggregation (Fan et al.)**: 집계된 가중치 중 양수 값만 사용하여 모델 업데이트를 수행함으로써 전역 최적화 방향을 보장.
    - **Federated Learning with Shared Label Distribution (Luo et al.)**: 전체 네트워크의 라벨 분포를 고려하여 각 클라이언트의 손실 가중치를 조정하는 방식.

### **3. 서버 엔드: 클라이언트 오류 및 이상 감지 (Client Corruption/Anomaly Detection)**

- **문제점**: 연합 학습 환경에서 일부 클라이언트가 잘못된 데이터(오류 라벨, 낮은 품질의 스캔 이미지, 악의적인 공격 등)로 인해 전체 모델을 오염시킬 위험이 있음.
- **연구 사례**:
    - **Distance-based Outlier Suppression (Alkhunaizi et al.)**: 클라이언트의 이상치 점수를 계산하여 신뢰도가 낮은 클라이언트의 가중치를 줄이는 방법.

---

### **클라이언트-서버 통신 (Client-Server Communication)**

### **1. 데이터 유출 및 공격 (Data Leakage and Attack)**

- **문제점**: 의료 이미지 데이터를 클라이언트 간 공유하지 않더라도, 모델의 그래디언트(gradient) 정보를 통해 원본 이미지가 복원될 수 있는 위험이 있음.
- **연구 사례**:
    - **Partial Weights Sharing (Yang et al.)**: 모델 전체가 아닌 일부 계층만 공유하여 개인 정보 보호 강화.
    - **Differential Privacy**: 그래디언트에 가우시안 노이즈를 추가하여 데이터 유출을 방지하는 방법.
    - **Gradient Attack & Defense**: 그래디언트 정보를 활용해 원본 데이터를 복원하는 공격 연구 및 이에 대한 방어 방법 연구.

### **2. 통신 효율성 (Communication Efficiency)**

- **문제점**: 클라이언트-서버 간 통신이 비효율적일 경우 학습 속도가 저하되고 전체 학습 과정이 지연됨.
- **연구 사례**:
    - **Dynamic Fusion-based FL (Zhang et al.)**: 성능이 낮거나 응답이 느린 클라이언트를 제외하여 학습 속도를 개선하는 방식.

[](https://www.notion.so)

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

- 

![Screenshot 2025-03-17 at 21.58.08.png](Week3_Task_%E1%84%8B%E1%85%B4%E1%84%8C%E1%85%B5%E1%86%AB%20md%201b9138dcada680b68bb4fb84437a3e89/Screenshot_2025-03-17_at_21.58.08.png)

### **Results & Observations**

(Table 2 provides detailed metrics.)

1. **Best Performance**:
    - The **Mix strategy** achieved the highest performance across all metrics because it **leverages the largest dataset**.
2. **Worst Performance**:
    - The **Cross strategy** performed the worst due to **domain shift** between ADNI-1 and ADNI-2 (different scanning parameters).
3. **Federated Learning (FL) Shows Strong Performance**:
    - **FL approaches (FedAvg, FedSGD, FedProx)** outperform the **Cross** and **Single** strategies by leveraging more distributed data without data sharing.
4. **FedAvg & FedProx are Superior to FedSGD**:
    - Aggregating **model weights** (FedAvg, FedProx) resulted in **better performance** than aggregating **gradients** (FedSGD).
- 

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

- FL의 의료 이미지 분석 분야에서의 **현재의 도전 과제**, **연구 기회**, **미래 방향**에 대해 논의합니다.
- 클라이언트, 서버 사이드, 통신 등 다양한 관점에서 보고, 향후 개선 방향 제시
- 

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

- A. Vabalas, E. Gowen, E. Poliakoff, A. J. Casson, Machine learning algorithm validation with a limited sample size,
PLOS ONE 14 (11) (2019) 1–20.
- P. Kairouz, H. B. McMahan, B. Avent, A. Bellet, M. Bennis, A. N. Bhagoji, K. Bonawitz, Z. Charles, G. Cormode,
R. Cummings, et al., Advances and open problems in federated learning, Foundations and Trends® in Machine
Learning 14 (1–2) (2021) 1–210

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

- FedLearn에서 네트워크 개선을 통해 모델 성능을 개선할 수 있을까?
- 추가 자료 조사 및 후속 연구

</aside>

---

<aside>

<aside>

## **🤔 Next Task**

> ℹ️ *차주에 리뷰 예정인 논문 기재(APA. 인용 방식 권고)*
> 

[arXiv:2403.02611](https://arxiv.org/abs/2403.02611)

</aside>

</aside>