# Essential GraphRAG  
## Chapter 3 — Advanced Vector Retrieval Strategies

## 한 줄 핵심
기본 RAG의 vector search는 정확도가 낮을 수 있기 때문에  
**query rewriting과 문서 구조 전략을 사용해 retrieval 정확도를 높이는 방법**을 설명하는 장이다.

---

# 1. 왜 Advanced Retrieval이 필요한가

기본 RAG 구조

User Question  
→ Embedding  
→ Vector similarity search  
→ Relevant text chunk  
→ LLM answer

### 문제점

1. 질문과 문서 표현 방식이 다르면 검색 실패
2. chunk가 너무 작으면 context 부족
3. embedding이 문서 핵심 의미를 잘 못 잡을 수 있음

### 예시

사용자 질문

[Estella Leopold went to which school between Aug and Nov 1954?]

문서 내용

[Leopold completed a PhD at Yale in 1955]

→ wording이 달라서 vector search가 놓칠 수 있음

그래서 **retrieval accuracy를 높이는 전략**이 필요하다.

---

# 2. 전략 1 — Query Rewriting (Step-back Prompting)

## 개념

사용자 질문을 **더 일반적인 질문으로 바꿔서 검색 정확도를 높이는 방법**

### 예시

사용자 질문

[Estella Leopold went to which school between Aug and Nov 1954?]

Step-back 질문

[What is Estella Leopold's education history?]

→ 더 많은 문서가 검색됨

---

## 왜 효과적인가

LLM이 질문을 일반화하면

specific question  
→ general concept  
→ documents match easier

즉

Query intent ≠ Document wording

문제를 해결한다.

---

## Step-back prompting 흐름

User question  
→ LLM (query rewriting)  
→ General question  
→ Embedding  
→ Vector search  
→ Relevant documents

---

# 3. 전략 2 — Hypothetical Question Embedding

## 개념

문서를 embedding하지 않고  

**"이 문서가 답할 수 있는 질문"을 embedding하는 방법**

### 예시

문서

[Leopold got a master's degree in botany from UC Berkeley in 1950]

embedding하는 것

[What did Leopold study at Berkeley?]

즉

document → possible questions

을 embedding한다.

---

## 왜 좋은가

검색은 보통 **question → question 매칭**이 더 정확하다.

기존 방식

question → document

새 방식

question → hypothetical question

그래서 retrieval 정확도가 올라간다.

---

# 4. 전략 3 — Parent Document Retriever

## 문제

기존 방식

document → chunk → embedding

retrieval 결과

small chunk only

문제

context 부족

---

## 해결 방법

Parent-child 구조 사용

Parent document  
→ split  
→ child chunks (embedding)

검색은 child chunk로 하지만  

**LLM에게는 parent document 전체를 전달**

---

## 예시

문서

[Einstein patents and inventions paper]

child chunk

paragraph1  
paragraph2  
paragraph3  

검색

query → chunk2 match

LLM 입력

전체 문서 (parent)

---

## 장점

child chunk  
→ 정확한 검색

parent document  
→ 충분한 context 제공

---

# 5. 기타 Retrieval 개선 전략

## 1. Embedding model fine-tuning

도메인 데이터로 embedding 모델 재학습

단점

- 비용 큼
- embedding 전체 재생성 필요

---

## 2. Reranking

vector search → 20개  
→ reranker model  
→ top 3

검색 결과를 **2차 모델로 재정렬**

---

## 3. Metadata filtering

문서 metadata 활용

예

date  
author  
topic  

검색 전에 필터 적용

---

# 6. 최종 Advanced RAG Pipeline

User question  
→ Step-back prompting (query rewrite)  
→ Embedding  
→ Vector search (child chunks)  
→ Retrieve parent documents  
→ LLM answer

효과

- retrieval accuracy ↑
- recall ↑
- context ↑
- hallucination ↓

---

# 7. 챕터 핵심 요약

## 핵심 문제

기본 vector search는  

**query와 문서 표현 차이 때문에 검색 실패 발생**

---

## 해결 전략

1️⃣ Step-back prompting  
- 질문을 더 일반화

2️⃣ Hypothetical question embedding  
- 문서를 질문 형태로 embedding

3️⃣ Parent document retriever  
- chunk로 검색  
- document 전체로 답변

---

## 결과

- retrieval accuracy ↑
- recall ↑
- LLM hallucination ↓

