# AIR Review Protocol v1.0

## 목적

구조적/정책 문서의 품질을 Claude2Critic 교차 검토로 높인다. 과잉 핑퐁 방지. 최대 3라운드.

## 구조

Round 1: Claude 초안  Critic 검토
Round 2: Claude 채택/기각 판단  수정본  Critic 재검토
Round 3: Claude 최종 확정. 추가 핑퐁 없음.

## Claude 판단 원칙

- 채택: 논리적으로 타당하고 시스템 정합성 높은 의견
- 기각: 중복/과잉/이미 커버된 의견. 반드시 이유 명시
- Critic이 스스로 "문제 없음"이라 한 것은 논의 불필요
- 3라운드 초과 의견은 자동 기각

## Critic 역할

- 구조 충돌 / 권한 침범 / 병목 가능성 / 누락만 지적
- 점수/칭찬 불필요
- 3라운드 초과 시 "확정 권고"로 마무리

## 적용 대상

- SOUL.md 작성/수정
- 정책 문서 작성/수정
- 구조적 결정: 에이전트 역할 재정의, ROUTING 변경, 디렉토리 구조 변경
- 단순 작업 지시에는 적용 안 함

## 운영 모드

- 현재: 수동 (사용자가 Claude2Critic 중계)
- 목표: 자동 (OpenClaw에서 Claude2GPT 자동 연결)
- 자동화 시 본 프로토콜 그대로 적용

## 시스템 위계 정합성

- 본 Protocol의 Critic은 조직 역할 Reviewer와 동일하지 않다.
- Critic은 문서 검토 단계의 외부 검토자다.
- Constitution 위반 또는 비가역/고위험 판단 발생 시 즉시 Chief 호출.
- 3라운드 종료 후 Claude 최종 확정은 Chief 호출 조건에 해당하지 않는 경우에 한한다.
