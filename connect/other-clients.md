# 기타 MCP 클라이언트 · 직접 호출

## MCP streamable HTTP를 지원하는 클라이언트 (Cursor 등)

MCP 서버 설정에 URL만 넣으면 됩니다. 일반형(예시):

```json
{
  "mcpServers": {
    "daejeon-junggu": {
      "url": "https://djmcp.up.railway.app/junggu/mcp"
    }
  }
}
```

## stdio만 지원하는 구형 클라이언트

`mcp-remote` 브리지로 연결할 수 있습니다:

```json
{
  "mcpServers": {
    "daejeon-junggu": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://djmcp.up.railway.app/junggu/mcp"]
    }
  }
}
```

## 직접 호출(개발자용) — 연결 없이 동작 확인

MCP는 JSON-RPC 2.0입니다. curl로 초기화 핸드셰이크:

```bash
curl -s -X POST https://djmcp.up.railway.app/junggu/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-03-26","capabilities":{},"clientInfo":{"name":"curl","version":"0"}}}'
```

서버 정보와 capabilities가 오면 정상입니다.

## 한도·계약 (모든 클라이언트 공통)

- IP당 분당 20회·일 500회 — 초과 시 429 + `Retry-After` + 정직한 한도 안내 JSON
- 요청 본문 256KB·도구 인자 2,000자 상한
- 모든 사실 응답에 근거 문서(source·URL) 동반 — 근거가 없으면 "확인되지 않습니다"로 거절
