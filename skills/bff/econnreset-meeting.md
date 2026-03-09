# ECONNRESET 미팅 준비

## 현재 상황

- Pay BE 배포 시 Pod IP 교체 → keep-alive 소켓 강제 종료 → BFF에서 ECONNRESET 발생
- 인프라 측에서 "완전히 막기 어렵다"는 입장 확인

---

## 확인할 것들

**인프라에게**
- LB(ALB/NLB)의 idle timeout이 얼마인지
- Pay BE 서버의 keepAliveTimeout 설정값
- Graceful shutdown(preStop + SIGTERM 처리)이 현재 적용되어 있는지
- Istio 같은 서비스 메시 사용 여부

**Pay BE 개발팀에게**
- 현재 ECONNRESET 발생 빈도와 영향 범위 파악이 되어 있는지
- 주요 API 중 Idempotency Key가 없는 상태 변경 엔드포인트가 있는지

---

## 논의할 해결 방향

| 방법 | 담당 | 난이도 | 효과 |
|------|------|--------|------|
| BFF: ECONNRESET retry (GET만) | FE | 낮음 | 즉효 |
| BFF + BE: Idempotency Key 도입 | FE + BE | 중간 | POST/PUT retry 안전화 |
| 인프라: keepAliveTimeout 조정 | 인프라 | 낮음 | 평상시 stale socket 제거 |
| BE: Graceful shutdown 강화 | BE + 인프라 | 중간 | 배포 중 영향 최소화 |

---

## 미리 입장 정해두면 좋은 것

1. **단기**: BFF에서 GET retry 먼저 적용 → 빠르게 개선
2. **중기**: POST/PUT 핵심 API에 Idempotency Key 도입 범위 합의
3. **근본**: Graceful shutdown or 서비스 메시 — 인프라 팀과 로드맵 논의

---

## 미팅에서 피해야 할 것

- "retry 하면 되는 거 아닌가요?" → POST retry는 중복 처리 위험, 사전 설명 필요
- 단일 해결책으로 끝내려는 시도 → 레이어별로 나눠서 각자 역할 분담하는 게 현실적

---

## 배경 지식 참조

- ECONNRESET 원인 및 방어 방법 → `bff/gotchas.md`
- Istio / 사이드카 패턴 → `infra/service-mesh.md`
