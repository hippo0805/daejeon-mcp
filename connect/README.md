# 지금 붙여보기 — 클라이언트별 연결 가이드

서버는 **MCP streamable HTTP** 방식입니다. 가입·키·비용 없이 URL만으로 연결됩니다.

| 엔드포인트 | 범위 |
|---|---|
| `https://djmcp.up.railway.app/junggu/mcp` | 대전 중구 |
| `https://djmcp.up.railway.app/all/mcp` | 통합(현재 중구와 동일 — 사이트 추가 시 자동 확대) |

- [Claude Desktop](claude-desktop.md) — 커넥터 추가(코드 불필요, 권장)
- [Claude Code](claude-code.md) — CLI 한 줄
- [기타 클라이언트](other-clients.md) — Cursor·기타 MCP 지원 도구·직접 호출

## 연결 후 첫 질문 예시

```
중구 청년 지원 정책 관련 문서를 찾아줘
2026년 사회복지과 예산 총액과 세부사업을 보여줘
주정차 단속 관련 자치법규를 찾아줘
공영주차장 관련 고시공고 타임라인을 보여줘
```

## 잘 안 되면

- 429가 뜨면: IP당 분당 20회·일 500회 한도입니다. 응답의 `retry_after_seconds` 뒤에 재시도하세요.
- 일시적 502: 배포 전환 중일 수 있습니다(수십 초). 잠시 후 재시도하세요.
- 그 외 문제: [GitHub Issues](../../../issues)에 남겨주세요 — 연결 문의도 환영합니다.
