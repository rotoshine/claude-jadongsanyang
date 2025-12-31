---
description: 자동사냥 Start (작업 구현 → PR 생성)
allowed-tools: Glob, Grep, Read, Edit, Write, Bash(git:*), Bash(gh:*), Bash(npm:*), Bash(yarn:*), Bash(npx:*), TodoWrite
---

# 자동사냥 Start

`/todos/`에서 우선순위가 높은 미완료 작업을 선택하여 구현하고 PR을 생성합니다.

## 인자
- `$ARGUMENTS`: 작업 파일명 또는 키워드 (선택사항)
  - 예: `자동사냥 시작 accessibility` → `accessibility-improvements.md` 선택
  - 미지정 시 우선순위 기반 자동 선택

## 수행 단계

### 1단계: 작업 선택

1. `/todos/` 디렉토리의 모든 `.md` 파일 읽기
   - **Glob 도구**로 `todos/**/*.md` 패턴 검색 (하위 폴더 포함)
   - **Read 도구**로 각 파일 내용 읽기
   - **절대 금지**: `bash(for...)` 루프나 `cat`, `head` 등 bash 명령어 사용 금지 (사용자에게 승인 요청이 발생하여 불편함)

2. **상태 필터링** - 다음 조건의 파일 제외:
   - `## 상태` 섹션에 `[x] 완료됨` 또는 `[x] PR 생성됨`이 있는 파일
   - frontmatter에 `completed: true` 또는 `isCompleted: true`가 있는 파일

3. **우선순위 정렬**:
   - High → Medium → Low 순서
   - 같은 우선순위 내에서는 예상 작업량 적은 것 우선

4. `$ARGUMENTS`가 있으면 해당 키워드로 필터링

5. 선택된 작업 정보 표시:
   ```
   선택된 작업: [제목]
   파일: [파일명]
   우선순위: [High/Medium/Low]
   ```

### 2단계: 브랜치 생성

```bash
# origin/main 최신화
git fetch origin main

# 새 브랜치 생성 (파일명 기반)
git checkout -b feat/[파일명-without-md] origin/main
```

예시:
- `accessibility-improvements.md` → `feat/accessibility-improvements`
- `form-validation-system.md` → `feat/form-validation-system`

### 3단계: 계획 확인

1. 선택된 todo 파일의 구현 계획 표시
2. Phase별 작업 내용 확인
3. 사용자에게 진행 여부 확인

### 4단계: 구현

1. TodoWrite로 Phase별 세부 작업 항목 생성
2. 각 Phase를 순차적으로 구현:
   - 코드 작성
   - 타입 체크
   - 린트 확인
3. 구현 완료 시 각 항목을 completed로 표시

### 5단계: 검증

프로젝트의 빌드/테스트 명령어 실행 (package.json 참조):
- TypeScript: `npx tsc --noEmit` 또는 프로젝트 스크립트
- Lint: `npm run lint` 또는 `yarn lint`
- Build: `npm run build` 또는 `yarn build`

오류 발생 시 수정 후 재검증

### 6단계: 커밋

```bash
git add -A
git commit -m "feat: [작업 제목 요약]

[상세 변경 내용]

🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>"
```

### 7단계: 상태 플래그 업데이트

Todo 파일의 `## 상태` 섹션 업데이트:

```markdown
## 상태
- [x] 완료됨
- 완료일: YYYY-MM-DD
- 브랜치: feat/[브랜치명]
- PR: #[PR번호] (PR 생성 후 업데이트)
```

### 8단계: PR 생성

```bash
# 원격에 푸시
git push -u origin feat/[브랜치명]

# PR 생성
gh pr create \
  --title "feat: [작업 제목]" \
  --body "## Summary
[구현 내용 요약]

## Changes
- [변경사항 1]
- [변경사항 2]

## Related
- Todo: \`/todos/[파일명].md\`

🤖 Generated with [Claude Code](https://claude.com/claude-code)"
```

### 9단계: 완료 처리

1. PR 생성 후 todo 파일 상태 업데이트:
   ```markdown
   ## 상태
   - [x] 완료됨
   - [x] PR 생성됨
   - 완료일: YYYY-MM-DD
   - 브랜치: feat/[브랜치명]
   - PR: #[PR번호]
   ```

2. 상태 변경 커밋:
   ```bash
   git add todos/[파일명].md
   git commit -m "docs: [파일명] 작업 완료 상태 업데이트"
   git push
   ```

3. 결과 출력:
   ```
   ✅ 작업 완료!

   브랜치: feat/[브랜치명]
   PR: [PR URL]

   변경된 파일:
   - [파일 목록]
   ```

## 상태 플래그 규칙

### 미시작 상태
```markdown
## 상태
- [ ] 미시작
```

### 진행 중 상태
```markdown
## 상태
- [ ] 진행 중
- 시작일: YYYY-MM-DD
- 브랜치: feat/[브랜치명]
```

### 완료 상태
```markdown
## 상태
- [x] 완료됨
- [x] PR 생성됨
- 완료일: YYYY-MM-DD
- 브랜치: feat/[브랜치명]
- PR: #[PR번호]
```

## 주의사항

- 구현 전 반드시 계획을 사용자에게 보여주고 승인받을 것
- 기존 코드 스타일과 컨벤션 준수
- 빌드 실패 시 PR 생성하지 않음
- 한 번에 하나의 todo만 처리
