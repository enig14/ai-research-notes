# Decoder

## 1. 한 줄 정의

내부 representation과 지금까지 생성된 token을 이용하여 출력 sequence를 생성하는 구조.

---

## 2. 상위 개념

```
AI
└ Neural Network
   └ Transformer
```

---

## 3. 해결하려는 문제

모델 내부의 representation을 실제 출력 token sequence로 변환해야 한다.

---

## 4. 핵심 아이디어

현재까지의 context를 사용하여 다음 token을 하나씩 생성한다.

---

## 5. 동작

```
Encoder-Decoder:
source → Encoder → representation
                     ↑
target → Decoder → Cross-Attention

Decoder-only:
prompt + generated tokens
          ↓
   Causal Self-Attention
```

---

## 6. 핵심 수식


---

## 7. 예시

```text
나는 고양이를
↓
좋아한다
```

처럼 이전 token들을 바탕으로 다음 token을 생성한다.

---

## 8. 관련 개념

- Encoder
- Masked Self-Attention
- Cross-Attention
- LM Head
- Transformer

---

## 9. Notes

Decoder가 representation을 곧바로 자연어 문자열로 바꾸는 것은 아니다.

최종적으로 LM Head와 token selection을 거쳐 token ID를 생성한다.

---
