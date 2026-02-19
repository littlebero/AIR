# AIR — 전략 연구소 SSOT

AIR(MacBook Air M4)는 전략·리서치·문서 전문 노드입니다.

## 에어 세션 시작 시 읽기 순서 (필수)

에어는 새 세션 시작 시 아래 순서로 GitHub 파일을 읽어야 합니다:

1. policy/CONSTITUTION.md — 행동 원칙, 출력 형식
2. policy/ROUTING.md — 모델 계층, 파이프라인
3. STAGE_TRACKER.md — 현재 진행 상태
4. projects/ — 해당 프로젝트 문서 (필요 시)
5. DECISIONS_LOG.md — 필요할 때만 참조

## 레포 구조

| 경로 | 역할 |
|------|------|
| policy/ | 에어 행동 원칙 (1순위 읽기) |
| projects/ | 프로젝트별 문서 |
| archive/ | 레거시 보존 |
| DECISIONS_LOG.md | 모든 구조적 결정 기록 |
| STAGE_TRACKER.md | 시스템 성숙도 단계 추적 |

## 원칙

- Git이 SSOT. 채팅은 작업 메모리일 뿐.
- 시크릿/토큰 절대 커밋 금지.
- 구조적 결정은 반드시 DECISIONS_LOG에 append.
