---
title: "비개발자 가이드: Windows에서 OpenClaw 완벽 설치하기"
description: "Windows 사용자를 위한 OpenClaw 설치부터 디스코드 봇 연동까지 완벽 가이드"
pubDate: 2026-02-01T11:00:00+09:00
category: "AI"
---

## 🎯 이 글의 목표

이 글 하나만 따라하면 Windows에서 OpenClaw 설치부터 디스코드 봇 연동까지 **완벽하게** 끝낼 수 있음. 비개발자 기준으로 작성했으니 겁먹지 말고 따라오셈.

예상 소요 시간: **30분 ~ 1시간**

---

## 📋 준비물

- Windows 10 (버전 1809 이상) 또는 Windows 11
- 인터넷 연결
- AI API 키 (Claude 또는 OpenAI) - 없으면 아래에서 발급 방법 설명함

---

## 🔧 1단계: PowerShell 열기

PowerShell은 Windows의 "고급 명령어 입력창"임. 명령 프롬프트(cmd)보다 강력함.

**여는 방법:**
1. 키보드에서 `Windows 키` 누르기
2. "PowerShell" 입력
3. **"Windows PowerShell"** 우클릭 → **"관리자 권한으로 실행"** 클릭
4. "이 앱이 디바이스를 변경할 수 있도록 허용하시겠어요?" 나오면 "예" 클릭

파란 배경의 창이 뜨고 `PS C:\Users\사용자명>` 같은 텍스트가 보이면 성공임.

**⚠️ 중요**: 반드시 **관리자 권한**으로 실행해야 함. 일반 실행하면 설치 중 권한 에러 남.

**💡 팁**: PowerShell 자주 쓸 거니까 작업 표시줄에 고정해두면 편함. 아이콘 우클릭 → "작업 표시줄에 고정" 선택.

---

## 📦 2단계: winget 확인 및 설치

winget은 Windows용 프로그램 설치 도구임. Windows 11에는 기본 설치되어 있고, Windows 10은 확인이 필요함.

### winget 설치 확인

PowerShell에 입력하고 Enter:

```powershell
winget --version
```

`v1.x.xxxxx` 같은 버전 정보가 나오면 **이미 설치된 거**니까 3단계로 넘어가면 됨.

`'winget'은(는) 내부 또는 외부 명령...` 에러가 나오면 설치 필요함.

### winget 설치하기 (Windows 10)

1. Microsoft Store 앱 열기
2. "앱 설치 관리자" 검색
3. "앱 설치 관리자" 클릭 → "업데이트" 또는 "설치" 버튼 클릭
4. 설치 완료 후 **PowerShell 완전히 껐다가 다시 열기** (관리자 권한으로)

### 다시 확인

```powershell
winget --version
```

버전 정보 나오면 성공임.

---

## 🦞 3단계: OpenClaw 설치

드디어 본론임. PowerShell에 입력:

```powershell
winget install openclaw
```

**진행 과정:**
1. "동의하십니까?" 나오면 `Y` 입력하고 Enter
2. 설치 진행 (2~5분 소요)
3. "설치 완료" 메시지 나오면 끝

### 설치 확인

**PowerShell을 껐다가 다시 열고** (관리자 권한으로):

```powershell
openclaw --version
```

버전 정보 나오면 설치 완료임.

---

## 🔑 4단계: API 키 설정

OpenClaw가 AI 기능을 쓰려면 AI 서비스의 API 키가 필요함. Claude(Anthropic) 또는 OpenAI 중 하나 선택하면 됨.

### Claude API 키 발급 (추천)

1. [console.anthropic.com](https://console.anthropic.com) 접속
2. 회원가입 또는 로그인
3. 좌측 메뉴에서 "API Keys" 클릭
4. "Create Key" 버튼 클릭
5. 이름 입력하고 생성
6. 생성된 키 복사 (한 번만 보여주니까 꼭 메모장에 저장해둘 것!)

### OpenAI API 키 발급

1. [platform.openai.com](https://platform.openai.com) 접속
2. 회원가입 또는 로그인
3. 우측 상단 프로필 → "View API keys"
4. "Create new secret key" 클릭
5. 생성된 키 복사

### API 키 등록

PowerShell에서 설정 폴더 생성:

```powershell
mkdir $env:USERPROFILE\.openclaw -Force
notepad $env:USERPROFILE\.openclaw\config.yaml
```

메모장이 열리면 아래 내용 입력 (Claude 사용 시):

```yaml
ai:
  provider: anthropic
  api_key: sk-ant-xxxxx  # 여기에 본인 API 키 입력
```

OpenAI 사용 시:

```yaml
ai:
  provider: openai
  api_key: sk-xxxxx  # 여기에 본인 API 키 입력
```

저장: `Ctrl + S` → 메모장 닫기

**💡 더 안전한 방법**: 환경변수 사용

1. Windows 검색에서 "환경 변수" 검색
2. "시스템 환경 변수 편집" 클릭
3. "환경 변수" 버튼 클릭
4. "사용자 변수"에서 "새로 만들기" 클릭
5. 변수 이름: `ANTHROPIC_API_KEY` (또는 `OPENAI_API_KEY`)
6. 변수 값: API 키 붙여넣기
7. 확인 → 확인 → 확인
8. **PowerShell 완전히 껐다가 다시 열기**

이렇게 하면 config 파일에 키를 직접 안 써도 됨.

---

## 🚀 5단계: OpenClaw 실행

PowerShell에서:

```powershell
openclaw
```

환영 메시지가 나오면 성공임! 이제 AI에게 뭐든 시켜볼 수 있음.

### 첫 번째 테스트

OpenClaw 프롬프트에서 이렇게 입력해봄:

```
현재 폴더에 있는 파일 목록 보여줘
```

파일 목록이 나오면 정상 작동하는 거임.

---

## 🤖 6단계: 디스코드 봇 연동 (선택사항)

OpenClaw를 디스코드 봇으로 연동하면 디스코드에서 AI에게 명령을 내릴 수 있음. 아침마다 뉴스 요약 받기 같은 자동화가 가능해짐.

### 디스코드 봇 생성

1. [Discord Developer Portal](https://discord.com/developers/applications) 접속
2. "New Application" 클릭
3. 이름 입력 (예: "MyOpenClaw")하고 생성
4. 좌측 메뉴에서 "Bot" 클릭
5. "Add Bot" → "Yes, do it!" 클릭
6. "Reset Token" 클릭해서 봇 토큰 복사 (이것도 한 번만 보여줌! 메모장에 저장!)

### 봇 권한 설정

1. 같은 페이지에서 "Privileged Gateway Intents" 섹션 찾기
2. "Message Content Intent" 활성화 (중요!)
3. 저장

### 봇을 서버에 초대

1. 좌측 메뉴에서 "OAuth2" → "URL Generator" 클릭
2. SCOPES에서 "bot" 체크
3. BOT PERMISSIONS에서 필요한 권한 체크:
   - Send Messages
   - Read Message History
   - Use Slash Commands
4. 생성된 URL 복사해서 브라우저에 붙여넣기
5. 봇을 추가할 서버 선택하고 승인

### OpenClaw에 디스코드 연동

config.yaml 파일 열기:

```powershell
notepad $env:USERPROFILE\.openclaw\config.yaml
```

아래 내용 추가:

```yaml
integrations:
  discord:
    enabled: true
    bot_token: "봇_토큰_여기에_입력"
    prefix: "!"  # 명령어 앞에 붙일 기호
```

저장하고 닫기.

### 봇 실행

```powershell
openclaw --discord
```

디스코드 서버에서 `!help` 입력해서 봇이 응답하면 성공임.

---

## 🔥 Troubleshooting: 자주 겪는 문제들

### ❌ "'winget'은(는) 내부 또는 외부 명령..."

**원인**: winget이 설치 안 됨

**해결**:
1. Microsoft Store에서 "앱 설치 관리자" 설치/업데이트
2. PowerShell 완전히 껐다가 다시 열기
3. 그래도 안 되면 PC 재부팅

### ❌ "'openclaw'은(는) 내부 또는 외부 명령..."

**원인**: 설치 후 PATH가 적용 안 됨

**해결**:
1. PowerShell 완전히 껐다가 다시 열기 (관리자 권한으로)
2. 그래도 안 되면 PC 재부팅
3. 재부팅 후에도 안 되면:
```powershell
winget uninstall openclaw
winget install openclaw
```

### ❌ "액세스가 거부되었습니다" 또는 권한 에러

**원인**: 관리자 권한 없이 실행함

**해결**:
1. PowerShell을 **관리자 권한으로** 다시 실행
2. 그래도 안 되면 Windows Defender나 백신 프로그램이 차단하는지 확인

### ❌ "API key invalid" 또는 인증 에러

**원인**: API 키가 잘못됐거나 만료됨

**해결**:
1. API 키 다시 확인 (복사할 때 앞뒤 공백 없는지)
2. API 서비스 대시보드에서 키가 활성 상태인지 확인
3. 결제 정보가 등록되어 있는지 확인 (무료 크레딧 소진됐을 수 있음)

### ❌ config.yaml 파일을 못 찾음

**원인**: 파일 경로 문제

**해결**:
```powershell
# 설정 폴더 확인
dir $env:USERPROFILE\.openclaw

# 폴더가 없으면 생성
mkdir $env:USERPROFILE\.openclaw -Force
```

### ❌ 디스코드 봇이 응답 안 함

**원인**: 여러 가지 가능성

**체크리스트**:
1. 봇이 서버에 제대로 초대됐는지 확인
2. "Message Content Intent" 활성화했는지 확인
3. 봇 토큰이 정확한지 확인
4. OpenClaw가 `--discord` 옵션으로 실행 중인지 확인
5. 방화벽이 차단하는지 확인

### ❌ Windows Defender가 차단함

**원인**: 새로운 프로그램이라 의심스러워함

**해결**:
1. Windows 보안 → 바이러스 및 위협 방지
2. 보호 기록에서 OpenClaw 관련 항목 찾기
3. "허용" 선택

---

## 🎉 설치 완료! 이제 뭐 함?

1. **기본 사용법 익히기**: 간단한 요청부터 시작해봄 ("현재 날짜 알려줘", "이 폴더에 test.txt 파일 만들어줘")
2. **프로젝트에 적용**: 실제 작업 폴더에서 OpenClaw 실행해보기
3. **자동화 설정**: 자주 하는 작업을 자동화하는 방법 찾아보기

---

## 💡 보안천재의 한마디

> "Windows에서 '관리자 권한으로 실행'은 양날의 검임. 설치할 때는 필요하지만, 평소에 OpenClaw 쓸 때는 일반 권한으로 실행하는 게 안전함. 관리자 권한으로 실행하면 AI가 시스템 파일까지 건드릴 수 있거든. 그리고 API 키는 절대 카톡이나 메일로 공유하지 말 것. 유출되면 요금 폭탄 맞음. 환경변수로 설정하는 게 config 파일에 직접 쓰는 것보다 훨씬 안전함!"
