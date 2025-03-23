# Week4_Task_우림

---

<aside>

<aside>

## **📘 Title**

> ℹ️ *APA. 인용 방식 권고*
> 
</aside>

- Kingma, D. P., & Welling, M. (2013). Auto-encoding variational Bayes. *arXiv preprint arXiv:1312.6114*. [https://arxiv.org/abs/1312.6114](https://arxiv.org/abs/1312.6114)
</aside>

---

<aside>

<aside>

## **📖 Abstract**

> ℹ️ *본인의 방식으로 재해석 해주세요. 그대로 가져오는 것은 금합니다.*
> 
</aside>

- **VAE(Variational Auto-Encoder)**를 소개하는 논문
- SGVB(Stochastic Graident Variational Bayes) estimator를 AEVB(Auto-Encoding Variational Bayes) 알고리즘에 적용하여 recognition model 최적화에 사용
    - 변분 추론+Auto Encoder → 데이터의 잠재 분포 학습
- 확률적 모델을 효율적으로 학습하는 방법에 대해 이야기함
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

- Build-up session 마지막 주차의 주제인 Generative AI & models에서 GAN(Generative Adversarial Nets) 논문 리딩 전, 배경 지식을 쌓기 위해 위 논문을 선택함.

**✅ 논문에서 다루고 있는 주요 연구 문제나 질문**

- 어떻게 연속형 잠재 변수, 파라미터를 다루기 힘든 사후 분포를 가지는 확률적 모델을 효율적으로 근사 추론하고 학습할 수 있을까?
- 변분적 하한의 재파라미터화가 어떻게 간단하고 미분 가능한 하한의 불편 추정량을 만드는지 보여 줄 것
- SGVB: 연속형 잠재 변수/파라미터를 가진 어느 모델에서도 효율적으로 근사 사후 추론을 하는 데 사용되고, 경사적 확률 하강법을 사용해서 최적화 가능함
- iid 데이터셋, 연속형 잠재 변수를 가진 경우에 대해 AEVB 알고리즘 제안
- AEVB: recognition model을 최적화하는 데에 SGVB 추정량을 사용해서 효율적으로 추론, 학습
- 학습된 근사 사후 추론 모델은 recognition, denoising, representation, visualization과 같은 task에서 사용될 수 있음
- Neural Network: recognition model에 사용됨
- 변분 추론 + 신경망 모델 → Variational auto-encoder; VAE
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

**📍 Auto-Encoder**

- 비지도학습 모델의 종류 중 하나로, 관측된 고차원 자료의 차원 축소를 목표로 하는 모형
- 주성분분석(Principal Component Analysis; PCA)와 유사하나, 관측치와 주성분 사이의 관계가 비선형
- Neural Networks의 구조를 가지며, input/output의 차원이 동일
- 출력값이 입력값을 근사하도록 신경망 함수를 학습
- 인코더(Encoder): 입력 데이터에 대한 핵심 특징을 추출하는 함수
- 디코더(Decoder): 추출된 값을 이용하여 원본 데이터를 재구성하는 함수
- 인코딩 과정에서 입력된 데이터의 핵심 정보만 hiddel layer에서 학습하고 나머지 정보는 손실시킴 → 디코딩 과정에서 hidden layer의 출력값을 뽑았을 때 완벽한 복사가 아닌 입력값의 근사치가 됨
- 출력값이 입력값과 최대한 비슷해지도록 학습

**📍 변분 추론(Variational Inference, VI)**

- 베이즈 추론을 근사하는 방법. 기존 VI 방법은 계산 비용이 높음.
- 이 task 안에서 자세한 설명과 정리는 생략… 다음 웹사이트 참고! [https://ratsgo.github.io/generative model/2017/12/19/vi/](https://ratsgo.github.io/generative%20model/2017/12/19/vi/)

**📍 wake-sleep algorithm**

- true posterior로 근사하는 recognition 모델 사용
- 이산형 잠재 변수도 적용 가능, AEVB와 동일한 컴퓨팅 복잡도

**📍 Stochastic variational inference**

- naive gradient 추정량의 높은 분산을 줄이고, 사후 분포의 지수족 근사 적용
- 지수족 근사 분포의 파라미터를 학습하는 데 확률적 변분 추론 알고리즘의 효율적 버전이 사용됨
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

**✅ Intractability**

- 가정: iid인 연속 또는 이산형 변수 x샘플 N개로 이루어진 데이터셋
    - 데이터가 관찰되지 않은 랜덤한 연속변수 z에 의해서 생성된다고 가정
    - 사전 분포 $p_{\theta^*}(z)$
    - 가능도 $p_{\theta^*}(x|z)$
    - 사전 분포, 가능도는 parametric families of distributions $p_{\theta}(z)$와 $p_{\theta}(x|z)$로부터 오고, PDF들은 $\theta,z$에 대해 미분 가능
- true posterior density 구하기 어려우므로 marginal likelihood의 적분 어려움 → EM, VB 알고리즘 사용 X
- large dataset의 경우, batch optimization 사용시 많은 비용 소요

**✅ The variational bound**

- Background - VI 파트에 첨부한 링크 참고

![image](https://github.com/user-attachments/assets/a4344e92-46b8-4c12-aece-f52ece25721c)



**✅ The SGVB estimator and AEVB algorithm**

![image](https://github.com/user-attachments/assets/5cfe13cc-a901-4760-9884-46e42626b55e)


**✅ The reparameterization trick**

- $z$: continuous random variable
- $z$ ~ $q_\phi(z|x)$ 조건부 분포
- z를 N(μ,σ)에서 직접적으로 샘플링하지 않고 Deterministic output vector + Gaussian Noise로 계산하는 것
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

**✅ 데이터셋**

- MNIST
- Frey Face

**✅ 비교 알고리즘**

- Wake-Sleep
- Monte Carlo EM; MCEM

**✅ 평가지표**

- ELBO
- Marginal likelihood
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

- 변분 자동 인코더(VAE)를 제안하여 확률적 생성 모델을 효율적으로 학습할 수 있도록 개선.
- 재파라미터화 트릭(Reparameterization Trick)을 도입하여 변분 추론을 신경망 기반 최적화로 변환.
- MNIST 및 얼굴 데이터셋을 활용한 실험을 통해 VAE의 생성 성능과 학습 안정성을 입증.
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

- 위 논문을 보다가 변분 추론, Monte carlo, ELBO가 뭔지 몰라 찾아서 공부
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

- 본 연구는 컴퓨터 비전 분야에서 한 획을 그은 방법론으로, 특히 image generation 분야에서는 엄청남
- 이후 GAN, diffusion 모델의 등장으로 인기가 줄어들긴 했지만 생성형 AI 모델의 시초가 되는 방법론이므로 다른 방법론들을 이해하기 위해서 꼭 알아야 함
- 아직 수식 이해를 완벽히 하지 못해서 수식을 따로 노트에 필기하면서 정리해 보는 시간이 필요할 것 같음
- 참고 자료
    - [https://ratsgo.github.io/generative model/2017/12/19/vi/](https://ratsgo.github.io/generative%20model/2017/12/19/vi/)
    - [VAE 논문 리뷰] - Auto-Encoding Variational Bayes ****[https://kyujinpy.tistory.com/88](https://kyujinpy.tistory.com/88)
    - [X:AI] VAE 논문 이해하기 [https://rahites.tistory.com/106](https://rahites.tistory.com/106)
    - VAE 설명 (Variational autoencoder란? VAE ELVO 증명) [https://process-mining.tistory.com/161](https://process-mining.tistory.com/161)
</aside>

---

<aside>

<aside>

## **🤔 Next Task**

> ℹ️ *차주에 리뷰 예정인 논문 기재(APA. 인용 방식 권고)*
> 
</aside>

Goodfellow, I. J., Shlens, J., & Szegedy, C. (2015). Explaining and harnessing adversarial examples. *International Conference on Learning Representations (ICLR)*. [https://arxiv.org/abs/1412.6572](https://arxiv.org/abs/1412.6572)

- 적대적 학습의 기초 개념
- FGSM(Fast Gradient Sign Method)을 소개하며 모델이 어떻게 적대적 공격을 받을 수 있는지 설명
</aside>

---
