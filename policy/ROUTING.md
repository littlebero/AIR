# AIR Routing v3.1

## 설계 원칙

Claude↔GPT 교차 검증으로 편향 방지. 중요도별 파이프라인 분리 → 비용 최적화. Chief(L2)는 Critical에만 제한 호출.

## 에이전트/모델 매핑

Front: gpt-4.1-nano — 접수/분류/라우팅
Researcher: claude-sonnet-4-6 — 조사/팩트체크
Ace: claude-sonnet-4-6 — 문서/코드/설계
Reviewer: gpt-4.1-mini — 검증/반려
Chief: claude-opus-4-6 — 최종승인/고위험

## 파이프라인

Simple (80%) 대상: 요약, 번역, 간단한 설명, 내부 메모
경로: Front → Ace

Standard (15%) 대상: 전략 문서, 보고서 초안, 내부 분석
경로: Front → Ace → Reviewer → Ace 수정
루프: 최대 2회 (초과 시 Critical 자동 승격)

Critical (5%) 대상: 투자자/정부 제출, 법/계약, 외부 공개 최종본
경로: Front → (Researcher) → Ace → Reviewer → Ace 수정 → Chief 최종 승인

Researcher는 조사 필요 시에만. Front가 판단.

## L2 승격 조건 (하나라도 해당 시 Critical)

- 투자자/정부 제출 최종본
- 법/규제/계약/책임 관련
- 1000만원 이상 의사결정
- 외부 공개 최종본
- Reviewer 루프 2회 초과

## 승인 정책

비가역 작업 및 승인 정책은 ALLOWLIST.md를 따른다.

## 변경 정책

변경 시 DECISIONS_LOG append + 본 문서 업데이트 필수
