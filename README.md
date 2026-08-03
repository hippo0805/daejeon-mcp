# 대전 행정정보 MCP (daejeon-mcp)

> 대전 자치구의 공개 행정문서를 **환각 0 · 원문 근거** 원칙으로 서빙하는 공개 MCP 서버.
> 현재 커버리지: **대전 중구 · 서구** (2개 자치구) + **전국 자치법규 161k·국가법령 5.6k**

누구나 MCP 클라이언트로 바로 연결해 쓸 수 있습니다. 가입·키 발급·비용 없음.

```
https://djmcp.up.railway.app/junggu/mcp     ← 대전 중구
https://djmcp.up.railway.app/seogu/mcp      ← 대전 서구
https://djmcp.up.railway.app/all/mcp        ← 통합(자치구 교차검색 · 기관 태깅)
```

**→ [지금 붙여보기: 클라이언트별 연결 가이드](connect/)**

이 레포는 **문서 전용(docs-only)** 입니다. 서버·파이프라인 코드는 비공개이며, 여기서 공개하는 것은
**데이터 접근(MCP)과 방법론** — "행정문서를 AI가 오류 없이 읽게 만들려면 무엇을 검증해야 하는가"에
대한 설계 사고입니다.

## 무엇을 할 수 있나

17개 도구로 대전 자치구의 공개 행정정보를 질의합니다:

| 축 | 내용 | 도구 예시 |
|---|---|---|
| 문서 축 | 구청 홈페이지 공개 문서 7.8만여 건(중구 5.5만·서구 2.3만) | `search_documents` `get_document` `topic_timeline` |
| 예산 축 | 세출예산서 계층 트리(부서→정책→단위→세부사업, 증감 분해) | `query_budget` `get_budget_page` |
| 재정 정형 축 | 지방재정365 확정 재정데이터(자치구별 편성·집행 이력) | `query_finance` `list_finance` |
| 자치법규 축 | 소속 구 자치법규(중구 472·서구 568, 일일 증분) | `search_ordinance` `get_ordinance` |
| 전국 법규 축 | **전국 자치법규 161,602건 + 국가법령 5,599건** — 타 지자체 조례 비교·상위법 확인 | `search_law_nationwide` `get_law_nationwide` |

## 답변 3원칙 — 서비스 계약

모든 도구 응답은 다음을 계약으로 지킵니다(서버가 강제):

1. **모든 사실에 근거 문서 명시** — source·URL·기관 인용 동반
2. **근거 기반 vs 추론 구분 표시** — 해석·제안은 근거와 섞이지 않게 표시
3. **근거 없으면 "확인되지 않습니다"** — 추측으로 메꾸지 않음

이 계약이 어떻게 검증되는지가 이 레포의 알맹이입니다 → [무결성 렌즈 5문항](docs/methodology.md)

## 데이터 스토리 (시각화)

- [전국 조례 지형도](https://hippo0805.github.io/daejeon-mcp/viz/ordinance-landscape.html) — 화제 정책 10개 주제, 전국 266개 기관의 현행 조례 보유 현황
- [주민참여예산 19년](https://hippo0805.github.io/daejeon-mcp/viz/junggu-participatory-budget.html) — 대전 중구 2007~2026, 흩어진 문서를 이어 읽은 제도의 성장기 · 연도별 선정 사업 341건 전체 목록
- [중구 예산 20년](https://hippo0805.github.io/daejeon-mcp/viz/junggu-budget-20years.html) — 1,240억→7,230억, 성장의 대부분을 만든 복지(35%→67%)
- [행정의 관심사 10년](https://hippo0805.github.io/daejeon-mcp/viz/junggu-keyword-trends.html) — 문서 제목 4.9만 건으로 본 코로나의 파도·재개발의 지속·AI의 급부상
- [닿지 않는 통지](https://hippo0805.github.io/daejeon-mcp/viz/junggu-public-notice.html) — 공시송달 5,455건, 고시의 23%가 '전달 실패'의 기록
- [102개 위원회, 시민 861명](https://hippo0805.github.io/daejeon-mcp/viz/junggu-committees.html) — 주민참여의 실제 지도: 참여 자리·성별 기준 점검 (공무원 실무용)

## 문서

- [methodology.md](docs/methodology.md) — 무결성 렌즈 5문항: 수집·변환·비식별·서빙·교차검증, 단계마다 계측기·오라클·골든
- [pipeline.md](docs/pipeline.md) — 수집→변환→비식별→색인→서빙 파이프라인과 교차검증 계층
- [architecture.md](docs/architecture.md) — 멀티사이트 구조·정형 축·증분 갱신 (다이어그램)
- [results.md](docs/results.md) — 검증 지표 실측치
- [connect/](connect/) — Claude Desktop · Claude Code · 기타 클라이언트 연결 가이드

## 고지

- **비공식 서비스**: 이 서비스는 대전 자치구의 공식 채널이 아닙니다. 원 출처는 각 구청 홈페이지·
  지방재정365·법제처 국가법령정보이며, 모든 답변은 근거 문서를 명시합니다. **법적 효력이 필요한
  사안은 반드시 원 출처에서 확인하세요.** 자치법규는 응답에 `데이터기준일`이 동반되며, 최종 확인은
  항상 [law.go.kr](https://www.law.go.kr) 원문입니다.
- **가용성**: 개인·비영리 운영, SLA 없음(best-effort). 성실 운영 장치(자가복구 워치독·업타임 감시·
  일일 델타 갱신)는 [methodology](docs/methodology.md)에 공개돼 있습니다. 배포 전환 시 수십 초의
  일시 오류(502)가 있을 수 있습니다.
- **레이트 리밋**: IP당 분당 20회·일 500회. 초과 시 429 응답에 한도·재시도 시점을 정직하게
  안내합니다. NAT 뒤 조직(기관 내부망 등)은 한 IP를 공유해 체감 한도가 낮아질 수 있습니다 —
  실사용에 부족하면 [Issues](../../issues)로 알려주세요.
- **쿼리로그 수집**: 품질 개선·결함 탐지(주간 교차검증 스캔) 목적으로 도구 호출 로그(질의·응답
  요약)를 수집합니다. 저장 시 PII 마스킹, **보존 180일**(초과분 자동 삭제).
- **원문 PII 비식별**: 서빙 문서는 수집 단계에서 개인정보를 마스킹합니다(과소=유출·과잉=행정데이터
  손상, 양방향 모두 오류로 취급하는 게이트로 검증).

## 출처·라이선스

- 구청 공개 자료(문서 축): 각 구청(중구·서구)이 [공공누리(KOGL)](https://www.kogl.or.kr) 기준으로
  개방한 공공저작물 — 문서별 유형 표시를 따르며, 인용 시 출처(기관명·URL)가 항상 동반됩니다.
- 재정 정형 축: [지방재정365](https://lofin365.go.kr) 공개 재정데이터 — 모든 수치 인용에
  [기관·지방재정365·상품명·연도] 출처 명시.
- 자치법규 축: 법제처 국가법령정보 공동활용 OPEN API — 인용에 [기관·자치법규명·공포번호·시행일자·
  법제처] + 원문 링크 동반.
- **이 레포의 문서·다이어그램: [CC BY 4.0](LICENSE)** — 출처 표시만으로 자유롭게 재사용하세요.
- 서버·파이프라인 코드 및 gov-doc-toolkit: 비공개.

## 피드백·문의 — 단일 창구

연결 문의 · 오류 제보 · 남용 신고 · 데이터 정정 요청: **[GitHub Issues](../../issues)**

오류 제보는 품질 루프의 원료입니다: 실사용 피드백 → 실패 재현 골든 추가 → 수정 → 배포.
(중구 온보딩에서 피드백 13건 전량 이 루프로 수정한 실증이 [results.md](docs/results.md)에 있습니다.)

---

**Team Hippo** · [team-hippo.com](https://team-hippo.com) — 정보격차 해소·AI 소외계층 지원을 우선하는
사회적 가치 기반 프로젝트입니다.
