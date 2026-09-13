# LM Head

## 1. 한 줄 정의

Transformer의 출력물인 contextual representation을 vocabulary 전체 token에 대한 점수(logit)로 변환하는 출력 layer.

---

## 2. 상위 개념

```
AI
└ LM
```

---

## 3. 해결하려는 문제

Transformer의 마지막 출력은 vector representation이므로 이를 실제 다음 token 후보들과 연결해야 한다.

---

## 4. 핵심 아이디어

Hidden representation을 vocabulary 크기의 vector로 선형 변환한다.

---

## 5. 동작

```text
    Contextual Representation
               ↓
            LM Head
               ↓
        Vocabulary Logits
```

---

## 6. 핵심 수식

마지막 hidden representation이

$$h \in \mathbb{R}^{d}$$

LM head를 W로 표현할 때 vocabulary 크기가 \(V\)라면:

$$z = W h $$ 
$$z \in \mathbb{R}^{V}$$

각 \(z_i\)가 vocabulary의 i번째 token에 대한 logit이다.

---

## 7. 예시


---

## 8. 관련 개념

- Decoder
- Vocabulary
- Logit
- Token
- Softmax

---

## 9. Notes

Contextual representation을 다시 embedding으로 바꾸는 것이 아니다.

`representation → vocabulary logits → token 선택`

순서로 출력 token이 결정된다.

---
