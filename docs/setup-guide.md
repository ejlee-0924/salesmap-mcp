# SalesMap MCP 설치 가이드

> Claude Code에서 "세일즈맵 딜 보여줘"처럼 말하면 세일즈맵 데이터를 바로 조회할 수 있게 해주는 도구입니다.
> 설치 시간: 약 15~20분 (처음이면 30분)

---

## 터미널 여는 법

이 가이드의 모든 작업은 **터미널**이라는 앱에서 진행합니다.

1. `Cmd + Space`를 눌러 Spotlight 검색을 엽니다
2. **"터미널"** 또는 **"Terminal"**을 입력합니다
3. 엔터를 누르면 검은 창이 열립니다

> 터미널은 컴퓨터에 글자로 명령을 내리는 도구입니다.
> 아래 가이드에서 회색 박스 안의 명령어를 **한 줄씩 복사해서 붙여넣고 엔터**를 누르면 됩니다.
> 복사: `Cmd + C` / 터미널에 붙여넣기: `Cmd + V`

---

## 0단계: 사전 준비 (최초 1회)

### Node.js 설치 확인

터미널에 아래를 입력하세요:

```
node -v
```

- `v20.x.x` 또는 `v22.x.x` 같은 숫자가 나오면 → 이미 설치됨, 다음으로 넘어가세요
- `command not found`가 나오면 → 설치가 필요합니다:
  1. https://nodejs.org 에 접속
  2. **LTS** (왼쪽 초록 버튼) 클릭해서 다운로드
  3. 다운받은 파일 실행 → "계속" 눌러서 설치 완료
  4. **터미널을 닫았다가 다시 열고** `node -v`로 확인

### Git 설치 확인

```
git --version
```

- 숫자가 나오면 → OK
- `command not found`가 나오면 → 팝업이 뜨면서 "Xcode 명령어 도구를 설치하시겠습니까?" 물어봅니다. **"설치"** 를 누르세요 (5~10분 소요)

### Claude Code 설치 확인

```
claude --version
```

- 숫자가 나오면 → OK
- `command not found`가 나오면 → 아래 명령어로 설치:

```
npm install -g @anthropic-ai/claude-code
```

---

## 1단계: 작업 폴더 만들기

```
mkdir -p ~/work
```

> `mkdir`은 "폴더 만들기" 명령어입니다. 이미 있으면 아무 일도 안 일어나니 걱정 마세요.

---

## 2단계: 서버 코드 받기

```
git clone https://github.com/ejlee-0924/salesmap-mcp.git ~/work/apps/salesmap-mcp
```

> 이 명령어는 GitHub에서 코드를 내 컴퓨터로 다운로드합니다.
> Private 저장소이므로 GitHub 로그인 팝업이 뜰 수 있습니다. 본인 GitHub 계정으로 로그인하세요.
> ⚠️ 접근 권한이 없다는 에러가 나면 관리자(은지)에게 GitHub 초대를 요청하세요.

---

## 3단계: 서버 빌드

아래 명령어를 **한 줄씩** 순서대로 입력하세요:

```
cd ~/work/apps/salesmap-mcp/server
```

```
npm install
```

> 여러 줄의 텍스트가 쭉 나오면서 패키지를 설치합니다. 1~2분 걸릴 수 있어요. 끝날 때까지 기다리세요.

```
npm run build
```

> `build/` 폴더가 만들어지면 성공입니다.

---

## 4단계: 설정 파일 만들기

### 4-1. 본인 유저명 확인

```
whoami
```

화면에 나오는 단어를 메모하세요 (예: `hong`, `kimjs` 등). 아래에서 사용합니다.

### 4-2. 설정 폴더 만들기

```
mkdir -p ~/work/.claude
```

### 4-3. 설정 파일 생성

아래 명령어를 **통째로** 복사해서 붙여넣으세요:

```
cat > ~/work/.claude/settings.json << 'EOF'
{
  "mcpServers": {
    "salesmap-mcp": {
      "type": "stdio",
      "command": "node",
      "args": [
        "/Users/여기에유저명입력/work/apps/salesmap-mcp/server/build/index.js"
      ],
      "env": {
        "BEARER_TOKEN_BEARERAUTH": "여기에토큰입력"
      }
    }
  }
}
EOF
```

### 4-4. 유저명과 토큰 입력

아래 명령어에서 `여기에유저명입력`을 4-1에서 확인한 유저명으로 바꿔서 실행하세요:

```
sed -i '' 's/여기에유저명입력/본인유저명/g' ~/work/.claude/settings.json
```

**예시:** 유저명이 `hong`이면 →

```
sed -i '' 's/여기에유저명입력/hong/g' ~/work/.claude/settings.json
```

토큰도 같은 방식으로 입력합니다. 관리자에게 받은 토큰을 아래 `본인토큰`에 넣으세요:

```
sed -i '' 's/여기에토큰입력/본인토큰/g' ~/work/.claude/settings.json
```

### 4-5. 설정 확인

```
cat ~/work/.claude/settings.json
```

출력에서 아래 두 가지를 확인하세요:
- `/Users/본인유저명/work/apps/...` → 유저명이 제대로 들어갔는지
- `BEARER_TOKEN_BEARERAUTH` → 토큰이 제대로 들어갔는지

---

## 5단계: 실행 확인

```
cd ~/work
```

```
claude
```

Claude Code가 실행되면 아래와 같이 입력해보세요:

```
세일즈맵 딜 목록 보여줘
```

딜 목록이 표시되면 설치 완료입니다! 🎉

---

## 다음부터 사용할 때

매번 이렇게 두 줄만 입력하면 됩니다:

```
cd ~/work
claude
```

---

## 문제가 생겼을 때

| 증상 | 해결 방법 |
|------|-----------|
| `command not found: node` | 0단계의 Node.js 설치를 다시 해주세요 |
| `command not found: git` | 0단계의 Git 설치를 다시 해주세요 |
| `command not found: claude` | `npm install -g @anthropic-ai/claude-code` 실행 |
| `permission denied` 또는 `not found` (git clone 시) | 관리자에게 GitHub 초대 요청 |
| `Cannot find module` (claude 실행 후) | 3단계 `npm run build`를 다시 실행하세요 |
| 세일즈맵이 안 보여요 | `cd ~/work`에서 실행했는지 확인. 홈(~)에서는 안 됩니다 |
| 설정 파일이 잘못된 것 같아요 | `cat ~/work/.claude/settings.json`으로 내용 확인 후 관리자에게 공유 |

위에 없는 에러가 나면 **에러 메시지를 캡처해서 관리자에게 보내주세요.**

---

## ⚠️ 보안 주의사항

- `settings.json` 파일에는 API 토큰(비밀번호 같은 것)이 들어있습니다
- **이 파일을 슬랙, 이메일, 어디에도 공유하지 마세요**
- 토큰이 유출된 것 같으면 즉시 관리자에게 알려주세요
