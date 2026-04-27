# STEP 4 — Git 작업 요청 및 GitHub PR #1 본문 작성

## 4-A. 변경사항 커밋·푸시 요청

### 사용자 요청 (요약)

> "변경사항을 정리하여 git add, git commit, git push 를 수행하라."

### 에이전트 측 메모

- 이 요청 직후 Cursor 의 셸 실행이 **Review cancelled or failed** 로 종료되어, 저장소 루트·원격·브랜치 상태를 확인하는 `git` 명령이 완료되지 않은 기록이 있음.
- 실제 커밋·푸시는 **로컬 터미널**에서 사용자가 직접 수행하는 것이 안전함. 특히 `my-plugins` 가 독립 저장소인 경우 아래 순서를 따르면 됨.

### 권장 명령 (참고)

```bash
cd path/to/my-plugins
git status
git add -A
git commit -m "feat(skills): add pr-description skill and evals"
git push origin <branch-name>
```

브랜치명은 PR 기준으로 `pr-description` 등 실제 작업 브랜치에 맞출 것.

---

## 4-B. GitHub PR #1 본문 형식 작성

### 사용자 요청 (요약)

> `https://github.com/kdkim2000/my-plugins/pull/1` PR 을 발행하였다. PR 본문 내용을 형식에 맞게 작성해 줘.

### 수행 내용

- 로컬 저장소에서 `plugins/my-skills/skills/pr-description/SKILL.md`, `evals/evals.json`, `plugin.json`, `marketplace.json` 등을 참고해 **GitHub 표준 PR 본문 형식**으로 마크다운 초안을 작성함.
- 섹션: **Summary**, **Changes**, **How to Test**, **Notes for Reviewers** (필요 시 **Related Issues**).
- PR 제목 맥락: [Pull Request #1 · kdkim2000/my-plugins](https://github.com/kdkim2000/my-plugins/pull/1) — `pr-description` 스킬 추가.

### 산출물

- 채팅 응답으로 붙여 넣기 가능한 한국어 PR 본문 블록 전체 제공.
- `pr-description-workspace/` 같은 로컬 eval 산출물이 커밋에 포함된 경우 **리뷰어 노트에서 `.gitignore` 또는 PR 제외** 권고 문구 포함.

### 학습 포인트

- PR 본문은 **리뷰어가 30초 안에 맥락을 잡을 수 있을 정도**로 Summary 를 앞에 두는 것이 좋음.
- 스킬 추가 PR 이면 **트리거 키워드**, **평가 파일 유무**, **버전 bump** 를 Changes 에 명시하면 마켓플레이스 사용자에게도 도움이 됨.

## 다음 단계

[STEP 5 — README 및 docs/cursor 동기화](05-readme-docs-cursor-sync.md)
