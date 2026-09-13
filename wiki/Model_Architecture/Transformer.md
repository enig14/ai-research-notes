# Transformer

## 1. 한 줄 정의

Self-Attention과 FFN을 핵심 연산으로 사용하여 sequence의 representation을 반복적으로 갱신하는 neural network 구조.

---

## 2. 상위 개념

```
AI
 └ Neural Network
```

---

## 3. 해결하려는 문제

Sequence 안의 token들 사이의 관계를 효율적으로 반영하여 문맥적인 representation을 만들 필요가 있다.

```
RNN 계열의 문제점
 - 순차처리에 의한 병목
 - 정보 손실
```

---

## 4. 핵심 아이디어

```text
Self-Attention → FFN
```

으로 이루어진 block을 여러번 반복하여 representation을 계속 갱신한다.

---

## 5. 동작


```text
Token Representation
↓
Self-Attention
↓
FFN
↓
Updated Representation
↓
...
```

---

## 6. 핵심 수식


---

## 7. 예시


---

## 8. 관련 개념

- Encoder
- Decoder
- Self-Attention
- Attention
- FFN
- RoPE
- RNN

---

## 9. Notes

Transformer와 Encoder/Decoder는 같은 계층의 개념이 아니다.

Transformer는 architecture이고, Transformer를 Encoder와 Decoder 또는 Decoder-only로(GPT 계열) 구성할 수 있다.

---
