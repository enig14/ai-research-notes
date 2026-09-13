# Masked Self-Attention

## 1. 한 줄 정의

현재 token이 미래의 token을 볼 수 없도록 제한한 Self-Attention

---

## 2. 상위 개념

```
AI
 └ LM
    └ Attention
       └ Self-Attention
```

---

## 3. 해결하려는 문제

생성형 모델에서는 Decoder가 다음 token을 예측한다. 그런데 Transformer 생성형 모델을 가정하고 만들어지지 않았기 때문에, Decoder는 token생성에 모든 다른 token을 참조할 수 있다. 그러나 만들어지지 않은 다음 token을 지금 token 생성에 사용하면 안된다. 

---

## 4. 핵심 아이디어

현재 위치보다 뒤에 있는 token에 Attention하지 못하도록 mask를 적용한다.

---

## 5. 동작

```text
현재 token
→ 과거 / 현재 token만 Attention
→ 미래 token은 차단
```

---

## 6. 핵심 수식

$$\mathrm{MaskedAttention}(Q,K,V)=\mathrm{softmax}\left(\frac{QK^T}{\sqrt{d_k}}+M\right)V$$

Causal Mask의 경우

$$M_{ij}=\begin{cases}0 & j\le i \\ -\infty & j>i\end{cases}$$

---

## 7. 예시

```text
나는 / 고양이를 / 좋아한다
```

`고양이를`까지 처리할 때 뒤의 `좋아한다`를 볼 수 없다.

---

## 8. 관련 개념

- Decoder
- Self-Attention
- Autoregressive Generation

---

## 9. Notes

Masked Self-Attention은 Mask가 존재하는 경우를 말할 뿐인다. 따라서 반드시 Causal Mask를 말하는 것은 아니지만, 보통은 (causal) Masked Self-Attention을 말한다.

---

## 10. 의문점

생성이 완료된 문장을 입력으로 하여 그에 맞게 학습된 mask가 없는 Transformer를 추가로 사용한다면, 더 좋은 결과를 얻지 않을까? 

---
