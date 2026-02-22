# AIR Routing v3.1
# 작성: 2026-02-21 | 상태: ACTIVE | 이전 버전: v3.0

## 목적

AIR 작업을 중요도에 따라 적절한 에이전트 파이프라인으로 분기한다. 비용 최적화와 품질 보장을 동시에 달성한다.

## 설계 원칙

Claude(Anthropic)↔GPT(OpenAI) 교차 검증으로 편향을 방지한다. 중요도별로 파이프라인을 분리해 비용을 최적화한다. Chief(opus)는 Critical 작업에만 호출한다. 5%만.

## 에이전트/모델 매핑

| 에이전트 | 모델 | 역할 |
|---------|------|------|
| Front | gpt-4.1-nano | 접수/분류/라우팅 |
| Researcher | claude-sonnet-4-6 | 조사/팩트체크 |
| Ace | claude-sonnet-4-6 | 문서/코드/설계 |
| Reviewer | gpt-4.1-mini | 검증/반려 |
| Chief | claude-opus-4-6 | 최종승인/고위험 |

## 파이프라인

### Simple (80%) — 저비용

대상: 요약, 번역, 간단한 설명, 내부 메모

경로: Front → Ace

루프 없음. 빠르고 저렴하게 처리.

### Standard (15%) — 중비용

대상: 전략 문서, 보고서 초안, 내부 분석

경로: Front → Ace → Reviewer → Ace 수정

루프 최대 2회. 초과 시 Critical 자동 승격.

### Critical (5%) — 고비용

대상: 투자자/정부 제출, 법/계약, 외부 공개 최종본

경로: Front → Researcher → Ace → Reviewer → Ace 수정 → Chief 최종 승인

가장 높은 신뢰도 보장. Chief 승인 없이 실행 불가.

## L2 승격 조건

다음 중 하나라도 해당하면 Critical 파이프라인 적용:

- 투자자/정부 제출 최종본
- 법/규제/계약/책임 관련
- 1000만원 이상 의사결정
- 외부 공개 최종본
- Reviewer 루프 2회 초과

## 승인 정책

비가역 작업 및 승인 정책은 ALLOWLIST.md를 따른다.

## 변경 정책

ROUTING 변경 시:

1. DECISIONS_LOG에 append
2. 본 문서 업데이트

수정 금지. 추가만 허용.