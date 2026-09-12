# Encoder

## 1. 한 줄 정의

LM에서 입력 token들의 초기 representation을 받아 문맥과 token 간 관계가 반영된 contextual representation으로 변환하는 구조.

## 2. 상위 개념
```
AI
└ LM
   └ Transformer
```
## 3. 해결하려는 문제

Token을 각각 독립적인 embedding으로만 표현하면 문맥과 token 간 관계를 표현할 수 없다.

## 4. 핵심 아이디어

각 token의 representation을 주변 token들과의 관계를 반영하여 갱신한다.

## 5. 동작

```text
Token Embeddings
↓ Positional Information
↓ Self-Attention + FFN
Contextual Representations
```

Self-Attention과 FFN을 여러 layer 반복하면서 representation을 갱신한다.

## 6. 핵심 수식



## 7. 예시

`bank`라는 동일한 token도

```text
deposit money → 금융기관의 bank
river → 강둑의 bank
```

처럼 주변 문맥에 따라 서로 다른 contextual representation을 가질 수 있다.

## 8. 관련 개념

- Embedding
- Self-Attention
- FFN
- Transformer
- Decoder

## 9. Notes

Encoder는 단순한 `token → vector` 변환기가 아니다.

초기 vector를 **문맥이 반영된 representation**으로 변환한다.
