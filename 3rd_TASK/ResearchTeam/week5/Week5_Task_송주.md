<aside>

<aside>

## **📘 Title**

> ℹ️ *APA. 인용 방식 권고*
> 
</aside>

- Sermanet, P., Eigen, D., Zhang, X., Mathieu, M., Fergus, R., & LeCun, Y. (2014). **OverFeat**: Integrated recognition, localization and detection using convolutional networks. arXiv. https://arxiv.org/abs/1312.6229
</aside>

---

<aside>

<aside>

## **📖 Abstract**

> ℹ️ *본인의 방식으로 재해석 해주세요. 그대로 가져오는 것은 금합니다.*
> 
</aside>

- classification, localization, detection이 모두 가능하도록 하는 하나의 통합된 CNN 프레임워크를 발표했다.
- multiscale과 sliding window 방식을 도입하여 이 방식들이 Convolutional Network에서 얼마나 효율적인지를 보여준다.
- object detection의 신뢰도를 높이기 위해 bounding box를 suppression하는 대신 "accumulation"하는 방식을 사용한다.
- 하나의 공유 네트워크를 사용하여 서로 다른 작업들을 **동시에** 학습 가능하다.

</aside>

---

<aside>

<aside>

## **💡 Introduction**

</aside>

**📍 논문 선택의 동기**

- 비교적 오래된 논문이지만, 하나의 프레임워크로 이미지 분류, 객체 탐지, 위치 추정 등의 여러 작업을 동시에 처리 가능하도록 했다는 점에서 컴퓨터 비전 분야에서 의미있는 모델이었다고 생각이 들어서 선택하게 되었다.

**✅ 논문에서 다루고 있는 주요 연구 문제나 질문**

- 하나의 CNN으로 object의 분류, 위치 추정, 탐지를 모두 수행 가능한가?
- multi-scale과 sliding window 방식을 CNN 내에서 어떻게 효율적으로 구현할 수 있는가?
- object detection 성능을 높이기 위해 바운딩 박스를 supperession하지 않고 accumulate하는 방식이 과연 효과적인가?
- Background를 별도로 학습시키지 않고도 detection을 잘 할 수 있는가?

</aside>

---

<aside>

<aside>

## **📚 Background**

> ℹ️ *논문의 주제와 관련된 기존 연구들 및 배경 지식을 소개*
> 
</aside>

1. 📍 Related Work : ConvNet은 end-to-end 학습이 가능하나, 많은 양의 라벨링된 데이터가 필요하다.
2. 📍 Related Work : 기존 detection 시스템은 sliding window + 전통적 feature 추출 + classifier 구조로 구성되었으나 비효율적이고 복잡하다.
3. 📍 Related Work : 기존 CNN은 classification에 특화 되어 있었고, detection 및 localization에는 직접적인 적용 예시가 부족했다.
4. 📍 Related Work : ConvNet을 detection에 사용하려는 시도는 있었으나, 이후에는 CNN을 detection에 사용하고자 했다.
</aside>

---

<aside>

<aside>

## **🔍 Methods**

</aside>

**✅ 사용된 연구 방법**

- **통합된 CNN** architecture 사용
- **Multi-scale & Sliding Window** 전략
- Classification, Localization, Detection
- 추론 시, FC Layer도 1x1 convolution으로 간주하여 sliding

**✅ 실험 설계**

**📍 모델 비교**

1. **OverFeat vs. AlexNet**
 
</aside>

---

<aside>

<aside>

## **🔍 Experiments**

</aside>

**✅ 데이터셋**

- **Classification (ILSVRC 2012)**
- **Localization (ILSVRC 2013)**
- **Detection (ILSVRC 2013)**

**✅ Models**

- **OverFeat 계열(Fast, Accurate, Ensemble)**


**✅ Evaluation Metrics**

- **Top-1 Error**: 모델의 첫 번째 예측이 정답이 아닐 확
- **Top-5 Error**: 모델의 상위 5개 예측 중 정답이 없을 확률
- **IoU (Intersection over Union)** : 예측 박스와 정답 박스의 겹친 정도
- **mAP (mean Average Precision)**: 탐지 정확도 전체를 대표하는 핵심 지표


**✅ Implementation Details**

- **Optimizer**: Stochastic Gradient Descent (SGD) + Momentum
- **Learning Rate**: 0.05
- **Dropout**: Fully connected layer 6, 7에 대해 적용 (Dropout rate = 0.5)
- **Data Augmentation**: 256으로 resize -> 221x221 크기의 random crop 5개 + 좌우 반전 flip
- **Batch size**: 128
- **Conv Layer**: ReLU 활성화 사용
- **Multi-scale**
- **Sliding Window**


**✅ 실험 결과**

- **Classification**
    - **AlexNet**: Top-1 Error: 40.7%, Top-5 Error: 18.2%
    - **OverFeat - fast, 1 scale, coarse stride**: Top-1 Error: 39.28%, Top-5 Error: 17.12%
    - **OverFeat - fast, 1 scale, fine stride**: Top-1 Error: 39.01%, Top-5 Error: 16.97%
    - **OverFeat - fast, 4 scales**: Top-1 Error: 38.57%, Top-5 Error: 16.39%
    - **OverFeat - fast, 6 scales**:Top-1 Error: 38.12%, Top-5 Error: 16.27%
    - **OverFeat - accurate, 5 views + flip**: Top-1 Error: 35.60%, Top-5 Error: 14.71%
    - **OverFeat - accurate, 4 scales**: Top-1 Error: 35.74%, Top-5 Error: 14.18%
    - **OverFeat - 7 fast models 앙상블**: Top-1 Error: 35.10%, Top-5 Error: 13.86%
    - **OverFeat - 7 accurate models 앙상블**: Top-1 Error: 33.96%, Top-5 Error: 13.24%
  ▶️ 다중 스케일, fine stride, 앙상블이 성능 향상에 효과적
  ▶️ OverFeat는 AlexNet보다 확실히 더 낮은 오류율을 달성

- **Localization**
    - **Single center crop만 사용**: Localization Error: 약 40%
    - **2개 스케일 활용**: Localization Error: 31.5%
    - **4개 스케일 활용**: Localization Error: 30.0%
    - **Per-class regression**:  Localization Error: 44.1%
    - **Single shared regression**: 31.3%
  ▶️ 여러 위치, 스케일에서 bounding box 예측 누적 → 정확도 향상
  ▶️ 클래스를 개별로 회귀하는 것보다 하나의 회귀기 공유하는 방식이 더 효과적

- **Detection**
  - **OverFeat (대회 출전 당시)**: mAP: 19.4%
  - **OverFeat (사후 개선 버전)**: mAP: 24.3%
  ▶️ 후처리 없이 bounding box를 누적함으로써 false positive 감소
  ▶️ Bootstrapping 없이도 높은 mAP 확보

</aside>

---

<aside>

<aside>

## **📖 Conclusion**

</aside>

**✅ Limitation**

- **Sliding Window 기반의 탐지 → 느림**: OverFeat는 객체 탐지를 위해 dense sliding window를 사용하고, 여러 스케일과 위치에 CNN을 반복적으로 적용하기 때문에 계산 비용이 큼
- **Bounding Box 예측에 직접 최적화된 손실 함수 사용 안함**: 실제 평가 기준인 IoU(Intersection over Union)와는 불일치
- **배경(Background) 클래스 학습의 간접적 처리**: background class를 완전히 학습하지 않고, bootstraping이나 offline mining도 하지 않음
- **Bounding Box 간 병합(merging)에 의존 → NMS 대체의 한계**: OverFeat는 NMS(Non-Maximum Suppression) 대신 bounding box 누적 및 병합 방식 사용
- **기존 AlexNet 수준의 아키텍처 기반 → 표현력 제한**


**✅ Contribution**

- **분류, 위치 추정, 탐지를 하나의 CNN으로 통합**: 이후 모델인 YOLO, SSD, Faster R-CNN 등 통합 detection model의 기반 아이디어 제
- **Sliding Window 기반 탐지 @CNN**: NN의 convolution 특성을 활용하여 overlapping 영역 계산을 공유 -> Sliding window 방식이 느리다는 고정관념을 깨고, GPU 친화적 구현을 통해 효율 확보
- **Localization을 위한 회귀(Regression) 기반 바운딩 박스 예측 도입**: bounding box를 suppress하는 대신 accumulate -> CNN이 bounding box 위치를 직접 regression
- **Dense Multi-scale 입력 전략을 통한 성능 향상**: 입력 이미지를 여러 스케일로 확대/축소하여 CNN에 통과시키는 multi-scale strategy -> 이후 FPN(Feature Pyramid Networks) 같은 구조로 이어짐
- **OverFeat Feature Extractor**: 사전 학습된 feature extractor를 공개 -> 이후 Transfer Learning과 Feature Reuse 연구의 초석이 되었음

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
</aside>

**📍 Bootstrapping**

**📍 Offline Mining** 

**📍 NMS(Non-Maximum Suppression)**

**📍 Feature extractor**

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

- 등장한 이유?: AlexNet을 시작으로 CNN이 classification에서 놀라운 성능을 보연으나 detection과 localization은 복잡한 기존의 전통 방식을 사용했다. -> localization과 detection에 딥러닝을 본격적으로 적용하기 위함
- Task 기여?: Classification: Multi-scale inference와 dense sliding window를 통해 이미지 전체를 다양한 해상도에서 분석, Localization: Non-Maximum Suppression 없이 bounding box 누적(accumulation) 방식 제안 → 더 정밀하고 자연스러운 위치 추정 가능, Detection: 단일 ConvNet으로 탐지 수행 
- 배운 점?: CNN 하나로 컴퓨터비전의 다양한 과제를 해결 가능하게 되었다.

</aside>

---

<aside>

<aside>


---
