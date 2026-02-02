---
title: "OpenClaw macOS 설치 가이드: 명령어 한 줄로 시작하기"
description: "macOS 사용자를 위한 OpenClaw 설치 풀 코스입니다. Node.js 설정부터 게이트웨이 데몬 등록까지 실전 명령어를 정리했습니다."
pubDate: 2026-02-02T10:00:00+09:00
category: "AI"
tags: ["OpenClaw", "macOS", "설치가이드", "Homebrew", "CLI"]
---

macOS는 OpenClaw를 가장 쾌적하게 사용할 수 있는 환경입니다.

## 1단계: 필수 도구 설치

먼저 Homebrew와 Node.js 22 버전이 필요합니다.

```bash
# Homebrew 설치 (이미 있다면 패스)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Node.js 22 설치
brew install node@22

# PATH 설정 (M1/M2/M3 Mac 기준)
echo 'export PATH="/opt/homebrew/opt/node@22/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# 설치 확인
node --version # v22.x.x 인지 확인
```

## 2단계: OpenClaw 설치

```bash
# npm으로 글로벌 설치
npm install -g openclaw@latest

# 설치 확인
openclaw --version
```

## 3단계: 초기 설정 및 온보딩

```bash
# 온보딩 마법사 실행 (가장 추천하는 방법)
# 인증 방식, 사용할 채널, 서비스 등록까지 한 번에 처리합니다.
openclaw onboard --install-daemon
```

## 4단계: 게이트웨이 관리

```bash
# 게이트웨이 상태 확인
openclaw status

# 문제 발생 시 진단
openclaw doctor

# 로그 실시간 모니터링
openclaw logs --follow

# 게이트웨이 재시작
openclaw gateway restart
```

## 5단계: 첫 대화 (TUI)

```bash
# 터미널에서 바로 대화하기
openclaw tui
```

---
**팁:** `OpenClaw.app`을 별도로 다운로드하면 메뉴 바에서 훨씬 편하게 관리할 수 있습니다! 🦞
