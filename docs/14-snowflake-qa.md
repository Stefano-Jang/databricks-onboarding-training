<!-- markdownlint-disable MD033 -->
⬅️ [이전: Streamlit→대시보드 데모(시청용)](./13-streamlit-genie-demo.md) | 🏠 [목차](../README.md)

# 14. Snowflake vs Databricks Q&A

> **이 실습에서 배우는 것** — Snowflake와 Databricks를 **균형 있고 사실 기반으로** 비교합니다. 두 플랫폼의 아키텍처·스토리지·컴퓨트·거버넌스·AI/ML·개방성 차이를 초심자 눈높이에서 이해하고, "언제 무엇을 쓰면 좋은지"까지 감을 잡는 것이 목표입니다.

| 항목 | 내용 |
|---|---|
| 예상 소요 시간 | 약 20분 (읽기 자료) |
| 난이도 | 입문 |
| 사전 준비 | 앞의 01~13 실습을 한 번 훑어보면 이해가 쉽습니다(필수 아님) |
| 필요 권한 | 없음(개념 학습 자료) |

## 학습 목표
- 레이크하우스(Lakehouse)와 전통적 데이터 웨어하우스(Data Warehouse)의 **근본적 차이**를 설명할 수 있습니다.
- Databricks와 Snowflake의 **스토리지·포맷·컴퓨트·과금 모델** 차이를 개념 수준에서 비교할 수 있습니다.
- **거버넌스**(Unity Catalog ↔ Snowflake Horizon), **AI/ML 지원**, **개방성/멀티클라우드** 관점의 차이를 이해합니다.
- **마이그레이션/공존** 접근 방식과 두 플랫폼의 **솔직한 강점·약점**을 균형 있게 파악합니다.

---

## 이 문서를 읽는 방법 (중요) 📌

이 자료는 **Snowflake Senior SE(세일즈 엔지니어) 출신 SA**가 강의에서 받은 질문에 답한 세션을, 셀프 스터디용으로 정리한 **Q&A + 비교 표** 형식입니다. 몇 가지 원칙을 먼저 밝혀 둡니다.

- **과장·비방 없이, 사실 위주로** 씁니다. 두 제품 모두 훌륭하며, 각자 잘 맞는 자리가 있습니다.
- **제품은 매우 빠르게 진화합니다.** "지금 시점의 차이"가 몇 달 뒤에는 좁혀지거나 뒤집힐 수 있습니다. 아래 내용은 **일반적인 경향/문서 기준**의 설명이며, 최종 판단 전에는 각 벤더의 **최신 공식 문서**로 확인하세요.
- Databricks 기능은 **공식 문서 링크로 검증**했습니다(문서 하단 "공식 문서 링크"). Snowflake 기능은 Snowflake 공식 문서 기준으로 서술하되, 세부 수치·정책은 변동이 크므로 **개념 위주**로만 다룹니다.
- **구체적 가격 수치는 다루지 않습니다.** 과금은 계약·리전·시점에 따라 달라지므로 "과금 모델의 개념"만 설명합니다.

---

## 개념 먼저 이해하기

세 가지 용어부터 정리하면 나머지가 쉬워집니다.

| 용어 | 한 줄 정의 | 잘하는 일 | 아쉬운 점 |
|---|---|---|---|
| **데이터 웨어하우스**(DW) | 정형 데이터를 SQL로 분석하기 위한, 잘 정돈된 창고 | 빠른 SQL 분석, BI, 높은 동시성 | 비정형 데이터·대규모 ML에는 부담 |
| **데이터 레이크**(Lake) | 온갖 형식의 원본 데이터를 값싸게 쌓아 두는 저수지 | 저렴한 대용량 저장, 원본 보관, ML 원천 | 품질·거버넌스·SQL 성능이 약함 |
| **레이크하우스**(Lakehouse) | **레이크의 개방성·저장 위에 DW의 성능·관리 기능을 얹은** 구조 | 하나의 저장소에서 BI + ML 모두 | 개념이 비교적 새로워 학습 필요 |

핵심 그림은 이렇습니다.

- **Snowflake**는 뿌리가 **클라우드 데이터 웨어하우스**입니다. "정돈된 창고"에서 출발해 최근 AI/개방형 포맷으로 영역을 넓히고 있습니다.
- **Databricks**는 뿌리가 **데이터 레이크 + Apache Spark**입니다. "저수지" 위에 **Delta Lake**(신뢰성)와 **Unity Catalog**(거버넌스)를 얹어 **레이크하우스**를 만들었고, 그 위에서 BI와 AI/ML을 함께 돌립니다.

> 즉, 두 회사는 **출발점이 반대**입니다(창고→개방 vs 저수지→관리). 그래서 서로의 강점 영역으로 계속 확장하며 만나는 중입니다. 이 "출발점 차이"를 기억하면 아래 Q&A가 훨씬 잘 이해됩니다.

<!-- 스크린샷 예정: ../assets/screenshots/14-snowflake-qa/01-lakehouse-vs-dw.png (레이크하우스 vs 데이터 웨어하우스 비교 다이어그램(플레이스홀더)) -->
*📸 캡처 안내(선택): 왼쪽에 "데이터 레이크+웨어하우스=레이크하우스", 오른쪽에 "저장(개방형 포맷)–컴퓨트(분리)–거버넌스(Unity Catalog)" 3층 구조가 보이도록 간단한 비교 다이어그램을 그려 캡처합니다. 실제 화면 캡처가 아니어도 됩니다.*

---

## 자주 나오는 질문 (Q&A)

### ❓ Q1. "레이크하우스가 정확히 뭔가요? Snowflake 같은 웨어하우스랑 뭐가 다른가요?"

**A.** 레이크하우스는 **데이터 레이크의 장점(값싼 개방형 저장)과 데이터 웨어하우스의 장점(빠른 SQL·신뢰성)을 하나로 합친** 데이터 관리 방식입니다. Databricks 공식 문서도 레이크하우스를 "데이터 레이크와 데이터 웨어하우스의 이점을 결합한 데이터 관리 시스템"으로 정의하고, 그 기반 기술로 **Apache Spark + Delta Lake + Unity Catalog**를 듭니다.

전통적 웨어하우스는 보통 "데이터를 창고 안 형식으로 적재(load)"한 뒤 그 안에서만 분석합니다. 레이크하우스는 반대로 **데이터가 개방형 포맷으로 스토리지에 그대로 있고**, 그 위에서 SQL·ML·스트리밍을 모두 돌립니다. 그래서 "BI 팀은 웨어하우스, ML 팀은 레이크"처럼 **데이터를 두 벌로 복사·이동하지 않아도** 되는 것이 큰 차이입니다.

---

### ❓ Q2. "데이터는 어디에 저장되나요? Snowflake처럼 벤더가 관리하는 독점 포맷인가요?"

**A.** 여기가 두 플랫폼의 가장 큰 철학적 차이입니다.

- **Databricks**: 기본 저장 포맷이 **Delta Lake**로, **오픈소스**입니다. Delta Lake는 Parquet 파일에 **트랜잭션 로그**를 더해 ACID 트랜잭션과 대규모 메타데이터 처리를 제공합니다. 데이터 파일은 (클래식 컴퓨트 기준) **여러분의 클라우드 계정(예: S3 버킷)에** 그대로 있고, 다른 엔진(Spark, Trino 등)으로도 열 수 있습니다.
- **Snowflake**: 네이티브 테이블은 Snowflake가 관리하는 **독자적(proprietary) 마이크로 파티션 형식**으로, Snowflake가 운영하는 스토리지에 저장됩니다. 사용자가 그 파일을 직접 다루지는 않습니다. 대신 Snowflake도 최근 **Apache Iceberg 테이블**을 지원해, 사용자가 관리하는 외부 클라우드 스토리지의 개방형 포맷 데이터를 다룰 수 있게 되었습니다.

> 정리하면, **개방형 포맷이 "기본값"인 쪽이 Databricks**, **완결적으로 관리되는 독자 포맷에서 출발해 개방형(Iceberg)을 추가로 지원하는 쪽이 Snowflake**입니다. "내 데이터를 내 스토리지에, 벤더 종속 없이 두고 싶다"가 중요하면 레이크하우스 접근이 유리합니다.

---

### ❓ Q3. "Delta Lake랑 Iceberg 얘기가 자주 나오는데, 개방형 포맷이 왜 그렇게 중요한가요?"

**A.** 개방형 테이블 포맷(Delta Lake, Apache Iceberg)은 "데이터를 특정 벤더 엔진 안에 가두지 않는다"는 뜻이라 중요합니다. 같은 데이터를 여러 도구가 읽을 수 있으면 **벤더 종속(lock-in)이 줄고**, 팀마다 다른 엔진을 써도 **데이터를 복사하지 않아도** 됩니다.

Databricks는 두 방향으로 개방성을 지원합니다.

- **Delta Lake**(기본): 오픈소스이며 Databricks가 원 개발·기여합니다.
- **UniForm**(Universal Format): Delta 테이블에 **Iceberg 메타데이터를 자동 생성**해, 파일을 다시 쓰지 않고도 **Iceberg 클라이언트가 그대로 읽게** 해 줍니다. 즉 "데이터 한 벌"을 Delta로도, Iceberg로도 읽습니다.

Snowflake도 **Iceberg 테이블**을 지원하므로, "Iceberg를 공통 분모로 두 플랫폼이 같은 데이터를 공유"하는 그림이 점점 현실적이 되고 있습니다. 초심자에게 요점은 하나입니다: **개방형 포맷 = 미래에 도구를 바꿀 자유.**

---

### ❓ Q4. "컴퓨트는 어떻게 동작하나요? Snowflake의 Virtual Warehouse랑 비슷한가요?"

**A.** "저장과 컴퓨트를 분리하고, 필요할 때 켜서 쓰고 끄면 과금이 멈춘다"는 큰 그림은 **두 플랫폼이 비슷**합니다. 다만 방식과 위치가 조금 다릅니다.

- **Snowflake — Virtual Warehouse**: X-Small부터 6X-Large까지 **T-shirt 사이즈**로 컴퓨트를 고르고, 미사용 시 **자동 일시중지(auto-suspend)**, 쿼리가 오면 **자동 재개(auto-resume)**됩니다. 컴퓨트는 **Snowflake가 자기 환경에서 완전 관리**하므로 사용자는 인프라를 거의 신경 쓰지 않습니다. (문서 기준: 초당 과금, 시작 시 60초 최소)
- **Databricks — 다양한 컴퓨트**: 대화형 분석/ETL용 클러스터, SQL 전용 **SQL 웨어하우스**, 그리고 **서버리스**가 있습니다. 서버리스는 여러분의 클라우드 계정에 자원을 미리 띄우지 않고 **Databricks가 온디맨드로 관리**해 빠르게 붙습니다. SQL 성능은 벡터화 엔진 **Photon**이 가속합니다(기존 Spark/SQL 코드 변경 없이 적용).
  - 참고: **클래식 컴퓨트**는 여러분의 클라우드 계정 안에서 돌고, **서버리스 컴퓨트**는 Databricks가 관리하는 환경에서 돕니다(아키텍처가 "컨트롤 플레인 + 컴퓨트 플레인"으로 나뉨).

> 초심자 관점 요약: Snowflake는 "사이즈만 고르면 끝"이라 **단순함**이 강점이고, Databricks는 워크로드(대화형/ETL/SQL/스트리밍/ML)에 따라 **선택지가 많은 대신 유연**합니다.

---

### ❓ Q5. "과금은 어떻게 되나요? 어느 쪽이 싼가요?"

**A.** **"어느 쪽이 싸다"고 단정하기 어렵습니다.** 워크로드 종류, 사용 패턴, 최적화 수준, 계약(약정) 조건에 따라 결과가 크게 달라지기 때문입니다. 개념만 정리하면:

- 두 플랫폼 모두 **소비 기반(consumption-based)** 과금입니다. 쓴 만큼 냅니다.
- Snowflake는 소비 단위를 **크레딧(credit)**으로, Databricks는 **DBU(Databricks Unit)**로 정규화해 측정합니다. DBU는 "처리 능력을 정규화한 단위"이며, **초 단위 사용량** 기반으로 과금됩니다.
- 두 곳 다 **선불 없이 종량제(pay-as-you-go)**로 시작할 수 있고, **일정 사용량을 약정하면 할인**을 받는 구조가 있습니다.
- **주의**: 여기에 컴퓨트뿐 아니라 스토리지, 데이터 전송, 서버리스/기능별 요율 등이 얽힙니다. 그래서 **실제 비용 비교는 여러분의 실제 워크로드로 PoC를 돌려 측정**하는 것이 정답입니다.

> 이 문서에서는 방침상 **구체적 달러 수치를 다루지 않습니다.** 가격은 시점·리전·계약에 따라 변동이 크니, 반드시 각 벤더의 최신 가격 페이지와 영업/SA 견적으로 확인하세요.

---

### ❓ Q6. "데이터 거버넌스는요? Unity Catalog랑 Snowflake Horizon을 비교해 주세요."

**A.** 거버넌스는 "누가 어떤 데이터에 접근할 수 있고, 데이터가 어디서 와서 어디로 가는지(계보)를 추적·감사하는" 기능입니다. **두 플랫폼 모두 성숙한 거버넌스를 제공**합니다.

- **Databricks — Unity Catalog**: 데이터 **와 AI 자산**을 아우르는 통합 거버넌스 계층입니다. 접근 제어(권한·속성 기반 정책·행/열 필터), **자동 계보(lineage)** 추적, 감사 로그, **민감정보 자동 분류·태깅**, 데이터 품질 모니터링, **Delta Sharing 기반 개방형 공유**, AI 자산·트래픽 거버넌스까지 한 곳에서 다룹니다. 또한 **오픈소스**로도 제공됩니다.
- **Snowflake — Horizon(Horizon Catalog)**: Snowflake의 거버넌스·규정준수·검색·상호운용 계층으로, 데이터 거버넌스와 AI 거버넌스, 컨텍스트, 상호운용성을 표방합니다.

**공통점**: 세분화된 접근 제어, 계보, 데이터 검색/카탈로그, 마스킹/분류, 감사 등 **핵심 거버넌스 기능은 양쪽 다** 갖췄습니다. **차이의 결**은, Databricks는 처음부터 **데이터 + ML/AI 자산(모델·특성·노트북 등)까지 하나의 카탈로그로** 묶는 데 초점을 두었고, Snowflake는 웨어하우스 중심의 강력한 거버넌스에서 AI 영역으로 확장하는 흐름이라는 점입니다. 세부 기능 매트릭스는 빠르게 바뀌므로 **최신 문서로 항목별 확인**을 권합니다.

---

### ❓ Q7. "머신러닝이나 생성형 AI(LLM)는 어느 쪽이 강한가요?"

**A.** **데이터 사이언스/ML 깊이**는 일반적으로 Databricks가 앞선다고 평가받습니다(뿌리가 Spark + MLflow라서요). **SQL 안에서 손쉽게 AI를 부르는 편의성**은 Snowflake Cortex가 매우 강합니다. 목적에 따라 갈립니다.

- **Databricks — Mosaic AI**:
  - **Model Serving**: 커스텀 모델(scikit-learn·XGBoost·PyTorch·Hugging Face 등 MLflow 포맷), **Databricks 호스팅 파운데이션 모델**(예: Llama), **외부 모델**(예: OpenAI GPT)을 **하나의 REST 인터페이스로 배포·거버넌스·조회**합니다.
  - **Foundation Model APIs**: 오픈 모델을 서빙 엔드포인트로 제공하며, **종량제(pay-per-token)**와 프로덕션용 **프로비저닝 처리량(provisioned throughput)** 옵션이 있습니다.
  - 그 밖에 MLflow(실험·모델 관리), 벡터 검색, 파인튜닝, 에이전트 등 **모델을 직접 만들고 학습·배포**하는 전체 수명주기가 강점입니다.
- **Snowflake — Cortex**: **SQL 함수 한 줄로 LLM을 호출**하는 방식이 특징입니다(`AI_COMPLETE`, `AI_CLASSIFY`, `AI_EMBED` 등). 문서 기준으로 OpenAI·Anthropic·Meta·Mistral·DeepSeek 등의 모델을 **Snowflake 서비스 경계 안에서** 제공하며, Snowpark로 Python/ML도 지원합니다. **SQL 분석가가 즉시 AI를 쓰기**에 진입장벽이 낮습니다.

> 요약: "모델을 직접 학습·파인튜닝·배포하고, 대규모 데이터 사이언스를 한다" → **Databricks 쪽이 넓고 깊습니다.** "정형 데이터 위에서 SQL로 빠르게 AI 기능을 붙인다" → **Snowflake Cortex가 간편합니다.**

---

### ❓ Q8. "개방성과 멀티클라우드, 데이터 공유(Data Sharing)는 어떤가요?"

**A.** 두 플랫폼 모두 **AWS·Azure·GCP** 위에서 동작하는 멀티클라우드 제품입니다. 차이는 "공유/개방"의 결에서 드러납니다.

- **Databricks — Delta Sharing**: **오픈 프로토콜**로, **Databricks를 쓰지 않는 상대에게도** 데이터·AI 자산을 공유할 수 있습니다. 받는 쪽은 Spark, Pandas, Power BI 등 **다양한 도구**로 받을 수 있어, 서로 같은 플랫폼일 필요가 없습니다.
- **Snowflake — Secure Data Sharing / Marketplace**: Snowflake 생태계 안에서 **매우 매끄러운** 공유·데이터 상품 거래 경험을 제공합니다. 여기에 Iceberg 지원이 더해지며 개방형 상호운용도 확대되는 추세입니다.

> "생태계 밖 파트너와도 표준 프로토콜로 나누고 싶다"면 개방형(Delta Sharing) 접근이, "Snowflake를 함께 쓰는 파트너망 안에서 매끄럽게" 나누는 것이 중요하면 Snowflake 공유가 편리합니다.

---

### ❓ Q9. "이미 Snowflake를 쓰고 있어요. 꼭 갈아타야 하나요? 마이그레이션은 어떻게 접근하죠?"

**A.** **"전부 갈아타라"가 정답인 경우는 드뭅니다.** 현실적인 접근은 보통 **공존(coexistence) → 점진적 이전**입니다.

- **먼저 공존**: Databricks의 **Lakehouse Federation**을 쓰면, 데이터를 옮기지 않고도 **Snowflake·PostgreSQL·MySQL 같은 외부 소스를 Unity Catalog에서 거버넌스된 상태로 쿼리**할 수 있습니다(쿼리 푸시다운 지원, 읽기 전용). 그래서 "새 워크로드(특히 ML·대규모 ETL)는 Databricks에서, 기존 BI는 당분간 Snowflake에서"처럼 **두 플랫폼을 동시에** 굴릴 수 있습니다.
- **개방형 포맷을 다리로**: 양쪽 다 지원하는 **Iceberg**를 공통 저장 포맷으로 삼으면, 데이터 복제를 줄이며 점진 이전이 가능합니다.
- **점진 이전 시 흔한 순서**: ① 데이터 원본을 개방형 포맷/Delta로 정리 → ② ETL/ML부터 이전 → ③ BI/대시보드 이전 → ④ 거버넌스를 Unity Catalog로 통합.

> 마이그레이션은 기술만큼 **비용·팀 역량·리스크** 문제입니다. 반드시 **작은 PoC로 성능·비용을 실측**하고, "왜 옮기는가(개방성? ML 통합? 비용? 단순화?)"를 먼저 명확히 하세요.

---

### ❓ Q10. "솔직히 Databricks의 약점은 뭔가요? Snowflake가 더 나은 점은?"

**A.** 균형을 위해 솔직히 말씀드립니다. (제가 Snowflake 출신이라 애정도 있습니다.)

**Snowflake가 일반적으로 잘하는 점 / Databricks가 상대적으로 더 신경 써야 하는 점**
- **단순함·운영 편의**: Snowflake는 "사이즈만 고르면 끝"에 가까워, 튜닝·운영 부담이 적고 **SQL 분석가 온보딩이 빠릅니다.** Databricks는 선택지가 많은 만큼 **초기 학습 곡선**이 있습니다(단, 서버리스·SQL 웨어하우스로 이 격차는 좁아지는 중).
- **BI 동시성/일관된 성능**: 정형 데이터에 대한 고동시성 BI에서 Snowflake는 오랜 기간 다져진 **예측 가능한 성능**으로 정평이 나 있습니다.
- **성숙한 SQL DW 경험**: 순수 SQL 데이터 웨어하우징만 필요하다면, Snowflake의 매끈함이 매력적입니다.

**Databricks가 일반적으로 잘하는 점**
- **ML/AI·데이터 사이언스 깊이**, **개방형 포맷 기본값(Delta/Iceberg)**, **하나의 플랫폼에서 ETL+BI+스트리밍+ML 통합**, **데이터가 내 클라우드 스토리지에 개방형으로** 남는 구조.

> 결론적으로 "약점 없는 만능 도구"는 없습니다. **워크로드 성격**(순수 BI냐, ML까지냐), **팀 역량**(SQL 중심이냐, 데이터 엔지니어링/DS까지냐), **개방성/락인 민감도**로 갈립니다.

---

### ❓ Q11. "그래서, 언제 어느 쪽을 고르면 되나요?"

**A.** 정답은 "상황에 따라"이지만, **출발점 가이드**를 드리면 다음과 같습니다(절대 규칙 아님).

| 이럴 때는… | 일반적으로 잘 맞는 선택 | 이유 |
|---|---|---|
| 순수 SQL 데이터 웨어하우징 + 고동시성 BI가 핵심, 운영 단순함 최우선 | Snowflake가 편안 | 낮은 운영 부담, 성숙한 SQL DW 경험 |
| ML/AI·데이터 사이언스·대규모 ETL·스트리밍을 하나로 통합 | Databricks가 강점 | 레이크하우스 + Mosaic AI 통합 |
| 개방형 포맷으로 벤더 종속을 줄이고, 데이터를 내 스토리지에 두고 싶음 | Databricks 접근이 유리 | Delta/Iceberg 기본, 데이터 소유권 |
| 비정형/멀티모달 데이터, LLM 파인튜닝·모델 서빙까지 | Databricks | 모델 학습·서빙 전 주기 지원 |
| SQL만으로 빠르게 AI 기능을 붙이고 싶은 분석 조직 | Snowflake Cortex가 간편 | SQL 함수로 즉시 LLM 호출 |
| 이미 한쪽을 잘 쓰고 있고 새 요구만 추가 | **공존** 후 점진 이전 | Lakehouse Federation·Iceberg로 다리 놓기 |

> 많은 기업이 실제로 **두 플랫폼을 함께** 씁니다. "둘 중 하나"라는 이분법보다 **"어떤 워크로드를 어디에 두는 게 최적인가"**를 묻는 편이 현실적입니다.

---

## 한눈에 보는 비교 표 📊

> 아래는 **개념 수준의 일반적 비교**입니다. 세부 기능·수치는 빠르게 바뀌므로 최신 공식 문서로 재확인하세요.

| 항목 | Databricks | Snowflake |
|---|---|---|
| **핵심 정체성** | 레이크하우스(Lake+DW 결합) | 클라우드 데이터 웨어하우스(→ AI 데이터 클라우드) |
| **출발점** | 데이터 레이크 + Apache Spark | 데이터 웨어하우스(SQL) |
| **기본 저장 포맷** | **Delta Lake(오픈소스)** + Iceberg 상호운용(UniForm) | 독자 마이크로 파티션 포맷 + **Iceberg 테이블 지원** |
| **데이터 위치(개념)** | (클래식) 사용자 클라우드 스토리지에 개방형으로 | Snowflake 관리 스토리지(Iceberg는 사용자 스토리지) |
| **컴퓨트** | 클러스터·SQL 웨어하우스·서버리스, Photon 가속 | Virtual Warehouse(T-shirt 사이즈), 완전 관리 |
| **과금 단위(개념)** | DBU(소비 기반, 초 단위) | 크레딧(소비 기반, 초 단위·시작 60초 최소) |
| **거버넌스** | Unity Catalog(데이터+AI 통합, 오픈소스 제공) | Horizon(Horizon Catalog) |
| **ML/AI** | Mosaic AI(모델 학습·서빙·FM API·벡터검색·에이전트) | Cortex(SQL LLM 함수)·Snowpark ML |
| **데이터 공유** | Delta Sharing(오픈 프로토콜, 비-Databricks 대상 포함) | Secure Data Sharing·Marketplace(+ Iceberg) |
| **멀티클라우드** | AWS·Azure·GCP | AWS·Azure·GCP |
| **일반적 강점** | ML/AI 깊이, 개방성, 통합 플랫폼 | 운영 단순함, 고동시성 BI, SQL 편의성 |

---

## 자주 겪는 오해 / 팁 💡

- **"레이크하우스는 그냥 데이터 레이크 아니야?"** — 아닙니다. 레이크의 저장 위에 **트랜잭션(Delta)·거버넌스(Unity Catalog)·SQL 성능(Photon)**을 얹어 **웨어하우스의 신뢰성**을 갖춘 것이 핵심입니다.
- **"Databricks는 어렵고 Snowflake는 쉽다"** — 과거 인식입니다. **서버리스·SQL 웨어하우스**가 나오며 Databricks의 진입장벽도 많이 낮아졌습니다. 반대로 Snowflake도 ML/AI가 강해지며 깊어졌습니다. **직접 써 보고 판단**하세요.
- **"둘 중 하나만 골라야 한다"** — 실제로는 **공존**이 흔합니다. Lakehouse Federation과 Iceberg로 **데이터를 옮기지 않고** 두 플랫폼을 연결할 수 있습니다.
- **가격 비교의 함정** — 벤치마크 수치는 조건(데이터·쿼리·최적화)에 매우 민감합니다. **여러분의 실제 워크로드로 PoC**를 돌려 비교하는 것이 유일하게 믿을 만한 방법입니다.
- **경쟁사 자료를 볼 때** — 어느 쪽 자료든 자사에 유리한 조건을 고르기 쉽습니다. **1차 출처(각 벤더 공식 문서)**로 교차 확인하는 습관을 들이세요.

---

## 공식 문서 링크 📚

**Databricks(아래 링크는 실제 문서로 확인함)**
- [데이터 레이크하우스란?](https://docs.databricks.com/aws/en/lakehouse/)
- [Databricks 고수준 아키텍처(컨트롤/컴퓨트 플레인)](https://docs.databricks.com/aws/en/getting-started/overview)
- [Delta Lake란?](https://docs.databricks.com/aws/en/delta/)
- [Delta 테이블을 Iceberg 클라이언트로 읽기(UniForm)](https://docs.databricks.com/aws/en/delta/uniform)
- [Databricks의 데이터 웨어하우징(Databricks SQL)](https://docs.databricks.com/aws/en/sql/)
- [Photon이란?](https://docs.databricks.com/aws/en/compute/photon)
- [서버리스 컴퓨트 연결](https://docs.databricks.com/aws/en/compute/serverless/)
- [Unity Catalog란?](https://docs.databricks.com/aws/en/data-governance/unity-catalog/)
- [Model Serving으로 모델 배포](https://docs.databricks.com/aws/en/machine-learning/model-serving/)
- [Foundation Model APIs](https://docs.databricks.com/aws/en/machine-learning/foundation-model-apis/)
- [Delta Sharing(안전한 데이터·AI 자산 공유)](https://docs.databricks.com/aws/en/delta-sharing/)
- [Lakehouse Federation(외부 DB·카탈로그 연결)](https://docs.databricks.com/aws/en/query-federation/)
- [Databricks 가격(DBU 개념)](https://www.databricks.com/product/pricing)

**Snowflake(공정한 비교를 위해 Snowflake 공식 문서로 확인함 — 세부 정책은 변동 가능)**
- [Overview of warehouses(Virtual Warehouse)](https://docs.snowflake.com/en/user-guide/warehouses-overview)
- [Apache Iceberg™ tables](https://docs.snowflake.com/en/user-guide/tables-iceberg)
- [Snowflake Cortex AI Functions(LLM functions)](https://docs.snowflake.com/en/user-guide/snowflake-cortex/llm-functions)
- [Snowflake Horizon(Horizon Catalog)](https://www.snowflake.com/en/product/features/horizon/)

> 참고: 위 Snowflake 링크는 초심자가 1차 출처를 직접 확인할 수 있도록 첨부했습니다. **가격·기능 세부는 시점에 따라 달라질 수 있으니** 항상 최신 문서를 확인하세요.

---

## 다음 단계 ➡️

축하합니다 — 이 문서로 **온보딩 커리큘럼(01~13)을 모두 마쳤습니다!** 🎉

- 🏠 [목차로 돌아가기](../README.md) — 배운 내용을 다시 훑어보세요.
- 실전 감을 키우려면, [06. 테이블·Volume·RBAC/ABAC](./06-tables-volumes-rbac-abac.md)와 [09. SQL 작성·저장](./09-sql.md)을 실제 데이터로 다시 따라 해 보세요.
- 이 Q&A의 결론은 하나입니다: **정답은 워크로드에 있습니다.** 작은 PoC로 직접 확인하는 습관이 가장 강력한 무기입니다.
