---
title: "OpenClaw 디스코드 봇 만들기 - 봇 생성 및 토큰 발급 가이드"
description: "OpenClaw AI를 디스코드에 연동하기 위한 봇 생성 방법. Discord Developer Portal에서 봇 만들기, 토큰 발급, Message Content Intent 설정, 서버 초대까지 완벽 가이드."
pubDate: 2026-02-02T10:00:00+09:00
category: "AI"
tags: ["OpenClaw", "디스코드봇", "Discord", "봇만들기", "토큰발급", "Developer Portal", "Intent설정", "AI봇"]
---

OpenClaw의 진정한 매력은 내가 주로 사용하는 메신저에서 AI와 대화하며 시스템 작업을 시키는 것이죠. 그 중에서도 가장 강력한 연동을 자랑하는 **디스코드(Discord)** 설정법을 3편에 걸쳐 아주 자세히 정리해 드립니다.

1편에서는 가장 기초가 되는 **디스코드 봇 생성과 토큰 발급** 과정을 다룹니다.

---

## 1단계: 디스코드 애플리케이션 만들기

먼저 내 AI 에이전트가 활동할 '몸체'인 봇 애플리케이션을 만들어야 합니다.

1. [Discord Developer Portal](https://discord.com/developers/applications)에 접속하여 로그인합니다.
2. 오른쪽 상단의 **'New Application'** 버튼을 클릭합니다.
3. 봇의 이름을 입력하고(예: `My-OpenClaw-Bot`) 이용약관에 동의한 뒤 **'Create'**를 누릅니다.

## 2단계: 봇 유저 생성 및 토큰 발급

애플리케이션이 생성되었다면, 이제 실제 봇 계정을 활성화해야 합니다.

1. 왼쪽 메뉴에서 **'Bot'** 탭을 선택합니다.
2. **'Reset Token'** (또는 처음이라면 'Copy Token') 버튼을 눌러 봇 토큰을 생성합니다.
3. **⚠️ 중요:** 이 토큰은 봇의 비밀번호와 같습니다. 복사해서 메모장 같은 곳에 안전하게 적어두세요. (한 번만 보여주니 꼭 복사해 두셔야 합니다!)

## 3단계: 필수 권한(Intents) 설정 (매우 중요!)

OpenClaw가 디스코드 메시지를 읽고 반응하려면 '특권 인텐트' 설정이 반드시 필요합니다. 이 설정을 놓치면 봇이 온라인이 되어도 대화가 안 됩니다.

1. 같은 'Bot' 페이지에서 아래로 스크롤하여 **'Privileged Gateway Intents'** 섹션을 찾습니다.
2. 다음 두 가지 항목을 **ON**으로 켭니다:
    - **Presence Intent** (선택사항이지만 권장)
    - **Server Members Intent** (사용자 검색 및 화이트리스트 확인용)
    - **Message Content Intent** (⚠️ 필수: 메시지 내용을 읽기 위해 반드시 필요합니다)
3. **'Save Changes'**를 눌러 저장합니다.

## 4단계: 서버에 봇 초대하기

이제 만든 봇을 내 디스코드 서버로 불러올 차례입니다.

1. 왼쪽 메뉴에서 **'OAuth2' -> 'URL Generator'**를 선택합니다.
2. **Scopes**에서 다음 항목을 체크합니다:
    - `bot`
    - `applications.commands` (슬래시 명령어 사용을 위해 필요)
3. **Bot Permissions**에서 필요한 권한을 체크합니다 (최소 권한):
    - `View Channels`
    - `Send Messages`
    - `Read Message History`
    - `Embed Links`
    - `Attach Files`
    - `Add Reactions`
4. 하단에 생성된 URL을 복사하여 브라우저에 붙여넣고, 봇을 추가할 서버를 선택하여 승인합니다.

---

### 요약 및 다음 단계

이제 여러분의 서버에 봇이 들어왔을 겁니다! 하지만 아직 오프라인 상태일 거예요.

**정리:**
- 디스코드 개발자 포털에서 앱 만들기
- 봇 토큰 복사해두기
- **Message Content Intent** 반드시 켜기
- 초대 링크로 서버에 추가하기

**2편**에서는 복사해둔 토큰을 OpenClaw 설정 파일에 입력하고, 실제로 봇을 가동하는 방법을 알아보겠습니다. 기대해 주세요! ㅎㅎ 🛡️
