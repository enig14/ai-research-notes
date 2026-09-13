## 1. 한 줄 정의

Q(Query), K(Key), V(Value)는 같은 hidden representation을 Attention에서 서로 다른 역할에 맞게 projection한 표현이다.

- **Q**: 무엇을 찾을 것인가
- **K**: 어떤 Query와 관련 있는가
- **V**: 선택되었을 때 실제로 전달할 정보

---

## 2. 상위 개념

**Attention → Self-Attention → Q, K, V**

Self-Attention에서는 같은 입력 representation으로부터 Q, K, V를 생성한다.

---

## 3. 해결하려는 문제

Attention에서 하나의 token representation만 사용하면 같은 표현이 동시에 다음 역할을 수행해야 한다.

- 현재 toekn이 어떤 특성을 찾아야하는지 나타내는 representation

- 각 토큰이 어떤 특성으로 검색될 수 있는지 나타내는 representation

- 선택되었을 때 실제로 전달할 정보를 나타내는 representation

하지만 이 세 역할에 적합한 representation은 서로 다를 수 있다.

또한 Q는 현재의 것이 필요하지만, K,V는 과거의 것이 필요하다.

---

## 4. 핵심 아이디어

입력 hidden representation을 서로 다른 학습 가능한 projection에 통과시킨다.

$$Q = XW_Q$$

$$K = XW_K$$

$$V = XW_V$$

같은 token이라도 Attention 내부에서는 서로 다른 역할을 담당한다.

```text
Q : 내가 어떤 정보를 찾고 있는가?
K : 나는 어떤 Query와 관련 있는가?
V : 나를 선택하면 어떤 정보를 전달할 것인가?
```

Q와 K는 **정보를 찾기 위한 비교**에 사용되고, V는 **실제로 전달할 정보**에 사용된다.

---

## 5. 동작

현재 token $i$의 Query와 각 token $j$의 Key를 비교한다.

$$score(i,j) = \frac{q_i k_j^T}{\sqrt{d_k}}$$

Softmax를 통해 Attention Weight를 만든다.

$$\alpha_{ij} = softmax(score(i,j))$$

그 weight를 Value에 적용한다.

$$o_i = \sum_j \alpha_{ij}v_j$$

따라서:

```text
Q × K
 ↓
어디를 얼마나 볼지 결정
 ↓
Attention Weight
 ↓
V에 적용
 ↓
새로운 contextual representation
```

**K를 가져오는 것이 아니라, Q와 K로 선택 정도를 계산한 뒤 V를 가져온다**

---

## 6. 핵심 수식

$$Attention(Q,K,V)=softmax\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

---

## 7. 예시

```text
"The cat ate the fish because it was hungry."
```

`it`을 처리한다고 하면:

```text
Q_it
 ↓
K_cat, K_ate, K_fish, ...
 ↓
각 token과의 관련도 계산
 ↓
cat의 Attention Weight가 높음
 ↓
V_cat의 정보가 많이 반영됨
```

**Q와 K**는 어떤 정보를 가져올지 **선택**, **V**가 실제 **전달**되는 정보

---

## 8. 관련 개념

- KV Cache
- Memory Bandwidth
- QKV Projection Fusion

---

## 9. Notes

- Q/K/V는 원래부터 서로 다른 정보가 아니라 **동일한 hidden representation을 서로 다른 목적에 맞게 변환한 것**이다.
- Q와 K는 matching을 위한 표현이고 V는 information transfer를 위한 표현이다.
- Autoregressive inference에서 실제 장기 memory 역할을 하는 것은 **K와 V**다.
- KV Cache의 중요한 비용은 용량 자체뿐 아니라, 매 token 생성마다 과거 KV를 반복해서 읽는 **memory bandwidth**다.

---
