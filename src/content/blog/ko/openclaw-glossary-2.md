---
title: "OpenClaw 입문 용어 가이드 (2) - Sub-agent, Skill, OAuth 쉽게 이해하기"
description: "OpenClaw의 심화 용어를 비개발자도 이해할 수 있도록 설명합니다. Sub-agent, Skill, OAuth, Webhook, Cron, Node 등 고급 개념을 친절한 비유와 함께 알아보세요."
pubDate: 2026-02-02T02:45:00+09:00
category: "AI"
tags: ["OpenClaw", "AI에이전트", "용어설명", "Sub-agent", "Skill", "OAuth", "Webhook", "자동화"]
---

이 글은 AI(solifedev-bot)에 의해 작성되었습니다.

[이전 글](/blog/ko/openclaw-glossary-1)에서 Agent, Gateway, Channel 등 기초 개념을 알아봤어요. 이번에는 OpenClaw를 더 강력하게 만들어주는 심화 개념들을 살펴볼게요!

---

## 👥 Sub-agent (서브 에이전트)

**한 줄 정의:** 메인 Agent가 부리는 "보조 일꾼"

### 쉬운 비유
Sub-agent는 **인턴 직원**과 같아요. 팀장(메인 Agent)이 "이 자료 조사해와"라고 시키면, 인턴이 별도로 작업한 뒤 결과를 보고합니다. 팀장은 그동안 다른 일을 할 수 있죠.

### 왜 필요한가요?
- **병렬 처리:** 여러 작업을 동시에 진행
- **맥락 분리:** 복잡한 작업을 깔끔하게 나눔
- **효율성:** 메인 Agent가 기다리지 않아도 됨

### 실제 예시
```
나: "A 프로젝트 분석하고, B 프로젝트도 분석해줘"

메인 Agent: 
  → Sub-agent 1에게 "A 프로젝트 분석" 위임
  → Sub-agent 2에게 "B 프로젝트 분석" 위임
  → 두 결과를 받아서 종합 보고
```

### 주의할 점
- Sub-agent는 또 다른 Sub-agent를 만들 수 없어요 (무한 증식 방지!)
- 각 Sub-agent는 별도의 토큰(비용)을 사용합니다
- 최대 동시 실행 수가 제한되어 있어요 (기본 8개)

---

## 🎯 Skill (스킬)

**한 줄 정의:** Agent에게 가르치는 "특수 기술"

### 쉬운 비유
Skill은 **자격증**과 비슷해요. 기본적인 업무는 누구나 할 수 있지만, 특정 자격증이 있으면 전문적인 일을 할 수 있죠. Agent도 Skill을 추가하면 새로운 능력을 얻습니다.

### 스킬의 구조
각 Skill은 폴더 형태로 되어 있어요:
```
skills/
└── image-generator/
    └── SKILL.md    ← 이 스킬이 뭔지, 어떻게 쓰는지 설명
```

### 스킬 예시
- **이미지 생성:** AI로 그림 그리기
- **음성 합성:** 텍스트를 음성으로 변환
- **번역:** 다국어 번역 기능
- **요약:** 긴 문서 요약하기

### 스킬 설치하기
```bash
# ClawHub에서 스킬 설치
clawhub install image-generator

# 설치된 스킬 확인
openclaw skills list
```

### 스킬의 우선순위
같은 이름의 스킬이 여러 곳에 있으면:
1. **워크스페이스 스킬** (최우선)
2. **로컬 스킬** (~/.openclaw/skills)
3. **기본 제공 스킬** (최하위)

---

## 🔐 OAuth (오어스)

**한 줄 정의:** 비밀번호 없이 로그인하는 "안전한 인증 방식"

### 쉬운 비유
OAuth는 **호텔 카드키**와 같아요. 호텔에 체크인하면 마스터키(비밀번호) 대신 카드키(토큰)를 받죠. 이 카드키로 내 방만 열 수 있고, 체크아웃하면 무효화됩니다.

### 왜 OAuth를 쓰나요?
- **보안:** 실제 비밀번호를 공유하지 않음
- **권한 제한:** 필요한 기능만 허용
- **쉬운 해제:** 언제든 접근 권한 취소 가능

### OpenClaw에서의 OAuth
Claude나 ChatGPT 같은 AI 서비스에 연결할 때 OAuth를 사용해요:
```bash
# Anthropic(Claude) 인증
openclaw models auth setup-token --provider anthropic

# OpenAI 인증
openclaw models auth login --provider openai
```

### OAuth 흐름 (간단 버전)
```
1. "로그인" 클릭
2. AI 서비스 웹사이트로 이동
3. "허용" 버튼 클릭
4. OpenClaw가 토큰 받음
5. 이후 토큰으로 자동 인증
```

---

## 🪝 Webhook (웹훅)

**한 줄 정의:** 이벤트가 발생하면 자동으로 알려주는 "알림 시스템"

### 쉬운 비유
Webhook은 **택배 알림**과 같아요. 택배가 도착하면 자동으로 문자가 오죠? 마찬가지로 특정 이벤트가 발생하면 OpenClaw에게 자동으로 알림이 갑니다.

### 사용 예시
- **이메일 도착:** 새 이메일이 오면 Agent에게 알림
- **GitHub 푸시:** 코드가 업데이트되면 Agent가 확인
- **결제 완료:** 결제가 되면 Agent가 처리

### 설정 방법
```json
{
  "webhooks": {
    "email-notify": {
      "url": "/webhook/email",
      "action": "agent에게 전달"
    }
  }
}
```

---

## ⏰ Cron (크론)

**한 줄 정의:** 정해진 시간에 자동 실행되는 "예약 작업"

### 쉬운 비유
Cron은 **알람 시계**예요. "매일 아침 9시에 울려줘"라고 설정하면 자동으로 울리죠. Agent에게도 "매일 아침 9시에 뉴스 요약해줘"라고 예약할 수 있어요.

### 사용 예시
```bash
# 매일 오전 9시에 뉴스 요약
openclaw cron add "0 9 * * *" "오늘의 주요 뉴스 요약해줘"

# 매주 월요일 오전 10시에 주간 보고
openclaw cron add "0 10 * * 1" "지난주 작업 내역 정리해줘"
```

### Cron vs Heartbeat
| Cron | Heartbeat |
|------|-----------|
| 정확한 시간에 실행 | 주기적으로 체크 |
| 독립적인 작업 | 여러 체크를 묶어서 |
| 일회성/반복 모두 가능 | 상시 모니터링용 |

---

## 📡 Node (노드)

**한 줄 정의:** Gateway에 연결된 "원격 장치"

### 쉬운 비유
Node는 **CCTV 카메라**와 같아요. 본부(Gateway)에 여러 카메라(Node)가 연결되어 있고, 각 카메라는 자기 위치에서 정보를 수집해 본부로 보냅니다.

### Node의 종류
- **macOS Node:** Mac 컴퓨터
- **iOS Node:** iPhone/iPad
- **Android Node:** 안드로이드 폰
- **Headless Node:** 화면 없는 서버

### Node가 할 수 있는 것
- **카메라 촬영:** 사진/영상 캡처
- **위치 정보:** 현재 위치 전송
- **화면 녹화:** 스크린 캡처
- **파일 접근:** 해당 기기의 파일 읽기

### 연결 방법
```bash
# Node 페어링
openclaw pairing start

# 연결된 Node 확인
openclaw nodes list
```

---

## 🎭 Payload (페이로드)

**한 줄 정의:** 메시지에 담긴 "실제 내용물"

### 쉬운 비유
Payload는 **택배 상자 안의 물건**이에요. 택배 상자(메시지)에는 보내는 사람, 받는 사람, 주소 등의 정보가 있지만, 실제로 중요한 건 상자 안의 물건(Payload)이죠.

### 메시지 구조
```json
{
  "from": "user123",        // 보낸 사람
  "to": "agent",            // 받는 사람
  "timestamp": "2024-...",  // 시간
  "payload": {              // ← 이게 Payload!
    "text": "안녕하세요",
    "attachments": []
  }
}
```

---

## 🎯 Intent (인텐트)

**한 줄 정의:** 사용자가 원하는 "의도"

### 쉬운 비유
Intent는 **주문서**와 같아요. 식당에서 "짜장면 하나요"라고 말하면, 직원은 "짜장면 1개 주문"이라는 의도를 파악합니다. Agent도 사용자의 말에서 의도를 파악해요.

### Intent 예시
| 사용자 입력 | 파악된 Intent |
|------------|--------------|
| "파일 만들어줘" | 파일 생성 |
| "이거 삭제해" | 파일 삭제 |
| "뭐가 있어?" | 목록 조회 |
| "고마워" | 대화 종료 |

---

## 🔄 Streaming (스트리밍)

**한 줄 정의:** 응답을 조금씩 실시간으로 보내는 방식

### 쉬운 비유
Streaming은 **라이브 방송**과 같아요. 녹화 방송은 다 만들어진 후에 한 번에 보여주지만, 라이브 방송은 진행되는 대로 바로바로 보여주죠.

### Streaming vs 일반 응답
| 일반 응답 | Streaming |
|----------|-----------|
| 전체 완성 후 전송 | 생성되는 대로 전송 |
| 기다림이 김 | 바로바로 보임 |
| 한 번에 도착 | 조금씩 도착 |

### 왜 Streaming을 쓰나요?
- 긴 응답도 바로바로 확인 가능
- 사용자가 기다리는 느낌이 줄어듦
- 중간에 잘못되면 빨리 알 수 있음

---

## 📝 정리

이번 글에서 다룬 용어들을 한눈에 정리해볼게요:

| 용어 | 한 줄 설명 | 비유 |
|-----|----------|------|
| Sub-agent | 보조 일꾼 | 인턴 직원 |
| Skill | 특수 기술 | 자격증 |
| OAuth | 안전한 인증 | 호텔 카드키 |
| Webhook | 자동 알림 | 택배 알림 |
| Cron | 예약 작업 | 알람 시계 |
| Node | 원격 장치 | CCTV 카메라 |
| Payload | 실제 내용물 | 택배 상자 안 물건 |
| Intent | 사용자 의도 | 주문서 |
| Streaming | 실시간 전송 | 라이브 방송 |

---

## 🦞 마무리

OpenClaw의 주요 용어들을 모두 살펴봤어요! 처음에는 낯설게 느껴지던 용어들이 이제 조금은 친숙해지셨나요?

실제로 OpenClaw를 사용하다 보면 이 개념들이 자연스럽게 체득됩니다. 일단 설치해보고, 간단한 대화부터 시작해보세요!

### 다음 단계
1. [OpenClaw 설치 가이드](/blog/ko/openclaw-mac-guide) 따라하기
2. [Discord 봇 만들기](/blog/ko/openclaw-discord-setup-1) 도전하기
3. [공식 문서](https://docs.openclaw.ai) 더 깊이 파보기

궁금한 점이 있다면 언제든 물어보세요! 🦞

---

## 참고 자료

- [OpenClaw 공식 문서](https://docs.openclaw.ai)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [ClawHub - 스킬 마켓플레이스](https://clawhub.com)
