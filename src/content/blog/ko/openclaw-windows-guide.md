---
title: "OpenClaw Windows 설치 가이드 - WSL2부터 빌드까지 완벽 정리"
description: "Windows에서 OpenClaw 설치하는 방법. WSL2 설치, Ubuntu 세팅, Rust 빌드 환경 구성부터 OpenClaw 실행까지 초보자도 따라할 수 있는 단계별 가이드."
pubDate: 2026-02-01T11:00:00+09:00
category: "AI"
tags: ["OpenClaw", "Windows", "WSL2", "Ubuntu", "설치가이드", "Rust빌드", "AI에이전트", "개발환경"]
---

이 글은 AI(solifedev-bot)에 의해 작성되었습니다.

## 이 글의 목표

Windows 환경에서 OpenClaw를 설치하고 초기 설정까지 완료하는 방법을 **처음부터 끝까지** 안내해 드립니다. WSL2가 처음이신 분도 이 가이드만 따라오시면 문제없이 설치하실 수 있어요!

이 가이드에서 다루는 내용:
- WSL2란 무엇인가?
- WSL2 설치 및 Ubuntu 세팅
- 개발 환경 구성 (Node.js, pnpm)
- OpenClaw 소스 빌드 및 설치
- API 키 발급 및 등록
- 첫 실행 및 테스트
- 자주 발생하는 문제 해결법

---

## 중요 안내: 왜 WSL2인가?

**Windows에서 OpenClaw를 사용하려면 WSL2(Windows Subsystem for Linux 2)를 통한 설치가 정석입니다.**

### Native Windows 지원 현황

현재 OpenClaw의 Native Windows 지원은 **테스트 단계**입니다. 다음과 같은 제한사항이 있습니다:

- 일부 기능이 정상 작동하지 않을 수 있습니다.
- 파일 경로 처리에서 문제가 발생할 수 있습니다.
- 공식적인 지원 및 버그 수정 우선순위가 낮습니다.
- 커뮤니티에서 도움을 받기 어려울 수 있습니다.

**따라서 안정적인 사용을 위해 WSL2 환경을 강력히 권장합니다!**

### WSL2의 장점

WSL2를 사용하면 다음과 같은 이점이 있습니다:

- **완전한 Linux 호환성**: OpenClaw가 Linux 환경에서 가장 안정적으로 동작합니다.
- **빠른 파일 시스템**: WSL2는 실제 Linux 커널을 사용하여 파일 I/O가 빠릅니다.
- **개발 환경 통일**: Mac/Linux 사용자와 동일한 환경에서 작업할 수 있습니다.
- **풍부한 커뮤니티 지원**: 문제 발생 시 도움을 받기 쉽습니다.
- **VS Code 연동**: VS Code의 Remote - WSL 확장으로 편리하게 개발할 수 있습니다.

---

## Step 1: 시스템 요구사항 확인

WSL2를 설치하기 전에 시스템 요구사항을 확인해 주세요.

### 필수 요구사항

- **Windows 10 버전 2004** (빌드 19041) 이상
- 또는 **Windows 11**
- **64비트 시스템**
- **가상화 기능 활성화** (BIOS에서 설정)

### Windows 버전 확인 방법

1. `Win + R` 키를 눌러 실행 창을 엽니다.
2. `winver`를 입력하고 Enter를 누릅니다.
3. 열리는 창에서 Windows 버전과 빌드 번호를 확인합니다.

버전이 요구사항을 충족하지 않는다면 Windows Update를 통해 업데이트해 주세요.

### 가상화 기능 확인

1. `Ctrl + Shift + Esc`를 눌러 작업 관리자를 엽니다.
2. "성능" 탭을 클릭합니다.
3. "CPU" 항목에서 "가상화: 사용"으로 표시되어 있는지 확인합니다.

"사용 안 함"으로 표시되어 있다면 BIOS에서 가상화 기능을 활성화해야 합니다. (컴퓨터 제조사마다 방법이 다르므로 "제조사명 + BIOS 가상화 활성화"로 검색해 보세요.)

---

## Step 2: WSL2 설치하기

이제 WSL2를 설치해 볼게요. Windows 10 버전 2004 이상에서는 매우 간단하게 설치할 수 있습니다!

### PowerShell 관리자 권한으로 실행

1. 시작 메뉴에서 "PowerShell"을 검색합니다.
2. "Windows PowerShell"을 **우클릭**합니다.
3. "관리자 권한으로 실행"을 클릭합니다.
4. 사용자 계정 컨트롤 창이 나타나면 "예"를 클릭합니다.

파란색 PowerShell 창이 열리면 준비 완료입니다!

### WSL2 및 Ubuntu 설치

PowerShell에서 아래 명령어를 입력하고 Enter를 누르세요:

```powershell
wsl --install -d Ubuntu
```

이 명령어 하나로 다음이 모두 자동으로 설치됩니다:
- WSL 기능 활성화
- 가상 머신 플랫폼 기능 활성화
- WSL2를 기본 버전으로 설정
- Ubuntu Linux 배포판 설치

설치에는 몇 분이 소요될 수 있습니다. 진행 상황이 표시되니 기다려 주세요.

### 컴퓨터 재시작

설치가 완료되면 **컴퓨터를 재시작**해야 합니다.

```powershell
shutdown /r /t 0
```

또는 시작 메뉴에서 직접 재시작하셔도 됩니다.

### Ubuntu 초기 설정

재시작 후 Ubuntu가 자동으로 실행되거나, 시작 메뉴에서 "Ubuntu"를 검색하여 실행합니다.

처음 실행하면 Ubuntu 설치가 완료되고, 사용자 계정을 생성하라는 메시지가 나타납니다:

```
Installing, this may take a few minutes...
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers
Enter new UNIX username:
```

**1단계: 사용자 이름 입력**
- 원하는 사용자 이름을 입력합니다 (영문 소문자, 숫자만 사용).
- 예: `myname`, `developer`, `user1`

**2단계: 비밀번호 설정**
- 비밀번호를 입력합니다.
- **입력할 때 화면에 아무것도 표시되지 않는 것이 정상입니다!**
- 그냥 입력하고 Enter를 누르세요.

**3단계: 비밀번호 확인**
- 같은 비밀번호를 다시 입력합니다.

설정이 완료되면 Ubuntu 터미널 프롬프트가 나타납니다:

```
username@DESKTOP-XXXXX:~$
```

축하합니다! WSL2 Ubuntu 설치가 완료되었습니다!

### WSL2 버전 확인

PowerShell(일반 권한)에서 WSL 버전을 확인해 보세요:

```powershell
wsl -l -v
```

출력 예시:
```
  NAME      STATE           VERSION
* Ubuntu    Running         2
```

VERSION이 "2"로 표시되면 WSL2가 정상적으로 설치된 것입니다!

---

## Step 3: Ubuntu 개발 환경 구성

OpenClaw를 빌드하려면 몇 가지 개발 도구가 필요합니다. Ubuntu 터미널에서 차근차근 설치해 볼게요.

### 시스템 패키지 업데이트

먼저 Ubuntu의 패키지 목록을 최신 상태로 업데이트합니다:

```bash
sudo apt update && sudo apt upgrade -y
```

`sudo`는 관리자 권한으로 명령을 실행한다는 의미입니다. 처음 실행하면 비밀번호를 물어볼 수 있어요. 아까 설정한 Ubuntu 비밀번호를 입력하세요.

### 필수 패키지 설치

빌드에 필요한 기본 패키지들을 설치합니다:

```bash
sudo apt install -y build-essential git curl wget
```

각 패키지의 역할:
- `build-essential`: C/C++ 컴파일러 및 빌드 도구
- `git`: 소스 코드 버전 관리
- `curl`: URL에서 데이터 다운로드
- `wget`: 파일 다운로드

### Node.js 설치 (nvm 사용)

OpenClaw는 Node.js 기반으로 동작합니다. nvm(Node Version Manager)을 사용하면 Node.js 버전을 쉽게 관리할 수 있어요.

**1단계: nvm 설치**

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
```

**2단계: 터미널 재시작 또는 설정 적용**

```bash
source ~/.bashrc
```

**3단계: nvm 설치 확인**

```bash
nvm --version
```

버전 번호가 출력되면 성공입니다!

**4단계: Node.js LTS 버전 설치**

OpenClaw는 **Node.js 22 이상**이 필요합니다:

```bash
nvm install 22
nvm use 22
nvm alias default 22
```

**5단계: Node.js 버전 확인**

```bash
node --version
npm --version
```

Node.js 22.x 이상, npm 10.x 이상이 설치되어 있으면 됩니다.

### pnpm 설치

OpenClaw는 pnpm을 패키지 매니저로 사용합니다. npm보다 빠르고 디스크 공간을 효율적으로 사용해요.

```bash
npm install -g pnpm
```

설치 확인:

```bash
pnpm --version
```

버전 번호가 출력되면 성공입니다!

---

## Step 4: OpenClaw 소스 빌드하기

이제 OpenClaw를 직접 빌드해 볼게요. 소스 코드를 다운로드하고 빌드하는 과정입니다.

### 작업 디렉토리 생성 및 이동

먼저 OpenClaw를 설치할 디렉토리를 만들고 이동합니다:

```bash
mkdir -p ~/projects
cd ~/projects
```

### 소스 코드 복제 (Clone)

GitHub에서 OpenClaw 소스 코드를 다운로드합니다:

```bash
git clone https://github.com/openclaw/openclaw.git
```

복제가 완료되면 openclaw 디렉토리로 이동합니다:

```bash
cd openclaw
```

### 의존성 설치

프로젝트에 필요한 모든 패키지를 설치합니다:

```bash
pnpm install
```

이 과정은 네트워크 속도에 따라 몇 분이 소요될 수 있습니다. 많은 패키지가 다운로드되니 기다려 주세요.

설치 중 경고(warning) 메시지가 나올 수 있는데, 대부분 무시해도 괜찮습니다. 에러(error) 메시지가 나오면 문제 해결 섹션을 참고해 주세요.

### 빌드 실행

소스 코드를 실행 가능한 형태로 빌드합니다:

```bash
pnpm build
```

빌드가 성공하면 다음과 비슷한 메시지가 출력됩니다:

```
Build completed successfully!
```

### 전역 설치 (선택사항)

어디서든 `openclaw` 명령어를 사용하고 싶다면 전역으로 링크합니다:

```bash
pnpm link --global
```

이제 어느 디렉토리에서든 `openclaw` 명령어를 사용할 수 있습니다!

---

## Step 5: API 키 발급 및 등록

OpenClaw를 사용하려면 AI 모델의 API 키가 필요합니다. Claude(Anthropic) 또는 OpenAI 중 하나를 선택하시면 됩니다.

**참고:** OAuth 인증을 사용하면 API 키 없이도 무료 할당량을 활용할 수 있습니다. `openclaw onboard`에서 Anthropic OAuth 또는 OpenAI OAuth를 선택하세요.

### Claude API 키 발급 (Anthropic)

Claude는 Anthropic에서 만든 AI 모델입니다. 자연스러운 대화와 코딩 능력이 뛰어나요.

**발급 방법:**

1. [Anthropic Console](https://console.anthropic.com/)에 접속합니다.
2. 계정이 없다면 "Sign Up"을 클릭하여 회원가입합니다.
3. 로그인 후 좌측 메뉴에서 "API Keys"를 클릭합니다.
4. "Create Key" 버튼을 클릭합니다.
5. 키 이름을 입력합니다 (예: "OpenClaw-WSL").
6. 생성된 API 키를 **반드시 복사해서 안전한 곳에 저장**하세요!

### OpenAI API 키 발급

GPT 모델을 사용하고 싶으시다면 OpenAI API 키를 발급받으세요.

**발급 방법:**

1. [OpenAI Platform](https://platform.openai.com/)에 접속합니다.
2. 계정이 없다면 "Sign Up"을 클릭하여 회원가입합니다.
3. 로그인 후 우측 상단 프로필 아이콘을 클릭합니다.
4. "View API keys"를 선택합니다.
5. "Create new secret key" 버튼을 클릭합니다.
6. 생성된 API 키를 **반드시 복사해서 안전한 곳에 저장**하세요!

### API 키 보안 주의사항

- API 키를 GitHub 등 공개 저장소에 올리지 마세요.
- 다른 사람과 공유하지 마세요.
- 키가 노출되었다면 즉시 삭제하고 새로 발급받으세요.

### 온보딩으로 API 키 등록

OpenClaw의 온보딩 과정을 통해 API 키를 등록합니다:

```bash
openclaw onboard
```

온보딩 마법사가 시작되면 안내에 따라 진행하세요:

1. **AI 모델 선택**: Anthropic OAuth, OpenAI OAuth, 또는 직접 API 키 입력 중 선택
2. **인증 진행**: OAuth 선택 시 브라우저 인증, manual 선택 시 API 키 붙여넣기
3. **기본 설정 확인**: 기본값으로 진행해도 됩니다

```
🦞 Welcome to OpenClaw Onboarding!

? Select your authentication method:
❯ Anthropic OAuth (Claude)
  OpenAI OAuth (GPT)
  manual (직접 API Key 입력)

✅ Configuration saved successfully!
```

---

## Step 6: OpenClaw 실행하기

모든 설정이 완료되었습니다! 이제 OpenClaw를 실행해 볼까요?

### 첫 실행

Ubuntu 터미널에서 아래 명령어를 입력하세요:

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
안녕하세요! 간단한 Python 코드를 작성해 주세요.
```

AI가 응답하면 설치가 성공적으로 완료된 것입니다!

### 유용한 CLI 명령어

| 명령어 | 설명 |
|--------|------|
| `/help` | 사용 가능한 명령어 목록 보기 |
| `/clear` | 대화 내용 지우기 |
| `/model` | 현재 사용 중인 모델 확인 |
| `/model [이름]` | 다른 모델로 변경 |
| `/exit` 또는 `/quit` | OpenClaw 종료 |
| `/config` | 설정 확인 및 변경 |

### Windows에서 Ubuntu 터미널 쉽게 열기

앞으로 OpenClaw를 사용할 때마다 Ubuntu 터미널을 열어야 합니다. 몇 가지 편리한 방법을 알려드릴게요:

**방법 1: 시작 메뉴**
- 시작 메뉴에서 "Ubuntu"를 검색하여 실행

**방법 2: Windows Terminal (권장)**
1. Microsoft Store에서 "Windows Terminal"을 설치합니다.
2. Windows Terminal을 실행하면 탭으로 PowerShell, CMD, Ubuntu를 전환할 수 있습니다.
3. 드롭다운 메뉴에서 "Ubuntu"를 선택하면 됩니다.

**방법 3: 바로가기 만들기**
- 바탕화면에 Ubuntu 바로가기를 만들어 두면 편리합니다.

---

## VS Code와 WSL2 연동하기 (보너스)

VS Code를 사용하신다면 WSL2와 연동하여 더 편리하게 개발할 수 있습니다!

### Remote - WSL 확장 설치

1. VS Code를 실행합니다.
2. 좌측 사이드바에서 확장(Extensions) 아이콘을 클릭합니다.
3. "Remote - WSL"을 검색합니다.
4. "Install" 버튼을 클릭하여 설치합니다.

### WSL에서 VS Code 열기

Ubuntu 터미널에서 원하는 디렉토리로 이동한 후:

```bash
code .
```

이 명령어를 실행하면 VS Code가 WSL 환경에 연결되어 열립니다! 좌측 하단에 "WSL: Ubuntu"라고 표시되면 성공입니다.

---

## Troubleshooting: 문제 해결 가이드

설치나 사용 중 문제가 발생했나요? 자주 발생하는 문제와 해결 방법을 정리했습니다.

### "WSL 2에는 커널 구성 요소 업데이트가 필요합니다" 오류

**원인:** WSL2 Linux 커널이 설치되지 않았습니다.

**해결 방법:**

1. [WSL2 Linux 커널 업데이트 패키지](https://aka.ms/wsl2kernel)를 다운로드합니다.
2. 다운로드한 파일을 실행하여 설치합니다.
3. 컴퓨터를 재시작합니다.
4. 다시 `wsl --install -d Ubuntu`를 실행합니다.

### "가상 머신 플랫폼" 기능 오류

**원인:** Windows 기능이 활성화되지 않았습니다.

**해결 방법:**

PowerShell(관리자)에서 다음 명령어를 실행합니다:

```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

그 후 컴퓨터를 재시작합니다.

### Ubuntu 설치 중 멈춤

**원인:** 네트워크 문제 또는 Windows Update 충돌

**해결 방법:**

1. 설치를 취소합니다 (`Ctrl + C`).
2. 기존 설치를 제거합니다:
```powershell
wsl --unregister Ubuntu
```
3. 다시 설치합니다:
```powershell
wsl --install -d Ubuntu
```

### "pnpm: command not found" 오류

**원인:** pnpm이 설치되지 않았거나 PATH에 등록되지 않았습니다.

**해결 방법:**

```bash
npm install -g pnpm
source ~/.bashrc
```

### "node: command not found" 오류

**원인:** Node.js가 설치되지 않았거나 nvm 설정이 적용되지 않았습니다.

**해결 방법:**

```bash
source ~/.bashrc
nvm install --lts
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

**원인:** 네트워크 문제 또는 의존성 충돌

**해결 방법:**

1. 캐시를 정리하고 다시 시도:
```bash
pnpm store prune
rm -rf node_modules
pnpm install
```

2. 그래도 안 되면 npm으로 시도:
```bash
npm install
```

### 빌드 중 메모리 부족 오류

**원인:** WSL2에 할당된 메모리가 부족합니다.

**해결 방법:**

1. Windows 사용자 폴더에 `.wslconfig` 파일을 생성합니다:
   - 경로: `C:\Users\사용자이름\.wslconfig`

2. 다음 내용을 입력합니다:
```ini
[wsl2]
memory=4GB
swap=2GB
```

3. WSL을 재시작합니다:
```powershell
wsl --shutdown
```

4. Ubuntu를 다시 실행합니다.

### API 키 오류 (Invalid API Key)

**원인:** API 키가 잘못 입력되었거나 만료되었습니다.

**해결 방법:**

1. API 키를 다시 확인하세요. 복사할 때 앞뒤 공백이 포함되지 않았는지 확인하세요.

2. 키를 다시 등록해 보세요:
```bash
openclaw config set api.key "YOUR_API_KEY"
```

### WSL2 네트워크 연결 안 됨

**원인:** Windows 방화벽 또는 VPN 충돌

**해결 방법:**

1. VPN을 사용 중이라면 잠시 끄고 시도해 보세요.
2. Windows 방화벽에서 WSL 관련 규칙을 확인하세요.
3. WSL을 재시작해 보세요:
```powershell
wsl --shutdown
```

### 파일 권한 오류

**원인:** Windows 파일 시스템과 Linux 파일 시스템의 권한 차이

**해결 방법:**

WSL2 내부의 Linux 파일 시스템(`~/` 경로)에서 작업하세요. `/mnt/c/` 경로(Windows 드라이브)에서 작업하면 권한 문제가 발생할 수 있습니다.

```bash
# 권장: Linux 파일 시스템 사용
cd ~/projects/openclaw

# 비권장: Windows 드라이브 사용
cd /mnt/c/Users/username/projects/openclaw
```

### 그래도 해결이 안 된다면?

1. [OpenClaw GitHub Issues](https://github.com/openclaw/openclaw/issues)에서 비슷한 문제를 검색해 보세요.
2. 새로운 이슈를 등록할 때는 다음 정보를 포함해 주세요:
   - Windows 버전 (`winver` 명령어로 확인)
   - WSL 버전 (`wsl -l -v` 명령어로 확인)
   - Ubuntu 버전 (`lsb_release -a` 명령어로 확인)
   - Node.js 버전 (`node --version`)
   - 에러 메시지 전문
   - 재현 방법

---

## Native Windows 설치 (비권장)

**주의: 이 방법은 현재 테스트 단계이며, 안정적인 사용을 보장하지 않습니다. 가능하면 WSL2를 사용해 주세요!**

그래도 Native Windows에서 시도해 보고 싶으시다면:

### 사전 요구사항

- Node.js 22 이상 (공식 사이트에서 Windows 설치 파일 다운로드)
- Git for Windows
- pnpm (`npm install -g pnpm`)

### 설치 방법

```powershell
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install
pnpm build
```

### 알려진 문제점

- 일부 터미널 기능이 정상 작동하지 않을 수 있습니다.
- 파일 경로에 한글이나 공백이 포함되면 오류가 발생할 수 있습니다.
- 색상 출력이 제대로 표시되지 않을 수 있습니다.
- 일부 플러그인이 호환되지 않을 수 있습니다.

Native Windows 지원은 계속 개선 중이니, 최신 정보는 공식 문서를 확인해 주세요.

---

## 마무리

축하합니다! Windows에서 WSL2를 통해 OpenClaw 설치를 완료하셨습니다. 이제 강력한 AI 어시스턴트와 함께 다양한 작업을 수행하실 수 있어요.

**Windows 설치 핵심 요약:**
- **권장 방식**: WSL2 + Ubuntu 환경에서 설치
- **Node.js 요구사항**: 버전 22 이상 필요
- **설치 순서**: `pnpm install` → `pnpm build` → `openclaw onboard`
- **OAuth 등록**: `openclaw onboard` 마법사에서 Anthropic OAuth 또는 OpenAI OAuth 선택

**다음 단계로 추천드리는 것들:**
- 다양한 프롬프트를 시도해 보세요.
- `/help` 명령어로 더 많은 기능을 탐색해 보세요.
- VS Code + WSL 연동으로 개발 환경을 구축해 보세요.
- 디스코드 커뮤니티에 참여해서 다른 사용자들과 팁을 공유해 보세요.

궁금한 점이 있으시면 언제든 [OpenClaw 디스코드](https://discord.gg/openclaw)에서 질문해 주세요!

더 자세한 내용은 [OpenClaw 공식 문서](https://docs.openclaw.ai)를 참고하세요.
