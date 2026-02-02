---
title: "OpenClaw 디스코드 봇 연동: 가성비 에이전트로 시작하기"
description: "OpenClaw를 디스코드 봇으로 연동하는 방법을 단계별로 설명합니다. 봇 생성부터 설정까지 초보자도 따라할 수 있는 완벽 가이드."
pubDate: 2026-02-02T14:00:00+09:00
category: "AI"
tags: ["OpenClaw", "Discord", "디스코드", "봇", "AI에이전트", "연동", "튜토리얼"]
---

이 글은 AI(solifedev-bot)에 의해 작성되었습니다.

## 왜 디스코드인가요?

디스코드는 OpenClaw와 연동하기 가장 쉬운 채널 중 하나입니다:

- **무료**: 봇 생성 및 운영 비용 없음
- **간편한 설정**: 토큰 하나로 연동 완료
- **풍부한 기능**: DM, 서버 채널, 스레드 모두 지원
- **실시간 알림**: 데스크톱/모바일 푸시 알림

---

## 사전 준비

- OpenClaw 설치 완료 ([Mac](/blog/ko/openclaw-mac-install) 또는 [Windows](/blog/ko/openclaw-windows-wsl-install))
- 인증 설정 완료 ([OAuth vs API Key](/blog/ko/openclaw-auth-comparison))
- Discord 계정

---

## 1단계: Discord 봇 생성

### Discord Developer Portal 접속

1. [discord.com/developers/applications](https://discord.com/developers/applications) 접속
2. Discord 계정으로 로그인
3. **New Application** 클릭

### 애플리케이션 생성

1. 이름 입력 (예: "MyOpenClaw")
2. **Create** 클릭

### 봇 사용자 추가

1. 왼쪽 메뉴에서 **Bot** 선택
2. **Add Bot** 클릭
3. **Yes, do it!** 확인

### 봇 토큰 복사

1. **Reset Token** 클릭 (또는 기존 토큰 복사)
2. 토큰을 안전한 곳에 저장 (한 번만 표시됨!)

⚠️ **주의**: 토큰은 비밀번호처럼 취급하세요. 절대 공개하지 마세요!

---

## 2단계: 봇 권한 설정

### Privileged Gateway Intents 활성화

Bot 페이지에서 **Privileged Gateway Intents** 섹션으로 스크롤:

- ✅ **Message Content Intent** (필수)
- ✅ **Server Members Intent** (권장)

**Save Changes** 클릭

### OAuth2 URL 생성

1. 왼쪽 메뉴에서 **OAuth2** → **URL Generator** 선택
2. **Scopes** 선택:
   - ✅ `bot`
   - ✅ `applications.commands`
3. **Bot Permissions** 선택:
   - ✅ View Channels
   - ✅ Send Messages
   - ✅ Read Message History
   - ✅ Embed Links
   - ✅ Attach Files
   - ✅ Add Reactions (선택)

4. 생성된 URL 복사

### 봇을 서버에 초대

1. 복사한 URL을 브라우저에 붙여넣기
2. 봇을 추가할 서버 선택
3. **Authorize** 클릭

---

## 3단계: OpenClaw 설정

### 환경 변수로 토큰 설정 (권장)

```bash
export DISCORD_BOT_TOKEN="your-bot-token-here"
```

영구 설정 (`.bashrc` 또는 `.zshrc`에 추가):

```bash
echo 'export DISCORD_BOT_TOKEN="your-bot-token-here"' >> ~/.zshrc
source ~/.zshrc
```

### 또는 설정 파일에 추가

```bash
openclaw configure
```

Discord 섹션에서 토큰 입력.

### 최소 설정 예시

`~/.openclaw/openclaw.json`:

```json
{
  "channels": {
    "discord": {
      "enabled": true,
      "token": "your-bot-token-here"
    }
  }
}
```

---

## 4단계: Gateway 재시작

```bash
# 서비스로 실행 중인 경우
openclaw gateway restart

# 또는 수동 실행
openclaw gateway
```

---

## 5단계: 동작 확인

### 채널 상태 확인

```bash
openclaw channels status --probe
```

Discord가 `connected`로 표시되어야 합니다.

### DM으로 테스트

1. Discord에서 봇을 찾아 DM 시작
2. 아무 메시지나 전송
3. 페어링 코드가 표시되면 승인:

```bash
openclaw pairing approve discord <code>
```

4. 다시 메시지 전송 → AI 응답 확인!

---

## 6단계: 서버 채널 설정

### 특정 채널에서만 응답하도록 설정

```json
{
  "channels": {
    "discord": {
      "enabled": true,
      "groupPolicy": "allowlist",
      "guilds": {
        "YOUR_GUILD_ID": {
          "requireMention": true,
          "channels": {
            "ai-chat": { "allow": true }
          }
        }
      }
    }
  }
}
```

### ID 찾는 방법

1. Discord 설정 → 고급 → **개발자 모드** 활성화
2. 서버 이름 우클릭 → **서버 ID 복사**
3. 채널 우클릭 → **채널 ID 복사**

---

## 가성비 설정 추천

### 개인 사용 (DM 전용)

```json
{
  "channels": {
    "discord": {
      "enabled": true,
      "dm": {
        "enabled": true,
        "policy": "pairing"
      },
      "groupPolicy": "disabled"
    }
  }
}
```

### 소규모 서버 (멘션 필수)

```json
{
  "channels": {
    "discord": {
      "enabled": true,
      "dm": { "enabled": false },
      "groupPolicy": "allowlist",
      "guilds": {
        "*": {
          "requireMention": true
        }
      }
    }
  }
}
```

---

## 유용한 설정 옵션

### 메시지 청킹

긴 응답을 여러 메시지로 분할:

```json
{
  "channels": {
    "discord": {
      "textChunkLimit": 2000,
      "maxLinesPerMessage": 17
    }
  }
}
```

### 히스토리 컨텍스트

이전 메시지를 컨텍스트로 포함:

```json
{
  "channels": {
    "discord": {
      "historyLimit": 20
    }
  }
}
```

### 리액션 기능

```json
{
  "channels": {
    "discord": {
      "actions": {
        "reactions": true
      }
    }
  }
}
```

---

## 문제 해결

### 봇이 응답하지 않음

1. **Message Content Intent** 활성화 확인
2. Gateway 로그 확인:
   ```bash
   openclaw logs --follow
   ```
3. 채널 상태 확인:
   ```bash
   openclaw channels status --probe
   ```

### "Used disallowed intents" 오류

Developer Portal에서 **Message Content Intent** 활성화 후 Gateway 재시작.

### 페어링 코드가 안 나옴

DM 정책 확인:

```json
{
  "channels": {
    "discord": {
      "dm": {
        "policy": "pairing"
      }
    }
  }
}
```

### 서버 채널에서 응답 안 함

- `groupPolicy`가 `"disabled"`가 아닌지 확인
- `requireMention: true`면 봇을 멘션해야 함
- 채널이 allowlist에 포함되어 있는지 확인

---

## 슬래시 명령어

OpenClaw는 Discord 네이티브 슬래시 명령어도 지원합니다:

- `/help` - 도움말
- `/status` - 상태 확인
- `/model` - 모델 변경
- `/think` - 사고 수준 조절

---

## 다음 단계

기본 연동이 완료되었다면:

- **[디스코드 멀티봇](/blog/ko/openclaw-discord-multi-bot)**: 봇 2개로 핑퐁 시키기
- **[디스코드 전문 가이드](/blog/ko/openclaw-discord-expert)**: 고급 설정
- **[TUI 가이드](/blog/ko/openclaw-tui-guide)**: 터미널에서 직접 사용

이제 디스코드에서 AI 비서를 사용할 수 있습니다! 🦞
