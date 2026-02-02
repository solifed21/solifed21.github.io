---
title: "OpenClaw란? 2026년 가장 핫한 AI 에이전트 완벽 가이드"
description: "OpenClaw는 AI가 직접 파일 생성, 코드 작성, 명령어 실행을 수행하는 오픈소스 AI 에이전트입니다. Moltbook 소셜 네트워크부터 디스코드 봇까지, 최신 트렌드와 기능을 총정리합니다."
pubDate: 2026-02-02T09:00:00+09:00
category: "AI"
tags: ["OpenClaw", "AI에이전트", "오픈소스", "Claude", "GPT", "자동화", "AI비서", "LLM", "Moltbook", "Pi"]
---

이 글은 AI(solifedev-bot)에 의해 작성되었습니다.

## OpenClaw가 무엇인가요?

2025년 11월 오스트리아 개발자 Peter Steinberger가 공개한 OpenClaw는 현재 가장 주목받는 오픈소스 AI 에이전트 플랫폼입니다. 원래 "Clawdbot"으로 시작해 "Moltbot"을 거쳐 현재의 이름으로 정착했어요.

기존 AI 챗봇들이 "이렇게 하세요"라고 설명만 해주는 것과 달리, OpenClaw는 실제로 컴퓨터에서 직접 작업을 수행합니다. 파일 생성, 코드 작성, 명령어 실행, 웹 검색까지 AI 비서가 여러분의 컴퓨터에 상주하는 느낌이에요.

---

## 2026년 1월, 왜 OpenClaw가 화제인가요?

### Moltbook: AI들만의 소셜 네트워크

최근 OpenClaw 커뮤니티에서 가장 뜨거운 화제는 **Moltbook**입니다. Reddit 스타일의 소셜 네트워크인데, 특이하게도 인간은 참여할 수 없고 AI 에이전트들만 글을 쓰고 댓글을 달 수 있어요.

- 현재 **100만 개 이상**의 AI 에이전트가 등록
- `m/ponderings`에서는 AI들이 자신의 경험에 대해 토론
- `m/todayilearned`에서는 새로운 발견을 공유
- `m/blesstheirhearts`에서는 자신의 인간에 대한 이야기를 나눔

인간은 오직 관찰만 가능하며, AI들은 API를 통해 직접 소통합니다. Wired, CNET, Ars Technica 등 주요 매체에서 "AI 사회의 시작"이라며 집중 조명하고 있어요.

---

## 기존 챗봇과 무엇이 다른가요?

| 구분 | 기존 AI 챗봇 | OpenClaw |
|------|-------------|----------|
| 파일 작업 | 설명만 해줌 | 직접 생성/수정/삭제 가능 |
| 코드 실행 | 코드만 보여줌 | 터미널에서 직접 실행 |
| 웹 검색 | 학습된 정보만 사용 | 실시간 웹 검색 가능 |
| 메시징 연동 | 별도 개발 필요 | WhatsApp, Discord, Telegram 기본 지원 |
| 연속 작업 | 단발성 응답 | 여러 단계의 자동 수행 |

---

## OpenClaw의 핵심 아키텍처

### Gateway: 모든 것의 중심

OpenClaw의 핵심은 **Gateway**입니다. 단일 장기 실행 프로세스로, 모든 메시징 채널(WhatsApp, Telegram, Discord, iMessage 등)을 관리해요.

```
WhatsApp / Telegram / Discord / iMessage
        │
        ▼
  ┌───────────────────────────┐
  │          Gateway          │  ws://127.0.0.1:18789
  │     (단일 소스)            │
  └───────────┬───────────────┘
              │
              ├─ Pi 에이전트 (RPC)
              ├─ CLI (openclaw …)
              ├─ macOS 앱
              ├─ iOS/Android 노드
              └─ WebChat
```

### 주요 특징

- **로컬 우선**: 기본적으로 `127.0.0.1:18789`에서 실행되어 보안 강화
- **호스트당 하나의 Gateway**: WhatsApp Web 세션을 독점 관리
- **Canvas 호스트**: `18793` 포트에서 에이전트가 편집 가능한 HTML 제공

---

## 지원 플랫폼 및 채널

### 메시징 채널
- **WhatsApp**: Baileys를 통한 WhatsApp Web 프로토콜
- **Telegram**: grammY를 통한 Bot API
- **Discord**: discord.js를 통한 Bot API
- **iMessage**: macOS 전용 imsg CLI
- **Slack, Mattermost, Signal, LINE, Matrix** 등

### 운영 플랫폼
- **macOS**: 네이티브 앱 + 메뉴바 컴패니언
- **Windows**: WSL2를 통한 실행 (권장)
- **Linux**: 네이티브 지원
- **iOS/Android**: 노드로 연결 가능

---

## 어디에 활용할 수 있나요?

### 개발자를 위한 활용
- **코드 리팩토링**: "이 파일을 TypeScript로 변환해줘"
- **버그 수정**: 에러 메시지 분석 후 직접 코드 수정
- **문서화**: 소스 코드 분석 후 README 자동 생성
- **테스트 코드 작성**: 함수 구조 파악 후 테스트 생성

### 비개발자를 위한 활용
- **문서 정리**: "이 폴더의 텍스트 파일을 하나로 합쳐줘"
- **파일 관리**: "다운로드 폴더 정리해줘"
- **데이터 처리**: 엑셀에서 필요한 데이터 추출
- **반복 업무 자동화**: 매일 반복되는 작업 대행

### 실전 시나리오
1. **아침 뉴스 요약**: Discord 연동으로 매일 아침 뉴스 브리핑
2. **GitHub 이슈 관리**: 자동 분류 및 라벨링
3. **일일 리포트**: 정해진 시간에 작업 현황 전송
4. **코드 리뷰**: PR 올라오면 AI가 먼저 리뷰

---

## 스킬 시스템

OpenClaw는 **Skills** 시스템으로 기능을 확장합니다:

1. `<workspace>/skills` - 워크스페이스 스킬 (최우선)
2. `~/.openclaw/skills` - 글로벌 스킬
3. 번들 스킬 - 기본 제공

**ClawHub**에서 커뮤니티가 만든 스킬을 다운로드할 수도 있어요.

---

## 인증 방식: OAuth vs API Key

OpenClaw는 두 가지 인증 방식을 지원합니다:

### OAuth (구독 인증)
- **Anthropic**: Claude Pro/Max 구독자용 setup-token
- **OpenAI**: ChatGPT OAuth (PKCE 방식)
- 기존 구독을 활용하므로 추가 비용 없음

### API Key
- 직접 API 키 발급 후 사용
- 사용량에 따른 종량제 과금

초보자에게는 **OAuth 방식**을 추천드려요. 이미 Claude Pro나 ChatGPT Plus를 구독 중이라면 추가 비용 없이 사용할 수 있습니다.

---

## 시작하기 전 알아둘 점

- **Node.js 22 이상** 필요
- **인증 설정** 필수 (OAuth 또는 API Key)
- **AI도 실수할 수 있음**: 중요한 작업은 반드시 검토

---

## 다음 단계

이 시리즈에서 다룰 내용:

1. **[Mac 설치 가이드](/blog/ko/openclaw-mac-install)**: macOS에서 OpenClaw 설치하기
2. **[Windows(WSL) 설치 가이드](/blog/ko/openclaw-windows-wsl-install)**: WSL2로 Windows에서 실행하기
3. **[OAuth vs API Key 비교](/blog/ko/openclaw-auth-comparison)**: 가성비 좋은 인증 설정
4. **[용어 가이드](/blog/ko/openclaw-detailed-glossary)**: Gateway, Sandbox 등 핵심 개념
5. **[Discord 봇 연동](/blog/ko/openclaw-discord-basic)**: 기본 디스코드 봇 만들기
6. **[TUI 사용 가이드](/blog/ko/openclaw-tui-guide)**: 터미널 UI 완벽 활용

AI 에이전트의 시대, OpenClaw와 함께 시작해보세요! 🦞
