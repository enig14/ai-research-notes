## 1. 한 줄 정의

Transformer는 순서 정보를 자체적으로 알 수 없기 때문에, 각 token의 위치 또는 token 간 상대적 거리를 Attention 연산에 반영하기 위한 별도의 positional information을 반영하는 과정

---

## 2. 상위 개념

```
AI
└ Neural Network
   └ Transformer
      └ Attention
```

---

## 3. 해결하려는 문제

Self-Attention은 기본적으로 token의 위치를 구분하지 않는다.

예를 들어 positional information이 없다면

```text
I love you
you love I
```

처럼 token의 순서가 바뀌어도 동일한 token 집합으로 취급될 수 있다.

따라서 Transformer에는 다음 정보가 필요하다.

```text
이 token은 어디에 있는가?
```

---

## 4. 핵심 아이디어

### 1. Absolute Positional Encoding

각 token embedding에 자신의 절대 위치 정보를 더한다.

```text
Token Embedding
      +
Position Encoding
      ↓
Transformer
```

대표적인 방식:

- Sinusoidal Positional Encoding
- Learned Position Embedding

### 2. Relative Positional Encoding

절대적인 위치보다 두 token 사이의 상대적 거리를 Attention에 반영한다.

```text
position 100
position 103
```

보다

```text
distance = +3
```

이라는 관계를 사용하는 방식이다.

### 3. RoPE

위치 정보를 embedding에 더하는 대신 Query와 Key를 위치에 따라 회전시킨다.

```text
Q_i → R(i)Q_i
K_j → R(j)K_j
```

그 결과 Q와 K의 dot product에서 상대적 위치 `j-i`가 자연스럽게 반영된다.

현대 LLM에서 널리 사용되는 방식이다.

### 4. RoPE Scaling

RoPE를 학습 때보다 훨씬 긴 sequence에서도 사용할 수 있도록 position 또는 frequency를 조절한다.

- Position Interpolation
- NTK-aware Scaling
- Dynamic NTK
- YaRN

---

## 5. 동작

### Absolute Positional Encoding

```text
Natural Language
      ↓
Tokenizer
      ↓
Token
      ↓
Embedding
      ↓
Token Embedding + Positional Encoding
      ↓
Transformer Layer
```

위치 정보가 Transformer에 들어가기 전에 representation에 포함된다.

### Relative Positional Encoding

```text
Q_i
K_j
 ↓
Content Similarity
+
Relative Position(i-j)
 ↓
Attention Score
```

위치 정보를 token 자체보다 Attention 관계에 직접 반영한다.

### RoPE

```text
Hidden State
    ↓
Q, K 생성
    ↓
position i에 따라 Q 회전
position j에 따라 K 회전
    ↓
Q_i' · K_j'
    ↓
relative position이 포함된
Attention Score
```

RoPE에서는 positional information이 별도의 hidden state로 계속 전달되는 것이 아니라 각 Transformer layer의 Attention에서 Q/K에 다시 적용된다.

---

## 6. 핵심 수식

### Sinusoidal Positional Encoding

$$PE(pos,2k)=\sin\left(\frac{pos}{10000^{2k/d}}\right)$$

$$PE(pos,2k+1)=\cos\left(\frac{pos}{10000^{2k/d}}\right)$$

입력 representation은

$$x_i=e_i+p_i$$

이다.

### Relative Position Bias

기본 Attention score:

$$s_{ij}=\frac{Q_iK_j^T}{\sqrt{d_k}}$$

Relative position을 추가하면:

$$s_{ij}=\frac{Q_iK_j^T}{\sqrt{d_k}}+b_{i-j}$$

### RoPE

위치 \(i\), \(j\)에서

$$Q_i'=R(i)Q_i$$

$$K_j'=R(j)K_j$$

Attention score는

$$Q_i'^T K_j'$$

이며 회전행렬의 성질 때문에 상대적 위치

$$j-i$$

에 의존하는 형태가 된다.

---

## 7. 예시

문장이 다음과 같다고 하자.

```text
The cat ate the fish
```

token 위치:

```text
The   → 0
cat   → 1
ate   → 2
the   → 3
fish  → 4
```

### Absolute Position

`cat`은

```text
Token Embedding(cat)
+
Position Encoding(1)
```

로 표현된다.

### Relative Position

`ate`가 `cat`을 Attention한다고 하면

```text
2 - 1 = 1
```

이라는 상대 위치가 Attention에 반영된다.

### RoPE

```text
Q_ate  → position 2만큼 회전
K_cat  → position 1만큼 회전
```

두 벡터의 dot product에서는 두 위치의 차이

```text
2 - 1 = 1
```

가 자연스럽게 반영된다.

---

## 8. 관련 개념

- Attention
- Self-Attention
- Query / Key
- KV Cache

---

## 9. Notes
회전은 주기성을 가지므로 RoPE를 사용할 때는 여러 주파수를 사용하여, aliasing 될 확률을 줄인다. 그럼에도 불구하고 긴 context에서는 문제가 될 수있고, 이를 해결하기 위해 RoPE sacling 기법을 사용할 수 있다.

---
