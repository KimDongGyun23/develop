# /commit — 커밋 메시지 생성

아래 템플릿 형식으로 커밋 메시지를 작성하고 커밋

```
<type>(<scope>): <message>
- ...
- ...
```

## 규칙

**type**: `feat`(새 기능), `fix`(버그 수정), `style`(로직 변경 없는 코드 정리), `perf`(성능 개선), `test`(테스트) 중 하나 선택
**scope**: 변경된 기능/영역 (예: `user-login`, `auth-token`, `profile-edit`)
**message**: 작업 내용을 한글 구어체로 간략히 (~합니다, ~추가합니다, ~수정합니다 등)
**body bullet**: 주요 변경 사항을 `-` 로 나열. 자명한 경우 생략 가능.

## 실행 절차

1. `git status` 와 `git diff --staged` 로 스테이징된 변경 내용 파악
2. 스테이징된 파일이 없으면 `git diff` (unstaged) 도 확인하고 사용자에게 스테이징 먼저 하도록 안내
3. 변경 내용을 분석해 type, scope, message, body 초안 작성
4. 생성한 커밋 메시지를 사용자에게 보여주고 수정 여부 확인
5. 승인 시 아래 형식으로 커밋 실행:

```bash
git commit -m "$(cat <<'EOF'
<type>(<scope>): <message>

- ...
EOF
)"
```

6. 커밋 성공 여부 확인 후 결과 출력
