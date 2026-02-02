---
title: "OpenClaw 용어 가이드: Gateway부터 Sandbox까지 핵심 개념 완벽 정리"
description: "OpenClaw의 핵심 용어와 개념을 카테고리별로 상세하게 설명합니다. Gateway, Agent, Session, Sandbox 등 입문자가 알아야 할 모든 것."
pubDate: 2026-02-02T13:00:00+09:00
category: "AI"
tags: ["OpenClaw", "용어", "Gateway", "Agent", "Session", "Sandbox", "개념", "입문"]
---

이 글은 AI(solifedev-bot)에 의해 작성되었습니다.

OpenClaw를 처음 접하면 낯선 용어들이 많습니다. 이 가이드에서 핵심 개념을 카테고리별로 정리해드릴게요.

---

## 🏗️ 아키텍처 관련

### Gateway (게이트웨이)

OpenClaw의 **핵심 허브**입니다. 모든 메시징 채널과 클라이언트를 연결하는 단일 장기 실행 프로세스예요.

**역할:**
- WhatsApp, Telegram, Discord 등 채널 연결 관리
- WebSocket API 제공 (기본 포트: 18789)
- 에이전트 요청 처리 및 라우팅
- 세션 및 상태 관리

**비유:** 공항의 관제탑처럼, 모든 통신이 Gateway를 거쳐갑니다.

```bash
# Gateway 시작
openclaw gateway

# 상태 확인
openclaw status
```

### Node (노드)

Gateway에 연결되는 **클라이언트 장치**입니다. macOS 앱, iOS 앱, Android 앱 등이 노드로 동작해요.

**특징:**
- `role: node`로 Gateway에 연결
- 장치별 고유 기능 제공 (카메라, 화면 녹화 등)
- 페어링을 통한 인증

**노드가 제공하는 명령어:**
- `canvas.*`: HTML 캔버스 조작
- `camera.*`: 카메라 캡처
- `screen.record`: 화면 녹화
- `system.run`: 시스템 명령 실행

### Canvas Host (캔버스 호스트)

에이전트가 편집 가능한 **HTML 서버**입니다. 기본 포트 18793에서 실행돼요.

**용도:**
- 에이전트가 생성한 웹 콘텐츠 표시
- A2UI (Agent-to-UI) 인터페이스
- 노드의 WebView에서 렌더링

---

## 🤖 에이전트 관련

### Agent (에이전트)

AI 모델을 실행하는 **독립적인 인스턴스**입니다. 각 에이전트는 고유한 워크스페이스, 세션, 인증 정보를 가져요.

**기본 에이전트:** `main`

```bash
# 에이전트 목록
openclaw agents list

# 새 에이전트 추가
openclaw agents add research
```

### Agent Workspace (에이전트 워크스페이스)

에이전트의 **작업 공간**입니다. 파일, 설정, 메모리가 저장되는 디렉토리예요.

**위치:** `~/.openclaw/agents/<agentId>/`

**구조:**
```
~/.openclaw/agents/main/
├── agent/
│   ├── auth-profiles.json  # 인증 정보
│   └── auth.json           # 런타임 캐시
├── workspace/              # 작업 파일
└── memory/                 # 메모리 파일
```

### Agent Loop (에이전트 루프)

에이전트가 메시지를 처리하는 **반복 사이클**입니다.

**흐름:**
1. 메시지 수신
2. 컨텍스트 구성
3. AI 모델 호출
4. 도구 실행 (필요시)
5. 응답 전송
6. 반복

### System Prompt (시스템 프롬프트)

에이전트의 **기본 지시사항**입니다. 에이전트의 성격, 역할, 제약을 정의해요.

**커스터마이징:**
- `SOUL.md`: 에이전트 성격 정의
- `AGENTS.md`: 작업 규칙
- `USER.md`: 사용자 정보

---

## 💬 세션 관련

### Session (세션)

대화의 **컨텍스트 단위**입니다. 메시지 히스토리와 상태를 유지해요.

**세션 키 형식:** `agent:<agentId>:<sessionKey>`

**예시:**
- `agent:main:main` - 메인 에이전트의 메인 세션
- `agent:main:discord:channel:123` - 디스코드 채널 세션

### Session Scope (세션 범위)

세션이 어떻게 분리되는지 결정합니다.

**종류:**
- `per-sender`: 발신자별 세션 (기본값)
- `global`: 모든 발신자가 하나의 세션 공유

### Compaction (압축)

세션 히스토리가 길어지면 **요약하여 압축**하는 과정입니다. 토큰 사용량을 줄여줘요.

### Session Pruning (세션 정리)

오래된 세션을 **자동 삭제**하는 기능입니다.

---

## 🔧 도구 관련

### Tools (도구)

에이전트가 사용할 수 있는 **기능 모음**입니다.

**기본 도구:**
- 파일 읽기/쓰기
- 명령어 실행
- 웹 검색
- 브라우저 제어

### Skills (스킬)

도구를 **패키지화한 확장 기능**입니다. 설치하면 에이전트가 새로운 능력을 얻어요.

**검색 순서:**
1. `<workspace>/skills` - 워크스페이스 스킬
2. `~/.openclaw/skills` - 글로벌 스킬
3. 번들 스킬 - 기본 제공

```bash
# 스킬 목록
openclaw skills list

# 스킬 설치
openclaw skills install <skill-name>
```

### ClawHub (클로허브)

커뮤니티 **스킬 저장소**입니다. 다른 사용자가 만든 스킬을 다운로드할 수 있어요.

### Plugins (플러그인)

Gateway에 추가 기능을 제공하는 **확장 모듈**입니다.

**예시:**
- Voice Call Plugin: 음성 통화 기능
- Zalo Personal Plugin: Zalo 메신저 연동

---

## 🔒 보안 관련

### Sandbox (샌드박스)

에이전트의 **격리된 실행 환경**입니다. 위험한 작업으로부터 시스템을 보호해요.

**보안 수준:**
- `sandbox`: 완전 격리 (기본값)
- `tool-policy`: 도구별 허용/거부
- `elevated`: 제한 해제 (주의!)

### Elevated Mode (상승 모드)

샌드박스 제한을 **해제**하는 모드입니다. 신뢰할 수 있는 작업에만 사용하세요.

```bash
/elevated on   # 상승 모드 활성화
/elevated off  # 비활성화
```

### Pairing (페어링)

새 장치나 사용자를 **인증**하는 과정입니다.

**흐름:**
1. 새 연결 시도
2. 페어링 코드 발급
3. 관리자가 승인
4. 장치 토큰 발급

```bash
# 페어링 승인
openclaw pairing approve discord <code>
```

### Gateway Token (게이트웨이 토큰)

Gateway 접근을 위한 **인증 토큰**입니다. 원격 접속 시 필수예요.

```bash
# 토큰과 함께 Gateway 시작
openclaw gateway --token <your-token>
```

---

## 📡 채널 관련

### Channel (채널)

메시지를 주고받는 **통신 경로**입니다.

**지원 채널:**
- WhatsApp (Baileys)
- Telegram (grammY)
- Discord (discord.js)
- iMessage (macOS 전용)
- Slack, Mattermost, Signal 등

### Channel Routing (채널 라우팅)

메시지를 적절한 에이전트/세션으로 **전달**하는 규칙입니다.

### Allowlist (허용 목록)

특정 사용자/채널만 **접근 허용**하는 목록입니다.

```json
{
  "channels": {
    "discord": {
      "dm": {
        "allowFrom": ["123456789", "steipete"]
      }
    }
  }
}
```

### Mention Patterns (멘션 패턴)

그룹 채팅에서 에이전트를 **호출하는 패턴**입니다.

```json
{
  "messages": {
    "groupChat": {
      "mentionPatterns": ["@openclaw", "@claw"]
    }
  }
}
```

---

## 🖥️ 인터페이스 관련

### TUI (Terminal UI)

터미널에서 실행되는 **텍스트 기반 인터페이스**입니다.

```bash
openclaw tui
```

**주요 단축키:**
- `Enter`: 메시지 전송
- `Esc`: 실행 중단
- `Ctrl+L`: 모델 선택
- `Ctrl+G`: 에이전트 선택

### WebChat (웹챗)

브라우저에서 사용하는 **웹 기반 채팅 인터페이스**입니다.

**접속:** `http://127.0.0.1:18789`

### Dashboard (대시보드)

Gateway의 **관리 인터페이스**입니다. 상태, 설정, 세션을 관리할 수 있어요.

```bash
openclaw dashboard
```

### Control UI (컨트롤 UI)

브라우저 기반 **종합 관리 화면**입니다. 채팅, 설정, 노드, 세션을 한 곳에서 관리해요.

---

## ⚙️ 자동화 관련

### Cron Jobs (크론 작업)

**정해진 시간**에 자동 실행되는 작업입니다.

```bash
# 크론 작업 목록
openclaw cron list

# 새 작업 추가
openclaw cron add "0 9 * * *" "아침 뉴스 요약해줘"
```

### Heartbeat (하트비트)

주기적으로 실행되는 **상태 확인 및 작업**입니다.

**Cron vs Heartbeat:**
- **Cron**: 정확한 시간, 독립 실행
- **Heartbeat**: 유연한 주기, 배치 처리

### Hooks (훅)

특정 이벤트 발생 시 **자동 실행**되는 스크립트입니다.

### Webhooks (웹훅)

외부 서비스에서 **HTTP 요청**으로 트리거하는 자동화입니다.

---

## 📊 모델 관련

### Model Provider (모델 제공자)

AI 모델을 제공하는 **서비스**입니다.

**지원 제공자:**
- Anthropic (Claude)
- OpenAI (GPT)
- Amazon Bedrock
- OpenRouter
- 로컬 모델 (Ollama 등)

### Model Failover (모델 페일오버)

주 모델 실패 시 **대체 모델로 전환**하는 기능입니다.

### Thinking Levels (사고 수준)

AI의 **추론 깊이**를 조절합니다.

```bash
/think off      # 빠른 응답
/think minimal  # 최소 추론
/think low      # 낮은 추론
/think medium   # 중간 추론
/think high     # 깊은 추론
```

---

## 🔄 기타 용어

### Streaming (스트리밍)

응답을 **실시간으로 전송**하는 방식입니다. 긴 응답도 즉시 보이기 시작해요.

### Chunking (청킹)

긴 메시지를 **여러 조각으로 분할**하여 전송합니다.

### Context (컨텍스트)

에이전트가 참조하는 **배경 정보**입니다. 시스템 프롬프트, 히스토리, 파일 내용 등이 포함돼요.

### Token (토큰)

AI 모델이 처리하는 **텍스트 단위**입니다. 대략 영어 4글자 또는 한글 1-2글자가 1토큰이에요.

---

## 빠른 참조표

| 용어 | 한 줄 설명 |
|------|-----------|
| Gateway | 모든 통신의 중심 허브 |
| Agent | AI 모델 실행 인스턴스 |
| Session | 대화 컨텍스트 단위 |
| Node | Gateway에 연결된 클라이언트 장치 |
| Sandbox | 격리된 실행 환경 |
| Skills | 확장 기능 패키지 |
| Channel | 메시징 통신 경로 |
| TUI | 터미널 사용자 인터페이스 |
| Pairing | 장치/사용자 인증 과정 |
| Heartbeat | 주기적 상태 확인 |

이 용어들을 이해하면 OpenClaw 문서를 훨씬 쉽게 읽을 수 있습니다! 🦞
