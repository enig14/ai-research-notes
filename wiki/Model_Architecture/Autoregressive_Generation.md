# Generation

## 1. 한 줄 정의

**Autoregressive Generation**은 현재까지의 token sequence를 조건으로 다음 token을 하나 생성하고, 그 token을 다시 입력에 추가하는 과정을 반복하여 sequence를 생성하는 방식

---

## 2. 상위 개념

### 개념적 계층

```text
Language Modeling
└─ Autoregressive Language Modeling
   ├─ Training
   │  └─ ...
   │
   └─ Inference
      └─ Autoregressive Generation
```

---

## 3. 해결하려는 문제

Language Model은 현재까지 주어진 sequence에 대해 다음 token의 분포를 예측할 수 있다. 
그러나 token 하나로는 답할 수 없다.

---

## 4. 핵심 아이디어

현재까지의 token을 $x_{\le t}$라고 하면 모델은 다음 token의 조건부 분포를 계산한다.

$$P(x_{t+1} \mid x_{\le t})$$

그 분포에서 다음 token을 선택한다.

Sampling을 사용하는 경우:

$$x_{t+1} \sim P(x_{t+1} \mid x_{\le t})$$

Greedy decoding을 사용하는 경우:

$$x_{t+1} = \arg\max_j z_j$$

선택된 $x_{t+1}$을 기존 sequence 뒤에 붙인 후 같은 과정을 반복한다.

---

## 5. 동작

예를 들어 prompt가:

```text
나는 오늘
```

이라고 하자.

첫 번째 generation step:

```text
나는 오늘
↓
Transformer
↓
Last Hidden Representation
↓
LM Head
↓
Logits
↓
학교에
```

생성 결과를 기존 sequence에 추가한다.

```text
나는 오늘 학교에
```

다시:

```text
나는 오늘 학교에
↓
Transformer
↓
LM Head
↓
Logits
↓
간다
```

결과:

```text
나는 오늘 학교에 간다
```

이 과정을 반복한다.

전체 흐름은:

```text
Prompt Tokens
↓
Transformer
↓
Last Hidden Representation
↓
LM Head
↓
Vocabulary Logits
↓
Token Selection
↓
Next Token
↓
기존 Sequence에 추가
↓
반복
```

---

## 6. 핵심 수식

현재 sequence:

$$x_1, x_2, \dots, x_t$$

다음 token의 분포:

$$P(x_{t+1} \mid x_1,\dots,x_t)$$

Autoregressive sequence 전체의 확률은:

$$P(x_1,\dots,x_N)=\prod_{t=1}^{N}P(x_t\mid x_{<t})$$

Greedy generation:

$$x_{t+1}=\arg\max_j z_j$$

Sampling generation:

$$x_{t+1}\sim Softmax(z)$$

---

## 7. 예시

초기 prompt:

```text
[BOS, 나는, 오늘]
```

Generation:

```text
[BOS, 나는, 오늘]
→ 학교에

[BOS, 나는, 오늘, 학교에]
→ 간다

[BOS, 나는, 오늘, 학교에, 간다]
→ EOS
```

최종 sequence:

```text
[BOS, 나는, 오늘, 학교에, 간다, EOS]
```

EOS가 생성되면 generation을 종료할 수 있다.

---

## 8. 관련 개념

- Autoregressive Language Modeling
- Next-token Prediction
- LM Head
- Logit
- Softmax
- Greedy Decoding
- Sampling
- Temperature
- Top-k
- Top-p
- EOS
- KV Cache

---

## 9. Notes

### Training과 Generation의 차이

Training에서는 정답 sequence가 이미 존재한다.

```text
Input :  [t0, t1, t2, t3]
Target:  [t1, t2, t3, t4]
```

따라서 causal mask를 사용하면서 sequence의 여러 위치에 대한 next-token prediction을 한 번에 계산할 수 있다.

반면 Generation에서는 다음 token을 알 수 없다.

```text
[t0, t1, t2]
↓
t3 생성

[t0, t1, t2, t3]
↓
t4 생성
```

즉 다음 step은 이전 step의 생성 결과에 의존한다.

따라서 **token 간 generation은 본질적으로 순차적**이다.

### KV Cache

매 generation step마다 이전 token들의 K/V를 모두 다시 계산하면 중복 계산이 발생한다.

그래서 이전 token의 K/V는 저장해두고 새 token의 K/V만 계산하여 추가한다.

```text
Previous KV Cache
+
New Token K/V
→ Updated KV Cache
```

### Softmax

Greedy decoding에서는:

$$\arg\max_i z_i = \arg\max_i Softmax(z)_i$$

이므로 softmax를 계산하지 않고 logits에서 바로 최대값을 선택할 수 있다.

반면 확률적 sampling에서는 token별 확률분포가 필요하므로 softmax가 사용된다.
