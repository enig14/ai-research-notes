# LM

## 1. 한 줄 정의

자연어를 처리하는 AI 모델

## 2. 상위 개념

AI

## 3. 해결하려는 문제

자연어에 대한 대답을 기계로 생성해낸다.

## 4. 핵심 아이디어



## 5. 동작

아래는 Transformer 기반의 경우 
```text
Natural Language
        ↓
     Tokenizer
        ↓
     Token IDs
        ↓
  Embedding Layer
        ↓
  Token Embeddings
        ↓
Positional Information / RoPE
        ↓
┌──────────────────────────┐
│       Transformer        │
│                          │
│  Self-Attention          │
│        ↓                 │
│       FFN                │
│        ↓                 │
│   (여러 layer 반복)       │
└──────────────────────────┘
        ↓
Contextual Representations
        ↓
      LM Head
        ↓
 Vocabulary Logits
        ↓
Softmax / Token Selection
        ↓
   Next Token ID
        ↓
   반복하여 생성
        ↓
    Detokenizer
        ↓
 Natural Language
```

## 6. 핵심 수식



## 7. 예시

- LLM 류

## 8. 관련 개념
- LLM
- Tokenizer
- Encoder
- Embedding
- Transformer
- Decoder

## 9. Notes
 LM이 반드시 tranformer를 사용해야만 하는 것은 아니다.