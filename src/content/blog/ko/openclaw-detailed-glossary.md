---
title: "OpenClaw 상세 용어 가이드: Gateway부터 Sandbox까지"
description: "OpenClaw의 핵심 개념들을 카테고리별로 나누어 상세히 설명합니다. 각 용어와 관련된 실전 명령어도 함께 정리했습니다."
pubDate: 2026-02-02T13:00:00+09:00
category: "AI"
tags: ["OpenClaw", "용어사전", "Gateway", "Sandbox", "개념정리"]
---

OpenClaw를 마스터하기 위해 반드시 알아야 할 핵심 용어들입니다.

## 🏗️ 시스템 아키텍처

### Gateway (게이트웨이)
모든 통신이 모이는 중앙 서버입니다.
- 💡 **명령어:** `openclaw gateway start / stop / restart`

### Node (노드)
게이트웨이에 연결되는 장치(PC, 스마트폰 등)입니다.
- 💡 **명령어:** `openclaw nodes list`

### Sandbox (샌드박스)
에이전트가 코드를 실행하는 격리된 안전한 공간입니다.
- 💡 **명령어:** `openclaw sandbox status`

---

## 🤖 에이전트 및 세션

### Agent (에이전트)
AI 모델과 설정이 결합된 독립적인 인스턴스입니다.
- 💡 **명령어:** `openclaw agents add / delete / list`

### Session (세션)
대화의 맥락이 유지되는 개별 채팅방 단위입니다.
- 💡 **명령어:** `openclaw sessions list`

### Skill (스킬)
에이전트에게 추가하는 새로운 기능 패키지입니다.
- 💡 **명령어:** `openclaw skills install <이름>`

---

## 🔒 보안 및 인증

### Pairing (페어링)
새로운 장치나 채널을 게이트웨이에 등록하는 인증 과정입니다.
- 💡 **명령어:** `openclaw pairing approve discord <코드>`

### Elevated Mode (상승 모드)
샌드박스 제한을 해제하고 시스템 권한을 사용하는 모드입니다.
- 💡 **TUI 명령어:** `/elevated on / off`

---

## 📡 채널 및 통신

### Channel (채널)
Discord, WhatsApp 등 AI와 대화하는 창구입니다.
- 💡 **명령어:** `openclaw channels status --probe`

### Payload (페이로드)
메시지에 담긴 실제 데이터 알맹이를 뜻합니다.
- 💡 **명령어:** `openclaw logs --verbose` (데이터 흐름 확인)
