# CSS 패턴

## Bootstrap 그리드 — Gutter 이해

Bootstrap col은 **여백을 col 자체의 padding으로 관리**한다.
- 기본 gutter: `30px` (양쪽 각 `15px`)
- col 너비 = `width + padding-left + padding-right`

### 문제
```
col-6 × 2 = 각 15px + 15px (총 60px 여백 소비)
col-12 × 1 = 15px + 15px (총 30px 여백 소비)
→ 중첩 시 col-6 안의 col-4 ≠ col-12 안의 col-2 (픽셀 차이 발생)
```

### 해결책

**1. `no-gutters`로 padding 제거 후 래퍼에 여백 직접 추가**
```vue
<div class="row no-gutters">
  <div class="col-6">
    <div class="pr-2">컨텐츠</div>
  </div>
  <div class="col-6">
    <div class="pl-2">컨텐츠</div>
  </div>
</div>
```

**2. CSS에서 일괄 관리**
```css
.row.no-gutters > [class*="col-"] {
  padding-left: 7.5px;
  padding-right: 7.5px;
}
```
