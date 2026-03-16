# SalesMap MCP 설치 가이드

> Claude Code에서 세일즈맵 데이터를 조회·관리할 수 있게 해주는 MCP 서버입니다.
> 설치 시간: 약 10~15분

---

## 사전 준비

아래 두 가지가 설치되어 있어야 합니다:

| 도구 | 확인 방법 | 설치 안내 |
|------|-----------|-----------|
| **Node.js** (v20 이상) | 터미널에서 `node -v` 입력 | https://nodejs.org 에서 LTS 다운로드 |
| **Claude Code** | 터미널에서 `claude --version` 입력 | `npm install -g @anthropic-ai/claude-code` |

---

## 1단계: 서버 코드 받기

```bash
# 이 저장소를 클론합니다 (최초 1회)
git clone https://github.com/ejlee-0924/salesmap-mcp.git ~/work/apps/salesmap-mcp
```

> Private repo이므로 GitHub 접근 권한이 필요합니다. 관리자에게 초대를 요청하세요.

---

## 2단계: 서버 빌드

```bash
cd ~/work/apps/salesmap-mcp/server
npm install
npm run build
```

`build/index.js` 파일이 생성되면 성공입니다.

---

## 3단계: Claude Code에 MCP 서버 등록

`~/work/.claude/` 폴더가 없으면 먼저 만들어주세요:

```bash
mkdir -p ~/work/.claude
```

그다음 `~/work/.claude/settings.json` 파일을 만들고 아래 내용을 붙여넣으세요:

```json
{
  "mcpServers": {
    "salesmap-mcp": {
      "type": "stdio",
      "command": "node",
      "args": [
        "/Users/본인유저명/work/apps/salesmap-mcp/server/build/index.js"
      ],
      "env": {
        "BEARER_TOKEN_BEARERAUTH": "관리자에게 받은 토큰"
      }
    }
  }
}
```

### 본인 유저명 확인 방법

터미널에서 아래 명령어를 입력하면 나옵니다:

```bash
whoami
```

예를 들어 결과가 `hong`이면, 경로를 `/Users/hong/work/apps/...`으로 수정하세요.

---

## 4단계: 확인

```bash
cd ~/work
claude
```

Claude Code가 실행되면 아래와 같이 입력해보세요:

```
세일즈맵 딜 목록 보여줘
```

딜 목록이 표시되면 설치 완료입니다!

---

## 자주 묻는 질문

### Q. "command not found: claude" 에러가 나요
→ Claude Code가 설치되지 않았습니다. `npm install -g @anthropic-ai/claude-code`를 실행하세요.

### Q. "Cannot find module" 에러가 나요
→ 2단계에서 `npm run build`를 실행했는지 확인하세요. `build/index.js` 파일이 있어야 합니다.

### Q. settings.json 경로에서 유저명을 모르겠어요
→ 터미널에서 `whoami`를 입력하면 본인의 macOS 유저명이 나옵니다.

### Q. 홈 디렉토리(~)에서 claude를 실행하면 세일즈맵이 안 보여요
→ 정상입니다. 보안을 위해 `~/work` 디렉토리에서만 동작하도록 설정되어 있습니다. 반드시 `cd ~/work && claude`로 실행하세요.

---

## 보안 주의사항

- `settings.json` 파일에는 API 토큰이 포함되어 있습니다
- **이 파일을 절대 git에 커밋하거나 다른 사람에게 공유하지 마세요**
- 토큰이 유출된 것 같으면 즉시 관리자에게 알려주세요
