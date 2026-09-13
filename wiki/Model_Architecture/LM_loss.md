# LM loss 

## 1. 한 줄 정의

**LM Loss**는 각 위치에서 모델이 예측한 다음 token의 확률분포와 실제 정답 token을 비교하여 계산하는 손실 함수이다.

---

## 2. 상위 개념
```
Language Modeling
 └ Autoregressive Language Modeling
    └ Next-token Prediction
       └ LM Training Objective
          └ LM Loss
             └ Cross-Entropy Loss
```
---

## 3. 해결하려는 문제

Language Model은 이전 token들을 보고 다음 token을 잘 예측하도록 학습되어야 한다.

예를 들어:

```text
나는 학교에 간다 <EOS>
```

라는 sequence가 있다면 모델은 다음 관계를 학습해야 한다.

```text
나는            → 학교에
나는 학교에      → 간다
나는 학교에 간다 → <EOS>
```

즉 각 위치에서:

> 지금까지의 token이 주어졌을 때 실제 다음 token의 확률을 높이는 것

이 LM 학습의 목적이다.

---

## 4. 핵심 아이디어

입력 sequence와 target sequence를 한 token씩 shift해서 만든다.

```text
Input :  [t0, t1, t2, t3]
Target:  [t1, t2, t3, t4]
```

Transformer는 causal mask를 사용하기 때문에 각 위치에서는 미래 token을 볼 수 없다.

따라서 한 번의 forward pass에서도 각 위치가 독립적으로 다음 token prediction을 수행할 수 있다.

```text
t0          → t1 예측
t0 t1       → t2 예측
t0 t1 t2    → t3 예측
t0 t1 t2 t3 → t4 예측
```

이 예측들을 실제 target과 비교해 loss를 계산한다.

---

## 5. 동작

각 token 위치의 hidden representation을 LM Head에 통과시키면 vocabulary 전체에 대한 logits가 나온다.

```text
Hidden Representation
        ↓
      LM Head
        ↓
Vocabulary Logits
```

각 위치에서 정답 token이 하나 존재한다.

예를 들어:

```text
Logits:
나는     1.2
학교     3.1
간다     5.4
먹는다   0.7

Target:
간다
```

라면 `간다`에 높은 확률이 부여될수록 loss가 작아진다.

학습에서는 sequence의 모든 유효한 위치에 대해 이 loss를 계산하고 평균 또는 합을 구한다.

---

## 6. 핵심 수식

보통:

$$L = -\frac{1}{N}\sum_{i=1}^{N}\log P(y_i \mid x_{\le i})$$

를 사용한다.

즉 LM Loss는 본질적으로 **cross entropy loss**이다.

---

## 7. 예시

Sequence가:

```text
[BOS, 나는, 학교에, 간다, EOS]
```

라면 학습에는 다음과 같이 사용된다.

```text
Input:
[BOS, 나는, 학교에, 간다]

Target:
[나는, 학교에, 간다, EOS]
```

각 위치에서 모델은:

```text
BOS             → 나는
BOS 나는         → 학교에
BOS 나는 학교에   → 간다
BOS 나는 학교에 간다 → EOS
```

를 예측한다.

각 위치의 prediction에 대해 cross entropy를 계산하고 이들을 평균하여 LM Loss를 얻는다.

---

## 8. 관련 개념

- LM Head
- Logit
- Softmax
- Cross Entropy
- Next-token Prediction
- Causal Mask
- Autoregressive Model

---

## 9. Notes

### Causal Mask와 LM Loss

LM Loss 자체가 미래 token을 차단하는 것은 아니다.

미래 token을 볼 수 없도록 만드는 것은 **causal mask**이고, LM Loss는 그 상태에서 나온 prediction이 실제 다음 token과 얼마나 다른지를 평가한다.

즉:

```text
Causal Mask
→ 미래 정보 차단

Input/Target Shift
→ 무엇을 예측할지 정의

LM Loss
→ 예측이 얼마나 틀렸는지 측정
```

### 학습에서는 Softmax가 필요하다.

Greedy inference에서는 가장 큰 logit만 선택하면 되므로 softmax를 생략할 수 있다.

하지만 LM 학습에서는 정답 token의 상대적인 확률을 기반으로 loss를 계산하므로 softmax 또는 그와 동등한 cross-entropy 계산이 필요하다.

### Padding

Batch에서 sequence 길이가 서로 다르면 padding token이 들어갈 수 있다.

Padding 위치는 실제 학습 대상이 아니므로 일반적으로 loss 계산에서 제외한다.