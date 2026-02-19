# Air Rulebook v1.0

This document contains **operational procedures**. High-level obligations belong in `AIR_UNIFIED_DEV_CONSTITUTION_v1.md`.

---

## Multi-Model Routing v1.0

### A) 라우팅 표
- Nano: 분류/요약/형식변환/체크리스트/로그 요약
- Mini: 짧은 Q&A/간단 문서/가벼운 기획/간단 코드 설명
- 5.2: 설계/정책/리스크 판단/복잡 디버깅/최종 판정

### B) 승급 트리거 체크리스트(Yes면 승급)
- 보안/키/권한? / 되돌릴 수 없는 실행?
- 헌법·룰북·프로필 변경?
- 요구사항이 모호해 실패 비용이 큰가?
- 다단계 추론/설계가 필요한가?
- 오류 원인이 여러 개일 가능성이 큰가?

### C) 요약 스냅샷 템플릿(승급 시 사용, 10줄)
- Goal:
- Current state:
- Constraints:
- What’s decided:
- What’s unknown:
- Next 3 actions:

### D) 실행 구성(가능한 범위)
- OpenClaw에서 “모델 alias/프로필”이 별도 기능으로 제공되지 않는 경우, 운영 절차로 대체한다:
  - (1) 기본은 저비용 계층(Nano/Mini)로 세션 모델을 유지
  - (2) P0 트리거 발생 시, **승급 스냅샷 10줄** 작성 후 상급(5.2)으로 전환
  - (3) 전환/복귀는 `session_status` 모델 오버라이드 또는 OpenClaw UI/명령으로 수행
- 모델/가격 수치는 문서에 고정하지 않고, 계층(Nano/Mini/5.2)으로만 기록한다.

---

## Agent Profile Management v1.0

### A) 저장 위치(표준)
- 프로필 파일: `~/clawd/profiles/AIR_PROFILE.md`
- 스냅샷 폴더: `~/clawd/profiles/snapshots/`
- 체크 로그: `~/clawd/profiles/logs/`

### B) 프로필 갱신 트리거(언제 업데이트?)
- `openclaw.json` 변경
- 기본 모델 변경(예: gpt-5.2 ↔ 다른 모델)
- gateway/telegram 연결 변경
- gemini CLI / node / pnpm / git / gh 등 도구 설치/업데이트
- 권한/키(.env, auth profile) 변경
- 프로젝트/레포 연결 상태 변경(GitHub auth 등)

### C) 갱신 방식(표준 루틴)
- “상태 수집 → 프로필 반영 → 스냅샷 저장 → 변경 요약 출력”
- 프로필은 항상 ‘사실(명령 결과)’ 기반으로만 채운다.
- 불명확하면 `UNKNOWN`으로 표기하고 가정 금지.

### D) 프로필 버전/스냅샷 규칙
- 갱신 시 `snapshots/AIR_PROFILE_YYYYMMDD-HHMM.md` 로 복제 저장
- 변경 요약(What changed)을 프로필 상단에 5줄 이내로 기록

### E) 상태 수집(권장 명령)
(민감정보 출력 금지: 키/토큰/이메일/시리얼 등)

- 시스템/사양:
  - `sw_vers`
  - `uname -m`
  - `system_profiler SPHardwareDataType` (프로필에는 요약 3줄만)
- OpenClaw 상태:
  - `openclaw status`
  - `openclaw gateway status`
- Gemini CLI:
  - `which gemini`
  - `gemini --version`
  - auth mode는 사실 기반으로만 기록(확인 불가하면 UNKNOWN)
- Node/pnpm/git:
  - `node -v`
  - `pnpm -v` (없으면 NOT INSTALLED)
  - `git --version`
- GitHub:
  - `gh --version`
  - `gh auth status` (세부정보 노출 금지, 연결 여부만)

---

## Daily Profile Management v1.0

- Daily Profile 생성 주기: 하루 1회(또는 큰 변경 직후 추가 1회 허용)
- 파일명 규칙: `AIR_YYYY-MM-DD.md`
- 저장 경로: `~/clawd/profiles/daily/`
- 생성 실패 시: `~/clawd/profiles/logs/daily-profile-YYYY-MM-DD.log`에 오류 원인 기록
- 중복 생성 방지:
  - 같은 날짜 파일이 이미 있으면 “append 금지”
  - 대신:
    - (a) 변경이 없으면 그대로 유지
    - (b) 변경이 있으면 새 파일이 아니라 `Today’s Changes` 섹션만 갱신 (단, 5줄 제한)

---

## GitHub PR Workflow v1.0

### Roles
- AIR: Dev Worker — 작업 브랜치 생성, 커밋, PR 생성
- LEVO: Release Engineer — PR 리뷰/머지, 배포

### Branch rule
- AIR는 항상 `air/<task-slug>` 브랜치에서 작업한다.
- **main 직접 commit/push 금지** (로컬에서도 main은 안정 상태 유지)

### PR template
- 경로: `.github/PULL_REQUEST_TEMPLATE.md`
- PR 작성 시 템플릿이 자동 적용되는지 확인(첫 테스트에서 체크)

### Main protection (GitHub settings)
- PR 없이 main 반영 불가
- force push 차단
- branch 삭제 금지
- Status checks: 현재 CI 미구축으로 강제하지 않음(후행)

### One-time smoke test (requires gh auth)
1) `gh auth login`
2) `air/test-pr-workflow` 브랜치 생성
3) README에 테스트 1줄 추가 → commit → push
4) PR 생성, 템플릿 자동 적용 여부 확인

---

## Parallel Orchestration v1.0

### A) 슬롯 정의(최대 3개)
- SLOT-A (Scout / Nano-Mini): 조사/정리/요약/체크리스트/스냅샷 생성
- SLOT-B (Builder / CLI): 구현/실행/빌드/테스트(주로 Gemini CLI), REPORT 생성
- SLOT-C (Judge / 5.2): 검수/판정/리스크 평가/다음 오더 확정

### B) 폴더/아티팩트 규칙(고정)
- 모든 병렬 산출물은 아래에만 저장:
  - `~/clawd/_artifacts/slot-a/`
  - `~/clawd/_artifacts/slot-b/`
  - `~/clawd/_artifacts/slot-c/`
- 템플릿:
  - `~/clawd/_artifacts/templates/slot-a_template.md`
  - `~/clawd/_artifacts/templates/slot-b_template.md`
  - `~/clawd/_artifacts/templates/slot-c_template.md`
- 파일명 규칙(충돌 방지):
  - `YYYYMMDD-HHMM__<task-slug>__slot-<a|b|c>.md` (또는 `.log/.patch`)
- 동일 task-slug로는 같은 슬롯에서 파일 1개만 “정본”으로 유지(필요 시 suffix `-v2`)

### C) 쓰기 권한 단일화(충돌 방지 핵심)
- SLOT-A: 읽기/요약/계획만 가능. 코드/설정 파일 직접 변경 금지.
- SLOT-B: 프로젝트 코드(레포)만 변경 가능. 헌법/룰북/프로필 변경 금지.
- SLOT-C: 판정/리뷰 문서만 작성. 코드/설정 변경 금지.
- 헌법/룰북/프로필/Daily Profile은 오직 AIR(단일 프로세스)만 변경한다.

### D) 병렬 vs 단일 선택 규칙
- Parallel YES:
  1) 서로 독립 작업(리서치 vs 구현 vs 리뷰) 분리 가능
  2) 결과가 파일로 명확히 회수됨(REPORT/요약/체크리스트)
  3) 실패해도 롤백 비용 낮음(읽기/요약/초안 단계)
- Parallel NO:
  1) 요구사항이 매우 모호해 대화형 уточ정이 핵심
  2) 동일 파일/동일 설계를 동시에 만져야 함
  3) 작은 작업인데 병렬 오버헤드가 더 큼
  4) 성능/품질이 중요한 핵심 설계/최종 판정 구간
- 기본값:
  - 병렬은 A+B까지만 먼저 허용
  - C(판정)는 결과가 모였을 때만 단일로 실행

### E) 병렬 운영 루프(표준)
1) Task 생성(슬롯 배정) → task-slug 부여
2) SLOT-A: 요약/체크리스트/리스크 목록 생성 → artifact 저장
3) SLOT-B: 구현/실행/빌드/테스트 → REPORT 저장
4) SLOT-C: A+B artifact 기반 판정/다음 지시 확정 → artifact 저장
5) AIR가 최종 “결론 10줄”을 LIVE 프로필/일일 스냅샷에 반영(선택)
