# 스킬 목록

이 플러그인(`my-skills`) 에 포함된 스킬들의 한눈 보기입니다. 각 스킬의 상세 사용법은 별도 문서로 분리되어 있습니다.

| 스킬 ID | 한 줄 설명 | 자동 트리거 | 상세 문서 |
| --- | --- | --- | --- |
| `emoji-summarizer` | 텍스트/문장/상황 설명을 이모지 3~5개와 한 줄 설명으로 요약합니다. | 예 | [skills/emoji-summarizer.md](skills/emoji-summarizer.md) |
| `blank-template` | 새 스킬을 만들 때 복사해서 시작하는 빈 템플릿. | 아니오 (의도적 비활성) | [skills/blank-template.md](skills/blank-template.md) |

## 호출 방식

- **자동 호출 (모델 인보케이션):** Claude 가 사용자 입력의 의도를 파악해 자동으로 스킬을 사용합니다. `description` 의 키워드가 정확히 작성되어 있어야 잘 동작합니다.
- **명시적 호출:** 슬래시 명령으로 직접 호출합니다.
  ```
  /my-skills:emoji-summarizer 오늘 비 와서 집에서 책 읽었어
  ```
  - 플러그인명이 네임스페이스로 붙어 `my-skills:` 접두어가 붙습니다.
  - 다른 플러그인의 동명 스킬과 충돌을 막기 위한 표준 동작입니다.

## 자동 트리거 활성화 / 비활성화

스킬의 `SKILL.md` frontmatter 에서 제어합니다.

| 설정 | 의미 |
| --- | --- |
| (기본) `description` 만 있음 | 자동 호출 가능 |
| `disable-model-invocation: true` | 자동 호출 차단. 슬래시 명령으로만 호출 |

`blank-template` 은 자동 트리거되지 않도록 description 에 "실제 작업에는 자동으로 사용되지 않습니다" 를 명시해 두었습니다. 더 강하게 차단하려면 `disable-model-invocation: true` 를 frontmatter 에 추가하세요.

## 새 스킬을 추가하려면

[contributing.md](contributing.md) 의 절차를 따르세요. `blank-template` 폴더를 복사해 시작하는 것이 가장 빠릅니다.
