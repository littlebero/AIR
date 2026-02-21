# DECISIONS_LOG.md

## 2026-02-20 — ai.air.llmworker 제거
불필요한 OpenAI 프록시 서버 비활성화 및 삭제. OpenClaw가 OpenAI를 직접 연결하므로 중복 제거.

## 2026-02-20 — OpenAI fallback 설정 완료
gpt-4.1-mini를 fallback으로 추가. Anthropic rate limit 시 자동 전환.
