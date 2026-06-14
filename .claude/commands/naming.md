# /naming — 네이밍 개선

IDE에서 현재 열린 파일(또는 `$ARGUMENTS` 경로)을 분석해 아래 규칙에 따른 네이밍 개선을 제안하고 적용합니다.

## 점검 항목

### 1. 변수, 상태 prefix

| 타입               | 권장 prefix                      | 예시                                            |
| ------------------ | -------------------------------- | ----------------------------------------------- |
| Boolean            | `is`, `has`, `can`, `should`     | `isVerified`, `hasMore`, `canSubmit`            |
| 이벤트 핸들러      | `handle`(내부 함수), `on`(props) | `handleClick`, `onSubmit`                       |
| 커스텀 훅          | `use`                            | `useTrendingNotices`                            |
| 조회성 비동기 함수 | `fetch`, `get`, `load`           | `fetchActivities`, `getProfile`, `loadComments` |

### 2. Props 타입명

- **로컬 전용** (export 없음): `type Props = {}` 사용

- 컴포넌트의 Props 타입은 사용 범위와 관계없이 항상 `{ComponentName}Props`로 명명
- 파일 내부에서만 사용하는 로컬 타입이어도 `Props`로 축약하지 않음

```tsx
export type ButtonProps = {
  disabled?: boolean;
  onClick: () => void;
};
```

```tsx
export type TrendingNoticeItemProps = {
  ...
};
```

### 3. 컴포넌트명 ↔ 파일명 일치

- `export default` 컴포넌트가 하나인 경우 컴포넌트명과 파일명이 반드시 일치해야 함
- 불일치 발견 시 컴포넌트명 또는 파일명 변경 제안
- 파일명 변경 선택 시 `git mv` 실행 후 import 경로 일괄 수정

### 4. 모호하거나 축약된 이름

- 단일 문자 변수(`d`, `v`, `e`) → 컨텍스트에 맞는 이름
- 과도한 축약(`res`, `err`, `cb`) → `response`, `error`, `callback` 또는 도메인 명칭
- 30자 이상이고 단순화 가능한 이름도 지적

## 실행 절차

1. 대상 파일 결정:
   - `$ARGUMENTS`가 있으면 해당 경로 사용
   - 없으면 IDE에서 현재 열린 파일(`ide_opened_file` 컨텍스트) 사용
   - 둘 다 없으면 사용자에게 경로 요청
2. 파일 전체를 읽고 위 5개 항목을 기준으로 개선 대상 목록 작성
3. 항목별로 **현재 → 제안** 형태로 정리해 사용자에게 보고
4. 사용자가 전체 또는 항목별 승인 시 코드에 반영
5. 컴포넌트명/파일명 변경이 포함된 경우:
   - `git mv <old> <new>` 로 파일 이동
   - 해당 파일을 import하는 파일 목록을 `grep`으로 찾아 import 경로 일괄 수정
6. 변경 요약 보고
