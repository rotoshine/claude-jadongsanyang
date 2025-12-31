---
description: 자동사냥 Setup (프로젝트에 자동사냥 설정 추가)
allowed-tools: Glob, Read, Edit, Write, Bash(mkdir:*)
---

# 자동사냥 Setup

현재 프로젝트에 자동사냥을 사용할 수 있도록 설정합니다.

## 수행 단계

### 1단계: todos 디렉토리 생성

프로젝트 루트에 `/todos/` 디렉토리가 없으면 생성합니다.

```bash
mkdir -p todos
```

### 2단계: CLAUDE.md 업데이트

프로젝트의 `CLAUDE.md` 파일에 자동사냥 커맨드 매핑을 추가합니다.

#### CLAUDE.md가 없는 경우
새로 생성하고 아래 내용을 추가합니다.

#### CLAUDE.md가 있는 경우
파일 끝에 아래 섹션을 추가합니다. 단, 이미 "자동사냥 커맨드" 섹션이 있으면 건너뜁니다.

#### 추가할 내용

```markdown
## 자동사냥 커맨드

사용자가 아래 명령어를 사용하면 해당 스킬을 실행합니다:

| 사용자 명령어 | 실행할 스킬 | 설명 |
|--------------|------------|------|
| "자동사냥 계획" | `/jadongsanyang:jadongsanyang-planning` | 코드베이스 분석 후 `/todos/`에 개선 아이디어 생성 |
| "자동사냥 계획 [경로]" | `/jadongsanyang:jadongsanyang-planning [경로]` | 특정 경로만 분석 (모노레포 지원) |
| "자동사냥 준비" | `/jadongsanyang:jadongsanyang-ready` | `/todos/` 목록 분석 및 다음 작업 추천 |
| "자동사냥 시작" | `/jadongsanyang:jadongsanyang-start` | 우선순위 높은 작업 구현 및 PR 생성 |
| "자동사냥 시작 [작업명]" | `/jadongsanyang:jadongsanyang-start [작업명]` | 지정된 작업 구현 및 PR 생성 |

**사용 예시:**
- "자동사냥 계획" → 전체 코드베이스 분석
- "자동사냥 계획 apps/web" → 특정 앱만 분석 (모노레포)
- "자동사냥 준비" → 작업 목록 및 추천 작업 출력
- "자동사냥 시작 accessibility" → 특정 작업 시작
```

### 3단계: 권한 설정 추가

`.claude/settings.json` 파일에 자동사냥에 필요한 권한을 추가합니다.

#### .claude 디렉토리가 없는 경우
```bash
mkdir -p .claude
```

#### settings.json 처리

1. `.claude/settings.json` 파일 읽기 (없으면 빈 객체 `{}`)
2. `permissions.allow` 배열에 다음 권한 추가 (중복 제외):

```json
{
  "permissions": {
    "allow": [
      "Glob(todos/**)",
      "Read(todos/**)",
      "Write(todos/**)",
      "Edit(todos/**)",
      "Bash(git:*)",
      "Bash(gh:*)"
    ]
  }
}
```

3. 기존 권한이 있으면 병합 (기존 값 유지, 새 값만 추가)

### 4단계: 결과 출력

설정 완료 후 다음을 표시합니다:

```
✅ 자동사냥 설정 완료!

설정된 항목:
- /todos/ 디렉토리 생성됨 (또는 이미 존재)
- CLAUDE.md에 자동사냥 커맨드 매핑 추가됨
- .claude/settings.json에 권한 설정 추가됨

이제 다음 명령어를 사용할 수 있습니다:
- "자동사냥 계획" - 코드베이스 분석 → Todo 생성
- "자동사냥 준비" - Todo 목록 확인 → 작업 추천
- "자동사냥 시작" - 작업 구현 → PR 생성
```

## 주의사항

- CLAUDE.md 파일을 직접 수정합니다
- .claude/settings.json 파일을 직접 수정합니다
- 기존 "자동사냥 커맨드" 섹션이 있으면 중복 추가하지 않습니다
- 기존 권한 설정은 유지하고 새 권한만 추가합니다
- 변경 사항은 git에 커밋되지 않습니다 (사용자가 직접 커밋)
