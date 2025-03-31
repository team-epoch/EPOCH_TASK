# Week5_한의진

<aside>

<aside>

## **📘 Title**

> ℹ️ *APA. 인용 방식 권고*
> 
</aside>

- SwinIR: Image Restoration Using Swin Transformer

</aside>

---

<aside>

<aside>

## **📖 Abstract**

> ℹ️ *본인의 방식으로 재해석 해주세요. 그대로 가져오는 것은 금합니다.*
> 
</aside>

- SwinIR은 Swin Transformer 기반의 이미지 복원 모델로, **얕은 특징 추출, 깊은 특징 추출, 고품질 이미지 복원**의 세 부분으로 구성. 여러 개의 Swin Transformer Block(RSTB)를 포함
- SwinIR은 **이미지 초해상도, 이미지 노이즈 제거, JPEG Compression Artifact Reduce** 등의 이미지 복원 작업에서 실험, 매개변수를 67%까지 감소시킬 수 있다.

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

- 이미지 해상도 증강 연구 중에 관심을 가지게 된 논문. 현재의 ESRGAN 방법보다 더욱 진보된 방법이라 관심이 생겼음.
- 기존 CNN 기반 방법들의 한계점을 개선하기 위한 연구
    - **컨볼루션 커널의 내용 독립성**: CNN은 동일한 컨볼루션 커널을 모든 이미지 영역에 적용하므로, 각 영역의 특성에 맞춰 복원하는 것이 어려움.
    - **장거리 의존성 부족**: CNN은 로컬 필터를 사용하여 국소적인 특징만 처리할 수 있어, 장거리 정보를 효과적으로 모델링하지 못함.
- CNN 기반 방법의 단점과 기존 Transformer 접근법의 단점(패치 분할로 인한 경계 문제, 높은 계산 비용)을 해결

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

- SRCNN, ESRGAN, DnCNN
- 자연에서 얻어진 영상, 물체, 자동차 자율주행 인식, 의료영상, 바이오현미경 영상 복원 등 다방면 활용 가능

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
1. **Shallow Feature Extraction**
    - 입력 저품질 이미지에서 **3×3 컨볼루션 레이어**를 사용해 얕은 특징 추출
    - 초기 컨볼루션은 네트워크 최적화를 안정시키고 입력 이미지를 더 높은 차원의 특징 공간으로 변환하는 역할을 함.
2. **깊은 특징 추출 (Deep Feature Extraction)**
    - 얕은 특징을 입력으로 받아Residual Swin Transformer Block (RSTB)을 통과하며 깊은 특징 을 추출.
    - 각 RSTB는 여러 개의 Swin Transformer Layer (STL)과 컨볼루션 레이어로 구성.
    - 마지막에는 **3×3 컨볼루션 레이어**를 사용하여 특징을 정리하고 다음 단계로 전달.
    - 컨볼루션 레이어를 추가함으로써 Swin Transformer의 공간적 변동성을 보완하고, 잔차 연결(residual connection)을 통해 깊은 특징을 안정적으로 전달.
3. **고품질 이미지 복원 (HQ Image Reconstruction)**
    - 얕은 특징을 더한 후, 복원 모듈을 통해 고품질 이미지 생성.
    - **업샘플링이 필요한 경우** (예: 초해상도) → **서브픽셀 컨볼루션(sub-pixel convolution)** 사용.
    - **업샘플링이 필요 없는 경우** (예: 이미지 노이즈 제거, JPEG 압축 복원) → **단일 컨볼루션 레이어** 사용.
    - 모델은 **잔차 학습(residual learning)** 방식을 사용한 후 복원된 차이값(residual)을 추가하는 방식으로 최종 결과를 생성.

### **Residual Swin Transformer Block (RSTB) 구조**

- 각 RSTB는 **여러 개의 Swin Transformer Layer (STL), 컨볼루션 레이어, 잔차 연결**로 구성.
- 특징을 Swin Transformer Layer를 통해 변환한 후, 마지막 컨볼루션 레이어를 거쳐 잔차 연결을 통해 원본 입력과 합산.
- **장점**:
    1. 지역적 특징과 장거리 모델링을 동시에 수행.
    2. 훈련 안정성과 다중 계층 특징의 효과적인 활용 가능

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

![Screenshot 2025-03-31 at 22.42.15.png](Week5_%E1%84%92%E1%85%A1%E1%86%AB%E1%84%8B%E1%85%B4%E1%84%8C%E1%85%B5%E1%86%AB%201c7138dcada680bbacf2ea43ca20658a/Screenshot_2025-03-31_at_22.42.15.png)

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

- SwinIR 모델에서 Swin Transformer의 RSTB를 응용하여 지역성과 모델의 장거리 Reliability를 모두 해결한 것이 이 연구의 가장 큰 의의

</aside>

---

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

- Transformer 및 Diffusion 모델이 현재 학계 트렌드인데, 이것보다 획기적으로 성능을 개선할 수 있는 방법 없을까
- Transformer 기반 모델 중에서 현재 2025년에 학계에 발표된 방법을 응용하면 성능이 향상될까?

</aside>

---

<aside>

<aside>

## **🤔 Next Task**

> ℹ️ *차주에 리뷰 예정인 논문 기재(APA. 인용 방식 권고)*
> 
</aside>

- [Multi-focal Conditioned Latent Diffusion for Person Image Synthesis](https://github.com/jqliu09/mcld)

</aside>

---