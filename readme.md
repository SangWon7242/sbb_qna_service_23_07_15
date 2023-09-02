# 점프 투 스프링부트를 통한 질의응답 게시판 소개
## 구현된 기능
### 질문작성
1. 질문 작성
    - 글 제목과 내용이 입력되지 않는 경우 '글 작성 해달라는 경고 메시지 출력'

2. 질문 수정
3. 질문 삭제

### 답변 작성
### 회원가입 기능
### 로그인 기능

## 남은 목표
- [.] 검색 기능 구현

### 검색 기능
#### 구현 : 질문 제목, 질문 내용, 질문 작성자
1. 제목검색
   ```sql
   SELECT Q.*
   FROM question AS Q
   WHERE Q.subject LIKE '%sbb%';
   ```

2. 제목 + 내용 검색
   ```sql
   SELECT Q.*
   FROM question AS Q
   WHERE (
     Q.subject LIKE '%sbb%'
     OR
     Q.content LIKE '%알고%'
   );
   ```

3. 제목 + 내용 검색 + 작성자
   ```sql
   SELECT Q.*
   FROM question AS Q
   LEFT JOIN site_user AS SU
   ON Q.author_id = SU.id
   WHERE (
     Q.subject LIKE '%user1%'
     OR
     Q.content LIKE '%user1%'
     OR
     SU.username LIKE '%user1%'
   );
   ```

#### 구현 : 답변 내용, 답변 작성
- 문제 1 : 1번 글이 답변이 10개라는 이유로 10개가 출력
- 해결 : DISTINCT or GROUP BY
---
- 문제 2 : 2번 글이 답변이 없다는 이유로 누락
- 해결 : LEFT JOIN

   ```sql
   # DISTINCT 붙여서 해결
   SELECT DISTINCT Q.id
   FROM question AS Q
   INNER JOIN answer AS A
   ON Q.id = A.question_id;
   
   # GROUP BY 붙여서 해결
   SELECT Q.id, A.id
   FROM question AS Q
   INNER JOIN answer AS A
   ON Q.id = A.question_id
   GROUP BY Q.id;
   ```