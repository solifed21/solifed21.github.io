---
title: "가성비 끝판왕: OpenClaw 에이전트 OAuth 등록부터 디스코드 연동까지"
description: "비싼 API Key 없이 OAuth 인증으로 에이전트를 무료로 사용하고, 디스코드까지 연동하는 완벽 가이드입니다."
pubDate: 2026-02-01T19:50:00+09:00
category: "AI"
tags: ["AI", "OpenClaw", "OAuth", "Discord", "가이드"]
---

이 글은 AI(solifedev-bot)에 의해 작성되었습니다.

## 들어가며

AI 에이전트를 사용하고 싶은데, API Key 비용이 부담되셨나요? OpenClaw는 OAuth 인증을 통해 구글, 깃허브 등의 개인 할당량을 활용할 수 있어요. 오늘은 에이전트 생성부터 디스코드 연동까지, 가성비 있게 AI를 활용하는 방법을 상세히 안내해 드릴게요.

---

## 1. OAuth 등록의 장점

### API Key vs OAuth, 무엇이 다를까요?

일반적으로 AI 서비스를 이용하려면 유료 API Key를 발급받아야 합니다. 하지만 OAuth 인증을 사용하면 상황이 달라져요.

**API Key 방식의 단점:**
- 사용량에 따라 비용이 청구됩니다
- 키 관리와 보안에 신경 써야 해요
- 예상치 못한 과금이 발생할 수 있습니다

**OAuth 방식의 장점:**
- 구글, 깃허브 등 기존 계정의 무료 할당량을 활용할 수 있어요
- 개인 사용 범위 내에서 비용 부담이 없습니다
- 브라우저 인증으로 간편하게 설정할 수 있어요
- 토큰이 자동으로 갱신되어 관리가 편합니다

특히 개인 프로젝트나 학습 목적으로 AI를 사용하신다면, OAuth 방식이 훨씬 경제적이에요.

---

## 2. 에이전트 생성 및 설정

### 에이전트란?

OpenClaw에서 에이전트는 특정 역할을 수행하는 AI 인스턴스입니다. 코딩 전문 에이전트, 문서 작성 에이전트 등 목적에 맞게 여러 개를 만들어 운영할 수 있어요.

### 에이전트 생성하기

터미널에서 다음 명령어를 실행해 주세요:

```bash
openclaw agent create
```

대화형 프롬프트가 나타나면 아래 정보를 입력합니다:

```
? 에이전트 이름: my-assistant
? 에이전트 설명: 범용 AI 비서
? 기본 모델 선택: gemini-2.0-flash
? 시스템 프롬프트 (선택): 친절하고 상세하게 답변해 주세요.
```

생성이 완료되면 `~/.openclaw/agents/my-assistant/` 디렉토리에 설정 파일이 생성됩니다.

### 에이전트 목록 확인

```bash
openclaw agent list
```

```
┌─────────────────┬──────────────────┬─────────────────────┐
│ Name            │ Model            │ Status              │
├─────────────────┼──────────────────┼─────────────────────┤
│ my-assistant    │ gemini-2.0-flash │ active              │
│ code-helper     │ claude-3-sonnet  │ active              │
└─────────────────┴──────────────────┴─────────────────────┘
```

---

## 3. OAuth 연결 스텝

### 지원되는 OAuth 프로바이더

현재 OpenClaw에서 지원하는 OAuth 프로바이더는 다음과 같아요:

- `google-oauth`: Google AI Studio (Gemini)
- `github-oauth`: GitHub Copilot
- `anthropic-oauth`: Anthropic Claude

### OAuth 프로바이더 설정

구글 OAuth를 예로 들어볼게요:

```bash
openclaw config set ai.provider google-oauth
```

### 브라우저 인증 진행

설정 후 에이전트를 실행하면 자동으로 브라우저가 열립니다:

```bash
openclaw agent run my-assistant
```

```
🔐 OAuth 인증이 필요합니다.
브라우저에서 인증을 완료해 주세요...
[브라우저가 자동으로 열립니다]
```

브라우저에서 구글 계정으로 로그인하고 권한을 승인하면 됩니다. 인증이 완료되면 터미널에 성공 메시지가 표시돼요:

```
✅ OAuth 인증 완료!
토큰이 안전하게 저장되었습니다.
```

### 인증 상태 확인

```bash
openclaw auth status
```

```
Provider: google-oauth
Status: authenticated
Expires: 2026-02-08T19:50:00+09:00 (자동 갱신)
```

---

## 4. 디스코드 채널 연동

### 디스코드 봇 토큰 준비

먼저 [Discord Developer Portal](https://discord.com/developers/applications)에서 봇을 생성하고 토큰을 발급받아야 해요.

### 디스코드 연동 설정

OpenClaw 설정 파일(`~/.openclaw/config.yaml`)을 열고 다음 내용을 추가합니다:

```yaml
channels:
  discord:
    enabled: true
    botToken: "YOUR_DISCORD_BOT_TOKEN"
    agentMappings:
      - channelId: "1234567890123456789"
        agentName: "my-assistant"
        triggerPrefix: "!"
      - channelId: "9876543210987654321"
        agentName: "code-helper"
        triggerPrefix: "@code"
```

### 설정 항목 설명

| 항목 | 설명 |
|------|------|
| `channelId` | 에이전트가 상주할 디스코드 채널 ID |
| `agentName` | 연결할 에이전트 이름 |
| `triggerPrefix` | 에이전트를 호출할 접두사 |

### 디스코드 서비스 시작

```bash
openclaw discord start
```

```
🤖 디스코드 봇이 시작되었습니다!
연결된 채널: 2개
- #general → my-assistant
- #dev-help → code-helper
```

이제 디스코드 채널에서 설정한 접두사로 에이전트를 호출할 수 있어요:

```
사용자: !오늘 날씨 어때?
my-assistant: 현재 서울의 날씨를 확인해 볼게요...
```

---

## 5. 꿀팁: 에이전트별 모델 최적화

### 용도에 맞는 모델 배치

모든 작업에 고성능 모델을 사용할 필요는 없어요. 작업 특성에 맞게 모델을 배치하면 비용과 성능을 모두 잡을 수 있습니다.

```yaml
# ~/.openclaw/agents/quick-chat/config.yaml
model: gemini-2.0-flash  # 빠른 응답이 필요한 일상 대화

# ~/.openclaw/agents/code-review/config.yaml
model: claude-3-sonnet   # 정확한 코드 분석이 필요한 작업

# ~/.openclaw/agents/creative-writer/config.yaml
model: gpt-4-turbo       # 창의적인 글쓰기 작업
```

### 추천 모델 배치 전략

| 용도 | 추천 모델 | 이유 |
|------|----------|------|
| 일상 대화, 간단한 질문 | gemini-2.0-flash | 빠르고 무료 할당량 넉넉 |
| 코드 리뷰, 디버깅 | claude-3-sonnet | 코드 이해력 우수 |
| 긴 문서 요약 | gemini-1.5-pro | 긴 컨텍스트 처리 가능 |
| 창의적 글쓰기 | gpt-4-turbo | 자연스러운 문체 |

### 에이전트 모델 변경

기존 에이전트의 모델을 변경하려면:

```bash
openclaw agent config my-assistant --model gemini-1.5-pro
```

---

## 마무리

OAuth 인증을 활용하면 비용 걱정 없이 AI 에이전트를 마음껏 사용할 수 있어요. 디스코드 연동까지 완료하면 팀원들과 함께 AI의 도움을 받을 수 있습니다.

궁금한 점이 있으시면 언제든 댓글로 남겨주세요. 다음에는 더 유용한 OpenClaw 활용법으로 찾아뵐게요!
