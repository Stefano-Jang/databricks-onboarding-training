<!-- markdownlint-disable MD033 -->
# Databricks 온보딩 실습 교육 (한국어)

> Databricks를 **처음 사용하는 분**을 위한 한국어 실습 교육 자료입니다.
> 강사의 2시간 데모 강의를 본 뒤, 이 저장소를 열어 **각 실습을 순서대로 따라 하면**
> Databricks의 핵심 기능을 자연스럽게 익힐 수 있도록 구성했습니다.

<p align="center">
  <a href="https://stefano-jang.github.io/databricks-onboarding-training/presentation/">
    <b>▶️ 발표자료(슬라이드) 바로 열기</b>
  </a>
</p>

---

## 🎯 이 교육의 대상과 방식
- **대상**: Databricks를 처음 접하는 완전 초심자(데이터 엔지니어·분석가·플랫폼 관리자 입문)
- **방식**
  1. **강사 데모(약 2시간)** — 강사가 발표자료로 개념을 설명하고 화면을 시연합니다.
  2. **셀프 스터디** — 강의 후 이 GitHub 저장소를 열어 실습을 **하나씩 따라 하기**로 복습합니다.
- **환경**: 대부분 **UI(화면) 따라하기**로 구성되어 CLI 설치가 없어도 실습할 수 있습니다.

> ℹ️ **이 교육의 범위**: 본 과정은 **완전 초심자**를 위한 것으로, 플랫폼의 **기본 기능**(워크스페이스·계정 콘솔·관리자/그룹·컴퓨트·데이터(테이블·볼륨·거버넌스)·노트북·SQL·잡·대시보드·앱)에 집중합니다.
> **LLM 등 생성형 AI 기능과 Genie(자연어 데이터 질의) 관련 기능은 이 과정에서 다루지 않습니다.** (13번 문서는 Genie가 무엇인지 **감만 잡는 시청용 데모**로, 직접 실습하지는 않습니다.) AI/ML·Genie 심화는 별도 과정에서 다룹니다.

## 🖥️ 발표자료 (동적 HTML 슬라이드)
강사용 발표 슬라이드는 브라우저에서 바로 열립니다.

- ▶️ **바로 보기 (GitHub Pages)**: <https://stefano-jang.github.io/databricks-onboarding-training/presentation/>
- 대체 링크 (htmlpreview): [열기](https://htmlpreview.github.io/?https://github.com/Stefano-Jang/databricks-onboarding-training/blob/main/presentation/index.html)
- 소스: [`presentation/index.html`](./presentation/index.html)
- 발표자료는 `main` 브랜치 기준 GitHub Pages로 자동 게시됩니다(내용 갱신 후 1~2분 뒤 반영).

## 📚 커리큘럼 (실습 순서)

| # | 실습 | 배우는 내용 |
|---|---|---|
| 01 | [Databricks 소개](./docs/01-databricks-intro.md) | 설립·연혁, 최근 투자·기업가치, Gartner MQ 위치 |
| 02 | [사용자 콘솔 & 계정 콘솔](./docs/02-consoles.md) | 워크스페이스 UI, **메뉴 한글로 바꾸기**, 계정 콘솔 개요 |
| 03 | [관리자 역할](./docs/03-admin-roles.md) | 계정 관리자 · 메타스토어 관리자 · 워크스페이스 관리자 |
| 04 | [그룹 관리](./docs/04-groups.md) | 그룹 생성 · 그룹 관리자 위임 · 그룹 활용 |
| 05 | [컴퓨트 유형](./docs/05-compute.md) | All-Purpose vs SQL Warehouse(DBSQL) vs 서버리스/실시간 |
| 06 | [테이블 · Volume · RBAC/ABAC](./docs/06-tables-volumes-rbac-abac.md) | Unity Catalog 테이블·볼륨 만들기, 권한(RBAC)·태그기반(ABAC) |
| 07 | [노트북 & S3 파일 읽기](./docs/07-notebooks.md) | 노트북 생성·실행, S3/샘플 데이터 읽기 |
| 08 | [Lakeflow Connect: S3 가져오기](./docs/08-lakeflow-connect-s3.md) | 코드 없이 UI로 S3 파일을 Unity Catalog 테이블로 적재(관리형 인제스트) |
| 09 | [SQL 작성·저장 & 스니펫](./docs/09-sql.md) | SQL 편집기, 쿼리 저장, 스니펫(snippet) 활용 |
| 10 | [Lakeflow Jobs — 태스크 & 스케줄](./docs/10-jobs.md) | Lakeflow Job(Task) 만들기, 스케줄 설정 |
| 11 | [AI/BI 대시보드 — 시각화 & 퍼블리싱](./docs/11-dashboards.md) | AI/BI 대시보드 만들기, 공유·퍼블리싱 |
| 12 | [Databricks Apps](./docs/12-apps.md) | 데이터 앱 만들고 배포하기 |
| 13 | [Streamlit→대시보드 데모(시청용)](./docs/13-streamlit-genie-demo.md) | Genie 기반 앱→대시보드 데모 소개 |
| 14 | [Snowflake vs Databricks Q&A](./docs/14-snowflake-qa.md) | SF 출신 SA와의 비교 Q&A |

> 💡 완전 초심자는 **01 → 14 순서대로** 진행하는 것을 권장합니다. 각 문서 하단의 "다음 단계"를 따라가세요.

## ✅ 사전 준비
- Databricks 워크스페이스 접속 권한(교육에서는 `coupang-appdemo` 샌드박스 사용)
- 최신 크롬/엣지 브라우저
- (선택) 실습용 별도 카탈로그·스키마 — 문서 내 안내에 따라 `onboarding_` 접두사로 생성

## 🖼️ 스크린샷 & 동영상
- 실습 문서에는 화면 캡처가 함께 제공됩니다. 이미지 규칙은 [`assets/screenshots/README.md`](./assets/screenshots/README.md) 참고.
- 일부 절차는 동영상으로도 제공됩니다(예: 13번 Streamlit→대시보드 데모).

## 📎 참고
- 모든 기능 설명에는 **Databricks 공식 문서 링크**가 첨부되어 있습니다.
- 본 자료는 교육용이며, 실제 화면/메뉴 명칭은 제품 업데이트에 따라 달라질 수 있습니다.

## 🚀 직접 해보기 — Databricks Free Edition
교육 워크스페이스(`coupang-appdemo`)는 교육용 샌드박스입니다. 교육이 끝난 뒤에도 **개인적으로 자유롭게 실습**하고 싶다면, **완전 무료·기간 제한 없는** **[Databricks Free Edition](https://www.databricks.com/learn/free-edition)** 으로 시작하세요. 노트북(Python·SQL)·SQL 분석·대시보드·Genie·ML/AI 등 이 교육에서 배운 대부분을 개인 계정에서 직접 해 볼 수 있습니다. (소규모 컴퓨트·공정 사용 한도가 있으며, 상업적 용도로는 사용할 수 없습니다.)

- ▶️ **시작하기**: <https://www.databricks.com/learn/free-edition>

---
<sub>License: 저장소의 [LICENSE](./LICENSE) 참고.</sub>
