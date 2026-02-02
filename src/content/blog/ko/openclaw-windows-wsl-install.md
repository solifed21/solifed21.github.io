---
title: "OpenClaw Windows(WSL2) 설치 가이드: 완벽 커맨드 정리"
description: "Windows 환경에서 WSL2를 사용하여 OpenClaw를 설치하는 방법입니다. systemd 활성화부터 포트 포워딩 명령어까지 상세히 안내합니다."
pubDate: 2026-02-02T11:00:00+09:00
category: "AI"
tags: ["OpenClaw", "Windows", "WSL2", "Ubuntu", "명령어"]
---

Windows에서는 WSL2(Linux용 Windows 하위 시스템)를 사용하는 것이 가장 안정적입니다.

## 1단계: WSL2 및 Ubuntu 설치

관리자 권한의 PowerShell에서 다음을 실행하세요.

```powershell
# WSL 설치 및 Ubuntu 기본 세팅
wsl --install

# 설치 확인
wsl --status
```
*재부팅이 필요할 수 있습니다.*

## 2단계: Linux 내부 설정 (Ubuntu)

Ubuntu 터미널을 열고 다음을 진행하세요.

```bash
# 시스템 업데이트
sudo apt update && sudo apt upgrade -y

# systemd 활성화 (OpenClaw 서비스 실행에 필수)
sudo tee /etc/wsl.conf >/dev/null <<'EOF'
[boot]
systemd=true
EOF

# 이후 PowerShell에서 'wsl --shutdown' 후 다시 접속
```

## 3단계: Node.js 및 OpenClaw 설치

```bash
# Node.js 22 설치
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs

# OpenClaw 설치
npm install -g openclaw@latest
```

## 4단계: 온보딩 및 게이트웨이 실행

```bash
# 초기 설정 시작
openclaw onboard --install-daemon

# 게이트웨이 상태 확인
openclaw status

# 게이트웨이 실행 (수동)
openclaw gateway start
```

## 5단계: 윈도우에서 접속하기

WSL 내부에서 돌아가는 게이트웨이에 윈도우 브라우저로 접속할 수 있습니다.
- 주소: `http://127.0.0.1:18789`

---
**고급:** 외부 기기에서 WSL 게이트웨이에 접속하고 싶다면 `netsh interface portproxy` 명령어를 사용하여 포트를 열어주어야 합니다. 🛡️
