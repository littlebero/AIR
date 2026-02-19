# AIR Routing v3.0

Date: 2026-02-18
Status: ACTIVE
이전 버전: v1.0 (단일 모델 opus) → v3.0으로 대체

## 설계 원칙

- Claude(Anthropic) + GPT(OpenAI) 교차 검증으로 편향 방지
- 작업 중요도에 따라 파이프라인 분리 → 비용 최적화
- L2(고비용)는 Critical 작업에만 제한적 호출

## 모델 계층

| 레이어 | 모델 | 역할 |
|--------|------|------|
| L0 | gpt-4.1-nano | 전처리/분류 |
| L1 Writer | claude-sonnet-4-6 | 초안 생성 (AIR primary) |
| L1 Critic | gpt-4.1-mini | Claude 초안 교차 검증 |
| L2 Validator | claude-opus-4-6 + gpt-4.1 | 고위험 최종 검증 |

## 작업 유형별 파이프라인

### Simple (약 80%) — 저비용
대상: 요약, 번역, 간단한 정리, 내부 메모
파이프라인: L0 → L1 Writer
루프: 없음

### Standard (약 15%) — 중간 비용
대상: 전략 문서, 보고서 초안, 내부 분석
파이프라인: L0 → L1 Writer → L1 Critic → L1 Writer 수정
루프: 최대 2회 (초과 시 자동 Critical 승격)

### Critical (약 5%) — 고비용, 최고 신뢰도
대상: 투자자/공공기관 제출, 법/계약/정관, 외부 공개 최종본
파이프라인: L0 → L1 Writer → L1 Critic → L1 Writer 수정 → L2 교차 검증
L2: claude-opus-4-6 + gpt-4.1 양쪽 일치 시 최종 승인

## L0 출력 규격

- [TYPE]: 작업 유형
- [RISK_LEVEL]: low / mid / high
- [ROUTE]: Simple / Standard / Critical

## L1 Writer 출력 규격

- [FACT] / [INFER] / [PLAN] / [OPIN]

## L1 Critic 출력 규격

- [A] 문제문장 / [B] 이유 / [C] 수정안 / [D] 확인필요

## L2 출력 규격

- 제출용 보수적 문장
- "확인 필요" 명시
- 리스크/팩트체크 리스트

## L2 승격 조건 (엄격 적용)

아래 중 하나 이상 해당 시 Critical 적용:
- 투자자/공공기관 제출 최종본
- 법/규제/계약/정관/책임 관련 문장
- 금액 1,000만원 이상 의사결정
- 외부 공개/배포 문서 최종본
- Critic 루프 2회 초과

## Safety Gate

승인 필요 (비가역 작업):
- credential/token 처리
- 시스템 설치/제거
- 권한, 계정, SSH, 방화벽, 네트워크
- LaunchAgent / OpenClaw 런타임 수정

승인 없이 허용:
- 리서치, 요약, 문서 초안
- 명령어/스크립트 제안 (NOT EXECUTED 라벨 필수)

## 변경 정책

변경 시 DECISIONS_LOG append + 본 문서 업데이트 필수
