코드 + 조회 결과 (이미지)

```sql
// ------------------------------------
// 0) 선택: 기존 예제 데이터 정리
// ------------------------------------
// 주의: DB 전체를 비웁니다.
// MATCH (n) DETACH DELETE n;


// ------------------------------------
// 1) 제약조건 / 인덱스
// ------------------------------------
CREATE CONSTRAINT movie_title_unique IF NOT EXISTS
FOR (m:Movie)
REQUIRE m.title IS UNIQUE;

CREATE CONSTRAINT person_name_unique IF NOT EXISTS
FOR (p:Person)
REQUIRE p.name IS UNIQUE;


// ------------------------------------
// 2) 사람(Person) 데이터
// oscars: 아카데미 수상 수(예제용)
// alive: 생존 여부
// ------------------------------------
MERGE (:Person {
  name: 'Keanu Reeves',
  born: 1964,
  alive: true,
  oscars: 0
});

MERGE (:Person {
  name: 'Carrie-Anne Moss',
  born: 1967,
  alive: true,
  oscars: 0
});

MERGE (:Person {
  name: 'Lana Wachowski',
  born: 1965,
  alive: true,
  oscars: 0
});

MERGE (:Person {
  name: 'Lilly Wachowski',
  born: 1967,
  alive: true,
  oscars: 0
});

MERGE (:Person {
  name: 'Tom Hanks',
  born: 1956,
  alive: true,
  oscars: 2
});

MERGE (:Person {
  name: 'Robin Wright',
  born: 1966,
  alive: true,
  oscars: 0
});

MERGE (:Person {
  name: 'Robert Zemeckis',
  born: 1951,
  alive: true,
  oscars: 1
});

MERGE (:Person {
  name: 'Leonardo DiCaprio',
  born: 1974,
  alive: true,
  oscars: 1
});

MERGE (:Person {
  name: 'Joseph Gordon-Levitt',
  born: 1981,
  alive: true,
  oscars: 0
});

MERGE (:Person {
  name: 'Christopher Nolan',
  born: 1970,
  alive: true,
  oscars: 1
});

MERGE (:Person {
  name: 'Katharine Hepburn',
  born: 1907,
  alive: false,
  oscars: 4
});

MERGE (:Person {
  name: 'Meryl Streep',
  born: 1949,
  alive: true,
  oscars: 3
});

MERGE (:Person {
  name: 'Jack Nicholson',
  born: 1937,
  alive: true,
  oscars: 3
});

MERGE (:Person {
  name: 'Frances McDormand',
  born: 1957,
  alive: true,
  oscars: 3
});


// ------------------------------------
// 3) 영화(Movie) 데이터
// ------------------------------------
MERGE (:Movie {
  title: 'The Matrix',
  released: 1999,
  tagline: 'Welcome to the Real World'
});

MERGE (:Movie {
  title: 'The Matrix Reloaded',
  released: 2003,
  tagline: 'Free your mind'
});

MERGE (:Movie {
  title: 'Forrest Gump',
  released: 1994,
  tagline: 'Life is like a box of chocolates'
});

MERGE (:Movie {
  title: 'Cast Away',
  released: 2000,
  tagline: 'At the edge of the world, his journey begins'
});

MERGE (:Movie {
  title: 'Inception',
  released: 2010,
  tagline: 'Your mind is the scene of the crime'
});

MERGE (:Movie {
  title: 'The Revenant',
  released: 2015,
  tagline: 'Blood lost. Life found.'
});

MERGE (:Movie {
  title: 'The Iron Lady',
  released: 2011,
  tagline: 'Never compromise'
});


// ------------------------------------
// 4) 출연 관계(ACTED_IN)
// ------------------------------------
MATCH (k:Person {name: 'Keanu Reeves'})
MATCH (c:Person {name: 'Carrie-Anne Moss'})
MATCH (m:Movie {title: 'The Matrix'})
MERGE (k)-[:ACTED_IN]->(m)
MERGE (c)-[:ACTED_IN]->(m);

MATCH (k:Person {name: 'Keanu Reeves'})
MATCH (c:Person {name: 'Carrie-Anne Moss'})
MATCH (m:Movie {title: 'The Matrix Reloaded'})
MERGE (k)-[:ACTED_IN]->(m)
MERGE (c)-[:ACTED_IN]->(m);

MATCH (t:Person {name: 'Tom Hanks'})
MATCH (r:Person {name: 'Robin Wright'})
MATCH (m:Movie {title: 'Forrest Gump'})
MERGE (t)-[:ACTED_IN]->(m)
MERGE (r)-[:ACTED_IN]->(m);

MATCH (t:Person {name: 'Tom Hanks'})
MATCH (m:Movie {title: 'Cast Away'})
MERGE (t)-[:ACTED_IN]->(m);

MATCH (l:Person {name: 'Leonardo DiCaprio'})
MATCH (j:Person {name: 'Joseph Gordon-Levitt'})
MATCH (m:Movie {title: 'Inception'})
MERGE (l)-[:ACTED_IN]->(m)
MERGE (j)-[:ACTED_IN]->(m);

MATCH (l:Person {name: 'Leonardo DiCaprio'})
MATCH (m:Movie {title: 'The Revenant'})
MERGE (l)-[:ACTED_IN]->(m);

MATCH (ms:Person {name: 'Meryl Streep'})
MATCH (m:Movie {title: 'The Iron Lady'})
MERGE (ms)-[:ACTED_IN]->(m);


// ------------------------------------
// 5) 감독 관계(DIRECTED)
// ------------------------------------
MATCH (lana:Person {name: 'Lana Wachowski'})
MATCH (lilly:Person {name: 'Lilly Wachowski'})
MATCH (m1:Movie {title: 'The Matrix'})
MATCH (m2:Movie {title: 'The Matrix Reloaded'})
MERGE (lana)-[:DIRECTED]->(m1)
MERGE (lilly)-[:DIRECTED]->(m1)
MERGE (lana)-[:DIRECTED]->(m2)
MERGE (lilly)-[:DIRECTED]->(m2);

MATCH (z:Person {name: 'Robert Zemeckis'})
MATCH (fg:Movie {title: 'Forrest Gump'})
MATCH (ca:Movie {title: 'Cast Away'})
MERGE (z)-[:DIRECTED]->(fg)
MERGE (z)-[:DIRECTED]->(ca);

MATCH (n:Person {name: 'Christopher Nolan'})
MATCH (i:Movie {title: 'Inception'})
MERGE (n)-[:DIRECTED]->(i);
```

<img width="1430" height="651" alt="image" src="https://github.com/user-attachments/assets/55e3829d-0ce1-4898-8d17-94b9d6121876" />

