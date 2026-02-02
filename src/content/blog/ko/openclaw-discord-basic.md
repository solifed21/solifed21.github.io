---
title: "OpenClaw 디스코드 봇 연동: 가성비 셋팅 실전 가이드"
description: "OAuth 인증으로 비용 부담 없이 디스코드에서 AI 에이전트를 사용하는 방법입니다. 토큰 설정부터 페어링 승인 명령어까지 완벽하게 정리했습니다."
pubDate: 2026-02-02T14:00:00+09:00
category: "AI"
tags: ["OpenClaw", "Discord", "디스코드봇", "가성비", "가이드"]
---

비싼 API 키 없이, 내가 이미 쓰고 있는 구독형 AI를 디스코드와 연결해 봅시다!

## 1단계: 디스코드 봇 토큰 설정

가장 먼저 발급받은 봇 토큰을 OpenClaw에 등록해야 합니다.

```bash
# 방법 1: 설정 도우미 사용 (추천)
openclaw configure
# 메뉴에서 'channels' -> 'discord'를 찾아 토큰 입력

# 방법 2: 환경 변수로 즉시 등록
export DISCORD_BOT_TOKEN="여기에_토큰_입력"
```

## 2단계: 서비스 가동 및 상태 확인

설정 후 디스코드 채널을 활성화해야 합니다.

```bash
# 게이트웨이 재시작 (설정 반영)
openclaw gateway restart

# 채널 연결 상태 확인
openclaw channels status --probe
```

## 3단계: 첫 대화와 페어링 승인 (매우 중요)

디스코드에서 봇에게 DM을 보내면 처음에는 응답이 없을 수 있습니다. 보안을 위한 **'페어링'** 과정이 필요하기 때문입니다.

```bash
# 1. 봇에게 DM을 보냅니다 (예: "안녕")
# 2. 터미널에서 대기 중인 페어링 목록 확인
openclaw pairing list

# 3. 발급된 코드를 승인합니다
openclaw pairing approve discord <여기에_코드_입력>
```

## 4단계: 가성비 에이전트 매핑

특정 채널에만 에이전트를 연결하고 싶다면:

```bash
# 특정 채널 ID에 에이전트 이름 매핑
openclaw config set channels.discord.agentMappings.[0].channelId "채널ID"
openclaw config set channels.discord.agentMappings.[0].agentName "main"
```

---
이제 디스코드에서 `!명령어` 또는 `@봇멘션`을 통해 똑똑한 AI 비서를 만나보세요! ㅎㅎ 🛡️
