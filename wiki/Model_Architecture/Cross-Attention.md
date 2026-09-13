# Cross-Attention

## 1. 한 줄 정의

Decoder가 Encoder가 만든 representation들을 참고하는 Attention.

---

## 2. 상위 개념

```
AI
 └ Neural Network
    └ Attention
```

---

## 3. 해결하려는 문제

Decoder가 출력을 생성할 때 입력 sequence를 참조하게 하고 싶다.

---

## 4. 핵심 아이디어

Decoder의 현재 representation을 기준으로 Encoder representation들의 중요도(가중치)를 계산한다.

---

## 5. 동작

```text
Decoder State
↓
Encoder Representations와 Attention
↓
필요한 입력 정보 반영
↓
Decoder Representation 갱신
```

---

## 6. 핵심 수식


---

## 7. 예시

번역에서 Decoder가 `좋아한다`를 생성할 때 입력 문장의 `love`에 강하게 attention할 수 있다.

---

## 8. 관련 개념

- Attention
- Encoder
- Decoder
- Transformer

---

## 9. Notes

Self-Attention:

> 같은 sequence를 본다.

Cross-Attention:

> 다른 sequence의 representation을 본다.

---
