---
title: "OpenClaw 디스코드 전문 가이드: 공식 문서 핵심 요약"
description: "고급 사용자를 위한 디스코드 채널 설정 완벽 정리. 진단 명령어부터 리액션, 스레드, 히스토리 관리까지 디테일한 옵션을 다룹니다."
pubDate: 2026-02-02T16:00:00+09:00
category: "AI"
tags: ["OpenClaw", "Discord", "전문가", "설정", "공식문서"]
---

OpenClaw의 디스코드 연동 기능을 100% 활용하기 위한 전문가용 가이드입니다.

## 🛠️ 전문 진단 명령어

봇이 대답을 안 하거나 문제가 생겼을 때 가장 먼저 입력해야 할 명령어입니다.

```bash
# 채널의 모든 기능(능력치)과 상태를 정밀 분석
openclaw channels status --probe

# 디스코드 인텐트(Intents) 및 권한 오류 확인
openclaw doctor

# 최근 로그 실시간 확인 (문제 해결의 열쇠)
openclaw logs --follow
```

## ⚙️ 주요 설정 옵션 (config.yaml)

### 1. DM 및 길드 정책
```yaml
channels:
  discord:
    groupPolicy: "allowlist" # 허용된 채널에서만 동작
    dm:
      policy: "pairing" # 보안을 위한 페어링 승인 필수
```

### 2. 히스토리 컨텍스트
AI가 대화의 앞 내용을 얼마나 기억하게 할지 설정합니다.
```bash
# 멘션 시 이전 메시지 20개까지 읽어옴
openclaw config set channels.discord.historyLimit 20
```

### 3. 리플라이 모드
AI의 답변을 원본 메시지의 '답장' 형태로 달지 결정합니다.
```bash
# 모든 조각 메시지를 답장 형태로 전송
openclaw config set channels.discord.replyToMode "all"
```

## 🔐 보안 및 승인

에이전트가 내 시스템에서 명령어를 실행(`exec`)하기 전, 나에게 확인 버튼을 띄우게 할 수 있습니다.

```yaml
channels:
  discord:
    execApprovals:
      enabled: true
      approvers: ["내_디스코드_유저_ID"]
```

---
더 자세한 기술적 사양은 [공식 문서](https://docs.openclaw.ai/channels/discord)를 참고하세요! 🦞
