---
title: "OpenClaw 게이트웨이 아키텍처: 중앙 집중형 구조의 이해"
description: "OpenClaw가 왜 여러 개의 채팅 앱을 동시에 지원할 수 있을까요? 핵심인 게이트웨이 시스템의 구조와 관리 명령어를 쉽게 풀어 드립니다."
pubDate: 2026-02-02T18:00:00+09:00
category: "AI"
tags: ["OpenClaw", "Gateway", "아키텍처", "관리", "시스템"]
---

OpenClaw의 모든 통신은 **'게이트웨이(Gateway)'**를 중심으로 돌아갑니다.

## 🗼 게이트웨이의 역할 (관제탑)

게이트웨이는 우리 컴퓨터에서 묵묵히 돌아가는 서버입니다.
1. **채널 연동:** WhatsApp, Discord 등으로부터 메시지를 받습니다.
2. **에이전트 연결:** 받은 메시지를 적절한 AI 에이전트에게 보냅니다.
3. **상태 관리:** 세션 정보와 보안 설정을 총괄합니다.

## 🛠️ 게이트웨이 관리 필수 명령어

게이트웨이가 멈추면 모든 연동이 끊깁니다. 다음 명령어로 상태를 관리하세요.

```bash
# 게이트웨이 건강 상태 체크
openclaw gateway health

# 서비스(launchd/systemd) 상태 및 프로세스 정보 확인
openclaw status

# 전체 시스템 정밀 진단 (가장 많이 쓰는 명령어)
openclaw doctor

# 대시보드(웹 화면) 열기
openclaw dashboard
```

## 🏗️ 전체 아키텍처 흐름

```
[사용자] ────→ [채널: Discord] ────→ [🗼 Gateway]
                                        │
                                        ▼
                                  [🤖 AI Agent] ────→ [📁 로컬 파일/명령어]
```

## 📡 노드(Node) 시스템

내 메인 PC가 아닌, 스마트폰(iOS/Android)이나 다른 서버를 연결하고 싶다면 **'노드'**로 등록하면 됩니다.

```bash
# 게이트웨이에 새로운 노드 페어링 시작
openclaw pairing start

# 연결된 모든 장치(노드) 목록 보기
openclaw nodes list
```

---
게이트웨이만 이해하면 여러분의 집 전체를 AI가 제어하는 스마트 홈으로도 바꿀 수 있습니다! ㅋㅋ 🛡️
