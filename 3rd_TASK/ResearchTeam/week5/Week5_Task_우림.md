# Week5_Task_우림

---

<aside>

<aside>

## **📘 Title**

> ℹ️ *APA. 인용 방식 권고*
> 
</aside>

- Goodfellow, I. J., Shlens, J., & Szegedy, C. (2015). Explaining and harnessing adversarial examples. *International Conference on Learning Representations (ICLR)*. [https://arxiv.org/abs/1412.6572](https://arxiv.org/abs/1412.6572)
</aside>

---

<aside>

<aside>

## **📖 Abstract**

> ℹ️ *본인의 방식으로 재해석 해주세요. 그대로 가져오는 것은 금합니다.*
> 
</aside>

- 적대적 공격(Adversarial attack)에 대한 베이직한 논문으로, Fast gradient sign method(FSGM)를 제안함
- 과거 적대적 공격에 대한 설명은 비선형성(nonlinearity)과 과적합(overfitting)에 중점을 두었음
- 본 논문의 저자는 신경망의 **선형성(linearity)**에 집중하여 설명하고자 함
    - GAN과 동일한 저자이고, 둘 다 Adversarial이라는 단어가 들어가지만 전혀 다른 기술
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

- DL이 성장함에 따라 DNN의 보안성이나 취약성에 대한 연구도 진행되고 있어 이에 대한 문제를 공부해 보고자 선택

**✅ 논문에서 다루고 있는 주요 연구 문제나 질문**

- adversarial examples의 원인은 미스테리
    - 비선형성과 불충분한 모델 정규화 때문이라는 추측이 있었음
- 고차원 스페이스에서 선형성이 adversarial examples을 야기하는 것이라고 반박
    - 이 관점은 a fast method of generating adversarial examples을 디자인하도록 함
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

**📍 Adversarial attack 적대적 공격**

- input 데이터에 미세한 perturbation(noise)을 가해서 모델이 잘못된 output을 내보내도록 만드는 것을 의미
- e.g., 판다의 사진(input)에 매우 작은 epsilon 값의 노이즈를 추가하면 gibbon(긴팔원숭이; output)를 출력하는 것
- Adversarial example: noise가 추가된 출력 이미지, 사람의 눈으로 보기에는 인풋과 동일한 이미지여야 함(구분 불가능)

![image](https://github.com/user-attachments/assets/259ffaee-df75-4887-954d-843b306dd29d)

**📍 선형적 설명**

- 일반적으로 이미지에 사용되는 RGB 채널의 경우 R, G, B 각각에 대해서 8비트 범위 내 256개의 값(0~255)으로 픽셀을 표현
    - 매우 작은 색상의 변화에 대한 차이를 표현하는데 한계
- e.g., 시스템 상에서는 노이즈의 크기가 만약 0.3 픽셀 정도의 차이이고 기존의 값이 100이었다면, 100.3이 아닌 100으로 표현되는데 모델은 고차원 공간 상에서 이러한 미세한 차이가 축적된다면 큰 변화를 이끌어 낼 수 있음

![image](https://github.com/user-attachments/assets/03a8f3bb-cbbc-4e1a-b78c-a76909323f74)

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

**✅ Fast gradient sign method (FGSM)**

- 본 논문에서는 NN이 linear adversarial perturbation을 피하기에는 너무 선형적이라고 가정
- LSTM과 ReLU, maxout 네트워크는 최적화를 쉽게 하기 위해서 매우 선형적으로 동작하도록 만들어짐
- Sigmoid와 같은 비선형 모델들은 같은 이유로 비포화적이고 더 선형적인 공간에서 대부분의 시간을 쓰도록 주의깊게 조정됨
- FGSM: 최적화 문제를 단 한 번의 gradient 계산으로 근사적으로 해결하는 방법
    - 손실 함수에서 입력 x에 대한 그래디언트 계산
    - 손실을 증가시키는 방향으로 x 변형
    - x'은 원본 x에 작은 변화만 추가한 적대적 examples이 됨
- 입력 데이터 $x$ 에 대해, 모델의 손실 함수에 대한 기울기를 계산
- 손실을 최대화하는 방향으로 작은 perturbation $\eta$를 추가하여 adversarial examples 생성

$$
w^\intercal \tilde{x}=w^\intercal x+w^\intercal \eta
$$

- $w$ : weight vector
- $\tilde{x}$ : adversarial example
- $\tilde{x}=x+\eta$ 에서 $\eta$가 매우 작을 때 분류기는 $x$와 $\tilde{x}$를 같은 클래스로 구분

$$
\eta=\epsilon sign(\nabla_xJ(\theta,x,y))
$$

- 이 식을 fast gradient sign method라고 부름
    - gradient는 backpropagation으로 계산
- $\epsilon$ : 작은 크기의 perturbation, 신경망이 오분류하도록 유도
- adversarial perturbation: $w^\intercal\eta$만큼 활성화의 증가를 야기
- max norm constraint에 따라 $\eta =sign(w)$ 으로 이 증가를 최대화할 수 있음
- **Max norm(Infinity-norm)**
    - 절댓값이 가장 큰 원소를 선택
    
    ![image](https://github.com/user-attachments/assets/7bec8758-15de-42fb-966f-dd9af7bf77cc)

    

**✅ FGSM에 근거한 정규화에 효율적인 objective function**

- Adversarial examples와 classification task에 대한 학습을 동시에 진행해야 하므로 clean images와 perturbed images를 섞어서 훈련 진행

$$
\tilde J(\theta, x, y)=\alpha J(\theta,x,y)+(1-\alpha)J(\theta,x+\epsilon sign(\nabla_xJ(\theta,x,y))
$$

- $\alpha$ = 0.5로 설정하여 실험
- Augmentation vs. Adversarial examples
    - Augmentation: 테스트셋에서 나타날 만한 변화(ex. translation, change brightness)를 만들어 주는 것
    - Adversarial examples: 자연적으로 발생하기 어렵지만 모델의 의사결정 능력을 망가뜨리는 input을 사용
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
- CIFAR-10

**✅ 평가지표**

- 정확도 (Accuracy)

![image](https://github.com/user-attachments/assets/77499a8a-dbf0-4590-82e4-86abfa2c6106)

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

- 단순한 1-step 공격 방식으로, 더 강력한 iterative 공격에는 취약할 수 있음
- Adversarial training이 일부 공격에 대해 효과적이지만 일반적인 방어법으로 완벽하지 않음

**✅ Contribution**

- 적대적 훈련(Adversarial Training)이 드롭아웃보다 더 효과적으로 정규화를 할 수 있음을 보임
- 신경망의 적대적 examples의 취약성을 체계적으로 분석하고, 그 원인을 선형성에서 찾음
- FGSM을 통해 적대적 예제를 생성하는 방법을 제시
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

- 적대적 공격과 적대적 예제가 무엇인지 알게 됨
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

- 본 논문은 Adversarial attack(적대적 공격)의 베이직한 논문으로, 향후 ML, DL의 보안 및 취약성에 대한 연구를 진행함에 있어서 도움이 될 것 같음
    - adversarial attack task에서 많이 인용, 응용되는 듯
- 지금까지 보안이나 취약성에 대한 고민은 해 보지 않았는데 새로운 걸 공부할 수 있었고, 수식이 생각보다 많았다…
- 참고 자료
    - [https://psleon.tistory.com/244](https://psleon.tistory.com/244)
    - [https://leedakyeong.tistory.com/entry/논문-FGSM-리뷰-EXPLAINING-AND-HARNESSING-ADVERSARIAL-EXAMPLES](https://leedakyeong.tistory.com/entry/%EB%85%BC%EB%AC%B8-FGSM-%EB%A6%AC%EB%B7%B0-EXPLAINING-AND-HARNESSING-ADVERSARIAL-EXAMPLES)
    - https://aistudy9314.tistory.com/37
</aside>

---

<aside>

<aside>

## **🤔 Next Task**

> ℹ️ *차주에 리뷰 예정인 논문 기재(APA. 인용 방식 권고)*
> 
</aside>

Build-up session 마무리!

</aside>
