# Chapter 03 확장 실습 답안 템플릿

> **과제:** PostgreSQL과 DBeaver로 실습 환경 검증하기  
> **사용 방법:** 이 파일을 내려받아 본인의 GitHub 저장소에 `chapter03_answer.md`라는 이름으로 저장한 뒤 실습하면서 바로 작성합니다.  
> **제출 방법:** LMS에는 파일을 직접 업로드하지 않고, **본인 GitHub 저장소의 `chapter03_answer.md` 파일 URL**을 제출합니다.

---

## 제출 전 보안 주의

이 과제 파일과 캡처 화면에는 다음 정보를 올리지 않습니다.

```text
실제 PostgreSQL 비밀번호
전체 DB 접속 URL
API Key / Token
개인정보
공개할 필요가 없는 사내 서버 주소
```

LMS에서 제출자를 확인할 수 있으므로 공개 저장소의 답안 파일에 학번이나 실명을 반드시 적을 필요는 없습니다.

```text
GitHub 계정 또는 별칭:
과제 작성일:
사용한 AI 도구:
```

---

# 1. PostgreSQL과 DBeaver 환경 확인

## 1-1. 내 환경

| 항목 | 작성 내용 |
| --- | --- |
| 운영체제 | macOS |
| PostgreSQL 버전 | 18.6(Homebrew) |
| DBeaver 버전 | 26.2.0 |
| Host | localhost |
| Port | 5432 |
| Database | postgres |
| Username | hanjaesun |

> 비밀번호는 기록하지 않습니다.

## 1-2. PostgreSQL과 DBeaver 역할 설명

```text
PostgreSQL은: 실제로 데이터를 저장하고 관리하는 프로그램(DBMS, 서버)이다.

DBeaver는: 그 PostgreSQL 서버에 접속해서 SQL을 보내고 결과를 눈으로 보기 편하게 보여주는 클라이언트(도구)다.

두 프로그램의 차이는: PostgreSQL 없이는 DBeaver만 있어도 데이터를 다룰 수 없고, 반대로 PostgreSQL만 있고 DBeaver가 없어도(터미널의 psql 등으로) 데이터는 다룰 수 있다. 즉 DBeaver는 필수가 아니라 "편의를 위한 도구"다.
```

---

# 2. 연결 테스트와 첫 SQL

## 2-1. DBeaver 연결 결과

- [O] PostgreSQL 연결 유형 선택
- [O] Host 확인
- [O] Port 확인
- [O] Database 확인
- [O] Username 확인
- [O] Test Connection 성공

### 연결 성공 화면

권장 이미지 경로:

```text
assignments/chapter03/images/step02_connection.png
```

`여기에 연결 성공 화면을 삽입하세요.`

## 2-2. 첫 SQL 실행

```sql
SELECT 1 + 1 AS result;
```

실행 전 예상: 2가 나올 것이다.

```text

```

실제 결과: result = 2

```text

```

이 결과가 의미하는 것: 이 결과가 의미하는 것: 1+1이라는 단순 계산이 정상적으로 실행되었다는 뜻이다. 이걸 통해 DBeaver가 PostgreSQL 서버에 실제로 연결되어 있고, SQL 명령을 서버에 전달해서 계산 결과를 정상적으로 돌려받고 있다는 것을 확인할 수 있다. 즉 "연결이 살아있다"는 걸 가장 간단한 SQL로 검증한 것이다.

```text

```

---

# 3. 현재 연결 위치를 SQL로 검증

다음 SQL을 실행합니다.

```sql
SELECT version();
SELECT current_database();
SELECT current_user;
SELECT current_schema();
SHOW search_path;
SHOW transaction_read_only;
SHOW TimeZone;
```

## 3-1. 결과 기록

| 확인 항목 | 실제 결과 | 내가 이해한 의미 |
| --- | --- | --- |
| `version()` | PostgreSQL 18.6 (Homebrew) on aarch64-apple... | 지금 서버에서 돌아가는 PostgreSQL의 버전과 빌드 정보 |
| `current_database()` | postgres | 지금 접속해 있는 데이터베이스 이름 |
| `current_user` | hanjaesun | 지금 로그인한 사용자 계정 |
| `current_schema()` | public | 지금 기본으로 사용 중인 스키마 |
| `search_path` | public, "$user" | 테이블 이름만 쓸 때 찾아볼 스키마 순서 |
| `transaction_read_only` | off | 지금 세션에서 데이터를 쓰기(수정/생성)할 수 있는 상태라는 뜻 (off = 쓰기 가능, on이면 읽기 전용) |
| `TimeZone` | Asia/Seoul | 이 세션에서 날짜/시간을 표시할 때 기준으로 삼는 시간대 |

## 3-2. 반드시 설명할 것

### DBeaver 연결 이름과 `current_database()`는 왜 같은 개념이 아닌가요?
DBeaver의 "연결(Connection)"은 내가 원하는 대로 지어 붙인 별명(예: "postgres", "my-local-db")일 뿐, 실제 DB 이름과 다를 수 있다. 반면 current_database()는 서버가 실제로 인식하는 진짜 데이터베이스 이름이다. 예를 들어 DBeaver에서 연결 이름을 "테스트DB"라고 지어도, 실제로 postgres라는 DB에 붙어있을 수 있다. 그래서 화면의 연결 이름만 보고 판단하면 안 되고, current_database()로 직접 확인해야 정확하다.
```text

```

### `current_schema()`와 `search_path`는 어떤 관계가 있나요?
search_path는 "스키마 이름을 안 적었을 때 찾아볼 순서 목록"이고, current_schema()는 그 목록 중에서 실제로 존재해서 "지금 기본으로 쓰이고 있는 스키마 하나"를 보여준다. 
```text

```

### `transaction_read_only = off`라는 결과만으로 모든 테이블을 만들 권한이 있다고 단정할 수 있나요?
아니다. transaction_read_only = off는 이 세션이 "읽기 전용 모드가 아니다"라는 뜻일 뿐이고, 실제로 테이블을 만들 수 있는지는 별개의 권한(permission) 문제다. 예를 들어 세션은 쓰기 모드여도, 특정 스키마에 CREATE 권한이 없으면 테이블 생성은 실패한다. 즉 이 값은 "쓰기 시도 자체가 막혀있지 않다"만 알려줄 뿐, 구체적인 권한까지 보장하진 않는다.
```text

```

## 3-3. 증거 화면

권장 경로:

```text
assignments/chapter03/images/step03_location_check.png
```

`여기에 현재 DB/사용자/스키마/search_path 결과 화면을 삽입하세요.`

---

# 4. `ai_database_book` 데이터베이스 확인

## 4-1. 현재 데이터베이스

```sql
SELECT current_database();
```

실제 결과:

```text

```

- [ ] 결과가 `ai_database_book`이다.
- [O] 다른 DB라면 올바른 연결로 전환했다.

## 4-2. 연결을 바꾼 뒤 다시 검증

```text
전환 전 데이터베이스: postgres
전환 후 데이터베이스: ai_database_book
전환 여부를 판단한 근거: SELECT current_database(); 를 ai_database_book 연결로 새로 실행해서, 결과가 실제로 "ai_database_book"으로 나오는 것을 직접 확인했다. DBeaver 창 제목이나 탭 이름에 표시되는 연결명만 보고 판단하지 않았다.
```

### 화면에서 보이는 연결 이름만 믿지 않고 SQL을 다시 실행해야 하는 이유
실제로 이번에 직접 겪었는데, SQL을 이전 postgres 탭에 이어서 입력했더니 왼쪽 연결 목록에서 ai_database_book을 눈으로 보고 있었는데도 결과는 계속 postgres로 나왔다. 즉 "어떤 연결이 화면에 보이는가"와 "지금 이 SQL 탭이 실제로 어느 연결을 통해 실행되는가"는 다를 수 있다. 눈으로 보이는 이름표는 착각을 일으킬 수 있으므로, SELECT current_database() 같은 SQL로 실제 서버 응답을 직접 확인해야 정확하다.
```text

```

---

# 5. SQL 실행 범위 실험

SQL Editor에 다음 세 문장을 입력합니다.

```sql
SELECT 'A' AS step;
SELECT 'B' AS step;
SELECT 'C' AS step;
```

## 5-1. 한 문장 실행

```text
내가 실행한 문장: SELECT 'A' AS step;
실제 결과: step = A
```

## 5-2. 선택 영역 실행

```text
선택한 문장: SELECT 'A' AS step;
SELECT 'B' AS step;
실제 결과: step = B
```

## 5-3. 전체 스크립트 실행

```text
실제 결과: A, B, C 세 문장이 모두 실행되었고, 각각 별도의 결과 탭(Results 1, Results 1(2), Results 1(3))으로 나뉘어 나왔다.

결과 탭 또는 실행 순서에서 관찰한 점: 화면에는 기본적으로 마지막 탭(C)만 보여서 처음엔 C만 실행된 줄 알았지만, 실제로는 세 문장이 입력한 순서(A→B→C) 그대로 각각 실행되어 탭이 3개 생긴 것이었다. 탭을 직접 눌러 확인해야 전체 실행 여부를 정확히 알 수 있었다.
```

## 5-4. 결과 해석

```text
한 문장 실행과 전체 스크립트 실행의 차이:
한 문장 실행(Cmd+Enter)은 커서가 있는(또는 선택한 마지막) 문장 하나만 실행하고 결과 탭도 하나만 생긴다. 반면 전체 스크립트 실행(Execute SQL Script)은 여러 문장을 순서대로 전부 실행하고, 문장마다 결과 탭을 따로 만든다. 다만 화면에는 마지막 탭만 기본으로 보이기 때문에, 겉보기에는 "하나만 실행된 것"처럼 착각하기 쉽다는 걸 직접 경험했다.

변경 SQL에서 실행 범위를 잘못 선택하면 위험한 이유:
만약 SELECT가 아니라 UPDATE나 DELETE 같은 데이터를 바꾸는 SQL이었다면, "한 문장만 실행할 생각"으로 전체 스크립트 실행을 눌렀을 때 의도하지 않은 여러 문장이 한꺼번에 실행되어 데이터가 잘못 바뀌거나 삭제될 수 있다. 반대로 "전체를 실행했다"고 착각했는데 실제로는 한 문장만 실행되어, 뒤의 SQL이 반영 안 된 채로 다음 작업을 진행하는 실수도 생길 수 있다. 그래서 실행 버튼을 누르기 전에 항상 "지금 선택된 범위가 뭔지" 확인하는 습관이 중요하다.
```

### 증거 화면

권장 경로:

```text
assignments/chapter03/images/step05_execution_scope.png
```

`여기에 실행 범위 비교 화면을 삽입하세요.`

---

# 6. 제공된 환경 확인 SQL 실행

Public 저장소의 Chapter 03 파일을 사용합니다.

```text
code/chapter03/setup_check.sql
code/chapter03/setup_validate_local.sql
```

## 6-1. `setup_check.sql`

실행 결과에서 확인한 항목:

```text
PostgreSQL 버전:
현재 DB:
현재 사용자:
현재 스키마:
search_path:
읽기 전용 여부:
TimeZone:
1 + 1 결과:
public 스키마 존재 여부:
public USAGE 권한:
public CREATE 권한:
```

### 이 파일을 여러 번 실행해도 비교적 안전한 이유

```text

```

## 6-2. `setup_validate_local.sql`

```text
실행 결과:
PASS / FAIL:
```

실패했다면 실패 항목:

```text

```

그 실패가 실제 문제인지 환경 차이인지 판단한 근거:

```text

```

---

# 7. 안전한 오류 진단 실습

실제 오류가 있었다면 그 오류를 사용합니다. 오류가 없었다면 **데이터를 삭제하거나 서버를 강제로 중지하지 말고**, 안전한 SQL 문법 오류를 하나 만들어 관찰합니다.

예:

```sql
SELEC 1;
```

> 오류를 확인한 뒤 올바른 `SELECT 1;`로 복구합니다.

## 7-1. 오류 기록

```text
오류 메시지 핵심 문장: syntax error at or near "SELEC"

내가 먼저 생각한 원인 1: SELEC이 뭔지 몰라서 오류가 난 것 같다. "1을 선택하라"고 하는데 정확히 뭘 어떻게 하라는 건지 이해가 안 갔다.

내가 먼저 생각한 원인 2: 오타가 났을 가능성이 있다 (SELECT를 잘못 쳤을 수도 있다는 생각이 들었다).

실제로 확인한 방법: SELEC을 SELECT로 고쳐서 다시 실행해봤다.

실제 원인: SELECT라는 SQL 키워드(명령어)의 철자를 SELEC으로 잘못 입력해서, PostgreSQL이 이 단어를 알아보지 못하고 문법 오류를 낸 것이었다.

수정한 내용: SELEC 1; 을 SELECT 1; 로 고쳤다.
```

## 7-2. 수정 후 재검증

```sql
SELECT 1;
SELECT current_database();
```

```text
재검증 결과:
SELECT 1; → 정상적으로 실행되어 값 1이 반환됨
SELECT current_database(); → ai_database_book 반환됨
두 문장 모두 오류 없이 정상 실행되어, SELEC을 SELECT로 고친 수정이 올바른 해결책이었음을 확인했다.
```

## 7-3. 오류를 유형으로 분류

- [ ] 서버 실행 문제
- [ ] Host 문제
- [ ] Port 문제
- [ ] Database 문제
- [ ] Username/인증 문제
- [O] SQL 문법 문제
- [ ] 권한 문제
- [ ] 기타

선택 이유: 서버 연결이나 DB, 권한 자체는 모두 정상이었다(같은 연결에서 앞서 다른 SQL들이 잘 실행됐음). 문제는 오직 SELEC이라는 SQL 키워드의 철자를 잘못 쓴 것뿐이었고, SELECT로 고치자마자 바로 해결됐다. 즉 이건 연결이나 권한 문제가 아니라 순수하게 SQL 문법(오타) 문제였다.

```text

```

---

# 8. AI를 오류 분석 보조 도구로 사용

## 8-1. AI에게 전달한 프롬프트

비밀번호·개인정보·전체 접속 URL은 제거하고 기록합니다.

```text

```

## 8-2. AI 답변 검토

| AI가 제안한 확인 방법 | 실제로 확인했는가? | 결과 | 수용 / 수정 / 거절 |
| --- | --- | --- | --- |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |

### AI가 오류 원인을 너무 빨리 단정한 부분이 있었나요?

```text

```

### 오류 메시지와 실제 환경 중 무엇을 확인해서 최종 판단했나요?

```text

```

### AI 활용에서 가장 유용했던 점

```text

```

### AI 답변을 그대로 실행하지 않고 확인해야 하는 이유

```text

```

---

# 9. Chapter 01~02 개인 서비스와 연결

앞에서 선택한 개인 서비스가 PostgreSQL을 사용한다고 가정합니다.

```text
서비스 이름:

사용할 데이터베이스 이름 후보:

사용할 스키마 이름 후보:

앞으로 만들고 싶은 테이블 후보 3개:
1.
2.
3.
```

### 아직 SQL을 만들지 않고 이름과 역할만 정하는 이유

```text

```

### Chapter 02에서 정리했던 한 행의 의미 중 수정할 부분이 있나요?

```text

```

---

# 10. 초보자용 연결 가이드 작성

친구가 자신의 PC에서 같은 실습을 시작한다고 가정합니다. 아래 순서를 자신의 말로 작성합니다.

```text
1. PostgreSQL 서버가 실행되는지 확인하는 방법:

2. DBeaver에서 PostgreSQL 연결을 만드는 방법:

3. Host / Port / Database / Username의 의미:

4. ai_database_book에 연결되었는지 확인하는 방법:

5. 현재 위치를 확인하는 SQL:

6. 한 문장과 전체 스크립트 실행을 구분해야 하는 이유:

7. 비밀번호를 GitHub나 AI 프롬프트에 넣으면 안 되는 이유:
```

---

# 11. 최종 성찰

아래 문장은 반드시 본인의 말로 작성합니다.

```text
1. DBeaver와 PostgreSQL의 가장 중요한 차이는
   ____________________________________________________________ 이다.

2. 내가 지금 어느 데이터베이스에 연결되어 있는지 확인할 때
   화면 이름만 보지 않고 ______________________________________ 해야 한다.

3. PostgreSQL 오류가 발생했을 때 가장 먼저 해야 할 일은
   ____________________________________________________________ 이다.

4. AI를 오류 해결에 사용할 때 가장 중요한 것은
   ____________________________________________________________ 이다.
```

---

# 12. 제출 체크리스트

- [ ] `chapter03_answer.md`의 빈 필수 항목을 작성했다.
- [ ] PostgreSQL과 DBeaver의 역할 차이를 설명했다.
- [ ] `current_database/current_user/current_schema/search_path`를 실제로 확인했다.
- [ ] `ai_database_book` 연결 여부를 SQL로 검증했다.
- [ ] SQL 실행 범위 세 가지를 비교했다.
- [ ] `setup_check.sql`을 실행했다.
- [ ] `setup_validate_local.sql` 결과를 확인했다.
- [ ] 오류 원인을 먼저 스스로 추정한 뒤 AI를 사용했다.
- [ ] AI 제안을 실제 환경에서 검증했다.
- [ ] 핵심 캡처 3~4장만 골라 넣었다.
- [ ] 캡처에 비밀번호·개인정보·전체 접속 URL이 없다.
- [ ] Markdown 이미지가 GitHub 웹 화면에서 실제로 보인다.
- [ ] 최종 답안 파일을 commit/push했다.

---

# 13. LMS 제출 URL

아래 형식의 **본인 GitHub 파일 URL**을 LMS에 제출합니다.

```text
https://github.com/<본인-GitHub-ID>/<본인-저장소>/blob/main/assignments/chapter03/chapter03_answer.md
```

내 제출 URL:

```text

```

> 저장소 메인 URL, 교수자 템플릿 URL, Raw URL이 아니라 **작성 완료된 본인 `chapter03_answer.md` 파일 화면 URL**을 제출합니다.
