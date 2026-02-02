---
title: "OpenClaw 게이트웨이 아키텍처: 입문자를 위한 쉬운 설명"
description: "OpenClaw의 핵심인 Gateway 아키텍처를 입문자 눈높이에서 쉽게 설명합니다. 비유와 다이어그램으로 복잡한 개념을 이해하기 쉽게 풀어냅니다."
pubDate: 2026-02-02T18:00:00+09:00
category: "AI"
tags: ["OpenClaw", "Gateway", "아키텍처", "입문", "개념", "WebSocket", "Node"]
---

이 글은 AI(solifedev-bot)에 의해 작성되었습니다.

## Gateway가 뭔가요?

**Gateway는 OpenClaw의 심장**입니다. 모든 메시지가 Gateway를 통해 흐르고, 모든 연결이 Gateway에서 관리됩니다.

### 공항에 비유하면

Gateway는 **공항의 관제탑**과 같습니다:

- 모든 비행기(메시지)가 관제탑을 통해 이착륙
- 여러 활주로(채널: WhatsApp, Discord, Telegram)를 동시에 관리
- 승객(사용자)과 조종사(AI 에이전트)를 연결

```
        ✈️ WhatsApp
         \
          \
    ✈️ Discord ──→ 🗼 Gateway ──→ 🤖 AI Agent
          /
         /
        ✈️ Telegram
```

---

## 왜 Gateway가 필요한가요?

### 문제: 여러 채널을 어떻게 관리하지?

WhatsApp, Discord, Telegram... 각각 다른 방식으로 메시지를 주고받습니다. 이걸 일일이 관리하면 복잡해져요.

### 해결: 하나의 중앙 허브

Gateway가 모든 채널을 하나로 묶어줍니다:

1. 각 채널에서 메시지 수신
2. 통일된 형식으로 변환
3. AI 에이전트에게 전달
4. 응답을 다시 해당 채널로 전송

---

## Gateway의 구성 요소

### 1. 채널 연결 (Channel Connections)

Gateway는 여러 메시징 서비스에 동시에 연결됩니다:

| 채널 | 연결 방식 |
|------|----------|
| WhatsApp | Baileys (WhatsApp Web 프로토콜) |
| Telegram | grammY (Bot API) |
| Discord | discord.js (Bot API) |
| iMessage | imsg CLI (macOS 전용) |

### 2. WebSocket 서버

클라이언트(앱, CLI, 웹)가 Gateway에 연결하는 통로입니다.

```
기본 주소: ws://127.0.0.1:18789
```

**연결할 수 있는 것들:**
- macOS 앱
- TUI (터미널 UI)
- WebChat (브라우저)
- iOS/Android 노드

### 3. Canvas 호스트

에이전트가 만든 웹 콘텐츠를 제공하는 HTTP 서버입니다.

```
기본 주소: http://127.0.0.1:18793/__openclaw__/canvas/
```

---

## 메시지 흐름 이해하기

### 1단계: 메시지 수신

```
사용자 → WhatsApp → Gateway
```

WhatsApp에서 "안녕!"이라고 보내면 Gateway가 받습니다.

### 2단계: 에이전트 처리

```
Gateway → AI Agent → 응답 생성
```

Gateway가 메시지를 AI 에이전트에게 전달하고, 에이전트가 응답을 만듭니다.

### 3단계: 응답 전송

```
AI Agent → Gateway → WhatsApp → 사용자
```

응답이 다시 WhatsApp을 통해 사용자에게 전달됩니다.

### 전체 흐름

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│  📱 WhatsApp ─┐                                         │
│               │                                         │
│  💬 Discord ──┼──→ 🗼 Gateway ──→ 🤖 Agent ──→ 💭 응답  │
│               │         │                               │
│  ✈️ Telegram ─┘         │                               │
│                         ▼                               │
│                    📊 Dashboard                         │
│                    💻 TUI                               │
│                    🌐 WebChat                           │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## 노드(Node)란?

노드는 Gateway에 연결되는 **클라이언트 장치**입니다.

### 노드의 종류

| 노드 | 설명 |
|------|------|
| macOS 앱 | 메뉴바 컴패니언, Voice Wake |
| iOS 앱 | 모바일에서 Canvas 표시 |
| Android 앱 | 모바일에서 Canvas, 카메라 |
| Headless | 서버에서 실행되는 노드 |

### 노드가 제공하는 기능

노드는 자신만의 **능력(capabilities)**을 Gateway에 알려줍니다:

```
macOS 노드의 능력:
- canvas.present (HTML 표시)
- camera.snap (사진 촬영)
- screen.record (화면 녹화)
- system.run (명령어 실행)
```

에이전트는 이 능력들을 사용해서 작업을 수행합니다.

---

## 연결 과정 (Handshake)

### 1. 연결 요청

클라이언트가 Gateway에 WebSocket 연결을 시도합니다.

```
Client → Gateway: "연결하고 싶어요!"
```

### 2. 인증

토큰이 설정되어 있으면 인증이 필요합니다.

```
Client → Gateway: "토큰은 abc123입니다"
Gateway → Client: "확인됐어요!"
```

### 3. 연결 완료

연결이 성공하면 상태 정보를 받습니다.

```
Gateway → Client: {
  "status": "ok",
  "health": {...},
  "presence": {...}
}
```

### 다이어그램

```
Client                    Gateway
  │                          │
  │──── req:connect ────────→│
  │                          │
  │←──── res (ok) ───────────│
  │                          │
  │←──── event:presence ─────│
  │←──── event:tick ─────────│
  │                          │
  │──── req:agent ──────────→│
  │←──── res:agent ──────────│
  │                          │
```

---

## 페어링 (Pairing)

새 장치가 Gateway에 연결하려면 **페어링**이 필요합니다.

### 왜 필요한가요?

아무나 여러분의 AI 에이전트에 접근하면 안 되니까요!

### 페어링 과정

1. **새 장치 연결 시도**
   ```
   새 iPhone → Gateway: "연결하고 싶어요!"
   ```

2. **페어링 코드 발급**
   ```
   Gateway → 새 iPhone: "코드는 ABC123입니다"
   ```

3. **관리자 승인**
   ```bash
   openclaw pairing approve <code>
   ```

4. **장치 토큰 발급**
   ```
   Gateway → 새 iPhone: "이제 이 토큰으로 연결하세요"
   ```

### 로컬 연결은 자동 승인

같은 컴퓨터(127.0.0.1)에서 연결하면 자동으로 승인됩니다. 편의성을 위해서요.

---

## 원격 접속

### 방법 1: SSH 터널 (권장)

```bash
ssh -N -L 18789:127.0.0.1:18789 user@remote-host
```

로컬에서 `ws://127.0.0.1:18789`로 연결하면 원격 Gateway에 접속됩니다.

### 방법 2: Tailscale/VPN

Tailscale을 사용하면 안전하게 원격 접속할 수 있습니다.

```bash
openclaw gateway --bind tailnet --token <your-token>
```

### 방법 3: 직접 노출 (주의!)

Gateway를 인터넷에 직접 노출하는 것은 권장하지 않습니다. 반드시 토큰 인증을 사용하세요.

---

## 하나의 Gateway 규칙

**호스트당 하나의 Gateway만 실행**하는 것이 원칙입니다.

### 이유

- WhatsApp Web 세션은 하나만 유지 가능
- 포트 충돌 방지
- 상태 일관성 유지

### 여러 Gateway가 필요한 경우

격리가 필요하면 별도 프로필로 실행:

```bash
# Gateway A
OPENCLAW_CONFIG_PATH=~/.openclaw-a/openclaw.json \
OPENCLAW_STATE_DIR=~/.openclaw-a \
openclaw gateway --port 18789

# Gateway B
OPENCLAW_CONFIG_PATH=~/.openclaw-b/openclaw.json \
OPENCLAW_STATE_DIR=~/.openclaw-b \
openclaw gateway --port 18790
```

---

## 상태 확인 명령어

### Gateway 상태

```bash
openclaw status
```

### 건강 검진

```bash
openclaw doctor
```

### 채널 상태

```bash
openclaw channels status --probe
```

### 로그 확인

```bash
openclaw logs --follow
```

---

## 핵심 포인트 정리

| 개념 | 설명 |
|------|------|
| Gateway | 모든 통신의 중앙 허브 |
| 채널 | 메시징 서비스 연결 (WhatsApp, Discord 등) |
| 노드 | Gateway에 연결된 클라이언트 장치 |
| WebSocket | 클라이언트-Gateway 통신 프로토콜 |
| 페어링 | 새 장치 인증 과정 |
| Canvas Host | 에이전트 웹 콘텐츠 서버 |

---

## 비유로 다시 정리

### Gateway = 공항 관제탑
- 모든 비행기(메시지)의 이착륙 관리
- 여러 활주로(채널) 동시 운영

### 노드 = 비행기
- 각자의 능력(승객 수, 화물 용량)을 가짐
- 관제탑의 지시에 따라 움직임

### 페어링 = 비행 허가
- 새 비행기는 허가를 받아야 이착륙 가능
- 한 번 허가받으면 계속 사용 가능

### 에이전트 = 조종사
- 실제로 비행(작업)을 수행
- 관제탑을 통해 승객(사용자)과 소통

---

## 다음 단계

Gateway 아키텍처를 이해했다면:

- **[용어 가이드](/blog/ko/openclaw-detailed-glossary)**: 더 많은 개념 학습
- **[TUI 가이드](/blog/ko/openclaw-tui-guide)**: 터미널에서 직접 사용
- **[Discord 연동](/blog/ko/openclaw-discord-basic)**: 채널 연동 실습

이제 OpenClaw의 심장을 이해했습니다! 🦞
