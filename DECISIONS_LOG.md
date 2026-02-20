# AIR — DECISIONS LOG

구조적 결정 기록. 기존 내용 수정 금지, append만 허용.

---

## [LEGACY] 2026-02-13 ~ 2026-02-18 — 레거시 결정 보존

상세 내용: archive/DECISIONS_LOG_LEGACY.md 참조

요약: Git SSOT 원칙, 단일 모델(opus) 라우팅, Stage 3 시작, 헌법 v1.4 배포, 6-heading 강제 등 12개 결정 포함.

---

## 2026-02-18 — 새 레포 구조 v2.0 채택

기존 AIR 레포 archive 처리. 새 AIR 레포로 완전 재시작.

구조: policy/ (1순위) / projects/ / archive/

읽기 순서: CONSTITUTION → ROUTING → STAGE_TRACKER → projects → DECISIONS_LOG

---

## 2026-02-18 — Primary 모델 변경: opus → sonnet

Primary: claude-sonnet-4-6
Alias: opus = claude-opus-4-6 (L2 전용)
근거: 비용 최적화, rate limit 완화

---

## 2026-02-18 — 라우팅 v3.0 채택: Claude+GPT 교차 검증

Writer: claude-sonnet-4-6 / Critic: gpt-4.1-mini / L2: 교차 검증
파이프라인: Simple / Standard / Critical 분리
근거: 편향 방지, 비용 최적화, 신뢰도 향상

---

## 2026-02-19 — Default 모델 변경: sonnet → haiku

Primary: anthropic/claude-haiku-4-5-20251001
근거: 일상 작업 비용 절감. sonnet/opus는 alias로 유지.
