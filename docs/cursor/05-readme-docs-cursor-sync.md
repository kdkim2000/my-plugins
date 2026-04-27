# STEP 5 — README 반영 및 docs/cursor 후속 정리

## 사용자 요청 (요약)

> 변경사항을 반영하여 `my-plugins/README.md` 파일을 업데이트 하라.  
> 지금까지 추가 대화를 `/docs/cursor` 이하 정리하라.

## 수행 작업

### 5-1. 루트 README.md 업데이트

반영한 내용:

| 항목 | 변경 |
| --- | --- |
| 플러그인 버전 | 문서 상단에 **my-skills 1.1.0** 명시 (plugin.json 과 정합) |
| 폴더 구조 | `docs/` 트리, `pr-description/` 및 `evals/evals.json` 추가 |
| 스킬 표 | `pr-description` 행 추가 (한 줄 설명) |
| 사용 예시 | PR 설명·git diff 트리거 예시, `/my-skills:pr-description` 명시 호출 |
| 문서 섹션 | `docs/` 링크 및 installation / skills / cursor 인덱스 안내 |
| 새 스킬 절차 | 7번째 단계로 `docs/skills.md`·`contributing.md` 갱신 권장 추가 |

### 5-2. docs/cursor/ 후속 문서

| 파일 | 내용 |
| --- | --- |
| [04-github-pr1-and-git-request.md](04-github-pr1-and-git-request.md) | Git add/commit/push 요청(셸 이슈 메모) + [PR #1](https://github.com/kdkim2000/my-plugins/pull/1) 본문 작성 요청 정리 |
| [05-readme-docs-cursor-sync.md](05-readme-docs-cursor-sync.md) | 본 문서 — README·cursor 인덱스 동기화 |

### 5-3. docs/cursor/README.md 인덱스 갱신

- mermaid 흐름도에 STEP 4, 5 노드 추가.
- 단계별 표에 `04`, `05` 행 추가.
- 결과 요약 트리에 `pr-description`, `docs/cursor` 의 신규 파일 반영.

## 변경된 파일 (이 STEP 기준)

| 경로 | 작업 |
| --- | --- |
| `README.md` | pr-description·docs·버전·예시·문서 링크 반영 |
| `docs/cursor/README.md` | STEP 4~5 및 트리·표 갱신 |
| `docs/cursor/04-github-pr1-and-git-request.md` | 신규 |
| `docs/cursor/05-readme-docs-cursor-sync.md` | 신규 |

## 후속 권장 (미수행)

- [`docs/skills.md`](../skills.md) 표에 `pr-description` 행과 상세 문서 링크 추가 (`docs/skills/pr-description.md` 신규 작성 시 일관성 최상).
- 로컬 전용 `pr-description-workspace/` 가 커밋에 포함되어 있다면 `.gitignore` 로 제외 검토.

## 관련 링크

- [GitHub Pull Request #1 · kdkim2000/my-plugins](https://github.com/kdkim2000/my-plugins/pull/1)
