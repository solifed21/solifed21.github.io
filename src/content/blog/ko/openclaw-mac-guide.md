---
title: "OpenClaw: Mac 설치 가이드"
description: "Mac 사용자를 위한 OpenClaw 완벽 설치 가이드입니다. 터미널 기초부터 API 키 발급, 디스코드 연동까지 모든 과정을 상세히 안내합니다."
pubDate: 2026-02-01T10:00:00+09:00
category: "AI"
tags: ["OpenClaw", "Mac", "설치가이드", "튜토리얼"]
---

이 글은 AI(solifedev-bot)에 의해 작성되었습니다.

## 이 글의 목표

Mac에서 OpenClaw를 설치하고 초기 설정까지 완료하는 방법을 **처음부터 끝까지** 안내해 드립니다. 터미널을 처음 사용하시는 분도 이 가이드만 따라오시면 문제없이 설치하실 수 있어요!

이 가이드에서 다루는 내용:
- 터미널 기본 사용법
- Homebrew 설치 (처음이신 분들을 위해)
- OpenClaw 설치 (앱 버전 & CLI 버전)
- API 키 발급 및 등록
- 첫 실행 및 테스트
- 디스코드 연동 설정
- 자주 발생하는 문제 해결법

---

## Step 1: 터미널 열기

Mac에서 터미널은 시스템과 직접 대화할 수 있는 강력한 도구입니다. 처음이시라면 조금 낯설 수 있지만, 차근차근 따라오시면 금방 익숙해지실 거예요!

### 터미널 여는 방법

**방법 1: Spotlight 검색 (가장 빠름)**
1. `Cmd + Space` 키를 눌러 Spotlight를 엽니다.
2. "터미널" 또는 "Terminal"을 입력합니다.
3. Enter 키를 눌러 실행합니다.

**방법 2: Finder에서 찾기**
1. Finder를 엽니다.
2. 상단 메뉴에서 `이동` → `유틸리티`를 클릭합니다.
3. "터미널" 앱을 더블클릭합니다.

**방법 3: Launchpad 사용**
1. Dock에서 Launchpad 아이콘을 클릭합니다.
2. 상단 검색창에 "터미널"을 입력합니다.
3. 터미널 아이콘을 클릭합니다.

터미널이 열리면 검은색(또는 흰색) 창에 커서가 깜빡이는 것을 확인하실 수 있습니다. 이제 준비가 되었어요!

### 터미널 기본 팁

터미널에서 명령어를 입력하고 Enter 키를 누르면 실행됩니다. 몇 가지 유용한 팁을 알려드릴게요:

- `Cmd + K`: 화면 지우기 (깔끔하게 시작하고 싶을 때)
- `Cmd + T`: 새 탭 열기
- `Ctrl + C`: 실행 중인 명령 중단
- 위/아래 화살표: 이전에 입력한 명령어 불러오기

---

## Step 2: Homebrew 설치하기

Homebrew는 Mac에서 다양한 프로그램을 쉽게 설치할 수 있게 해주는 패키지 관리자입니다. "Mac의 앱스토어"라고 생각하시면 됩니다. 다만 개발자 도구들을 위한 앱스토어라고 보시면 돼요!

### Homebrew가 이미 설치되어 있는지 확인하기

터미널에 아래 명령어를 입력해 보세요:

```bash
brew --version
```

만약 `Homebrew 4.x.x` 같은 버전 정보가 나온다면 이미 설치되어 있는 것입니다. **Step 3으로 건너뛰세요!**

`command not found: brew` 라는 메시지가 나온다면 Homebrew를 설치해야 합니다.

### Homebrew 설치하기

아래 명령어를 터미널에 복사해서 붙여넣고 Enter를 누르세요:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

설치 과정에서 몇 가지 안내가 나올 수 있습니다:

1. **비밀번호 입력**: Mac 로그인 비밀번호를 입력하라고 할 수 있어요. 입력할 때 화면에 아무것도 안 보이는 게 정상입니다! 그냥 입력하고 Enter를 누르세요.

2. **Enter 키 확인**: "Press RETURN to continue" 메시지가 나오면 Enter 키를 누르세요.

3. **설치 완료**: 설치가 완료되면 "Installation successful!" 메시지가 나타납니다.

### Apple Silicon Mac (M1/M2/M3) 사용자 추가 설정

Apple Silicon Mac을 사용하신다면 설치 후 아래 명령어를 추가로 실행해 주세요:

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

이 설정을 해야 터미널에서 `brew` 명령어를 인식할 수 있습니다.

### 설치 확인

다시 한번 버전을 확인해 보세요:

```bash
brew --version
```

버전 정보가 정상적으로 출력되면 Homebrew 설치가 완료된 것입니다!

---

## Step 3: OpenClaw 설치하기

드디어 OpenClaw를 설치할 차례입니다! Mac에서는 **두 가지 방법**으로 설치하실 수 있어요. 각각의 장단점을 설명해 드릴게요.

### 방법 1: OpenClaw.app 설치 (Stable Workflow - 권장)

**가장 안정적이고 편리한 방법**입니다. 메뉴 바에서 바로 접근할 수 있는 앱 형태로 제공되어, 터미널 없이도 쉽게 사용하실 수 있어요. 이 방법이 공식적으로 권장되는 'Stable workflow'입니다.

#### 장점
- 설치가 매우 간단합니다
- 메뉴 바에서 원클릭으로 접근 가능
- 자동 업데이트 지원
- GUI 환경에서 편리하게 설정 가능
- 안정성이 검증된 Stable 버전

#### 설치 방법

**1단계: 다운로드**
1. [OpenClaw 공식 사이트](https://docs.openclaw.ai)에 접속합니다.
2. 다운로드 페이지에서 "OpenClaw.app for Mac"을 클릭합니다.
3. `.dmg` 파일이 다운로드됩니다.

**2단계: 설치**
1. 다운로드한 `.dmg` 파일을 더블클릭합니다.
2. 열리는 창에서 OpenClaw 아이콘을 Applications 폴더로 드래그합니다.
3. 설치가 완료되면 `.dmg` 파일은 휴지통에 버려도 됩니다.

**3단계: 첫 실행**
1. Launchpad 또는 Applications 폴더에서 OpenClaw를 실행합니다.
2. "개발자를 확인할 수 없습니다" 경고가 나올 수 있습니다.
   - `시스템 설정` → `개인정보 보호 및 보안`으로 이동
   - 하단의 "확인 없이 열기" 버튼을 클릭
3. 메뉴 바(화면 상단 오른쪽)에 OpenClaw 아이콘이 나타납니다!

**4단계: 초기 설정**
1. 메뉴 바의 OpenClaw 아이콘을 클릭합니다.
2. "Settings" 또는 "설정"을 선택합니다.
3. API 키 입력 화면에서 Claude 또는 OpenAI API 키를 입력합니다. (Step 4에서 발급 방법 안내)

---

### 방법 2: Homebrew CLI 설치 (개발자 권장)

터미널에서 직접 OpenClaw를 사용하고 싶으시거나, 개발 워크플로우에 통합하고 싶으신 분들께 추천드립니다.

#### 장점
- 터미널에서 빠르게 실행 가능
- 스크립트나 자동화에 활용하기 좋음
- 다양한 CLI 옵션 사용 가능
- 개발자 친화적인 환경

#### 설치 방법

**1단계: Node.js 버전 확인**

OpenClaw는 **Node.js 22 이상**이 필요합니다. 먼저 버전을 확인하세요:

```bash
node --version
```

버전이 22 미만이거나 설치되어 있지 않다면, nvm을 사용하여 설치하세요:

```bash
# nvm이 없다면 먼저 설치
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.zprofile

# Node.js 22 설치
nvm install 22
nvm use 22
```

**2단계: OpenClaw 설치**

터미널에서 아래 명령어를 실행하세요:

```bash
brew install openclaw
```

설치에는 보통 1~2분 정도 소요됩니다. 진행 상황이 터미널에 표시되니 기다려 주세요.

**3단계: 설치 확인**

설치가 완료되면 버전을 확인해 보세요:

```bash
openclaw --version
```

`openclaw version 1.x.x` 같은 형태로 버전 정보가 출력되면 성공입니다!

**4단계: 온보딩 (Onboard)**

온보딩 과정에서 API 키를 등록하고 기본 설정을 완료합니다:

```bash
openclaw onboard
```

온보딩 마법사가 시작되면 안내에 따라 진행하세요:

```
🦞 Welcome to OpenClaw Onboarding!

? Select your authentication method:
❯ Anthropic OAuth (Claude)
  OpenAI OAuth (GPT)
  manual (직접 API Key 입력)
```

1. 사용할 AI 모델 선택 (Claude의 경우 Anthropic OAuth, OpenAI의 경우 OpenAI OAuth)
2. 브라우저 인증 또는 API 키 입력
3. 기본 설정 확인

**설치 순서 요약:**
```
brew install openclaw → openclaw onboard
```

이 순서를 따르시면 Mac에서 CLI 버전 OpenClaw를 완벽하게 설정하실 수 있어요!

---

## Step 4: API 키 발급받기

OpenClaw를 사용하려면 AI 모델의 API 키가 필요합니다. Claude(Anthropic) 또는 OpenAI 중 하나를 선택하시면 됩니다. 둘 다 발급받아 두시면 상황에 따라 선택해서 사용하실 수 있어요!

**참고:** OAuth 인증을 사용하면 API 키 없이도 무료 할당량을 활용할 수 있습니다. `openclaw onboard`에서 Anthropic OAuth 또는 OpenAI OAuth를 선택하세요.

### Claude API 키 발급 (Anthropic)

Claude는 Anthropic에서 만든 AI 모델입니다. 자연스러운 대화와 코딩 능력이 뛰어나요.

**발급 방법:**

1. [Anthropic Console](https://console.anthropic.com/)에 접속합니다.
2. 계정이 없다면 "Sign Up"을 클릭하여 회원가입합니다.
   - 이메일 인증이 필요합니다.
3. 로그인 후 좌측 메뉴에서 "API Keys"를 클릭합니다.
4. "Create Key" 버튼을 클릭합니다.
5. 키 이름을 입력합니다 (예: "OpenClaw-Mac").
6. 생성된 API 키를 **반드시 복사해서 안전한 곳에 저장**하세요!
   - 이 키는 한 번만 표시되며, 다시 볼 수 없습니다.

**요금 안내:**
- 신규 가입 시 무료 크레딧이 제공될 수 있습니다.
- 이후에는 사용량에 따라 과금됩니다.
- 자세한 요금은 [Anthropic 요금 페이지](https://www.anthropic.com/pricing)를 참고하세요.

### OpenAI API 키 발급

GPT 모델을 사용하고 싶으시다면 OpenAI API 키를 발급받으세요.

**발급 방법:**

1. [OpenAI Platform](https://platform.openai.com/)에 접속합니다.
2. 계정이 없다면 "Sign Up"을 클릭하여 회원가입합니다.
3. 로그인 후 우측 상단 프로필 아이콘을 클릭합니다.
4. "View API keys" 또는 "API keys"를 선택합니다.
5. "Create new secret key" 버튼을 클릭합니다.
6. 키 이름을 입력하고 생성합니다.
7. 생성된 API 키를 **반드시 복사해서 안전한 곳에 저장**하세요!

**요금 안내:**
- 신규 가입 시 무료 크레딧이 제공됩니다 (기간 제한 있음).
- 이후에는 사용량에 따라 과금됩니다.
- 자세한 요금은 [OpenAI 요금 페이지](https://openai.com/pricing)를 참고하세요.

### API 키 보안 주의사항

API 키는 여러분의 계정과 직결되는 중요한 정보입니다. 다음 사항을 꼭 지켜주세요:

- API 키를 GitHub 등 공개 저장소에 올리지 마세요.
- 다른 사람과 공유하지 마세요.
- 키가 노출되었다면 즉시 삭제하고 새로 발급받으세요.
- 정기적으로 키를 교체하는 것이 좋습니다.

---

## Step 5: OpenClaw 실행하기

설치와 설정이 모두 완료되었습니다! 이제 OpenClaw를 실행해 볼까요?

### OpenClaw.app 사용자

1. 메뉴 바의 OpenClaw 아이콘을 클릭합니다.
2. "New Chat" 또는 "새 대화"를 선택합니다.
3. 대화창이 열리면 원하는 내용을 입력하세요!

### CLI 사용자

터미널에서 아래 명령어를 입력하세요:

```bash
openclaw
```

환영 메시지와 함께 OpenClaw가 시작됩니다!

```
🦞 Welcome to OpenClaw!
Type your message and press Enter to chat.
Type /help for available commands.
```

### 첫 대화 테스트

간단한 테스트를 해볼까요? 아래 내용을 입력해 보세요:

```
안녕하세요! 오늘 날씨가 어때요?
```

AI가 응답하면 설치가 성공적으로 완료된 것입니다!

### 유용한 CLI 명령어

OpenClaw CLI에서 사용할 수 있는 유용한 명령어들입니다:

| 명령어 | 설명 |
|--------|------|
| `/help` | 사용 가능한 명령어 목록 보기 |
| `/clear` | 대화 내용 지우기 |
| `/model` | 현재 사용 중인 모델 확인 |
| `/model [이름]` | 다른 모델로 변경 |
| `/exit` 또는 `/quit` | OpenClaw 종료 |
| `/config` | 설정 확인 및 변경 |

---

## Step 6: 디스코드 연동하기

OpenClaw를 디스코드와 연동하면 디스코드 서버에서 바로 AI와 대화할 수 있습니다. 팀원들과 함께 사용하거나, 커뮤니티에서 활용하기 좋아요!

### 사전 준비

- 디스코드 계정이 필요합니다.
- 연동할 디스코드 서버의 관리자 권한이 필요합니다.

### 디스코드 봇 생성하기

**1단계: Discord Developer Portal 접속**
1. [Discord Developer Portal](https://discord.com/developers/applications)에 접속합니다.
2. 디스코드 계정으로 로그인합니다.

**2단계: 새 애플리케이션 생성**
1. 우측 상단의 "New Application" 버튼을 클릭합니다.
2. 애플리케이션 이름을 입력합니다 (예: "OpenClaw Bot").
3. 약관에 동의하고 "Create"를 클릭합니다.

**3단계: 봇 설정**
1. 좌측 메뉴에서 "Bot"을 클릭합니다.
2. "Add Bot" 버튼을 클릭합니다.
3. "Yes, do it!" 을 클릭하여 확인합니다.

**4단계: 봇 토큰 복사**
1. Bot 페이지에서 "Reset Token" 버튼을 클릭합니다.
2. 생성된 토큰을 복사합니다.
3. 이 토큰은 **절대 공개하지 마세요!**

**5단계: 봇 권한 설정**
1. 좌측 메뉴에서 "OAuth2" → "URL Generator"를 클릭합니다.
2. SCOPES에서 "bot"을 체크합니다.
3. BOT PERMISSIONS에서 다음 권한을 체크합니다:
   - Send Messages
   - Read Message History
   - Use Slash Commands
4. 하단에 생성된 URL을 복사합니다.

**6단계: 서버에 봇 초대**
1. 복사한 URL을 브라우저에 붙여넣습니다.
2. 봇을 추가할 서버를 선택합니다.
3. "승인"을 클릭합니다.

### OpenClaw에 디스코드 연동하기

**OpenClaw.app 사용자:**
1. 메뉴 바에서 OpenClaw 아이콘 클릭
2. Settings → Integrations → Discord
3. 봇 토큰 입력
4. "Connect" 클릭

**CLI 사용자:**

설정 파일(`~/.openclaw/config.yaml`)을 열고 다음과 같이 설정합니다:

```yaml
channels:
  discord:
    enabled: true
    botToken: "YOUR_BOT_TOKEN"
    agentMappings:
      - channelId: "채널ID"
        agentName: "에이전트이름"
```

그 후 디스코드 서비스를 시작합니다:

```bash
openclaw discord start
```

연동이 완료되면 디스코드 서버에서 봇을 멘션하거나 슬래시 명령어로 OpenClaw를 사용할 수 있습니다!

---

## Troubleshooting: 문제 해결 가이드

설치나 사용 중 문제가 발생했나요? 자주 발생하는 문제와 해결 방법을 정리했습니다.

### "command not found: brew" 오류

**원인:** Homebrew가 설치되지 않았거나, PATH에 등록되지 않았습니다.

**해결 방법:**

1. Homebrew를 다시 설치해 보세요:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2. Apple Silicon Mac이라면 PATH 설정을 확인하세요:
```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
source ~/.zprofile
```

3. Intel Mac이라면:
```bash
echo 'eval "$(/usr/local/bin/brew shellenv)"' >> ~/.zprofile
source ~/.zprofile
```

### "command not found: openclaw" 오류

**원인:** OpenClaw가 설치되지 않았거나, PATH에 등록되지 않았습니다.

**해결 방법:**

1. 설치 여부 확인:
```bash
brew list | grep openclaw
```

2. 설치되어 있지 않다면 다시 설치:
```bash
brew install openclaw
```

3. 설치되어 있는데 안 된다면 터미널을 재시작하거나:
```bash
source ~/.zprofile
```

### Node.js 버전 오류

**원인:** Node.js 22 미만 버전이 설치되어 있습니다.

**해결 방법:**

```bash
# nvm으로 Node.js 22 설치
nvm install 22
nvm use 22
nvm alias default 22
```

### API 키 오류 (Invalid API Key)

**원인:** API 키가 잘못 입력되었거나, 만료되었습니다.

**해결 방법:**

1. API 키를 다시 확인하세요. 복사할 때 앞뒤 공백이 포함되지 않았는지 확인하세요.

2. 키를 다시 등록해 보세요:
```bash
openclaw config set api.key "YOUR_API_KEY"
```

3. 키가 만료되었다면 새로 발급받으세요.

### "개발자를 확인할 수 없습니다" 경고 (OpenClaw.app)

**원인:** macOS의 보안 정책으로 인해 확인되지 않은 개발자의 앱 실행을 차단합니다.

**해결 방법:**

1. `시스템 설정` → `개인정보 보호 및 보안`으로 이동합니다.
2. 하단에 "OpenClaw.app이 차단되었습니다" 메시지를 찾습니다.
3. "확인 없이 열기" 버튼을 클릭합니다.
4. 다시 앱을 실행합니다.

### 네트워크 연결 오류

**원인:** 인터넷 연결 문제 또는 방화벽 설정

**해결 방법:**

1. 인터넷 연결을 확인하세요.
2. VPN을 사용 중이라면 잠시 끄고 시도해 보세요.
3. 방화벽 설정에서 OpenClaw를 허용해 주세요.

### 응답이 너무 느린 경우

**원인:** API 서버 부하 또는 네트워크 지연

**해결 방법:**

1. 잠시 후 다시 시도해 보세요.
2. 다른 모델로 변경해 보세요:
```bash
openclaw config set model "claude-3-haiku"
```
3. 스트리밍 모드를 활성화하면 응답을 실시간으로 볼 수 있습니다:
```bash
openclaw config set streaming true
```

### Homebrew 업데이트 후 오류

**원인:** Homebrew 업데이트로 인한 의존성 문제

**해결 방법:**

1. OpenClaw를 재설치해 보세요:
```bash
brew uninstall openclaw
brew install openclaw
```

2. Homebrew 자체를 업데이트하고 정리:
```bash
brew update
brew upgrade
brew cleanup
```

### 그래도 해결이 안 된다면?

1. [OpenClaw GitHub Issues](https://github.com/openclaw/openclaw/issues)에서 비슷한 문제를 검색해 보세요.
2. 새로운 이슈를 등록할 때는 다음 정보를 포함해 주세요:
   - macOS 버전 (`sw_vers` 명령어로 확인)
   - OpenClaw 버전 (`openclaw --version`)
   - 에러 메시지 전문
   - 재현 방법

---

## 마무리

축하합니다! Mac에서 OpenClaw 설치를 완료하셨습니다. 이제 강력한 AI 어시스턴트와 함께 다양한 작업을 수행하실 수 있어요.

**Mac 설치 핵심 요약:**
- **Stable Workflow (권장)**: OpenClaw.app 메뉴 바 앱 설치
- **CLI 설치 순서**: `brew install openclaw` → `openclaw onboard`
- **Node.js 요구사항**: 버전 22 이상 필요
- **OAuth 등록**: `openclaw onboard` 마법사에서 Anthropic OAuth 또는 OpenAI OAuth 선택

**다음 단계로 추천드리는 것들:**
- 다양한 프롬프트를 시도해 보세요.
- `/help` 명령어로 더 많은 기능을 탐색해 보세요.
- 디스코드 커뮤니티에 참여해서 다른 사용자들과 팁을 공유해 보세요.

궁금한 점이 있으시면 언제든 [OpenClaw 디스코드](https://discord.gg/openclaw)에서 질문해 주세요!

더 자세한 내용은 [OpenClaw 공식 문서](https://docs.openclaw.ai)를 참고하세요.
