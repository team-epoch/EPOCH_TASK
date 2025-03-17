# Week3_Task_재희

---

<aside>

<aside>

## **📘 Title**

> ℹ️ *APA. 인용 방식 권고*
> 
</aside>

- Radford, A., Kim, J. W., Hallacy, C., Ramesh, A., Goh, G., Agarwal, S., Sastry, G., Askell, A., Mishkin, P., Clark, J., Krueger, G., & Sutskever, I. (2021). Learning transferable visual models from natural language supervision. arXiv preprint arXiv:2103.00020. https://arxiv.org/abs/2103.00020
</aside>

---

<aside>

<aside>

## **📖 Abstract**

> ℹ️ *본인의 방식으로 재해석 해주세요. 그대로 가져오는 것은 금합니다.*
> 
</aside>

- auto-regressive하게 텍스트와 이미지 토큰을 하나의 스트림으로 받아들여 transformer를 학습하는 text-to-image 방식을 제안
- 충분히 많은 데이터와 큰 스케일을 사용할 때 zero-shot 방식의 평가에 있어서 이전의 도메인 특화 모델과 경쟁력 있는 성능을 보임
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

- 이전에 리뷰했던 CLIP 모델이 활용된 논문이라 기존의 개념이 어떤 식으로 활용되는지 궁금

**✅ 논문에서 다루고 있는 주요 연구 문제나 질문**

- auto-regressive transformer을 기반으로 한 text-to-image 작업에 있어 모델 사이즈와 데이터를 잘 scale up 하는 방식이 많은 성과를 기록. 
  -> 모델과 데이터의 스케일을 극한으로 키워 성능을 향상시켜보자!
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

**📍 *Autoencoder*

- 주어진 데이터의 분포를 잠재 공간에 압축하는 방법을 학습하는 신경망 모델로 압축하는 encoder, 압축을 푸는 decoder로 나뉨.
  
  ![image](https://github.com/user-attachments/assets/590d5fda-a767-4a56-a916-4a35c21e6fda)

    

**📍 VAE(Variational Autoencoder)**

- 학습 이미지를 변환한 잠재 공간(z)을 잘 정된되게 배치한 Autoencoder
    
  ![image](https://github.com/user-attachments/assets/f38abbf2-290a-4ea6-a47d-80e8ec9a0e66)

    

**📍 dVAE(Discrete Variational Autoencoder) **
- 이미지의 연산량을 줄이기 위해 연속적인 픽셀 값 대신 이산적인 코드(visual tokens)로 변환하는 모델로 변환 과정은 아래와 같음
  1) 이미지 데이터 encoder를 통해 잠재공간으로 변환 
  2)	미리 학습된 visual codebook에서 가장 가까운 코드 선택해 이산적인 토큰으로 매핑 (256x256 픽셀 이미지 -> 32x32 토큰 시퀀스로 변환)


**📍 Visual codebook **
- 원본 이미지를 토큰으로 변환할 때 참조되는 것으로 잠재 공간 상의 벡터들이 각각 실제 이미지의 어떤 부분과 매칭하는지 정리해놓은 리스트. 


**📍 ELB(Evidence Lower Bound, 증거 하한 경계) **
- VAE와 같은 모델에서 학습을 안정적으로 만들기 위한 손실함수로 모델이 최적의 확률분포를 학습할 수 있도록 도움. 


**📍 BPE-encoded(Byte Pair Encoding) **
- 자연어 처리에서 텍스트를 압축하여 토큰화하는 방법 중 하나. 일반적인 단어 기반 토큰화와 달리 자주 등장하는 문자 쌍을 병합해 더 작은 단위로 subword 토큰을 생성 (Ex. lowest -> [“low”, “est”])


**📍 BPE-encoded(Byte Pair Encoding) **
- 신경망에서 역전파 과정 중 기울기의 크기를 조절하는 기법

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

- 모델 파라미터 120억개, 텍스트-이미지 쌍 데이터 2억 5천만 장

- 256개의 BPE-인코딩 된 텍스트 토큰과 32x32=1024 이미지 토큰을 결합하여 트랜스포머에 입력한 뒤 확률분포를 학습

- 전체적인 학습 과정은 이미지 x, 캡션 y, 토큰 z에 대한 ELB(evidence lower bound)를 최대화하는 과정으로 해석할 수 있음.
  
  ![image](https://github.com/user-attachments/assets/956d74ba-d1c3-4aeb-8009-70fa38e72d1e)
  확률분포 모델링
  
- lower bound
  
  ![image](https://github.com/user-attachments/assets/bd5bfe12-7798-4dea-abc6-185522bdc613)
  
  Phi와 theta를 고정한 채 텍스트와 이미지 토큰에 대한 prior distribution을 학습
  Phi에 대한 ELB를 극대화하며 phi는 120억 파라미터를 가지는 sparse transformer를 사용
  
- Mixed-Precision Training
  GPU 메모리 절약, throughput 증가를 위해 16-bit precision parameter 사용
  Underflow를 피하며 large scale 모델을 16-bit precision으로 학습시킬 수 있도록 하기 어려움 -> resblock에서 activation의 gradient norm이 일정하게 뒤쪽 layer로 갈수록 감소하다가 16-bit로 표현할 수 있는 범위를 벗어나 문제 발생
  solution -> resblock마다 gradient scale 적용


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

- 평가지표
  1) FID(Frechet Inception Distance): 생성된 이미지와 실제 이미지의 분포 차이를 측정하는 거리
  2) IS(Inception Score): 생성된 이미지의 다양성과 선명도를 평가하는 지표
  
  ![image](https://github.com/user-attachments/assets/d90f7c77-55b4-4217-8481-de4c4a5d9e39)


  (a) MS-COCO 데이터셋에 대해서 기존 모델들보다 FID, IS가 더 좋게 나옴
  (b) CUB 데이터셋에 대해서는 DM-GAN, DF-GAN보다 FID 성능은 더 안좋게 IS는 가장 안좋은 성능을 보임
  (c) clip에게 많은 candidates를 주고 가장 좋은 결과를 뽑으면 성능이 향상(32개까지 FID, IS 상승)

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

- Autoregressive transformer을 기반으로 한 text-to-image generation에 대한 간단한 접근법을 연구
- Model, data scale을 높이는 것이 zero-shot, 단일 생성형 모델 관점에서 일반화 성능을 향상

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

- background 부분 참고

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

- 대규모 모델, 데이터를 통해 학습시킬 때 어떤 부분에서 어려움이 있고 이를 해겷하는 기법들에 대해 알게 되었음. 
- 구체적인 모델 아키텍쳐가 드러나지 않아 아쉬운 부분이 있음

</aside>

---

<aside>

<aside>

## **🤔 Next Task**

> ℹ️ *차주에 리뷰 예정인 논문 기재(APA. 인용 방식 권고)*
> 
</aside>

- Rombach, R., Blattmann, A., Lorenz, D., Esser, P., & Ommer, B. (2022). High-resolution image synthesis with latent diffusion models. In Proceedings of the IEEE/CVF conference on computer vision and pattern recognition (CVPR) (pp. 10684-10695)

</aside>

---
