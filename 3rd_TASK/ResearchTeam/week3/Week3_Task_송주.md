<aside>

<aside>

## **📘 Title**

> ℹ️ *APA. 인용 방식 권고*
> 
</aside>

- Long, J., Shelhamer, E., & Darrell, T. (2015). Fully convolutional networks for semantic segmentation. Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 3431-3440. https://doi.org/10.1109/CVPR.2015.7298965
</aside>

---

<aside>

<aside>

## **📖 Abstract**

> ℹ️ *본인의 방식으로 재해석 해주세요. 그대로 가져오는 것은 금합니다.*
> 
</aside>

- FCN은 End-to-End, Pixel-to-Pixel 학습이 가능하도록 한 sematic segmentation 문제를 해결하기 위해 도입된 모델이다. 
- 기존 CNN 모델을 Fully Convolutional 형태로 변환하여 End-to-End, Pixel-to-Pixel 학습이 가능하도록 했다.
- 새로운 skip architecture을 사용하여 깊은 층과 얕은 층을 결합하여 "정확한 segmenatation을 가능"하도록 했다는 것이 주요 내용이다.

</aside>

---

<aside>

<aside>

## **💡 Introduction**

</aside>

**📍 논문 선택의 동기**

- 지난 주에 리뷰한 U-Net이 FCN 모델 구조를 기반으로 만들어진 것이라고 해서, 기존 모델인 FCN에 대해 좀 더 깊게 알아보고자 선택했다.

**✅ 논문에서 다루고 있는 주요 연구 문제나 질문**

- 기존 CNN 기반 classification 네트워크를 어떻게 변형했는가?
- down-sampling으로 인해 coarse한 feature map을 어떻게 복원하여 segmentation 해상도를 높일 수 있는가?
- Deep feature hierarchy에서 skip architecture를 적용한다면 segmentation 성능이 향상될까?

</aside>

---

<aside>

<aside>

## **📚 Background**

> ℹ️ *논문의 주제와 관련된 기존 연구들 및 배경 지식을 소개*
> 
</aside>

1. 📍 Related Work : 기존의 classification 모델(AlexNet, VGGNet, GoogLeNet)들은 pixel 단위의 예측(즉, segmentation)에는 적절하지 않았다.
2. 📍 Related Work : sliding window detection을 통해 segmentation을 수행했지만, fully convolutional 방식은 아니었음(OverFeat (2014))
3. 📍 Related Work : Patchwise Training 방법이 널리 사용되었으나, 연산량이 많고 공간적 지속성이라는 한계가 있다. (Farabet et al. (2013), Pinheiro & Collobert (2014))
</aside>

---

<aside>

<aside>

## **🔍 Methods**

</aside>

**✅ 사용된 연구 방법**

- 기존 CNN 분류 모델을 **fully convolutional**로 변형 (Fully Connected Layer -> 1x1 Convolutional Layer)
- Feature Map 해상도 복원을 위한 **Deconvolution Layer (Upsampling)** 사용
- patchwise training 대신 **whole image training** 적용
- 다양한 feature layer를 결합하는 **Skip Architecture** 설계

**✅ 실험 설계**

**📍 모델 비교**

1. **FCN vs. 기존 CNN 모델(AlexNet, VGGNet, GoogLeNet)**
    
2. **Shift-and-Stich방법 vs. Deconvolution방법**
 
</aside>

---

<aside>

<aside>

## **🔍 Experiments**

</aside>

**✅ 데이터셋**

- **PASCAL VOC 2011 & 2012**
- **NYUDv2 (RGB-D 데이터셋)**
- **SIFT Flow (Scene Parsing 데이터셋)**

**✅ Models**

각 layer
- **FCN-32s**
- **FCN-16s**
- **FCN-8s**

**✅ Evaluation Metrics**

- **Pixel Accuracy**: 전체 픽셀 중 정확하게 분류된 비율
- **Mean Accuracy**: 클래스별 정확도의 평균
- **Mean IoU**: IoU의 클래스별 평균
- **Frequency Weighted IoU**: 클래스 빈도의 IoU

**✅ Implementation Details**

- **Loss Function**: Per-pixel Multinomial Logistic Loss (Softmax Loss) 사용
- **Optimizer**: Stochastic Gradient Descent (SGD)
- **Fine-tuning**: 기존 CNN 분류 모델에서 사전 학습된 weight를 초기화, 추가 segmentation layer에는 초기 weight를 0으로 설정
- **Patchwise Training vs. whole Image Training**
- **학습시간**: FCN-32s는 3일 학습, FCN-16s & FCN-8s는 4일 학습

**✅ 실험 결과**

- **PASCAL VOC 2012 결과(SOTA 비교)**
    - **R-CNN**: Mean IU: 47.9%, 추론 속도: 느림
    - **SDS (기존 SOTA)**: Mean IU: 52.6%, 추론 속도: 50초
    - **FCN-32s**: Mean IU: 59.4%, Pixel Accuracy: 89.1%, 추론 속도: 210ms
    - **FCN-16s**: Mean IU: 62.4%, Pixel Accuracy: 90%, 추론 속도: 210ms
    - **FCN-8s**: Mean IU: 62.7%, Pixel Accuracy: 90.3%, 추론 속도: 175ms
  ▶️ FCN-8s가 기존 SOTA 대비 약 20% 향상된 Mean IU 성능을 달성
  ▶️ FCN은 기존 방법보다 추론 속도가 훨씬 빠름

- **NYUDv2 결과**
    - **FCN-32s (RGB)**: Mean IU: 29.2%, Mean Accuracy: 42.2%, Pixel Accuracy: 60% 
    - **FCN-16s (RGB + HHA)**: Mean IU: 34%, Mean Accuracy: 46.1%, Pixel Accuracy: 65.4%
  ▶️ RGB-D 데이터를 활용하면 segmentation 성능이 향상됨
  ▶️ HHA Encoding을 추가한 경우 성능이 가장 우수함

</aside>

---

<aside>

<aside>

## **📖 Conclusion**

</aside>

**✅ Limitation**

- **해상도 loss 문제**: Pooling과 Stride로 인해 fine detail이 손실될 수 있음
- **global context 정보 부족**: Long-range dependency를 고려하기 어려움
- **연산량 문제**: Fully Convolutional 구조로 인해 deep model 사용 시 계산 비용 증가 가능성
- **클래스 간 혼동 발생 가능**: 픽셀 단위 예측 방식으로 인해 경계가 흐려지는 문제 발생


**✅ Contribution**

- FCN 개념 도입
- Skip Architecture를 활용한 Fine Segmentation 가능
- End-to-End 학습이 가능한 Fully Convolutional 구조 제안
- 기존 SOTA 대비 높은 성능 달성
- 실시간 Semantic Segmentation 모델 개발에 기여

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

**📍 Skip Architecture**

**📍 HHA Encoding** 

**📍 Framework 종류(e.g. Caffe)**

**📍 Per-pixel Multinomial Logistic Loss**

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

- 등장한 이유?
: Patchwise training의 문제점을 보완(이미지 전체를 학습할 수 있는 Fully Convolutional 구조 필요),
 기존 방식의 성능(해상도, 속도, 연산량 문제)을 높이기 위해서 
- Task 기여?: 이후 DeepLab, U-Net, PSPNet 등 다양한 Semantic Segmentation 모델의 발전에 핵심적인 역할을 함
- 배운 점?: 기존 CNN classification 모델과의 차이점, Skip connection architecture, Deconvoultional을 적용한 이유, whole Image Training
- 추가 궁금증?: Semantic Segmentation 모델의 구조와 등장하게 된 이유에 대해 좀 더 공부해봐야겠다.

</aside>

---

<aside>

<aside>

## **🤔 Next Task**

> ℹ️ *차주에 리뷰 예정인 논문 기재(APA. 인용 방식 권고)*
> 
</aside>

- Zhao, H., Shi, J., Qi, X., Wang, X., & Jia, J. (2017). Pyramid scene parsing network. Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2881-2890. https://doi.org/10.1109/CVPR.2017.660

</aside>

---
