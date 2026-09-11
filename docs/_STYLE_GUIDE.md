# 작성 스타일 가이드 (콘텐츠 작성자/에이전트 전용)

> 이 파일은 튜토리얼 콘텐츠를 작성하는 모든 작성자(사람·AI 에이전트)가 **반드시 먼저 읽고 그대로 따르는** 규칙입니다.
> 모든 문서가 하나의 시리즈처럼 보이도록 형식·문체·링크 규칙을 통일합니다.

---

## 0. 프로젝트 한 줄 요약
Databricks를 **처음 접하는 완전 초심자**를 위한 한국어 실습 교육 자료. 강사가 2시간 데모로 시연하고,
수강생은 이후 GitHub에서 **명령/절차를 그대로 따라 하면** 자연스럽게 기능을 익히도록 구성한다.

## 1. 대상 독자 & 톤
- 대상: Databricks를 처음 쓰는 사용자(데이터 엔지니어·분석가·관리자 입문). 클라우드/SQL 기초만 안다고 가정.
- 언어: **한국어**, 정중한 존댓말("~합니다", "~하세요"). 전문용어는 한글(영문) 병기 예) 카탈로그(Catalog).
- 눈높이: 화면에 보이는 그대로 안내. "왼쪽 사이드바에서 **카탈로그(Catalog)** 아이콘을 클릭합니다"처럼 **어디를 클릭하는지** 명확히.
- **UI 따라하기 중심**. `databricks` CLI 사용을 강요하지 않는다(초심자 대상). SQL/노트북 코드는 필요.

## 2. 워크스페이스 정보 (스크린샷/URL 기준)
- 스크린샷 촬영 프로필/워크스페이스: **`coupang-appdemo`**
- 워크스페이스 URL: `https://fe-sandbox-stefano-coupang-appdemo.cloud.databricks.com`
- 클라우드: **AWS** (문서 링크도 AWS 기준: `https://docs.databricks.com/aws/en/...`)
- 계정 콘솔(AWS): `https://accounts.cloud.databricks.com`

## 3. 각 튜토리얼 문서 공통 구조 (이 순서를 지킬 것)
```markdown
# NN. <섹션 제목>

> **이 실습에서 배우는 것** — 한두 문장 요약.

| 항목 | 내용 |
|---|---|
| 예상 소요 시간 | 약 NN분 |
| 난이도 | 입문 / 초급 / 중급 |
| 사전 준비 | (예: 워크스페이스 로그인, 앞 실습 완료 등) |
| 필요 권한 | (예: 워크스페이스 관리자, 메타스토어 관리자, 없음 등) |

## 학습 목표
- ...(불릿 3~5개)

## 개념 먼저 이해하기
간단한 배경 설명(초심자가 "왜"를 이해하도록). 표/비유 적극 활용.

## 따라 하기 (Step-by-step)
### 1단계: ...
1. ...구체적 클릭/입력 지시...
   ![캡션](../assets/screenshots/<folder>/01-<slug>.png)
   *스크린샷: <무엇을 캡처하는지 설명>*
### 2단계: ...

## 자주 겪는 문제 / 팁 💡
- ...

## 공식 문서 링크 📚
- [문서 제목](검증된 URL)

## 다음 단계 ➡️
- [다음 실습 제목](./다음파일.md)
```
- 문서 맨 위에는 이전/다음 네비게이션 한 줄을 둔다:
  `⬅️ [이전: ...](./NN-....md) | 🏠 [목차](../README.md) | [다음: ...](./NN-....md) ➡️`

## 4. 스크린샷 규칙 (매우 중요)
- 실제 이미지는 **나중에 일괄 촬영**한다. 지금은 **플레이스홀더 + 캡션**만 넣는다.
- 형식(반드시 이 형식):
  ```markdown
  ![간단한 대체텍스트](../assets/screenshots/<folder>/NN-<slug>.png)
  *📸 캡처 안내: <어느 화면에서 무엇이 보이도록 캡처할지 구체적으로>*
  ```
- `<folder>`는 아래 표의 값을 **정확히** 사용(디렉터리는 이미 생성됨).
- 파일명: `01-`, `02-` … 순번 두 자리 + 하이픈 + 짧은 영문 슬러그(예: `03-create-catalog.png`).
- 스크린샷은 실습의 **결정적 순간**마다 넣는다(초심자가 길을 잃지 않도록 넉넉히).
- 민감정보(토큰/이메일 전체/계정ID)는 캡처 시 마스킹하라고 캡션에 명시.

### 문서 ↔ 스크린샷 폴더 매핑
| 문서 파일 | 스크린샷 폴더 |
|---|---|
| `01-databricks-intro.md` | `assets/screenshots/01-intro` |
| `02-consoles.md` | `assets/screenshots/02-consoles` |
| `03-admin-roles.md` | `assets/screenshots/03-admin-roles` |
| `04-groups.md` | `assets/screenshots/04-groups` |
| `05-compute.md` | `assets/screenshots/05-compute` |
| `06-tables-volumes-rbac-abac.md` | `assets/screenshots/06-tables-volumes` |
| `07-notebooks.md` | `assets/screenshots/07-notebooks` |
| `09-sql.md` | `assets/screenshots/09-sql` |
| `10-jobs.md` | `assets/screenshots/10-jobs` |
| `11-dashboards.md` | `assets/screenshots/11-dashboards` |
| `12-apps.md` | `assets/screenshots/12-apps` |
| `13-streamlit-genie-demo.md` | `assets/screenshots/13-streamlit-genie` |
| `14-snowflake-qa.md` | `assets/screenshots/14-snowflake-qa` |

## 5. 공식 문서 링크 규칙 (필수, 날조 금지)
- 모든 기능 설명에는 **Databricks 공식 문서 링크**를 첨부한다(요구사항).
- **반드시 실재하는 URL만** 사용한다. 확신이 없으면 `WebFetch`로 해당 URL을 열어 **제목/내용이 맞는지 확인**한 뒤 넣는다.
- 기준 도메인: `https://docs.databricks.com/aws/en/...` (신규 구조). 검색은 `https://docs.databricks.com` 사용.
- 링크는 한국어 설명 + 실제 문서 제목으로: `- [Unity Catalog란?](https://docs.databricks.com/aws/en/data-governance/unity-catalog/)`
- 확인 못 한 링크는 넣지 말고, 대신 "공식 문서에서 '<검색어>'로 검색" 문구를 남긴다.

## 6. 코드/SQL/명령 규칙
- 수강생이 **그대로 복사·실행하면 동작**해야 한다(요구사항: 변경 없이 따라 하기).
- 재현성 우선: 가능하면 워크스페이스 기본 제공 데이터를 사용
  - `samples` 카탈로그(예: `samples.nyctaxi.trips`, `samples.tpch.*`)
  - `/databricks-datasets/...`(DBFS, S3 백엔드) 예: `/databricks-datasets/nyctaxi/`
- 실습용으로 새로 만드는 카탈로그/스키마/테이블 이름은 충돌을 피하기 위해 접두사 `onboarding_` 사용 권장(예: 스키마 `onboarding_training`).
- 코드 블록에는 언어 태그(```sql, ```python) 지정.

## 7. 하지 말 것
- 존재하지 않는 메뉴/버튼/URL/문서 링크를 **추측해서 쓰지 않는다**. UI 명칭이 불확실하면 캡션에 "(화면에서 실제 명칭 확인)" 표기.
- 초심자에게 CLI 설치를 전제로 강요하지 않는다.
- 다른 섹션 파일을 수정하지 않는다(본인 담당 파일만 작성).

## 8. 참고 — 커리큘럼 전체 순서 (상호 링크용)
1. `01-databricks-intro.md` — Databricks 소개(연혁·투자·Gartner MQ)
2. `02-consoles.md` — 사용자 콘솔(한글 메뉴) & 계정 콘솔
3. `03-admin-roles.md` — 관리자 역할(계정/메타스토어/워크스페이스)
4. `04-groups.md` — 그룹 관리
5. `05-compute.md` — 컴퓨트 유형
6. `06-tables-volumes-rbac-abac.md` — 테이블·Volume·RBAC/ABAC
7. `07-notebooks.md` — 노트북 & S3 파일 읽기
8. `09-sql.md` — SQL 작성·저장 & 스니펫
9. `10-jobs.md` — Job(Task) & 스케줄
10. `11-dashboards.md` — 대시보드 & 퍼블리싱
11. `12-apps.md` — Databricks Apps
12. `13-streamlit-genie-demo.md` — Streamlit→대시보드(Genie) 데모(시청용)
13. `14-snowflake-qa.md` — Snowflake vs Databricks Q&A
