# AIR SSOT v2.0
# Owner: 이세로 (Sero) | 2026-02-21 | LOCKED

## 문서 위상

본 문서는 AIR 전략 연구소의 메타 거버넌스 문서다. 정책/역할/라우팅의 참조 구조를 정의하며 행동 규범 자체는 각 정책 문서를 따른다. 구조 변경 시 Chief 승인 후 DECISIONS_LOG append + 본 문서 업데이트.

## AIR 목적

- 글로벌/국내 자료 수집 및 구조화
- 전략/사업/정책 문서 작성
- 근거 기반 의사결정 자료 제공
- RAG 기반 내부 검색 및 참조 허브 (Stage 4)

## 정책 우선순위 (충돌 시 적용)

P1: 절대 금지 규칙  CONSTITUTION.md / ALLOWLIST.md
P2: 고위험 작업 검증  ROUTING.md L2 승격 조건
P3: GitHub SSOT 운영 원칙  본 문서
P4: LLM 라우팅 규칙  ROUTING.md
P4-1: 정책 문서 검토 절차  REVIEW_PROTOCOL.md
P5: 기타 운영 정책  상황에 따라 유연 적용

## OpenClaw 버전 정책

- Stable Channel 최신 공식 릴리스만 사용
- 테스트/베타/RC 버전 금지
- 설치 완료 후 버전 로그 기록: ~/.openclaw/ssot_version_log.txt

## 장기 유지 전략

- 문서 자산 지속 갱신 및 RAG 반영 (Stage 4)
- 신규 프로젝트 추가 시 ROUTING 규칙만 업데이트
- 실행/테스트 노드는 AIR와 분리 유지
