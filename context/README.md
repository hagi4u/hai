# Context 인덱스

> 이 폴더는 플랫폼 독립적입니다. Claude Code, Openclaw 등 어떤 AI 에이전트에서도 재사용 가능합니다.
> 핵심 행동 규칙은 각 플랫폼의 진입점(CLAUDE.md 등)에 인라인으로 유지하고, 이 파일들은 필요 시에만 참조한다.

## 파일 구조

| 파일 | 설명 | 로드 시점 |
|------|------|----------|
| `ai_identity.md` | Hai 운영 원칙 (Persistence, Reweave 등) | 자기업데이트/구조 변경 시 |
| `user_profile.md` | 사용자 프로필, 기술 스택, 관심사 | 기술/업무 컨텍스트 필요 시 |
| `growth.md` | Hai의 주간 성장 기록 | 금요일 정오 이후 |
| `schedule.md` | 일정 관리 | 일정 관련 대화 시 |
| `error.md` | 실수 및 오류 로그 | 참조 시 |

## 외부 참조

| 경로 | 설명 |
|------|------|
| `../works/projects.md` | 프로젝트 히스토리 및 진행 현황 |
| `../works/goals/2026.md` | 2026년 목표 및 Action Items |
| `../skills/principles.md` | 코딩 원칙 |
