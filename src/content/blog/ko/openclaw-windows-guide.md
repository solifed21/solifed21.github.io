---
title: "비개발자 가이드: Windows에서 OpenClaw 시작하기"
description: "Windows 사용자를 위한 OpenClaw 설치 가이드"
pubDate: 2026-02-01
category: "AI"
---

## 준비물

- Windows 10 또는 11 컴퓨터

## 설치 방법

### 1단계: PowerShell 열기

시작 메뉴에서 "PowerShell" 검색함. "관리자 권한으로 실행" 클릭함.

### 2단계: winget 확인

Windows 11이면 이미 설치되어 있음. Windows 10이면 Microsoft Store에서 "앱 설치 관리자" 업데이트함.

### 3단계: OpenClaw 설치

PowerShell에 입력:

```powershell
winget install openclaw
```

설치 끝임.

## 실행 방법

PowerShell이나 명령 프롬프트에서:

```powershell
openclaw
```

입력하면 됨.

## 안 되면?

- PowerShell 껐다 켜보셈
- 컴퓨터 재부팅 해보셈
- winget이 제대로 설치됐는지 확인

어렵지 않음. 그냥 따라하면 됨.
