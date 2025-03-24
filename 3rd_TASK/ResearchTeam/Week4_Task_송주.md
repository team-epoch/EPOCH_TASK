<aside>

<aside>

## **📘 Title**

> ℹ️ *APA. 인용 방식 권고*
> 
</aside>

- Zhao, H., Shi, J., Qi, X., Wang, X., & Jia, J. (2017). Pyramid scene parsing network. Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2881–2890. https://doi.org/10.1109/CVPR.2017.660
</aside>

---

<aside>

<aside>

## **📖 Abstract**

> ℹ️ *본인의 방식으로 재해석 해주세요. 그대로 가져오는 것은 금합니다.*
> 
</aside>

- Scene parsing은 제한되지 않은 open vocabulary와 다양한 scenes으로 인해 어려움이 있다.
- Pyramid Pooling module을 통한 different-region-based context를 사용해 global context 정보의 활용 가능성을 탐구한다.
- PSPNet은 global 정보를 효과적으로 표현해 scene parsing 작업에서 높은 품질의 결과를 생성하며, 다양한 데이터셋에서 최고의 성능을 달성했다.

</aside>

---

<aside>

<aside>

## **💡 Introduction**

</aside>

**📍 논문 선택의 동기**

- 지난 주에 리뷰한 FCN 모델의 이후 Semantic Segmentation 모델인 PSPNet에 대해 더욱 딥하게 알아보고자 선택했다.

**✅ 논문에서 다루고 있는 주요 연구 문제나 질문**

- 복잡학 다양한 scene을 정확하게 parsing하려면 어떤 정보가 필요한가?
- FCN 기반 모델이 global context 정보를 왜 활용하기 어려우며, 이를 극복할 방법은 무엇인가?
- Golobal context 정보를 효과적으로 학습하면서도 pixel 단위의 local context 정보를 유지하는 효과적인 architecture는 무엇인가?
- Deep Network의 깊이에 따른 학습을 할 때, 안정화되고 성능을 높일 수 있는 효과적인 최적화 방법은 무엇인가?

</aside>

---

<aside>

<aside>

## **📚 Background**

> ℹ️ *논문의 주제와 관련된 기존 연구들 및 배경 지식을 소개*
> 
</aside>

1. 📍 Related Work : Dilated convolution을 통해 신경망의 receptive field를 넓히고자 했다. 베이스라인 네트워크는 FCN과 dilated network이다.
2. 📍 Related Work : Multi-scale feature ensembling으로 다양한 scale의 feature를 조합함으로써 성능을 향상시키고자 하였으나, 복잡한 scene에서는 한계가 있다.
3. 📍 Related Work : Structured prediction-based 접근으로 End-to-End 방식으로 네트워크를 정제하고 개선하였으나, 여전히 복잡한 scene에서 필요한 정보의 활용에는 한계가 있다.
4. 📍 Related Work : Global Image 정보의 활용으로 성능을 향상시키고자 하였으나, ADE20K와 같이 어려운 데이터셋에서는 한계가 있다.
</aside>

---

<aside>

<aside>

## **🔍 Methods**

</aside>

**✅ 사용된 연구 방법**

- **Pyramid Pooling Module**을 도입
- **Dilated Convolution** 사용
- 깊은 네트워크를 더 안정적으로 학습하기 위한 **보조 Loss** 부여
- 다양한 깊이의 **사전 학습된 ResNet** 모델 사용
- 테스트 시 입력 이미지를 여러 크기로 스케일링하여 예측 후 평균을 내는 **Multi-scale 테스트**

**✅ 실험 설계**

**📍 모델 비교**

1. **FCN, DeepLab, DPN, CRF-RNN 등 vs. PSPNet**
 
</aside>

---

<aside>

<aside>

## **🔍 Experiments**

</aside>

**✅ 데이터셋**

- **ADE20K (ImageNet Scene Parsing Challenge 2016)**
- **PASCAL VOC 2012 (20 Object Classes)**
- **Cityscapes (Urban Scene Understanding)**

**✅ Models**

- **ResNet 계열(ResNet-50, ResNet-101, ResNet-152, ResNet-269) + Pyramid Pooling Module**


**✅ Evaluation Metrics**

- **Mean Intersection over Union (Mean IoU)**: 각 클래스에 대해 예측 결과와 정답 간의 교집합/합집합을 계산한 후, 모든 클래스에 대해 평균
- **Pixel Accuracy**: 전체 픽셀 중 정답 클래스를 맞춘 픽셀의 비
- **Instance-level Intersection over Union (iIoU)** @Cityscapes: 객체의 크기를 고려한 IoU/ 작은 객체일수록 더 많은 가중치 부여

**✅ Implementation Details**

- **Framework**: Caffe 사용
- **Optimizer**: Stochastic Gradient Descent (SGD)
- **Learning Rate Schedule**: "poly" decay policy 사용
- **Data Augmentation**: Random mirror, Random resize, Random rotation, Random Gaussian blur
- **Batch size**: 16
- **Auxiliary loss weight**: 0.4

**✅ 실험 결과**

- **ADE20K (ImageNet Scene Parsing Challenge 2016)**
    - **FCN**: Mean I0U: 	29.39%, Pixel Acc.: 71.32%
    - **DilatedNet**: Mean IoU: 32.31%, Pixel Acc.: 73.55%
    - **ResNet50-Baseline**: Mean IoU: 34.28%, Pixel Acc.: 76.35%
    - **ResNet50 + DA + Aux Loss**: Mean IoU: 37.23%, Pixel Acc.: 78.01%
    - **ResNet50 + DA + Aux + PPM**: Mean IoU: 41.68%, Pixel Acc.: 80.04%
    - **ResNet269 + All + MS**: Mean IoU: 44.94, Pixel Acc.: 81.69%
  ▶️ PSPNet 단일 모델이 ImageNet Scene Parsing Challenge 2016에서 1위 달
  ▶️ 다른 팀들의 ensemble보다 성능이 우수

- **PASCAL VOC 2012**
    - **FCN**: Pretraining: VOC only, mIoU: 62.2%
    - **DeepLab**: Pretraining: VOC only, mIoU:	71.6%
    - **CRF-RNN**: Pretraining: VOC only, mIoU:	72.0%
    - **PSPNet**: Pretraining: VOC only, mIoU: 82.6%
    - **DeepLab†**: Pretraining: VOC + COCO, mIoU: 79.7%
    - **PSPNet†**: Pretraining: VOC + COCO, mIoU: 85.4%
  ▶️ PSPNet은 VOC-only 기준으로도 COCO-pretrained 모델보다 우수한 성능 달성
  ▶️ VOC+COCO 학습 시 모든 클래스에서 최고 정확도 기록

- **Cityscapes**
  - **FCN**: Dataset 사용: fine only, mIoU:	65.3, iIoU: 41.7
  - **DeepLab**: Dataset 사용: fine only, mIoU:	70.4, iIoU:	42.6
  - **Piecewise**: Dataset 사용: fine only, mIoU: 71.6, iIoU:	51.7
  - **PSPNet**: Dataset 사용: fine only, mIoU: 78.4, iIoU: 56.7
  - **PSPNet†**: Dataset 사용: 	fine + coarse, mIoU: 80.2, iIoU: 58.1
  ▶️ Cityscapes에서도 모든 방법 중 최고 성능
  ▶️ fine + coarse 데이터셋을 함께 쓸 경우 80.2% mIoU 달성

</aside>

---

<aside>

<aside>

## **📖 Conclusion**

</aside>

**✅ Limitation**

- **연산량과 메모리 사용량 문제**: 깊은 ResNet을 사용하고, Pyramid Pooling Module에서 추가 연산이 발생해, 메모리 사용량이 크고 느림
- **고해상도 입력 이미지 처리에 한계**: Cityscapes와 같은 고해상도 이미지는 crop해서 학습해야 함
- **공간 정보 손실 가능성 존재**: 정확한 경계선 정보(edge) 또는 작은 객체의 위치를 희석시킬 가능성이 있음
- **후처리 부재**: CRF 등 세밀한 경계 정제가 없음


**✅ Contribution**

- **PSPNet** 제안: 기존 FCN의 한계였던 전역 정보 활용 부족 문제를 해결
- **Pyramid Pooling Module (PPM)** 도입: 단순한 global average pooling보다 더 정교하고 효과적인 전역 표현 생성
- **Auxiliary Loss**을 적용해 최적화 난이도를 낮추고 전체 네트워크의 성능을 향상시킴
- 세 가지 주요 데이터셋에서 SOTA 성능 달성

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

**📍 Scene Parsing**

**📍 Pyramid Pooling Module** 

**📍 Dilated Convolution**

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

- 등장한 이유?: FCN의 한계-> global context 정보 부족, 유사 클래스 간 혼동 등/ 복잡한 scene을 더 잘 이해하고, 다양한 크기의 객체와 다양한 context를 동시에 포착 가능함
- Task 기여?: Scene Parsing: 다양한 객체/배경이 혼재한 복잡한 장면에서 더 정확한 픽셀 예측 가능, mantic Segmentation:각 픽셀을 정확한 클래스에 할당하는 성능을 SOTA 수준으로 끌어올림
- 배운 점?: global context는 sematic segmentation에서 핵심이다. 단순한 구조(Pyramid Pooling Block)로도 성능을 크게 향상시킬 수 있다. 단일 모델로도 최고 성능을 낼 수 있다


</aside>

---

<aside>

<aside>

## **🤔 Next Task**

> ℹ️ *차주에 리뷰 예정인 논문 기재(APA. 인용 방식 권고)*
> 
</aside>

- 
Sermanet, P., Eigen, D., Zhang, X., Mathieu, M., Fergus, R., & LeCun, Y. (2014). OverFeat: Integrated recognition, localization and detection using convolutional networks. arXiv. https://arxiv.org/abs/1312.6229


</aside>

---
