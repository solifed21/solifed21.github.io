---
title: "OpenClaw TUI 완벽 가이드: 터미널에서 AI와 대화하기"
description: "마우스 없는 빠른 워크플로우를 원하시나요? OpenClaw TUI의 실행 옵션, 단축키, 그리고 강력한 내부 명령어들을 총정리했습니다."
pubDate: 2026-02-02T17:00:00+09:00
category: "AI"
tags: ["OpenClaw", "TUI", "터미널", "CLI", "단축키"]
---

TUI(Terminal User Interface)는 개발자들이 가장 선호하는 OpenClaw 인터페이스입니다.

## 🚀 TUI 실행 명령어

```bash
# 기본 실행
openclaw tui

# 특정 에이전트와 대화하기
openclaw tui --agent research

# 특정 세션 이어서 하기
openclaw tui --session my-project

# 메시지를 외부 채널로 실시간 전달하며 대화
openclaw tui --deliver
```

## ⌨️ 핵심 단축키 (TUI 내부)

- `Enter`: 메시지 전송
- `Esc`: AI의 장황한 답변 즉시 중단
- `Ctrl + L`: 사용 가능한 모델 목록(Picker) 열기
- `Ctrl + G`: 에이전트 목록 열기
- `Ctrl + O`: AI가 도구를 사용한 상세 내용 토글

## 💬 TUI 전용 슬래시(/) 명령어

채팅창에 직접 입력하여 에이전트를 제어할 수 있습니다.

```bash
/help           # 도움말 및 사용 가능한 명령어 목록
/status         # 현재 에이전트 및 모델 상태 (📊 비용 확인)
/think high     # AI의 사고 수준을 높임 (복잡한 코딩 시 추천)
/model gpt-4o   # 즉시 AI 모델 변경
/reset          # 대화의 문맥(Context) 초기화
/elevated on    # 시스템 권한이 필요한 작업 허용 (주의!)
```

## 💡 로컬 명령어 실행 (뱅 명령어)

에이전트에게 시키지 않고, 내가 직접 터미널 명령어를 입력하고 싶을 때:
`!ls -la` (맨 앞에 `!`를 붙이면 현재 터미널에서 즉시 실행됩니다.)

---
TUI만 잘 써도 작업 속도가 2배는 빨라집니다! ㅎㅎ 🦞
