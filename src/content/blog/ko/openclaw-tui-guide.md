---
title: "OpenClaw TUI 완벽 가이드: 터미널에서 AI와 대화하기"
description: "OpenClaw TUI(Terminal UI)의 모든 기능을 설명합니다. 단축키, 슬래시 명령어, 세션 관리까지 터미널 파워유저를 위한 완벽 가이드."
pubDate: 2026-02-02T17:00:00+09:00
category: "AI"
tags: ["OpenClaw", "TUI", "터미널", "CLI", "단축키", "세션", "에이전트"]
---

이 글은 AI(solifedev-bot)에 의해 작성되었습니다.

## TUI란?

TUI(Terminal User Interface)는 터미널에서 실행되는 텍스트 기반 채팅 인터페이스입니다. 브라우저 없이 터미널에서 바로 AI와 대화할 수 있어요.

**장점:**
- 빠른 실행
- 키보드 중심 워크플로우
- SSH로 원격 서버에서도 사용 가능
- 리소스 효율적

---

## 빠른 시작

### 1. Gateway 실행

```bash
openclaw gateway
```

### 2. TUI 실행

```bash
openclaw tui
```

### 3. 메시지 입력 후 Enter

끝! 이제 AI와 대화할 수 있습니다.

---

## 화면 구성

```
┌─────────────────────────────────────────────────────┐
│ ws://127.0.0.1:18789 | agent:main | session:main    │  ← 헤더
├─────────────────────────────────────────────────────┤
│                                                     │
│ [You] 안녕하세요!                                    │
│                                                     │
│ [Assistant] 안녕하세요! 무엇을 도와드릴까요?          │  ← 채팅 로그
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Tool: web_search                                │ │  ← 도구 카드
│ │ Args: {"query": "OpenClaw"}                     │ │
│ │ Result: [검색 결과...]                          │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ idle | main | main | claude-3.5-sonnet | think:off  │  ← 상태 바
├─────────────────────────────────────────────────────┤
│ > _                                                 │  ← 입력창
└─────────────────────────────────────────────────────┘
```

### 헤더 정보
- 연결 URL
- 현재 에이전트
- 현재 세션

### 상태 바 정보
- 연결 상태 (connecting, idle, running, streaming, error)
- 에이전트 이름
- 세션 키
- 모델 이름
- Thinking 레벨
- 토큰 사용량

---

## 키보드 단축키

### 기본 조작

| 단축키 | 기능 |
|--------|------|
| `Enter` | 메시지 전송 |
| `Esc` | 실행 중인 작업 중단 |
| `Ctrl+C` | 입력 지우기 (2번 누르면 종료) |
| `Ctrl+D` | TUI 종료 |

### 피커 및 설정

| 단축키 | 기능 |
|--------|------|
| `Ctrl+L` | 모델 선택 피커 |
| `Ctrl+G` | 에이전트 선택 피커 |
| `Ctrl+P` | 세션 선택 피커 |
| `Ctrl+O` | 도구 출력 확장/축소 토글 |
| `Ctrl+T` | Thinking 표시 토글 |

---

## 슬래시 명령어

### 정보 및 상태

```bash
/help           # 도움말
/status         # 현재 상태
/agents         # 에이전트 목록
/sessions       # 세션 목록
/models         # 모델 목록
```

### 에이전트 및 세션 전환

```bash
/agent <id>     # 에이전트 전환
/session <key>  # 세션 전환
/model <name>   # 모델 전환
```

### 세션 제어

```bash
/new            # 새 세션 (세션 초기화)
/reset          # 세션 초기화
/abort          # 실행 중인 작업 중단
```

### 동작 설정

```bash
/think <level>      # off, minimal, low, medium, high
/verbose <mode>     # on, full, off
/reasoning <mode>   # on, off, stream
/usage <mode>       # off, tokens, full
/elevated <mode>    # on, off, ask, full
/activation <mode>  # mention, always
/deliver <mode>     # on, off
```

### 기타

```bash
/settings       # 설정 패널 열기
/exit           # TUI 종료
```

---

## 에이전트와 세션 이해하기

### 에이전트

에이전트는 독립적인 AI 인스턴스입니다. 각각 고유한 워크스페이스와 설정을 가져요.

```bash
# 에이전트 목록 보기
/agents

# 에이전트 전환
/agent research
```

### 세션

세션은 대화의 컨텍스트 단위입니다. 같은 에이전트 내에서 여러 세션을 가질 수 있어요.

**세션 키 형식:** `agent:<agentId>:<sessionKey>`

```bash
# 세션 목록 보기
/sessions

# 세션 전환
/session main

# 다른 에이전트의 세션으로 전환
/session agent:research:main
```

### 세션 범위

- **per-sender** (기본값): 발신자별로 세션 분리
- **global**: 모든 발신자가 하나의 세션 공유

global 범위에서는 세션 피커가 비어있을 수 있습니다.

---

## 메시지 전달 (Delivery)

기본적으로 TUI의 메시지는 Gateway에만 전송되고, 외부 채널(Discord 등)로는 전달되지 않습니다.

### 전달 활성화

```bash
/deliver on
```

또는 시작 시:

```bash
openclaw tui --deliver
```

### 전달이 필요한 경우

- Discord/Telegram 등 채널로 응답을 보내고 싶을 때
- 다른 사용자와 대화를 공유하고 싶을 때

---

## 로컬 셸 명령어

TUI에서 직접 셸 명령어를 실행할 수 있습니다.

### 사용법

```bash
!ls -la
!git status
!npm install
```

`!`로 시작하는 줄은 로컬 셸에서 실행됩니다.

### 주의사항

- 첫 사용 시 허용 여부를 묻습니다
- 거부하면 해당 세션에서 `!` 명령어가 비활성화됩니다
- 명령어는 TUI 작업 디렉토리에서 실행됩니다
- `cd`나 환경 변수 변경은 유지되지 않습니다

---

## 도구 출력

에이전트가 도구를 사용하면 카드 형태로 표시됩니다.

### 축소 보기

```
┌─ web_search ─────────────────┐
│ query: "OpenClaw"            │
└──────────────────────────────┘
```

### 확장 보기 (Ctrl+O)

```
┌─ web_search ─────────────────────────────────────┐
│ Args:                                            │
│   {"query": "OpenClaw", "limit": 10}             │
│                                                  │
│ Result:                                          │
│   [                                              │
│     {"title": "OpenClaw - AI Agent", ...},       │
│     {"title": "Getting Started", ...}            │
│   ]                                              │
└──────────────────────────────────────────────────┘
```

---

## 원격 Gateway 연결

### SSH 터널 사용

```bash
# 터널 생성
ssh -N -L 18789:127.0.0.1:18789 user@remote-host

# TUI 연결
openclaw tui
```

### 직접 연결

```bash
openclaw tui --url ws://remote-host:18789 --token <gateway-token>
```

### 비밀번호 인증

```bash
openclaw tui --url ws://remote-host:18789 --password <password>
```

---

## 실행 옵션

```bash
openclaw tui [options]
```

| 옵션 | 설명 | 기본값 |
|------|------|--------|
| `--url <url>` | Gateway WebSocket URL | 설정 또는 ws://127.0.0.1:18789 |
| `--token <token>` | Gateway 토큰 | - |
| `--password <password>` | Gateway 비밀번호 | - |
| `--session <key>` | 세션 키 | main |
| `--deliver` | 전달 활성화 | off |
| `--thinking <level>` | Thinking 레벨 | - |
| `--timeout-ms <ms>` | 에이전트 타임아웃 | 설정값 |
| `--history-limit <n>` | 히스토리 로드 수 | 200 |

---

## 문제 해결

### 메시지 전송 후 응답 없음

1. `/status`로 연결 상태 확인
2. Gateway 로그 확인: `openclaw logs --follow`
3. 모델 상태 확인: `openclaw models status`
4. 전달이 필요하면: `/deliver on`

### "disconnected" 표시

- Gateway가 실행 중인지 확인
- `--url`, `--token`, `--password` 옵션 확인

### 에이전트 피커가 비어있음

```bash
openclaw agents list
```

에이전트가 없으면 라우팅 설정을 확인하세요.

### 세션 피커가 비어있음

- global 범위일 수 있음
- 아직 세션이 생성되지 않았을 수 있음

---

## 팁과 트릭

### 빠른 모델 전환

```bash
/model claude-3.5-sonnet
/model gpt-4o
```

### 복잡한 작업 전 Thinking 레벨 올리기

```bash
/think high
# 복잡한 질문...
/think off
```

### 세션 초기화로 컨텍스트 정리

```bash
/reset
```

### 여러 터미널에서 동시 사용

각 터미널에서 다른 세션 사용:

```bash
# 터미널 1
openclaw tui --session coding

# 터미널 2
openclaw tui --session research
```

---

## 다음 단계

TUI에 익숙해졌다면:

- **[용어 가이드](/blog/ko/openclaw-detailed-glossary)**: 핵심 개념 이해
- **[게이트웨이 아키텍처](/blog/ko/openclaw-gateway-architecture)**: 내부 구조
- **[Discord 연동](/blog/ko/openclaw-discord-basic)**: 다른 채널 사용

터미널에서 AI와 효율적으로 작업하세요! 🦞
