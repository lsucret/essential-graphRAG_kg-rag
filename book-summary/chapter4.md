
# chapter 4 Generating Cypher queries from natural language questions
이 장을 끝내고 나면
- text2Cypher retriever를 구현할 수 있다
- few-shot 샘플, 스키마와 용어 매핑, 형식 지시를 제공해 성능을 향상시킬 수 있다
- LLM 모델 선택 시에 대규모 LLM 모델도 있지만, text2Cypher용으로 특화된 LLM을 사용할 수도 있다.


- 쿼리 생성의 기본
- 어디의 쿼리 언어 생성이 RAG 파이프라인에 잘 맞는지.
- 유용한 프랙티스. 쿼리 언어 생성에 대한
- 베이스 모델에 text2cyper 적용
- text2cyper 에 특화된 LLM들

쿼리로만 찾을 수 있는 경우가 있다.
- 그래프에서 가장 유사한 것을 찾는 것이 아닌 다른 것을 찾거나
- 데이터 집계가 필요할 때


## Cypher query 문법 간단 정리
### 구문
```
MATCH ...
WHERE ...
RETURN ...
ORDER BY ...
```

```
MATCH (m:Movie)
WHERE m.year >= 2000
RETURN m.title, m.year
ORDER BY m.year DESC
```

### 기타 구문
- CREATE 노드, 관계를 생성
- MERGE 있으면 재사용, 없으면 생성
- SET 속성을 수정
- DELETE 삭제 (DETACH DELETE 연결관계도 같이 지움)
- WITH 중간 결과를 다음 단계로 넘김


#### 노드 `()`
(n:Person)
(n:Person {name: 'Alice'})
#### 관계 `[]`
-[r:FRIEND]-
(a)-[:LIKES]->(b)
#### 속성 접근 `.`
p.name


LLM이 사이퍼 쿼리를 잘 작성하지 못하는 경우
- 질문이 복잡하거나 모호할 때
- db 스키마 이름이 시멘틱한(직관적인) 이름이 아닐 때

프롬프트에 스키마를 설명한다.
모든 스키마를 다 보여줄 필요는 없다.(비효율적)

Neo4j에서 스키마 정보를 자동으로 추론해서 그 결과를 LLM에 넣어줄 수 있다.
- 사람이 일일히 스키마를 프롬프트에 작성하는게 아닌
- Neo4j에서 자동으로 추론
- 방식
	- APOC 라이브러리의 프로시저 사용 필요 (무료)
	- APOC? 스키마/메타데이터를 추출하는 도구
		- 어떤 노드 라벨이 있는지
		- 어떤 관계 타입이 있는지
		- 어떤 속성이 있는지
		- 어떤 라벨끼리 연결되는지
		- 와 같은 구조화된 정보를 뽑는걸 지원해줌
	- 보통 db 샘플 데이터를 대상으로 스키마를 추론

text2cyper를 위한 Neo4j가 파인튜닝한 LLM 모델도 있다.
- https://huggingface.co/datasets/neo4j/text2cypher-2025v1

### Clean UP 
MATCH (n) DETACH DELETE n MATCH (n) CALL (n) { DETACH DELETE n } IN TRANSACTIONS OF 10000 ROWS

SHOW INDEXES; DROP INDEX index_name; 
SHOW CONSTRAINTS; DROP CONSTRAINT constraint_name;

