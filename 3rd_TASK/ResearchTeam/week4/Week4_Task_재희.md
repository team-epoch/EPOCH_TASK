# Week4_Task_재희

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

diffusion model은 높은 품질의 이미지를 합성할 수 있지만 training과 inference 시 computing cost가 많이 필요한데 본 논문은
1. pretrained autoencoder의 latent space에서 diffusion model을 훈련
2. cross-attention을 모델 아키텍처에 도입  

함으로써 이미지 합성의 퀄리티는 유지하면서 computing cost는 감소시키고, diffusion 모델을 text나 bounding box
와 같은 다양한 조건에서 이미지 합성 가능하게 만들었다.

</aside>

---

<aside>

<aside>

## **💡 Introduction**

</aside>

**📍 논문 선택의 동기**

다양한 최신의 multi-modal 모델들이 어떻게 동작하는지 알고싶었다. 

**✅ 논문에서 다루고 있는 주요 연구 문제나 질문**

diffusion 모델과 같은 likelihood-based 모델은 데이터의 사람이 인지 불가능한 특징을 표현하는데에 많은 리소스가 낭비
diffusion 모델은 데이터에서 의미 없는 부분을 제거하는 과정(perceptual compression)과 데이터의 특징을 학습하는 과정(semantic compression)으로 구성  

-> 이 과정에서 인지 가능하면서도 차원이 낮은 공간을 찾아 연산량을 줄여보자!

</aside>

---

## **📚 Background**

> ℹ️ *논문의 주제와 관련된 기존 연구들 및 배경 지식을 소개*
> 
> 
> **📍 Related Work 1**
> 
> **📍 Related Work 2**
> 
</aside>

**📍Generative Models for Image Synthesis**

  GAN 모델은 최적화 수렴이 어렵고 Full Data Distribution을 포착하기 어려움  
  반면 Likelihood-based 방법(VAE, Flow-based 모델)은 데이터 분포를 잘 포착하여 효율적으로 고해상도 이미지 합성이 가능

**📍Two-Stage Image Synthesis**

  최근의 2-Stage 모델 구조로의 접근방식
  
  1. VQ-VAE  
     이전 Latent Space를 충분히 학습시키기 위해 AutoRegressive model을 사용
     
  2. VQ-GAN
     AutoRegressive Transformers를 큰 이미지에 적용시키기 위해 Adversarial & Perceptual Objective를 첫번째 단계에 사용하였다.

  이런 모델들의 경우 Encoder와 Decoder를 같이 학습하거나 따로 학습하게 되는데, 같이 학습하는 경우 Reconstruction과 Generative Capability 사이에 Weighting이 필요하기 때문에 따로 학습하는 방식이 주목받고 있음


</aside>

---

<aside>

<aside>

## **🔍 Methods**

</aside>

**✅ 사용된 연구 방법**

**1. Perceptual Image Compression** 

Perceptual Image Compression이란 Autoencoder에서 Latent space를 학습하는 것을 의미.  

Latent Space의 분산이 크면 Latent Space가 가지는 정보가 이질적이므로 작은 분산을 가지도록 Regularization 적용
-> KL-reg: 학습된 Latent 에 KL-penalty 적용
-> VQ-reg: Decoder 안에 vector quantization 사용


**2. Latent Diffusion Models** 

![image](https://github.com/user-attachments/assets/f9e59c25-cec7-4eaf-89b6-cec202b6ddb4)

Stable Diffusion은 이미지에 노이즈를 주고 이를 다시 역으로 계산하여 일반 이미지로 복원시키도록 학습시키는 과정  
Latent Diffusion은 노이즈에서 복원하는 것은 같지만 이미지를 바로 복원하는 것이 아니라 Latent vector를 복원한 뒤 이를 다시 VAE를 거치도록 하여 이미지를 복원하는 방식

- 위 사진에서 왼쪽의 빨간 부분은 Auto Encoder, 가운데 초록 부분은 Latent Diffusion Model, 오른쪽의 회색 부분은 Condition 입력 부분을 의미.  
- 입력된 Condition은 Diffusion Model 내부에서 Cross Attention이 적용됨.
- U-Net Backbone과 Cross-Attention 매커니즘을 활용해 Diffusion Model을 더 유연하게 조건을 줄 수 있도록 만듦

![image](https://github.com/user-attachments/assets/75200371-5f60-4ea2-b44d-5d304046dac0)
위 LDM 구조의 목적함수







</aside>

---

<aside>

<aside>

## **🔍 Experiments**

</aside>

**✅ Models**

ImageBART, U-Net GAN, UDM, StyleGAN, ProjectedGAN, DDPM etc...


**✅ Datasets**

image: CelebA-HQ, FFHQ, LSUN-Churches, LSUN-Bedrooms, ImageNet, COCO
text: LAION


**✅ Evaluation Metrics**

FID, IS, Precision, Recall

![image](https://github.com/user-attachments/assets/c3889224-15f8-483a-a228-16f5d54e7d49)
![image](https://github.com/user-attachments/assets/cd6721e1-8bcf-475d-814b-30e71e25b27f)



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

**✅ Limitation**

- Latent Space 기반의 방법을 사용하여 Pixel 기반 방법론 보다는 빠르지만 Sampling Process가 아직 GAN보다 느fla
- LDM모델은 높은 Precision이 요구되는 작업에서 적합하지 않을 수 있다.
- 데이터 조작, 윤리적인 문제 발생 가능

**✅ Contribution**

- diffusion 모델의 학습과 sampling 효율성을 크게 개

</aside>

---

<aside>

<aside>

## **📚 하향식 접근**

ℹ️ VAE(Variational Autoencoder)
  - 확률적 오토인코더로, 데이터의 분포를 학습하여 새로운 샘플을 생성하는 모델
  - 인코더가 입력 데이터를 잠재 공간(latent space)으로 변환하고, 디코더가 이를 다시 원래 데이터로 복원
  - 잠재 공간을 정규 분포로 유도하기 위해 KL-penalty를 사용

ℹ️ flow-based model
  - 데이터의 분포를 정확하게 학습하기 위해 가역적인(역변환이 가능한) 함수 변환을 사용하는 확률 모델
  - 확률 밀도 함수를 직접 계산할 수 있어 Likelihood-based 모델로 분류됨
  - ex) RealNVP, Glow

ℹ️ autoregressive model(ARM)
  - 데이터의 각 요소를 순차적으로 예측하는 방식의 모델
  - 확률적 종속성을 이용하여 데이터를 생성
  - ex) PixelCNN, WaveNet 

ℹ️ likelihood-based model
  - 데이터가 주어진 확률을 최대화하도록 학습하는 모델
  - VAE, Flow-based model, ARM 등이 여기에 포함됨
  - 생성 모델에서 데이터 분포를 정확하게 학습하는 것이 목표

ℹ️ KL-penalty
  - VAE에서 잠재 공간의 분포를 정규 분포와 유사하게 만들기 위해 사용하는 정규화 항
  - KL Divergence는 두 확률 분포 간 차이를 측정하는 지표로 이를 최소화하면 모델이 안정적인 잠재 공간을 학습할 수 있음.

ℹ️ Vector Quantization
  - 연속적인 잠재 공간 대신, 이산적인(Discrete) 코드북(Codebook)을 사용하여 데이터를 표현하는 방식으로 이미디, 오디오 등의 생성 모델에서 효과적으로 사용
  - ex) VQ-VAE (Vector Quantized VAE)

ℹ️ latent space
  - 데이터가 압축된 형태로 표현되는 공간
  - 고차원 데이터를 낮은 차원으로 변환하여 특징을 추출하는 데 사용
  - 생성 모델에서 중요한 역할을 하며, 이 공간에서 샘플링을 통해 새로운 데이터를 생성할 수 있음.



</aside>


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


</aside>

---

<aside>

<aside>

## **🤔 Next Task**

> ℹ️ *차주에 리뷰 예정인 논문 기재(APA. 인용 방식 권고)*
> 
</aside>
- Karras, T., Laine, S., & Aila, T. (2021). A style-based generator architecture for generative adversarial networks. IEEE Transactions on Pattern Analysis and Machine Intelligence, 43(12), 4217–4228. https://doi.org/10.1109/TPAMI.2020.2970919
</aside>

---
