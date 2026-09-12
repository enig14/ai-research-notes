# Attention

## 1. 한 줄 정의

현재 계산에서 여러 representation 중 어떤 정보를 얼마나 참고할지를 결정하는 메커니즘.

## 2. 상위 개념
```
AI
 └ Neural Network
   └ Transformer
```
## 3. 해결하려는 문제

현재 representation을 계산할 때 다른 representation들이 모두 동일하게 중요하지 않다.

## 4. 핵심 아이디어

참고할 representation마다 중요도를 계산하고, 그 중요도에 따라 정보를 가중하여 결합한다.

## 5. 동작

```text
현재 정보
↓
참고 가능한 representation들과 관계 계산
↓
Attention Weight
↓
가중합
↓
새로운 representation
```

## 6. 핵심 수식

\[
Attention(Q,K,V)
=
softmax
\left(
\frac{QK^T}{\sqrt{d_k}}
\right)V
\]

## 7. 예시

```text
고양이 ↔ 나는       낮은 weight
고양이 ↔ 좋아한다   높은 weight
```

처럼 현재 token과 다른 token 사이의 관련도를 계산할 수 있다.

## 8. 관련 개념

- Q uery
- K ey
- V alue
- Self-Attention
- Cross-Attention
- Transformer

## 9. Notes

Attention은 특정 언어마다 다른 알고리즘을 사용하는 것이 아니다.

같은 Attention 계산을 사용하지만 학습 결과 어떤 정보에 attention하는지가 달라진다.
