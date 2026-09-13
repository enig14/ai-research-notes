# Token Selection

## 1. 한 줄 정의

**Token Selection**은 Language Model이 출력한 logits를 바탕으로 실제 다음 token 하나를 결정하는 과정이다.

---

## 2. 상위 개념


```text
Language Modeling
└─ Autoregressive Language Modeling
   └─ Inference
      └─ Autoregressive Generation
```

---

## 3. 해결하려는 문제

LM Head는 다음 token 하나를 직접 출력하지 않는다.

대신 vocabulary 전체에 대한 logits를 출력한다.

Generation을 계속하려면 이 logits로부터 실제 다음 token이 될 **1 개를 결정**해야 한다.

---

## 4. 핵심 아이디어

### Greedy Decoding

가장 높은 logit을 가진 token을 선택한다.

$$x_{t+1} = \arg\max_i z_i$$

결과가 결정적이며 softmax가 필요하지 않다.

### Sampling

Logits를 확률분포로 변환한 뒤 그 분포에 따라 token을 확률적으로 선택한다.

$$P_i = Softmax(z)_i$$

$$x_{t+1} \sim P$$

Sampling에서는 필요에 따라 먼저 Temperature, Top-k, Top-p 등을 적용해 분포나 후보 집합을 조정한다.

---

## 5. 동작

### Greedy

```text
Logits
↓
Argmax
↓
Next Token
```

예를 들어:

```text
A : 5.1
B : 4.8
C : 3.2
```

라면 항상 `A`가 선택된다.

---

### Sampling

일반적인 개념 흐름은:

```text
Logits
↓
Temperature
↓
Top-k / Top-p Filtering
↓
Softmax
↓
Random Sampling
↓
Next Token
```

연산 순서가 일부 달라지거나 없을 수 있다.

---

## 6. 핵심 수식

### Greedy Decoding

가장 큰 logit 선택:

$$x_{t+1} = \arg\max_i z_i$$

Softmax는 순서를 바꾸지 않으므로:

$$\arg\max_i z_i = \arg\max_i Softmax(z)_i$$

따라서 Greedy에서는 softmax를 생략할 수 있다.

---

### Temperature

Logits의 상대적 차이를 조절한다.

$$P_i = \frac{\exp(z_i/T)}{\sum_j \exp(z_j/T)}$$

- $T < 1$: 분포가 더 뾰족해짐
- $T = 1$: 원래 분포
- $T > 1$: 분포가 더 평평해짐

즉 Temperature는 **어떤 token을 제거하는 것이 아니라 확률분포의 날카로움을 조절한다.**

---

### Top-p

확률이 높은 순서대로 token을 정렬하고, 누적 확률이 $p$ 이상이 되는 최소 후보 집합을 사용한다.

$$\sum_{i \in S} P_i \ge p$$

---

## 7. 예시

### top-k, top-p 비교
```text
Top-k
→ 후보 개수를 고정

Top-p
→ 후보의 누적 probability mass를 기준으로 결정
```

p=0.9 k=3 인 경우를 비교하면:

아래와 같은 분포에서

```text
A : 0.92
B : 0.03
C : 0.02
...
```

`top-p = 0.9`이면  A 하나만 남는다.
`top-k = 3`이면  A,B,C 가 남는다.

아래와 같은 분포에서

```text
A : 0.20
B : 0.18
C : 0.16
D : 0.14
...
```

`top-p = 0.9`이면  A,B,C,D, ... 가 남는다.
`top-k = 3`이면  A,B,C 가 남는다.

따라서 Top-p는 모델의 출력 분포에 따라 후보 집합의 크기가 동적으로 변한다.

---

## 8. 관련 개념

- Autoregressive Generation
- LM Head
- Logit
- Softmax
- Greedy Decoding
- Sampling
- Temperature
- Top-k
- Top-p
- EOS

---

## 9. Notes

### 모델의 불확실성과 Sampling

모델이 가진 불확실성 자체는 logits의 상대적 크기에 이미 반영되어 있다.

Temperature, Top-k, Top-p는 새로운 불확실성을 만드는 것이 아니라:

> **모델이 가진 분포를 generation 결과에 어떤 방식으로 반영할지를 조절한다.**

---

### Softmax가 필요한 경우

```text
Greedy
Logits
→ Argmax
```

에서는 softmax가 필요하지 않다.

반면:

```text
Sampling
Logits
→ Probability Distribution
→ Random Sampling
```

에서는 각 token의 상대적인 선택 확률이 필요하므로 softmax 또는 그와 동등한 확률 계산이 필요하다.

---

### 역할 요약

| 방법 | 역할 |
|---|---|
| Greedy | 가장 큰 logit의 token 선택 |
| Temperature | 분포의 날카로움 조절 |
| Top-k | 상위 k개로 후보 제한 |
| Top-p | 누적 확률 기준으로 후보 제한 |
| Sampling | 최종 후보에서 실제 token 선택 |