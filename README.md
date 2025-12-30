# 자동사냥 (Jadongsanyang)

Claude Code를 위한 자동화된 코드 개선 워크플로우 플러그인입니다.

## 기능

| 커맨드 | 설명 |
|--------|------|
| `자동사냥 설치` | 프로젝트에 자동사냥 설정 (CLAUDE.md 업데이트) |
| `자동사냥 계획` | 코드베이스 분석 → `/todos/`에 개선 아이디어 생성 |
| `자동사냥 계획 [경로]` | 특정 경로만 분석 (모노레포 지원) |
| `자동사냥 준비` | `/todos/` 분석 → 다음 작업 추천 |
| `자동사냥 시작` | 추천된 작업 구현 → PR 생성 |
| `자동사냥 시작 [작업명]` | 지정된 작업 구현 → PR 생성 |

## 설치

### 1단계: 플러그인 설치

**로컬에서:**
```bash
claude plugin install --path ~/roto_workspaces/jadongsanyang
```

**GitHub에서:**
```bash
claude plugin marketplace add roto/jadongsanyang
claude plugin install jadongsanyang
```

### 2단계: 프로젝트 설정

Claude Code 재실행 후, 프로젝트에서:

```
자동사냥 설치
```

이 명령어는:
- `/todos/` 디렉토리 생성
- `CLAUDE.md`에 자동사냥 커맨드 매핑 추가

## 사용 예시

```
# 전체 코드베이스 분석
자동사냥 계획

# 특정 앱만 분석 (모노레포)
자동사냥 계획 apps/web

# 특정 영역 집중
자동사냥 계획 performance

# 작업 목록 확인
자동사냥 준비

# 특정 앱 작업만 필터링
자동사냥 준비 apps/web

# 추천된 작업 시작
자동사냥 시작

# 특정 작업 시작
자동사냥 시작 accessibility-improvements
```

## 워크플로우

```
┌─────────────────┐
│  자동사냥 계획   │  코드베이스 분석 → Todo 생성
└────────┬────────┘
         ▼
┌─────────────────┐
│  자동사냥 준비   │  Todo 목록 확인 → 작업 추천
└────────┬────────┘
         ▼
┌─────────────────┐
│  자동사냥 시작   │  작업 구현 → PR 생성
└─────────────────┘
```

## Todo 파일 형식

`/todos/` 디렉토리에 저장되는 파일:

```markdown
# 제목

## 상태
- [ ] 미시작

## 대상
- 경로: `apps/web` (또는 `전체`)
- 카테고리: refactor / feature / ux / performance / dx / infra

## 개요
간단한 설명 (1-2문장)

## 구현 계획

### Phase 1: [단계명]
- 작업 내용 1
- 작업 내용 2

## 영향받는 파일
- `src/path/to/file.tsx`

## 우선순위
High / Medium / Low

## 예상 작업량
- 👤 사람: [시간/일 단위]
- 🤖 Claude Code: [시간/일 단위]
```

## 모노레포 지원

특정 경로를 지정하면:
- 해당 경로만 분석
- Todo 파일이 `/todos/[경로]/`에 저장됨
- 예: `자동사냥 계획 apps/feed` → `/todos/apps/feed/feature-name.md`

## 플러그인 관리

```bash
# 설치 확인
claude plugin list

# 업데이트
claude plugin update jadongsanyang

# 비활성화
claude plugin disable jadongsanyang

# 제거
claude plugin uninstall jadongsanyang
```

## 파일 구조

```
jadongsanyang/
├── .claude-plugin/
│   ├── plugin.json         # 플러그인 메타데이터
│   └── marketplace.json    # 마켓플레이스 매니페스트
├── commands/
│   ├── jadongsanyang-setup.md      # 프로젝트 설정
│   ├── jadongsanyang-planning.md   # 코드 분석 → Todo 생성
│   ├── jadongsanyang-ready.md      # Todo 목록 → 작업 추천
│   └── jadongsanyang-start.md      # 작업 구현 → PR 생성
└── README.md
```

## 라이선스

MIT
