# Vue 3 주의사항

## watch 사용 지양

`watch`는 연쇄 호출, 타이밍 이슈, 의도치 않은 emit 등 버그의 원인이 되므로 **가능한 한 사용하지 않는다**. Vue 3, Nuxt 3 모두 동일.

### 대안

| 상황 | watch 대신 |
|------|-----------|
| 상태 변경 → 부수효과 (emit 등) | template `@change`/`@input` 이벤트 핸들러에서 직접 처리 |
| 파생 상태 | `computed` |
| 여러 watch가 서로 트리거 | 이벤트 핸들러 1개로 통합 |
| props → 내부 ref 동기화 | watch 허용하되 **emit 없이 동기화만** |

### Bad: watch 연쇄 (버그 유발)

```ts
watch(selectedType, emitChange);
watch(isLooping, () => { playCount.value = 0; emitChange(); });
watch(playCount, () => { isLooping.value = count === 0; emitChange(); });
```

- props.value watch가 ref를 업데이트 → 하위 watch들이 연쇄 트리거 → 아직 세팅 안 된 값으로 emit → 데이터 유실

### Good: 이벤트 핸들러에서 직접 처리

```vue
<b-form-select :value="selectedType" @change="handleChangeType" />
```

```ts
const handleChangeType = (value: string | null): void => {
    selectedType.value = value;
    emitChange();
};
```

### watch가 허용되는 경우

- **props → 내부 ref 동기화** (emit 없이 값만 반영)
- **외부 비동기 데이터 fetch** (route params 변경 감지 등, 대안이 없는 경우)
