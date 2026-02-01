---
title: "OpenClaw 디스코드 봇 화이트리스트 설정 및 채널별 에이전트 매핑 가이드"
description: "OpenClaw 디스코드 봇의 화이트리스트(allowlist)로 특정 유저/채널만 허용하고, 채널별로 다른 AI 에이전트를 매핑하는 멀티 페르소나 설정법을 단계별로 안내합니다."
pubDate: 2026-02-02T12:00:00+09:00
category: "AI"
tags: ["OpenClaw", "디스코드봇", "Discord", "화이트리스트", "allowlist", "멀티에이전트", "채널매핑", "봇보안", "AI봇설정"]
---

디스코드 봇이 잘 작동하시나요? 이제 마지막 단계로 **보안과 효율성**을 챙길 시간입니다. 아무나 내 봇을 써서 API 비용을 발생시키거나, 엉뚱한 채널에서 봇이 튀어나오는 걸 방지하는 법을 알아볼게요. ㅋㅋ

---

## 1단계: 화이트리스트(Allowlist) 설정

특정 유저와 특정 채널에서만 봇이 응답하도록 설정해 봅시다. `~/.openclaw/config.yaml` 파일을 엽니다.

### 예시: 특정 서버와 채널만 허용하기
```yaml
channels:
  discord:
    enabled: true
    token: "YOUR_TOKEN"
    groupPolicy: "allowlist" # 허용된 곳만 응답
    guilds:
      "123456789012345678": # 내 디스코드 서버 ID
        users: ["내_유저_ID", "친구_유저_ID"] # 허용할 유저
        channels:
          help: { allow: true, requireMention: true } # #help 채널 허용
          general: { allow: false } # #general 채널 차단
```

**💡 팁:** 디스코드 설정 -> 고급 -> **개발자 모드**를 켜면 서버나 채널 이름을 우클릭해서 ID를 쉽게 복사할 수 있습니다.

---

## 2단계: 채널별 에이전트 매핑 (멀티 페르소나)

채널마다 다른 성격의 AI를 배치할 수 있습니다. 예를 들어 `#개발-도움` 채널에는 코딩 전문 에이전트를, `#일상-대화` 채널에는 친절한 비서를 매핑하는 식이죠.

```yaml
channels:
  discord:
    agentMappings:
      - channelId: "채널_ID_1"
        agentName: "pi"        # 코딩 전문 에이전트
        triggerPrefix: "!"     # !명령어로 호출
      - channelId: "채널_ID_2"
        agentName: "assistant" # 일반 비서
        triggerPrefix: "@"     # @멘션으로 호출
```

---

## 3단계: DM 보안 정책 설정

모르는 사람이 내 봇에게 DM을 보내는 것이 걱정된다면 `dm.policy`를 조정하세요.

- `pairing` (기본값): 처음 대화 시 터미널에서 승인 코드를 입력해야 함.
- `allowlist`: `allowFrom`에 명시된 유저 ID만 대화 가능.
- `disabled`: 모든 DM 무시.

```yaml
channels:
  discord:
    dm:
      policy: "allowlist"
      allowFrom: ["내_유저_ID"]
```

---

## 4단계: 고급 옵션 (Reaction 및 History)

봇이 더 사람처럼 느껴지게 하는 설정들입니다.

- **historyLimit:** 멘션 시 이전 대화의 맥락을 얼마나 가져올지 설정합니다 (기본 20개).
- **reactionNotifications:** 다른 사람의 반응(Reaction)을 에이전트가 인지하게 할지 선택합니다.

```yaml
channels:
  discord:
    historyLimit: 10
    guilds:
      "*":
        reactionNotifications: "own" # 내 메시지에 달린 반응만 인지
```

---

## 마치며: 3단계 요약

1. **1편:** 봇 생성, 토큰 발급, 필수 Intents(Message Content) 체크
2. **2편:** 토큰 등록 및 `openclaw discord start`로 실행
3. **3편:** 서버/채널 ID로 화이트리스트 설정 및 에이전트 매핑

이제 여러분만의 완벽한 AI 에이전트 환경이 구축되었습니다! ㅎㅎ 디스코드에서 AI와 함께 더 즐거운 개발 생활 되시길 바랍니다. 🛡️
