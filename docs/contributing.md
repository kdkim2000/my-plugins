# 새 스킬 추가하기

`my-skills` 플러그인에 새 스킬을 추가하는 표준 절차입니다. `blank-template` 을 베이스로 시작하는 것이 가장 빠릅니다.

## 절차

### 1. 템플릿 복사

```bash
cd my-plugins/plugins/my-skills/skills
cp -r blank-template <new-skill-name>
```

- `<new-skill-name>` 은 **kebab-case** 로 짓습니다 (예: `commit-message-helper`, `meeting-notes`).
- **폴더 이름이 곧 스킬 이름**이 됩니다. 호출 시 `/my-skills:<new-skill-name>` 형태로 사용됩니다.

### 2. frontmatter 작성

새 폴더의 `SKILL.md` 첫머리 frontmatter 를 다시 씁니다.

```markdown
---
name: <new-skill-name>
description: (이 스킬이 무엇을 하는지) + (언제 트리거되어야 하는지 키워드). "..." 같은 표현이 나오면 반드시 이 스킬을 사용하세요.
---
```

작성 팁:

- `description` 은 **모델이 이 스킬을 언제 써야 할지** 판단하는 가장 중요한 정보입니다.
- 트리거하고 싶은 자연어 키워드를 큰따옴표로 직접 인용해 두면 매칭률이 올라갑니다.
- 한 문장 요약 + 트리거 키워드 + 사용 시나리오 순서로 쓰는 것을 권장합니다.

### 3. 본문 작성

`blank-template/SKILL.md` 의 섹션 골격(개요 → 규칙 → 출력 형식 → 엣지 케이스 → 예시)을 채워 넣고, 안내용 인용 블록(`>` 부분)은 모두 삭제합니다.

권장 분량: 50~150 줄. 너무 짧으면 동작이 일관되지 않고, 너무 길면 모델이 핵심을 놓칠 수 있습니다.

### 4. 예시는 최소 2개 이상

다양한 톤·길이·주제를 커버하는 예시를 2~4개 작성합니다. 예시는 모델이 출력 패턴을 학습하는 가장 강력한 신호입니다.

### 5. 버전 올리기

새 스킬을 추가했다면 두 곳의 `version` 을 함께 올립니다.

- `plugins/my-skills/.claude-plugin/plugin.json` 의 `version` (예: `1.0.0` → `1.1.0`)
- `.claude-plugin/marketplace.json` 의 plugin 항목 `version` (동일하게 맞춤)

semver 가이드:
- 새 스킬 추가 / 기능 추가 → minor 업
- 기존 스킬 수정 / 버그 픽스 → patch 업
- 호환성 깨지는 변경 (스킬 삭제, 폴더명 변경 등) → major 업

### 6. 테스트

```bash
# 로컬 개발 모드로 즉시 확인
claude --plugin-dir ./plugins/my-skills
```

세션 안에서 변경 사항을 다시 불러오기:

```
/reload-plugins
```

자동 호출 / 슬래시 명령 호출 두 가지 모두 시도해 보세요.

### 7. 문서 업데이트

- [docs/skills.md](skills.md) 의 표에 새 스킬 한 줄 추가
- `docs/skills/<new-skill-name>.md` 상세 문서 작성 (선택이지만 권장)

## 체크리스트

- [ ] 폴더 이름이 kebab-case 인가
- [ ] frontmatter `name` 이 폴더 이름과 일치하는가
- [ ] `description` 에 트리거 키워드가 포함되어 있는가
- [ ] 안내용 인용 블록(`>`)을 모두 삭제했는가
- [ ] 예시가 2개 이상인가
- [ ] `plugin.json` / `marketplace.json` 의 `version` 을 올렸는가
- [ ] `/reload-plugins` 후 자동 호출과 슬래시 호출 모두 동작하는가
- [ ] [docs/skills.md](skills.md) 에 등록했는가

## 자주 하는 실수

| 실수 | 결과 | 해결 |
| --- | --- | --- |
| 폴더 이름과 frontmatter `name` 불일치 | 자동 호출 안 됨 | 둘을 동일하게 맞춤 |
| `description` 이 너무 짧음 | 모델이 트리거 못 함 | 키워드·시나리오 보강 |
| 안내 인용 블록(`>`) 미삭제 | 출력에 메타 가이드가 섞여 나옴 | 본문 작성 후 통째 삭제 |
| 코드펜스 안의 출력 예시를 모델이 그대로 흉내냄 | 응답에 ``` 가 끼어 나옴 | "실제 응답에서는 코드펜스를 쓰지 말 것" 한 줄 명시 |
| 버전 미증가 | 사용자 측에서 업데이트 알림 안 옴 | semver 규칙대로 bump |
