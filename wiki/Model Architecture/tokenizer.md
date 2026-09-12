# Tokenizer

## 1. 한 줄 정의

자연어를 모델이 처리할 수 있는 discrete token들의 sequence로 분할하고 각각을 token ID로 변환하는 모듈.

## 2. 상위 개념

```
AI
 └ LM
```

## 3. 해결하려는 문제

문자열 자체는 숫자가 아니므로 신경망이 직접 수치 연산할 수 없으므로 변환이 필요하다. 

## 4. 핵심 아이디어

```text
문자열 → token → token ID
```

Token ID는 의미를 나타내는 수치가 아니라 단순한 식별자이다.

## 5. 동작

```text
"I like cats"
↓
["I", "like", "cats"]
↓
[104, 5821, 9314]
```

실제 tokenizer는 단어뿐 아니라 subword 등으로 분해할 수 있다.

Tokenizer의 자연어 분할 방법은 일반적으로 LM 본체 학습 전에 corpus를 이용해 별도로 학습한 후 고정한다.

## 6. 핵심 수식



## 7. 예시

Tokenizer에 따라

```text
나는 고양이를 좋아한다
→ 나 / 는 / 고양이 / 를 / 좋아 / 한다
```

처럼 나뉠 수 있다.

## 8. 관련 개념

- Token
- Token ID
- Vocabulary
- Embedding Layer
- Detokenizer

## 9. Notes

Tokenizer는 token을 vector로 변환하지 않는다.
`문자열 → token ID`까지가 tokenizer이고, `token ID → vector`는 Embedding Layer의 역할이다.