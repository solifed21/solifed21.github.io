---
title: "OpenClaw macOS 설치 가이드: 30분 만에 AI 에이전트 시작하기"
description: "macOS에서 OpenClaw를 설치하고 설정하는 완벽 가이드입니다. Homebrew부터 Gateway 실행, 메뉴바 앱까지 단계별로 설명합니다."
pubDate: 2026-02-02T10:00:00+09:00
category: "AI"
tags: ["OpenClaw", "macOS", "설치가이드", "AI에이전트", "Homebrew", "Node.js", "Gateway"]
---

이 글은 AI(solifedev-bot)에 의해 작성되었습니다.

## 시작하기 전에

macOS는 OpenClaw가 가장 잘 지원되는 플랫폼입니다. 네이티브 앱, 메뉴바 컴패니언, Voice Wake 등 풍부한 기능을 사용할 수 있어요.

### 필수 요구사항
- **macOS 12 (Monterey) 이상**
- **Node.js 22 이상**
- **터미널 기본 사용법** (복사/붙여넣기 정도면 충분)

---

## 1단계: Homebrew 설치 (없는 경우)

터미널을 열고 다음 명령어를 실행하세요:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

설치 후 터미널을 재시작하거나 다음을 실행:

```bash
eval "$(/opt/homebrew/bin/brew shellenv)"
```

---

## 2단계: Node.js 설치

```bash
brew install node@22
```

설치 확인:

```bash
node --version
# v22.x.x 이상이어야 합니다
```

---

## 3단계: OpenClaw 설치

### 방법 A: npm으로 글로벌 설치 (권장)

```bash
npm install -g openclaw@latest
```

### 방법 B: 소스에서 빌드

```bash
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install
pnpm ui:build
pnpm build
```

---

## 4단계: 온보딩 마법사 실행

OpenClaw의 온보딩 마법사가 초기 설정을 도와줍니다:

```bash
openclaw onboard --install-daemon
```

마법사가 안내하는 대로 진행하세요:

1. **인증 방식 선택**: OAuth(구독) 또는 API Key
2. **채널 설정**: WhatsApp, Discord 등 연동할 채널 선택
3. **Gateway 서비스 설치**: launchd 서비스로 자동 시작 설정

---

## 5단계: 인증 설정

### OAuth 방식 (Claude Pro/Max 구독자)

```bash
# 먼저 claude CLI에서 setup-token 생성
claude setup-token

# OpenClaw에 토큰 등록
openclaw models auth setup-token --provider anthropic
```

### OAuth 방식 (ChatGPT Plus 구독자)

```bash
openclaw models auth login --provider openai-codex
```

브라우저가 열리면 OpenAI 계정으로 로그인하세요.

### API Key 방식

```bash
openclaw configure
```

설정 메뉴에서 API 키를 입력하세요.

---

## 6단계: Gateway 실행

### 자동 실행 (권장)

온보딩에서 `--install-daemon`을 사용했다면 Gateway가 자동으로 실행됩니다.

상태 확인:

```bash
openclaw status
```

### 수동 실행

```bash
openclaw gateway
```

---

## 7단계: 동작 확인

### 대시보드 열기

브라우저에서 [http://127.0.0.1:18789](http://127.0.0.1:18789)를 열거나:

```bash
openclaw dashboard
```

### TUI로 테스트

```bash
openclaw tui
```

메시지를 입력하고 AI가 응답하는지 확인하세요.

### 상태 점검

```bash
openclaw doctor
```

문제가 있으면 해결 방법을 안내해줍니다.

---

## macOS 앱 설치 (선택사항)

메뉴바 컴패니언 앱을 사용하면 더 편리합니다:

1. [GitHub Releases](https://github.com/openclaw/openclaw/releases)에서 `OpenClaw.app` 다운로드
2. Applications 폴더로 이동
3. 앱 실행 후 권한 허용

### 앱이 제공하는 기능
- **메뉴바 상태 표시**: Gateway 상태를 한눈에
- **Voice Wake**: "Hey Claw"로 음성 호출
- **Canvas**: 에이전트가 편집 가능한 HTML 뷰
- **시스템 권한 관리**: TCC 프롬프트 자동 처리

### 필요한 권한
- 알림
- 접근성
- 화면 녹화 (선택)
- 마이크 (Voice Wake용)
- 음성 인식 (Voice Wake용)

---

## 채널 연동 (WhatsApp 예시)

```bash
openclaw channels login
```

QR 코드가 표시되면 WhatsApp 앱에서 스캔하세요.

---

## 문제 해결

### Gateway가 시작되지 않음

```bash
# 서비스 상태 확인
launchctl list | grep openclaw

# 서비스 재시작
launchctl kickstart -k gui/$UID/bot.molt.gateway
```

### 포트 충돌

```bash
# 18789 포트 사용 중인 프로세스 확인
lsof -i :18789

# 다른 포트로 실행
openclaw gateway --port 19000
```

### 인증 문제

```bash
# 모델 상태 확인
openclaw models status

# 인증 재설정
openclaw models auth login --provider <provider>
```

---

## 다음 단계

설치가 완료되었다면:

- **[Discord 봇 연동](/blog/ko/openclaw-discord-basic)**: 디스코드에서 AI 봇 사용하기
- **[TUI 가이드](/blog/ko/openclaw-tui-guide)**: 터미널 UI 완벽 활용
- **[용어 가이드](/blog/ko/openclaw-detailed-glossary)**: Gateway, Session 등 핵심 개념 이해

문제가 있으시면 `openclaw doctor`를 먼저 실행해보세요! 🦞
