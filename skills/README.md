# Skills 인덱스

> 기술스택별 지식 베이스. 패턴, 주의사항, 실전 노하우를 축적합니다.

## 기술 목록

| 폴더 | 기술 | 사용처 |
|------|------|--------|
| `typescript/` | TypeScript | KREAM 웹, KREAM Pay 공통 |
| `nuxt3/` | Nuxt 3 | KREAM 웹 서비스 |
| `nextjs/` | Next.js 16+ | KREAM Pay 서비스 |
| `vue3/` | Vue 3 | Nuxt 3 기반 (컴포넌트/컴포지션 API) |
| `react/` | React | Next.js 기반 |
| `platforms/` | Android / iOS 제약사항 | TPM 역할 수행 시 참조 |
| `python/` | Python | kream 어드민 API 서버 |

## 파일 구조 (각 기술 공통)

| 파일 | 설명 |
|------|------|
| `README.md` | 핵심 개념 요약, 빠른 참조용 |
| `patterns.md` | 자주 쓰는 패턴, 실전 코드 |
| `gotchas.md` | 주의사항, 삽질 기록, 트러블슈팅 |

## 참조 우선순위
기술적인 질문이나 코드 제안 시 `skills/`를 먼저 참조한다.
1. `principles.md` — 설계 원칙 확인
2. 해당 기술 폴더의 `README.md` → `patterns.md` → `gotchas.md` 순으로 참조

## 업데이트 방식

1. **초기 시드** — 공식 문서 기반 핵심 내용 정리
2. **세션 누적** — 작업 중 발견한 패턴/해결책 즉시 추가
3. **사용자 인풋** — 공유한 아티클/트릭을 구조화해서 저장
