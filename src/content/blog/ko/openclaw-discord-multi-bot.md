---
title: "OpenClaw 디스코드 멀티봇: 봇 2개로 핑퐁 시스템 만들기"
description: "서로 다른 성격의 AI 봇 두 개가 대화하도록 설정하는 방법입니다. 환경 변수를 활용한 다중 인스턴스 구동 명령어를 상세히 설명합니다."
pubDate: 2026-02-02T15:00:00+09:00
category: "AI"
tags: ["OpenClaw", "Discord", "멀티봇", "핑퐁", "고급설정"]
---

하나의 게이트웨이에서 여러 개의 봇을 돌리거나, 서로 다른 에이전트끼리 대화하게 하고 싶으신가요?

## 1단계: 멀티 에이전트 생성

먼저 서로 대화할 두 개의 AI 페르소나를 만듭니다.

```bash
# 봇 A와 봇 B 추가
openclaw agents add bot-a
openclaw agents add bot-b
```

## 2단계: 다중 인스턴스 실행 (포트 구분)

두 개의 봇이 각자 독립적인 환경에서 돌게 하려면 환경 변수(`OPENCLAW_STATE_DIR`)와 포트 번호를 다르게 주어야 합니다.

### 터미널 1 (봇 A 전용)
```bash
OPENCLAW_STATE_DIR=~/.openclaw-a \
OPENCLAW_GATEWAY_TOKEN="token-a" \
openclaw gateway start --port 18789
```

### 터미널 2 (봇 B 전용)
```bash
OPENCLAW_STATE_DIR=~/.openclaw-b \
OPENCLAW_GATEWAY_TOKEN="token-b" \
openclaw gateway start --port 18790
```

## 3단계: 봇 간의 상호작용 허용

디스코드 봇은 기본적으로 다른 봇의 메시지를 무시합니다. 이걸 풀어주어야 핑퐁이 가능합니다.

```bash
# 특정 설정 파일에서 봇 허용 활성화
openclaw config set channels.discord.allowBots true
```

## 4단계: 핑퐁 트리거

에이전트 한쪽에 **'상대방 봇이 말을 걸면 답변하라'**는 지침을 `SOUL.md`나 시스템 프롬프트에 넣어주세요.

---
**⚠️ 주의:** 무한 루프에 빠져 API 토큰이 순식간에 녹아버릴 수 있으니, 대화 횟수를 제한하거나 쿨다운 설정을 꼭 확인하세요! ㅋㅋ 🛡️
