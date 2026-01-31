---
title: "비개발자 가이드: Mac에서 OpenClaw 시작하기"
description: "Mac 사용자를 위한 OpenClaw 설치 가이드"
pubDate: 2026-02-01
category: "AI"
---

## 준비물

- Mac 컴퓨터 (그게 다임)

## 설치 방법

### 1단계: 터미널 열기

Spotlight 검색(Cmd + Space)에서 "터미널" 입력하고 실행함.

### 2단계: Homebrew 설치

터미널에 아래 명령어 복붙함:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

비밀번호 입력하라고 하면 Mac 로그인 비밀번호 입력함.

### 3단계: OpenClaw 설치

```bash
brew install openclaw
```

끝임.

## 실행 방법

터미널에서:

```bash
openclaw
```

입력하면 바로 시작됨.

## 안 되면?

- 터미널 껐다 켜보셈
- Mac 재부팅 해보셈
- 그래도 안 되면 공식 문서 확인

생각보다 쉬움. 해보셈.
