# My Custom Claude Plugins

kdkim2000의 개인 커스텀 Claude Code 플러그인 마켓플레이스입니다.

현재 플러그인 버전: **my-skills 1.1.0** (마켓플레이스 `my-plugins` 의 플러그인 항목 `version` 과 동기화)

## 폴더 구조

```
my-plugins/
├── .claude-plugin/
│   └── marketplace.json        # 마켓플레이스 카탈로그 (상위 진입점)
├── docs/                        # 설치·아키텍처·배포·스킬별 상세 가이드
│   ├── installation.md
│   ├── skills.md
│   ├── contributing.md
│   ├── architecture.md
│   ├── distribution.md
│   ├── skills/
│   └── cursor/                  # Cursor 에서의 작업·결정 기록
├── plugins/
│   └── my-skills/              # 실제 플러그인 (marketplace.json 의 source)
│       ├── .claude-plugin/
│       │   └── plugin.json     # 플러그인 메타데이터
│       └── skills/
│           ├── emoji-summarizer/
│           │   └── SKILL.md    # 텍스트를 이모지로 요약
│           ├── pr-description/
│           │   ├── SKILL.md    # GitHub PR 본문 자동 작성
│           │   └── evals/
│           │       └── evals.json
│           └── blank-template/
│               └── SKILL.md    # 새 스킬을 만들 때 복사하는 빈 템플릿
└── README.md
```

## 포함된 스킬

| 스킬 | 설명 |
| --- | --- |
| `emoji-summarizer` | 텍스트/문장/상황 설명을 이모지 3~5개와 한 줄 설명으로 요약합니다. |
| `pr-description` | `git diff`, 코드 변경, 또는 자연어 설명을 바탕으로 GitHub PR 본문(Summary, Changes, How to Test 등)을 마크다운으로 작성합니다. |
| `blank-template` | 새 스킬을 만들 때 복사해서 시작하는 빈 템플릿입니다. 실제 작업에는 자동으로 사용되지 않습니다. |

## 설치 방법

Claude Code 의 인터랙티브 슬래시 명령으로 마켓플레이스를 추가하고 플러그인을 설치합니다.

```
/plugin marketplace add E:\apps\claude\my-plugins
/plugin install my-skills@my-plugins
```

마켓플레이스 내용을 갱신했을 때는 다음 명령으로 새로고침합니다.

```
/plugin marketplace update my-plugins
```

### 로컬 개발 모드로 빠르게 테스트

플러그인을 마켓플레이스에 등록하지 않고 바로 테스트하려면 `--plugin-dir` 플래그를 사용합니다.

```bash
claude --plugin-dir ./plugins/my-skills
```

스킬/매니페스트를 수정한 뒤에는 Claude Code 세션 안에서 `/reload-plugins` 로 변경 사항을 반영할 수 있습니다.

## 사용 예시

설치 후 Claude Code 에서 자연어로 트리거하면 스킬이 자동으로 사용됩니다.

- "오늘 비 와서 집에서 책 읽었어. **이모지로 요약**해줘." → `emoji-summarizer` 가 호출되어 이모지 3개와 한 줄 설명을 출력합니다.
- "로그인 폼에 이메일 검증 넣었어. **PR 설명 써줘**." 또는 "**git diff** 보고 PR 본문 작성해줘." → `pr-description` 이 PR용 마크다운을 생성합니다.
- 명시적 호출 예: `/my-skills:pr-description` 뒤에 변경 내용이나 diff 를 이어서 입력합니다.

## 문서

설치 절차, 마켓플레이스 구조, 여러 도구에서의 재사용 방법은 [`docs/`](docs/) 디렉터리를 참고하세요.

- 처음 설치: [`docs/installation.md`](docs/installation.md)
- 스킬 목록·호출 방식: [`docs/skills.md`](docs/skills.md)
- Cursor 작업 기록: [`docs/cursor/README.md`](docs/cursor/README.md)

## 새 스킬 추가 절차

1. `plugins/my-skills/skills/blank-template/` 폴더를 통째로 복사합니다.
2. 복사된 폴더 이름을 새 스킬 이름(kebab-case)으로 변경합니다. 폴더명이 곧 스킬명이 됩니다.
3. 새 폴더 안의 `SKILL.md` frontmatter 에서 `name` 과 `description` 을 새 스킬에 맞게 다시 작성합니다.
   - `description` 에는 **이 스킬이 언제 트리거되어야 하는지** 알려주는 키워드를 충분히 포함해야 합니다.
4. 본문의 `(TODO)` 표시를 모두 채우고, 안내용 인용 블록(`>` 부분)을 삭제합니다.
5. `plugin.json` 의 `version` 을 올리고, 필요하면 `marketplace.json` 의 플러그인 항목 `version` 도 함께 올립니다.
6. `/reload-plugins` 또는 `/plugin marketplace update my-plugins` 로 변경 사항을 반영하고 동작을 확인합니다.
7. (권장) [`docs/skills.md`](docs/skills.md) 표와 [`docs/contributing.md`](docs/contributing.md) 체크리스트를 갱신합니다.

## 라이선스

MIT
