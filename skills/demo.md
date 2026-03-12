# 데모 발표자료 준비 스킬

개발 완료 후, `/설계`로 만든 설계문서에 코드 변경점 기반의 시연 시나리오와 로직 설명을 채워넣는 스킬입니다.

## 사용법

```
/demo
```

입력하면 아래 정보를 물어본다:

```
🤖 데모 발표자료를 준비할게요! 아래 정보를 알려주세요.

1. 📄 설계문서 URL (또는 타이틀):
2. 🔀 Git branch:
3. 📋 테크스펙 문서 URL (없으면 엔터):
```

### 입력 예시

```
1. https://wiki.navercorp.com/pages/viewpage.action?pageId=4888182406
2. feature/KREAM-40960
3. https://wiki.navercorp.com/pages/viewpage.action?pageId=5678
```

### 모드 판별

- 설계문서에 `http`가 포함되면 → **기존 문서 업데이트** (4. 시연 + 로직 설명 섹션만 채움)
- 타이틀 텍스트면 → **새 문서 생성** (전체 템플릿 생성 + 4. 시연/로직 채움)

## 워크플로우 위치

```
   /설계 → 1~3 섹션 작성
   ↓
   테크스펙 리뷰 (팀 미팅)
   ↓
   브랜치 생성 & 개발
   ↓
👉 /demo  ← 지금 여기 (4. 시연 + 로직 설명 작성)
```

## 작동 방식

### Step 1: 컨텍스트 수집 (병렬)

**A) 코드 변경점 분석**
```bash
git fetch origin
git log --oneline origin/develop..origin/<branch>       # 커밋 히스토리
git diff origin/develop...origin/<branch> --stat         # 변경 파일 목록
git diff origin/develop...origin/<branch> -- <target>/   # 상세 diff
```
- diff를 기능 단위로 그룹핑 (타입/컴포넌트/composable/스타일/로깅/설정)

**B) 설계문서 읽기**
- `confluence_get_page` (convert_to_markdown: false) → 원본 storage HTML 확보
- 기존 콘텐츠 위치 기록 (절대 건드리지 않을 섹션 파악)

**C) 테크스펙 문서 읽기** (URL 제공 시)
- `confluence_get_page` (convert_to_markdown: true) → 테크스펙 내용 파싱
- 추출 대상:
  - 합의된 기술 결정 사항
  - 아키텍처/설계 방향
  - 리뷰에서 나온 피드백 반영 사항
- 시연 시나리오와 로직 설명에 테크스펙 맥락을 반영

### Step 2: 시연 시나리오 작성

그룹핑된 기능 단위로 시연 테이블 작성:
- 기능별로 관련 시나리오를 묶어서 작성 (atom 단위 분리 X, 5~7행 이내)
- 시나리오 셀에는 `<ul>` 로 세부 항목 나열
- 비고에는 기술적 구현 방식 간략 기재

### Step 3: 로직 설명 작성

파일 단위로 코드 변경점을 구조화:
- 파일별 h4 헤더 (`#### 1) 제목 — 파일경로`)
- 변경 의도를 한 줄 설명
- 실제 변경된 코드를 코드 블록 매크로로 삽입 (핵심 로직만 발췌)
- 테크스펙 문서가 있으면, 리뷰에서 합의된 설계 결정이 코드에 어떻게 반영됐는지 연결

### Step 4: Confluence 업데이트

⚠️ **반드시 섹션 단위 수정** (전체 덮어쓰기 금지)

```
1. 원본 HTML에서 수정 대상 섹션만 식별
   - 시연: <h2>4. 시연</h2> ~ 다음 <h2> 사이
2. 해당 섹션만 새 HTML로 교체
3. 나머지(1~3, 5~7) 원본 유지
4. confluence_update_page (content_format: "storage")
```

- 완료 후 문서 URL을 사용자에게 반환

## Confluence storage format 규칙

| 요소 | 사용할 태그 |
|------|-------------|
| 테이블 | `<table><colgroup><col/></colgroup><thead><tr><th><p>...</p></th></tr></thead><tbody>...</tbody></table>` |
| 중첩 리스트 | `<ul><li>...<ul><li>...</li></ul></li></ul>` |
| 코드 블록 | `<ac:structured-macro ac:name="code"><ac:parameter ac:name="language">ts</ac:parameter><ac:plain-text-body><![CDATA[...]]></ac:plain-text-body></ac:structured-macro>` |
| 인라인 코드 | `<code>...</code>` |
| 이미지 (첨부) | `<ac:image><ri:attachment ri:filename="..." /></ac:image>` — 절대 삭제하지 않기 |

## 주의사항

- **1~3, 5~7 섹션은 절대 수정하지 않는다** (/설계에서 이미 작성됨)
- 기존 첨부 이미지(`<ac:image>`) 참조를 보존한다
- 시연 시나리오는 기능 단위로 그룹핑하여 5~7행 이내로 정리한다
- 로직 설명의 코드는 핵심 변경점만 발췌한다 (전체 파일 복붙 X)
- content_format은 항상 "storage" 사용 (markdown 금지)
