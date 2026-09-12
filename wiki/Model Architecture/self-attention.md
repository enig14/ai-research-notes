# Self-Attention

## 1. 한 줄 정의

같은 sequence 내부의 representation들이 서로를 참고하도록 하는 Attention.

## 2. 상위 개념
```
AI
 └ LM
    └ Attention
```
## 3. 해결하려는 문제

개별 token embedding만으로는 token의 의미가 주변 문맥에 따라 달라지는 것을 표현할 수 없다.

## 4. 핵심 아이디어

각 token이 같은 sequence의 다른 token들을 참고하여 자신의 representation을 갱신한다.

## 5. 동작

```text
Token Representation
↓
같은 sequence의 다른 token들과 관계 계산
↓
Attention Weight
↓
다른 token들의 정보 반영
↓
Contextualized Representation
```

## 6. 핵심 수식

$$
\mathrm{SelfAttention}(X)
=
\mathrm{softmax}\left(
\frac{QK^T}{\sqrt{d_k}}
\right)V
$$

여기서 Q, K, V 모두 같은 input sequence \(X\)에서 만들어진다.

## 7. 예시

```text
I deposited money at the bank
```

에서 `bank`가 `deposited`, `money` 등을 참고하여 금융기관이라는 문맥적 의미를 갖게 된다.

## 8. 관련 개념

- Attention
- Encoder
- Transformer
- FFN
- Masked Self-Attention

## 9. Notes

Embedding:

> 이 token은 무엇인가?

Self-Attention을 거친 representation:

> 이 문맥에서 이 token은 무엇을 의미하는가?