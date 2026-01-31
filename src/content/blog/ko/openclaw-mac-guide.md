---
title: "비개발자 가이드: Mac에서 OpenClaw 완벽 설치하기"
description: "Mac 사용자를 위한 OpenClaw 설치부터 디스코드 봇 연동까지 완벽 가이드"
pubDate: 2026-02-01T10:00:00+09:00
category: "AI"
---

## 🎯 이 글의 목표

이 글 하나만 따라하면 Mac에서 OpenClaw 설치부터 디스코드 봇 연동까지 **완벽하게** 끝낼 수 있음. 비개발자 기준으로 작성했으니 겁먹지 말고 따라오셈.

예상 소요 시간: **30분 ~ 1시간**

---

## 📋 준비물

- Mac 컴퓨터 (macOS 12 Monterey 이상 권장)
- 인터넷 연결
- AI API 키 (Claude 또는 OpenAI) - 없으면 아래에서 발급 방법 설명함

---

## 🔧 1단계: 터미널 열기

터미널은 Mac의 "명령어 입력창"임. 무서워 보이지만 그냥 텍스트 입력하는 창일 뿐임.

**여는 방법:**
1. `Cmd + Space` 눌러서 Spotlight 검색 열기
2. "터미널" 또는 "Terminal" 입력
3. Enter 눌러서 실행

검은 배경(또는 흰 배경)에 텍스트 커서가 깜빡이는 창이 뜨면 성공임.

**💡 팁**: 터미널 자주 쓸 거니까 Dock에 고정해두면 편함. 터미널 아이콘 우클릭 → "옵션" → "Dock에 유지" 선택.

---

## 🍺 2단계: Homebrew 설치

Homebrew는 Mac용 프로그램 설치 도구임. 앱스토어 같은 건데 개발자 도구 전문임. OpenClaw 설치하려면 이게 먼저 필요함.

### 이미 설치되어 있는지 확인

터미널에 아래 명령어 입력하고 Enter:

```bash
brew --version
```

`Homebrew 4.x.x` 같은 버전 정보가 나오면 **이미 설치된 거**니까 3단계로 넘어가면 됨.

`command not found: brew` 라고 나오면 설치 필요함.

### Homebrew 설치하기

터미널에 아래 명령어를 **통째로** 복사해서 붙여넣기함:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

**진행 과정:**
1. Enter 누르면 설치 시작됨
2. 중간에 Mac 로그인 비밀번호 입력하라고 함 (입력해도 화면에 안 보이는 게 정상임)
3. "Press RETURN to continue" 나오면 Enter 누름
4. 설치 완료까지 5~10분 정도 걸림

### ⚠️ Apple Silicon Mac (M1/M2/M3) 사용자 필수 작업

설치 끝나면 터미널에 이런 메시지가 나올 수 있음:

```
==> Next steps:
- Run these commands in your terminal to add Homebrew to your PATH
```

아래 명령어들을 **순서대로** 실행해야 함:

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

이거 안 하면 `brew` 명령어가 안 먹음. 진짜 중요함.

### 설치 확인

```bash
brew --version
```

버전 정보 나오면 성공임.

---

## 🦞 3단계: OpenClaw 설치

드디어 본론임. 터미널에 입력:

```bash
brew install openclaw
```

설치 완료까지 2~3분 정도 걸림. 중간에 여러 줄 텍스트가 쭉 나오는데 정상임.

### 설치 확인

```bash
openclaw --version
```

버전 정보 나오면 설치 완료임.

---

## 🔑 4단계: API 키 설정

OpenClaw가 AI 기능을 쓰려면 AI 서비스의 API 키가 필요함. Claude(Anthropic) 또는 OpenAI 중 하나 선택하면 됨.

### Claude API 키 발급 (추천)

1. [console.anthropic.com](https://console.anthropic.com) 접속
2. 회원가입 또는 로그인
3. 좌측 메뉴에서 "API Keys" 클릭
4. "Create Key" 버튼 클릭
5. 이름 입력하고 생성
6. 생성된 키 복사 (한 번만 보여주니까 꼭 저장해둘 것!)

### OpenAI API 키 발급

1. [platform.openai.com](https://platform.openai.com) 접속
2. 회원가입 또는 로그인
3. 우측 상단 프로필 → "View API keys"
4. "Create new secret key" 클릭
5. 생성된 키 복사

### API 키 등록

터미널에서 OpenClaw 설정 파일 생성:

```bash
mkdir -p ~/.openclaw
nano ~/.openclaw/config.yaml
```

nano 에디터가 열리면 아래 내용 입력 (Claude 사용 시):

```yaml
ai:
  provider: anthropic
  api_key: sk-ant-xxxxx  # 여기에 본인 API 키 입력
```

OpenAI 사용 시:

```yaml
ai:
  provider: openai
  api_key: sk-xxxxx  # 여기에 본인 API 키 입력
```

저장하고 나가기: `Ctrl + X` → `Y` → `Enter`

**💡 더 안전한 방법**: 환경변수 사용

```bash
echo 'export ANTHROPIC_API_KEY="sk-ant-xxxxx"' >> ~/.zshrc
source ~/.zshrc
```

이렇게 하면 config 파일에 키를 직접 안 써도 됨.

---

## 🚀 5단계: OpenClaw 실행

터미널에서:

```bash
openclaw
```

환영 메시지가 나오면 성공임! 이제 AI에게 뭐든 시켜볼 수 있음.

### 첫 번째 테스트

OpenClaw 프롬프트에서 이렇게 입력해봄:

```
현재 폴더에 있는 파일 목록 보여줘
```

파일 목록이 나오면 정상 작동하는 거임.

---

## 🤖 6단계: 디스코드 봇 연동 (선택사항)

OpenClaw를 디스코드 봇으로 연동하면 디스코드에서 AI에게 명령을 내릴 수 있음. 아침마다 뉴스 요약 받기 같은 자동화가 가능해짐.

### 디스코드 봇 생성

1. [Discord Developer Portal](https://discord.com/developers/applications) 접속
2. "New Application" 클릭
3. 이름 입력 (예: "MyOpenClaw")하고 생성
4. 좌측 메뉴에서 "Bot" 클릭
5. "Add Bot" → "Yes, do it!" 클릭
6. "Reset Token" 클릭해서 봇 토큰 복사 (이것도 한 번만 보여줌!)

### 봇 권한 설정

1. 같은 페이지에서 "Privileged Gateway Intents" 섹션 찾기
2. "Message Content Intent" 활성화 (중요!)
3. 저장

### 봇을 서버에 초대

1. 좌측 메뉴에서 "OAuth2" → "URL Generator" 클릭
2. SCOPES에서 "bot" 체크
3. BOT PERMISSIONS에서 필요한 권한 체크:
   - Send Messages
   - Read Message History
   - Use Slash Commands
4. 생성된 URL 복사해서 브라우저에 붙여넣기
5. 봇을 추가할 서버 선택하고 승인

### OpenClaw에 디스코드 연동

config.yaml에 추가:

```yaml
integrations:
  discord:
    enabled: true
    bot_token: "봇_토큰_여기에_입력"
    prefix: "!"  # 명령어 앞에 붙일 기호
```

### 봇 실행

```bash
openclaw --discord
```

디스코드 서버에서 `!help` 입력해서 봇이 응답하면 성공임.

---

## 🔥 Troubleshooting: 자주 겪는 문제들

### ❌ "command not found: brew"

**원인**: Homebrew PATH 설정이 안 됨 (특히 M1/M2/M3 Mac)

**해결**:
```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
source ~/.zprofile
```

### ❌ "command not found: openclaw"

**원인**: 설치가 제대로 안 됐거나 터미널 재시작 필요

**해결**:
```bash
# 터미널 완전히 껐다가 다시 열기
# 그래도 안 되면 재설치
brew reinstall openclaw
```

### ❌ "API key invalid" 또는 인증 에러

**원인**: API 키가 잘못됐거나 만료됨

**해결**:
1. API 키 다시 확인 (복사할 때 앞뒤 공백 없는지)
2. API 서비스 대시보드에서 키가 활성 상태인지 확인
3. 결제 정보가 등록되어 있는지 확인 (무료 크레딧 소진됐을 수 있음)

### ❌ "Permission denied"

**원인**: 권한 문제

**해결**:
```bash
sudo chown -R $(whoami) ~/.openclaw
```

### ❌ 디스코드 봇이 응답 안 함

**원인**: 여러 가지 가능성

**체크리스트**:
1. 봇이 서버에 제대로 초대됐는지 확인
2. "Message Content Intent" 활성화했는지 확인
3. 봇 토큰이 정확한지 확인
4. OpenClaw가 `--discord` 옵션으로 실행 중인지 확인

### ❌ Homebrew 설치 중 에러

**원인**: Xcode Command Line Tools 미설치

**해결**:
```bash
xcode-select --install
```

팝업 나오면 "설치" 클릭하고 완료 후 Homebrew 재설치.

---

## 🎉 설치 완료! 이제 뭐 함?

1. **기본 사용법 익히기**: 간단한 요청부터 시작해봄 ("현재 날짜 알려줘", "이 폴더에 test.txt 파일 만들어줘")
2. **프로젝트에 적용**: 실제 작업 폴더에서 OpenClaw 실행해보기
3. **자동화 설정**: 자주 하는 작업을 자동화하는 방법 찾아보기

---
