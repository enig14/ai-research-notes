# FFN

## 1. 한 줄 정의

Feed-Forward Network. Transformer에서 각 token representation을 개별적으로 비선형 변환하는 작은 neural network.

## 2. 상위 개념
```
AI
 └ Neural Network
   └ Transformer
```
## 3. 해결하려는 문제

Self-Attention으로 token 간 정보를 교환한 이후 각 token 내부 representation의 가중치는 정해진다. 그러나 추가로 이들을 어떻게 해석할지에 대한 비선형성이 없다.

## 4. 핵심 아이디어

Self-Attention이 token 간 정보를 교환한다면 FFN은 각 token의 representation 자체를 변환한다.

## 5. 동작

```text
token representation
↓ Linear
↓ Non-linear activation
↓ Linear
updated token representation
```

## 6. 핵심 수식

$$
\mathrm{FFN}(x)
=
W_2 \sigma(W_1 x + b_1) + b_2
$$

## 7. 예시



## 8. 관련 개념

- Transformer
- Self-Attention
- Representation

## 9. 구분 포인트

Self-Attention:

> token들 사이에서 정보를 교환(Where)

FFN:

> 각 token의 representation을 가공(How)