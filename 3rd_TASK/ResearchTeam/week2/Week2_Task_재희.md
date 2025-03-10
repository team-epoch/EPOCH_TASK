# Week2_Task_재희.md

---

<aside>

<aside>

## **📘 Title**

> ℹ️ *APA. 인용 방식 권고*
> 
</aside>

- Radford, A., Jong Wook, K., Ramesh, Aditya., Goh, Gabriel., Agarwal, Sandhini., Sastry, Girish., Askell, A., Mishkin, P., Clark, J., Krueger, G., & Sutskever, I. (2021). Learning transferable visual models from natural language supervision. Proceedings of the 38th International Conference on Machine Learning, PMLR 139, 8748–8763. Retrieved from https://proceedings.mlr.press/v139/radford21a.html

</aside>

---

<aside>

<aside>

## **📖 Abstract**

> ℹ️ *본인의 방식으로 재해석 해주세요. 그대로 가져오는 것은 금합니다.*
> 
</aside>

기존 SOTA computer vision 모델들은 사전에 정해진 레이블에 대해서만 예측할 수 있도록 학습이 진행됐다. 따라서 새로운 대상을 학습하려면 그에 대한 추가적인 라벨 데이터가 필요해 일반화가 어렵다는 문제가 있었다. 이를 해결하기 위해 이미지와 그에 대응하는 text를 짝지어(caption-image pair) 사전학습하였고 이를 바탕으로 OCR, 비디오 동작 감지, 객체 분류 등의 여러 후속 과제들에 대해 zero-shot prediction이 가능해졌다.

</aside>

---

<aside>

<aside>

## **💡 Introduction**

</aside>

**논문 선정 이유**

- 멀티모달 기술에 대한 관심이 많아 해당 기술의 기초격인 논문을 선정
- Dall-E나 Stable Diffution과 같은 유명한 text-to-image 기술의 기반 모델로도 사용되고 있음.

**논문 개괄**

자연어 처리 분야는 인터넷 상에서 다양한 텍스트를 추출하여 사전학습 시킨 GPT, BERT와 같은 모델이 고품질의 라벨링 된 대규모 데이터셋을 통한 학습보다 성능이 더 좋았음. 이와 같은 대규모 텍스트 데이터를 통한 사전학습의 방식을 Computer Vision 분야에 적용함으로써 일반화 및 확장성 문제를 해결하고자 연구 진행.

</aside>

---

<aside>

<aside>

## **📚 Background**

> ℹ️ *논문의 주제와 관련된 기존 연구들 및 배경 지식을 소개*
> 
</aside>

1. **NLP 분야에서 좋은 성능을 보인 웹 기반 대규모 raw data를 통한 pre-training 방식**
    - 별도의 fine-tuning 없이 다양한 작업에서 우수한 성능 보임(zero-shot transfer)
      
2. **컴퓨터 비전에서의 자연어 기반학습 시도**
    : 이미지와 이를 나타내는 텍스트를 짝지어 학습에 활용하는 방식
    - Natural Language Supervision 방식

      CNN을 기반으로 이미지 캡션의 단어를 예측(Houlin et al. 2016)
        - 이미지의 제목, 설명, 해시태그 메타데이터를 학습에 활용하여 이미지에서 해당 이미지를 나타내는 단어를 예측하도록 CNN모델을 학습.  
        - 이미지-텍스트 관계 학습을 통해 ImageNet 기반 사전학습과 유사한 성능 보임
          
      해당 방식으로 zero-shot 분류 가능성을 탐구(Li et al. 2017)했으나 ImageNet에서 11.5% 정확도로 제한적이었음
      

    - Weak Supervision을 활용한 방식
      - ImageNet 관련 해시태그를 예측하는 모델(Mahajan et al. 2018)
     
      - 노이즈 라벨이 포함된 대규모 데이터셋(Kolesnikov et al. 2019, Dosovitskiy et al. 2020)을 통해 학습한 모델
     
       정적인 softmax 분류기 사용으로 zero-shot 예측이 불가능
  
    - Natural Language Supervision 방식의 최신 연구들
      - VirTex(Desai & Johnson., 2020): transformer based language modeling
        
      - ICMLM(Bulent Sariyildiz et al., 2020): masked language modeling
        
      - ConVIRT(Zhang et al., 2020): contrastive learning
        
      해당 연구들을 통해 zero-shot 학습 성능 일부 향상. 하지만 이런 방식은 weak supervision 방식보다 현저히 적은 데이터셋을 바탕으로 진행됨.

  3. **연구 방향성**: Natural Language Supervision 방식 + Weak Supervision 방식
      - Weak Supervision과 같이 대규모 데이터셋을 통해 학습할 수 있도록 하기 위해 4억개의 이미지-텍스트 쌍으로 이루어진 데이터셋 구성.
     
      - 기본 모델은 Natural Language Supervision 방식의 ConVIRT를 사용하되 대규모 데이터셋도 효율적으로 학습할 수 있도록 더 간소화시킴.
      


</aside>

---

<aside>

<aside>

## **🔍 Methods**

</aside>

1. **Creating a Large Dataset**
- 기존 사용되던 데이터셋은 MS-COCO, Visual Genome, YFCC100M, MS-COCO, Visual Genome: 크기가 너무 작음 10만개의 사진 (이전 Mahajan et al. 2018의 모델은 35억개의 인스타그램 사진 활용)
- YFCC100M: 1억개 사진이지만 파일명이 자연어, 영어로만 된 것을 세면 600만~1500만개의 사진밖에 안됨.
  
- WIT(Web Image Text)라는 새로운 데이터셋을 만듦
  
    인터넷에서 4억개의 이미지, 텍스트 쌍을 추출하여 새로운 데이터셋 만듦
  
    많은 개념들을 학습하기 위해 50만개의 쿼리(검색어)를 활용해 이미지-텍스트 쌍을 찾음
  
    불균형 해소를 위해 쿼리(검색어)당 최대 2만개의 이미지-텍스트 쌍을 가지도록 함

2. **Natural Language Supervsion 방식**
   
- 인터넷 상에서 이미지마다 달려있는 자연어 문장들을 그대로 이미지에 대한 supervision으로 사용. 따라서 따로 labeling을 해줄 필요가 없다.

- 자연어를 통해 학습하면 표현을 배울 뿐만 아니라 그 표현을 언어와 연결지어 유연한 zero-shot transfer를 가능하게 한다. 



3. Pre-training Method
- Train: Contrastive pre-training
 
Contrastive learning: 서로 매칭되는 데이터는 가까워지도록, 매칭되지 않는 데이터는 멀어지도록 학습하는 방식. 이때 데이터가 가까워진다는 의미는 코사인 유사도가 증가한다는 것.
학습 과정

N개의 이미지-텍스트 쌍의 배치 데이터가 주어지면 해당 데이터가 각각 image, text encoder를 거쳐 벡터 형태로 변환. 즉 n개의 이미지에 대한 n개의 text, image 벡터 생성. 
해당 벡터들이 multi-modal embedding space에서 학습될 수 있도록 각각 linear projection을 시켜줌. 
 
서로 매칭되는 N개의 text-image 쌍의 코사인 유사도는 증가시키고 매칭되지 않는 N^2-N개의 text-image 쌍의 코사인 유사도는 감소시키는 방향으로 학습 진행. 손실함수는 Symmetric Cross Entropy loss를 사용. 

![image](https://github.com/user-attachments/assets/3979e591-dad3-4494-89cf-3daa420faaf6)Psudocode

logits이 이미지, 텍스트 벡터에 대한 코사인 유사도 matrix

matrix에서 대각선에 있는 부분이 label, 즉 정답

따라서 n개의 batch 데이터에서 순서대로 label의 인덱스 번호를 주고 loss 계산

- Test: setting the label to the text & zero-shot prediction
![image](https://github.com/user-attachments/assets/0a201250-2980-4604-9c9e-a1f02d587d53)

다양한 문장이 이미지와 매칭되도록 학습된 text encoder를 통해 zero-shot prediction이 가능해짐. Text encoder에서는 학습에 활용된 개념과 연관된 새로운 label이 주어지면 학습된 개념들을 토대로 새로운 label에 대한 벡터가 생성. Image encoder에서도 학습에 활용된 이미지를 토대로 그것과 유사한 벡터 생성. 이 text, image 벡터의 코사인 유사도를 증가시키도록 학습이 이루어지기 때문에 둘을 매칭 가능.  이때 text encoder는 문장을 통해 학습하였으므로 A photo of a {object}라고 문장 형식으로 넣어줌(prompt engineering). 

4. 사용 모델
Base architecture: ConVIRT(Zhang et al., 2020)

: CLIP은 전체적인 구조는 ConVIRT 형식을 따라가되 pre-training dataset이 ConVIRT 학습시킬 때 보다 훨씬 많기 때문에 모델 구조 자체는 간소화시키는 방향으로 설계되었다
- Image encoder와 Text encoder를 초기화시키는(initializing) 작업을 배제
- Image encoder와 Text encoder에서 나온 벡터를 multi-modal embedding space로 연결시킬 때 오직 linear projection만을 사용(non-linear는 배제)
- 텍스트 변환 함수인 t_u를 배제 (t_u는 여러 문장 후보군 중 하나를 무작위 추출하는 함수. CLIP에는 문장 후보군이 오직 하나이기 때문에 배제)
- 이미지 데이터 증강에는 random square crop만을 사용
- 소프트맥스 온도를 자동으로 최적화할 수 있도록 함: 소프트맥스 온도는 contrastive learning에서 중요한 하이퍼파라미터로 두 데이터 간의 유사도를 얼마나 강조할지 조절하는 역할을 함. 


② Image Encoder

  - ResNet-50을 활용한 구조

    He et al.(2019), Zhang(2019)에 나온 방식으로 구조 변형

    Global average pooling layer를 attention pooling mechanism으로 바꿈

  - Vision Transformer(VIT)를 활용한 구조

    합쳐지는 부분에 layer normalization 추가

    Transformer 들어가기 전에 position embedding 추가

    VIT와는 조금 다른 초기화 방식 사용


③ Text Encoder

  Transformer을 활용한 구조에서 Radford et al.(2019) 방식으로 변형

  크기: 63M(6300만) 개의 파라미터, 12개의 레이어, 512차원의 hidden size, 8개의 self-attention head

  49,152개의 단어로 Byte Pair Encoding(BPE) 진행

  최대 시퀀스 길이 76개 토큰

  입력 텍스트는 [SOS] (Start of Sentence)와 [EOS] (End of Sentence) 토큰으로 감싸짐

  입력 텍스트가 Transformer를 통과해 최상위 레이어에서 [EOS] 토큰의 출력 벡터를 추출해 문장을 대표하는 표현으로 사용. 이 벡터는 Layer Normalization을 거친 후 Image Encoder의 출력과 함게 멀티모달 임베딩 공간으로 선형변환됨. 

  기존 연구는 너비, 깊이, 해상도 중 하나의 요소만 확장하는 방식을 취했지만 CLIP은 Image Encoder에서 너비, 깊이 해상도를 균등하게 증가시킴. CLIP의 성능은 Text Encoder는 깊이에 크게 민감하지 않아 Text Encoder는 너비만 증가



</aside>

---

<aside>

<aside>

## **🔍 Experiments**

</aside>
①	Zero shot transfer 성능
 ![image](https://github.com/user-attachments/assets/b9106730-5acf-4c54-af8b-d6e8922c048f)


Dataset: 27개의 이미지 데이터셋
Baseline: Zero-shot CLIP, ResNet50

세부적인 표현 학습이 필요한 Fine Grained Classification 데이터셋에서는 성능이 안 좋고 일반적인 표현학습만으로 풀 수 있는 데이터셋에서는 좋은 성능을 보임
 
![image](https://github.com/user-attachments/assets/dfacc3de-2970-434d-b527-85a0e3be7d2c)

Dataset: 27개 이미지 데이터셋
Baseline: Zero-Shot CLIP, BIT-M, CimCLRv2, ResNet50

Zero shot에 linear layer를 추가하여 학습한 것으로 클래스당 학습 이미지를 N개 추가하여 학습한 실험으로 zero-shot은 4-shot과 성능이 동일하였고 클래스당 이미지 수가 증가할수록 성능은 더욱 증가


②	Representation Learning

![image](https://github.com/user-attachments/assets/dca487f2-96f1-4a4c-80f0-f537332c4cda)

모델별로 이미지에 대한 feature를 추출한 뒤 linear layer를 통해 CLIP의 이미지 표현 성능을 다른 모델과 비교

Dataset: Kornblith et al.(2019) 12개의 이미지 데이터셋(이미지 편향 최소화 시킨 데이터셋), 27개의 이미지 데이터셋

Baseline: CLIP-ViT, CLIP-ResNet, EfficientNet, EfficientNet-NoisyStudent, BiT-M, BiT-S, ViT(ImageNet-21k), Resnet, MoCo, BYOL, SimCLRv2, Instagram-pretrained
 
두 데이터 셋 모두에서 SOTA 성능을 보임



③	Robustness to natural distribution shift

![image](https://github.com/user-attachments/assets/3cd982a0-6537-4bf4-a5d4-edf609a53d04)


ImageNet의 데이터에 다양한 변형을 주어 robustness에 대한 실험을 진행한 결과 Zero-Shot CLIP 모델은 이미지의 변형에 큰 영향을 받지 않는 것을 확인. 


</aside>

---

<aside>

<aside>

## **📖 Conclusion**

</aside>

- 의의
  
  자연어와 이미지를 결합하여 해석함으로써 이미지를 더욱 풍부하고 정확하게 분석

  Zero shot 학습 방법의 가능성을 보여줌 -> 한번도 본 적이 없는 데이터도 분류가능

  CLIP의 이미지 임베딩을 피쳐로 사용시 SOTA 모델을 앞지르는 성능을 보임


- 한계
  
  Task-specific 모델과 비교해서 여러 유형의 세분화된 분류에서 현재 SOTA 모델보다 낮은 성능을 보임(자동차 종류 구분, 꽃의 품종 분류 등)

  사전훈련에 없는 데이터에서 성능이 낮음(ex. 사진에서 가장 가까운 자동차까지의 거리를 분류하는 task, MNIST 손글씨 분류 등)

  인터넷의 데이터를 따로 가공하지 않아 사회적 편견이 포함된 문장들도 그대로 학습


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

- Weak supervision이란?
![image](https://github.com/user-attachments/assets/91ce88b5-d8cc-421d-88b0-9215a30b3f57)


- Zero shot prediction이 무엇인가?
  학습하지 않은 문제를 맞출 수 있다는 것 Ex) 호랑이 이미지를 학습하지 않았더라도 예측 시 text encoder에 “a photo of a tiger”라는 문장을 넣어주면 맞출 수 있음 How? 사자, 표범, 치타와 같은 비슷한 고양이과 맹수에 대한 학습이 이루어졌을 때 호랑이를 학습하지 않았더라도 연관된 개념을 학습했기 때문에 이를 바탕으로 판단할 수 있음.


- ConVIRT
  
  Medical domain에서 다른 진단이지만 매우 유사해 보이는 이미지들을 구분해내기 위해 고안된 모델로 contrastive learning을 처음 제안한 모델이다. 

  Contrastive learning은 Image encoder와 Text encoder 각각을 통과해 나온 이미지, 텍스트 벡터가 짝이 맞는 부분은 유사도가 높아지도록 그 외의 부분은 유사도가 낮아지도록 학습하는 방법이다. 

  https://rubato-yeong.github.io/medical/convirt/ 참고


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
</aside>

해당 모델을 통해 text-to-image 모델을 만드려면 text가 입력데이터로 주어지고 text encoder를 거쳐 나온 벡터와 유사도가 높은 image 관련 벡터를 생성해내야 할 것으로 보이는데 이를 어떻게 만들 수 있을지에 대한 의문점이 생김

</aside>

---

<aside>

<aside>

## **🤔 Next Task**

> ℹ️ *차주에 리뷰 예정인 논문 기재(APA. 인용 방식 권고)*
> 
</aside>

- Ramesh, A., Pavlov, M., Goh, G., Gray, S., Voss, C., Radford, A., Chen, M., & Sutskever I. (2021). Zero-Shot Text-to-Image Generation. Proceedings of the 38th International Conference on Machine Learning, PMLR 139, 8821-8831 Retrieve from https://proceedings.mlr.press/v139/ramesh21a.html.

</aside>

---
