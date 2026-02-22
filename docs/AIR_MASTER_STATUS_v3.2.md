# AIR 전략 연구소 — 마스터 상태 문서 v3.2

# 작성: 2026-02-21 | 새 채팅에서 이어갈 때 이 문서를 가장 먼저 읽을 것

# GitHub: https://github.com/littlebero/AIR

---

## 0. AIR 한눈에 보기

AIR는 이세로(Sero)의 전략 연구소다. MacBook Air M4에서 OpenClaw + Telegram으로 24시간 운영되는 AI 에이전트 시스템.

[사용자] ↓ Telegram

[Front] — 접수, 분류, 라우팅, 보고, 미션 관리

↓

[Researcher] [Ace] 조사/팩트체크 문서/코드/설계

↓ ↓

[Reviewer] 검증/반려

↓

[Chief] 최종 승인


핵심 철학

- GitHub가 유일한 정본 (SSOT).
- 채팅은 작업 메모리일 뿐.
- Writer와 Reviewer는 가능하면 서로 다른 벤더로 구성.
- 비용 최적화: 값싼 모델이 많은 일, 비싼 모델은 5%만.
- 비가역 작업은 반드시 승인 후 실행.
- WRITER/CRITIC/EDITOR는 내부 사고 단계.
- Front/Researcher/Ace/Reviewer/Chief는 조직 역할.

---

## 1. 하드웨어 사양

- 기기: MacBook Air 15 (2025) / 칩: Apple M4
- RAM: 24GB / 저장공간: 1TB SSD
- OS: macOS Tahoe 26.2 / 쿨링: 팬리스
- 소유자: 이세로 (Sero) / 텔레그램 봇: @sero_AIR_BOT

---

## 2. 룰북 개요

CONSTITUTION v2.0

- 모든 실질적 응답은 WRITER → CRITIC → EDITOR 3단계
- 리서치 요약은 6개 헤딩 구조 필수
- 시크릿/토큰 출력 절대 금지
- 비가역 작업 무단 실행 금지

ROUTING v3.0

- Simple(80%): Front → Ace
- Standard(15%): Front → Ace → Reviewer → 수정 (최대 2회)
- Critical(5%): 전체 파이프라인 + Chief 최종 승인
- L2 승격 조건: 투자자 제출, 법/계약, 1000만원↑, 외부 공개

ALLOWLIST

- 승인 없이 가능: 리서치, 요약, 문서 초안, 명령어 제안
- 승인 필수: 토큰/키 처리, 시스템 설치, 네트워크 변경, LaunchAgent 수정

DECISIONS_LOG

- 모든 구조 변경은 여기에 append. 수정 금지, 추가만 허용.

STAGE_TRACKER

- Stage 1,2: 완료 / Stage 3: 진행 중 / Stage 4: 미시작

---

## 3. GitHub SSOT 레포

- URL: https://github.com/littlebero/AIR (Public)
- 로컬 경로: ~/AIR-new / 브랜치: main

세션 시작 시 에어한테 보낼 것: 세션 시작 시 자동 읽기 규칙에 따라 3개 URL 읽어줘.

읽을 3개 URL (고정):

1. https://raw.githubusercontent.com/littlebero/AIR/main/policy/CONSTITUTION.md
2. https://raw.githubusercontent.com/littlebero/AIR/main/policy/ROUTING.md
3. https://raw.githubusercontent.com/littlebero/AIR/main/STAGE_TRACKER.md

---

## 4. OpenClaw 현재 설정

- Default: openai/gpt-4.1-mini
- Fallback: openai/gpt-4.1-mini
- sonnet = anthropic/claude-sonnet-4-6
- opus = anthropic/claude-opus-4-6
- OpenAI/Anthropic API 키 등록됨 ✅
- session-memory hook, commands.restart 활성화 ✅

---

## 5. Agency Architecture v1.0

에이전트 현황:

| 에이전트 | 모델 | 역할 | 생성 | SOUL.md |
|---------|------|------|------|---------|
| Front | gpt-4.1-nano | 접수/라우팅/보고/미션관리 | ✅ | ⏳ v3 미저장 |
| Researcher | claude-sonnet-4-6 | 조사/팩트체크/분석 | ✅ | ⏳ 미저장 |
| Ace | claude-sonnet-4-6 | 문서/코드/설계 | ✅ | ⏳ 미저장 |
| Reviewer | gpt-4.1-mini | 검증/반려 | ✅ | ⏳ 미저장 |
| Chief | claude-opus-4-6 | 최종승인/고위험 | ✅ | ⏳ 미저장 |

긴급도 시스템: 없음 → 일반 처리 ! → 강조. 주의해서 처리 !! → 우선 처리. 진행 중인 것보다 먼저 !!! → 긴급.

현재 작업 즉시 중단 !!!! → 비상. 모든 것 정지. Chief 즉시 개입

무한 루프 방지: Reviewer 반려 → Ace 수정: 최대 2회
2회 초과: Chief 자동 개입

Chief 반려 → 수정: 최대 1회
1회 초과: 작업 보류 + 사용자 판단 요청

---

## 6. Stage 현황

| Stage | 내용 | 상태 |
|-------|------|------|
| 1 | Infrastructure Stabilized | ✅ COMPLETE |
| 2 | Operational Documentation | ✅ COMPLETE |
| 3 | Strategic Document Engine | 🔄 IN PROGRESS |
| 4 | Autonomous Research Node | ⏳ NOT STARTED |

---

## 7. 다음 할 것 (우선순위순)

1. Front SOUL.md v3 에어에 저장
2. Researcher SOUL.md 정교화 및 저장
3. Ace SOUL.md 정교화 및 저장
4. Reviewer SOUL.md 정교화 및 저장
5. Chief SOUL.md 정교화 및 저장
6. 에이전트 간 라우팅 연결 테스트
7. 자동화 문서 템플릿 (Stage 3 완료 조건)
8. RAG 기반 문서 허브 (별도 세션)

---

## 8. 새 채팅에서 이어갈 때

1. 이 문서(v3.2)를 먼저 읽고 현재 상태 파악
2. GitHub AIR 레포가 SSOT
3. 에이전트 5개 생성됐지만 SOUL.md 전부 미저장
4. Front SOUL.md v3 별도 파일(FRONT_SOUL_v3.md) 참조
5. 다음 할 것 1번부터 시작

---

## 9. 절대 원칙 (변경 금지)

- Git이 SSOT. 채팅은 작업 메모리일 뿐.
- 시크릿/토큰 절대 커밋 금지.
- 구조적 결정은 반드시 DECISIONS_LOG에 append.
- 비가역 작업은 반드시 승인 후 실행.
- Writer와 Reviewer는 가능하면 다른 벤더로 구성.
- Chief(opus)는 Critical 작업에만 호출.
- Reviewer 반려 2회 초과 시 반드시 Chief 개입.
