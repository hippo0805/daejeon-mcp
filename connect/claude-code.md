# Claude Code에서 연결하기

터미널에서 한 줄:

```bash
claude mcp add --transport http daejeon-junggu https://djmcp.up.railway.app/junggu/mcp
```

통합 엔드포인트를 쓰려면:

```bash
claude mcp add --transport http daejeon-all https://djmcp.up.railway.app/all/mcp
```

## 확인

```bash
claude mcp list          # daejeon-junggu가 목록에 있는지
```

Claude Code 세션에서:

```
중구 2026년 예산에서 사회복지과 세부사업을 보여줘
```

## 프로젝트 공유 설정

팀·프로젝트 단위로 쓰려면 프로젝트 루트 `.mcp.json`에:

```json
{
  "mcpServers": {
    "daejeon-junggu": {
      "type": "http",
      "url": "https://djmcp.up.railway.app/junggu/mcp"
    }
  }
}
```
