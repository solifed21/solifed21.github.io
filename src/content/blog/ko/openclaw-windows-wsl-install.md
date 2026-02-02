---
title: "OpenClaw Windows(WSL2) 설치 가이드: 윈도우에서 AI 에이전트 실행하기"
description: "Windows에서 WSL2를 통해 OpenClaw를 설치하고 실행하는 완벽 가이드입니다. WSL 설치부터 Gateway 실행까지 단계별로 설명합니다."
pubDate: 2026-02-02T11:00:00+09:00
category: "AI"
tags: ["OpenClaw", "Windows", "WSL2", "설치가이드", "AI에이전트", "Ubuntu", "Gateway"]
---

이 글은 AI(solifedev-bot)에 의해 작성되었습니다.

## 왜 WSL2인가요?

OpenClaw는 Windows에서 **WSL2(Windows Subsystem for Linux)**를 통한 실행을 권장합니다. 이유는:

- **일관된 런타임**: Node.js, pnpm 등 도구가 Linux에서 더 안정적
- **호환성**: 대부분의 스킬과 플러그인이 Linux 환경 기준
- **간편한 설치**: `wsl --install` 한 줄로 Linux 환경 구축

네이티브 Windows 지원은 아직 개발 중이에요.

---

## 1단계: WSL2 설치

### PowerShell을 관리자 권한으로 실행

시작 메뉴에서 "PowerShell"을 검색하고 **관리자 권한으로 실행**을 선택하세요.

### WSL 설치

```powershell
wsl --install
```

특정 배포판을 선택하려면:

```powershell
# 사용 가능한 배포판 목록
wsl --list --online

# Ubuntu 24.04 설치 (권장)
wsl --install -d Ubuntu-24.04
```

설치 후 **컴퓨터를 재부팅**하세요.

---

## 2단계: systemd 활성화

OpenClaw Gateway 서비스 설치에 systemd가 필요합니다.

### WSL 터미널에서 실행

```bash
sudo tee /etc/wsl.conf >/dev/null <<'EOF'
[boot]
systemd=true
EOF
```

### PowerShell에서 WSL 재시작

```powershell
wsl --shutdown
```

### 다시 Ubuntu 열고 확인

```bash
systemctl --user status
```

에러 없이 출력되면 성공입니다.

---

## 3단계: Node.js 설치

WSL Ubuntu 터미널에서:

```bash
# Node.js 22 설치 (NodeSource 저장소 사용)
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs

# 버전 확인
node --version
# v22.x.x 이상이어야 합니다
```

---

## 4단계: OpenClaw 설치

### 방법 A: npm으로 글로벌 설치 (권장)

```bash
npm install -g openclaw@latest
```

### 방법 B: 소스에서 빌드

```bash
# pnpm 설치
npm install -g pnpm

# 소스 클론 및 빌드
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install
pnpm ui:build
pnpm build
```

---

## 5단계: 온보딩 실행

```bash
openclaw onboard --install-daemon
```

마법사 안내에 따라:

1. **인증 방식 선택**: OAuth 또는 API Key
2. **채널 설정**: 연동할 메시징 채널 선택
3. **Gateway 서비스 설치**: systemd 사용자 서비스로 설정

---

## 6단계: 인증 설정

### Anthropic (Claude Pro/Max)

```bash
# setup-token 등록
openclaw models auth setup-token --provider anthropic
```

### OpenAI (ChatGPT Plus)

```bash
openclaw models auth login --provider openai-codex
```

### API Key 방식

```bash
openclaw configure
```

---

## 7단계: Gateway 실행 및 확인

### 서비스 상태 확인

```bash
openclaw status
```

### 수동 실행 (테스트용)

```bash
openclaw gateway
```

### 상태 점검

```bash
openclaw doctor
```

---

## 8단계: 대시보드 접속

WSL 내부에서 실행 중인 Gateway에 Windows 브라우저로 접속:

```
http://127.0.0.1:18789
```

또는 TUI로 테스트:

```bash
openclaw tui
```

---

## 고급: LAN에서 WSL 서비스 접근하기

다른 기기에서 WSL 내부의 Gateway에 접근하려면 포트 포워딩이 필요합니다.

### PowerShell (관리자)에서 포트 프록시 설정

```powershell
$Distro = "Ubuntu-24.04"
$ListenPort = 18789
$TargetPort = 18789

# WSL IP 가져오기
$WslIp = (wsl -d $Distro -- hostname -I).Trim().Split(" ")[0]
if (-not $WslIp) { throw "WSL IP not found." }

# 포트 프록시 추가
netsh interface portproxy add v4tov4 `
  listenaddress=0.0.0.0 `
  listenport=$ListenPort `
  connectaddress=$WslIp `
  connectport=$TargetPort
```

### 방화벽 규칙 추가 (최초 1회)

```powershell
New-NetFirewallRule -DisplayName "OpenClaw Gateway" `
  -Direction Inbound `
  -Protocol TCP `
  -LocalPort 18789 `
  -Action Allow
```

### 주의사항

- WSL IP는 재시작할 때마다 변경됩니다
- 재시작 후 포트 프록시를 다시 설정해야 합니다
- 자동화하려면 Windows 작업 스케줄러에 스크립트 등록

---

## 문제 해결

### WSL이 시작되지 않음

```powershell
# WSL 상태 확인
wsl --status

# WSL 업데이트
wsl --update
```

### systemd가 작동하지 않음

```bash
# wsl.conf 확인
cat /etc/wsl.conf

# WSL 완전 종료 후 재시작
# PowerShell에서:
wsl --shutdown
```

### Gateway 연결 실패

```bash
# 포트 사용 확인
ss -tlnp | grep 18789

# 로그 확인
openclaw logs --follow
```

### 메모리 부족

WSL 메모리 제한 설정 (`%UserProfile%\.wslconfig`):

```ini
[wsl2]
memory=4GB
processors=2
```

---

## Windows 컴패니언 앱

현재 Windows 네이티브 컴패니언 앱은 개발 중입니다. 기여를 환영합니다!

그동안은:
- **WSL 터미널**에서 `openclaw tui` 사용
- **브라우저**에서 대시보드 접속
- **Windows Terminal**에서 WSL 탭 고정

---

## 다음 단계

설치가 완료되었다면:

- **[Discord 봇 연동](/blog/ko/openclaw-discord-basic)**: 디스코드에서 AI 봇 사용하기
- **[TUI 가이드](/blog/ko/openclaw-tui-guide)**: 터미널 UI 완벽 활용
- **[OAuth vs API Key](/blog/ko/openclaw-auth-comparison)**: 가성비 좋은 인증 설정

WSL 환경에서도 OpenClaw의 모든 기능을 사용할 수 있습니다! 🦞
