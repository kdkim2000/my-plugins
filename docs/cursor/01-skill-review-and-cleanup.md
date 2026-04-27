# STEP 1 — SKILL 전체 구성 검토 및 정비

## 사용자 요청 (요약)

> "SKILL 을 만들었는데 전체 구성을 확인하고 누락된 부분이 없는지 추가하여 어떤 작업이 필요한지 검토하라."

## 발견된 문제

이 시점의 `my-plugins/` 폴더를 둘러보고 다음 문제를 식별했습니다.

| # | 문제 | 영향 |
| --- | --- | --- |
| 1 | `my-plugins/my-skills/` 와 `my-plugins/plugins/my-skills/` **두 곳에 동일 플러그인** 존재 | `marketplace.json` 의 `source: "./plugins/my-skills"` 만 정식이고 루트 `my-skills/` 는 미참조 — 혼란 유발 |
| 2 | 두 `plugin.json` 의 `author` 필드 형식 불일치 (string vs object) | 마켓플레이스 표준은 object — string 은 무시되거나 경고 |
| 3 | `marketplace.json` 의 description 이 최상위에 있음 | 공식 스키마는 `metadata.description` |
| 4 | `plugin.json` 에 `keywords` / `license` 누락 | 검색·메타정보 부족 |
| 5 | `emoji-summarizer/SKILL.md` 예시 2개뿐, 엣지 케이스 가이드 부재 | 모델이 다양한 입력에 일관된 응답 못 함 |
| 6 | 재사용 가능한 빈 스킬 **템플릿이 없음** | 새 스킬을 매번 처음부터 작성해야 함 |
| 7 | `README.md` 14줄로 빈약, 폴더 구조·사용 절차 부재 | 협업자/미래의 본인이 구조를 파악하기 어려움 |

## 사용자 결정사항 (AskQuestion)

| 질문 | 선택 |
| --- | --- |
| 중복 폴더 처리 | **루트 `my-skills/` 삭제, `plugins/my-skills/` 만 정식 유지** |
| 작업 범위 | **정리 + 보강 + README + 새 스킬 템플릿 1개 추가** |
| 새 스킬 종류 | **blank-template (메타데이터만 있는 빈 템플릿, 사용자 재사용용)** |

## 수행 작업

### 2-1. 중복 폴더 삭제

```
my-plugins/my-skills/                  ← 삭제
my-plugins/plugins/my-skills/          ← 정식 위치로 유지
```

근거: `marketplace.json` 의 `source: "./plugins/my-skills"` 가 가리키는 경로가 실제 진실의 원천(SoT). 루트의 `my-skills/` 는 어떤 매니페스트에서도 참조되지 않음.

### 2-2. plugin.json 표준화 + 보강

대상: [`plugins/my-skills/.claude-plugin/plugin.json`](../../plugins/my-skills/.claude-plugin/plugin.json)

```json
{
  "name": "my-skills",
  "description": "나의 커스텀 스킬 모음",
  "version": "1.0.0",
  "author": { "name": "kdkim2000" },
  "keywords": ["skills", "emoji", "korean", "productivity", "template"],
  "license": "MIT"
}
```

핵심 변경:
- `author` → object 형태 유지 (이미 정상)
- `keywords`, `license` 추가

### 2-3. marketplace.json 공식 스키마 맞춤

대상: [`.claude-plugin/marketplace.json`](../../.claude-plugin/marketplace.json)

핵심 변경:
- 최상위 `description` → `metadata.description` 으로 이동
- `metadata.version` 추가
- 각 plugin 항목에 `version`, `keywords`, `tags` 추가

근거: 공식 문서 `https://docs.claude.com/en/docs/claude-code/plugin-marketplaces` 의 스키마 표 — 마켓플레이스 최상위 옵션 필드는 `metadata.*` 형태로 그룹핑되어 있음. 플러그인 항목에서는 manifest 필드(`keywords`)와 marketplace 전용 필드(`tags`) 모두 허용.

### 2-4. emoji-summarizer SKILL.md 보강

대상: [`plugins/my-skills/skills/emoji-summarizer/SKILL.md`](../../plugins/my-skills/skills/emoji-summarizer/SKILL.md)

추가 항목:
- 시간순/인과순 배치 규칙 (규칙 4)
- "응답에는 코드펜스를 쓰지 말 것" 한 줄 (규칙 5)
- 엣지 케이스 섹션 신설 (5가지: 매우 짧은 입력 / 모호 / 부정 / 민감 / 추상 개념)
- 예시 2개 → **4개**로 확장 (감정·일정·뉴스·관계 다양화)

### 2-5. blank-template 신규 작성

신규: [`plugins/my-skills/skills/blank-template/SKILL.md`](../../plugins/my-skills/skills/blank-template/SKILL.md)

설계 포인트:
- frontmatter `description` 에 "실제 작업에는 자동으로 사용되지 않습니다 (Internal template — do not auto-trigger)" 명시 → 모델이 트리거하지 않음
- 본문에 안내용 인용 블록(`>`)으로 사용 절차 6단계 명시
- 모든 섹션(개요·규칙·출력 형식·엣지 케이스·예시)에 `(TODO)` 마커 배치
- 사용자가 새 스킬을 만들 때 폴더 복사 → frontmatter 교체 → TODO 채움 → 인용 블록 삭제 순으로 진행

### 2-6. README.md 전면 보강

대상: [`README.md`](../../README.md)

기존 14줄 → 폴더 구조 다이어그램, 스킬 표, 인터랙티브/로컬 개발 두 가지 설치 방법, 자연어 사용 예시, 새 스킬 추가 6단계 절차로 확장.

## 검증

```
node -e "JSON.parse(fs.readFileSync('marketplace.json'))"   # OK
node -e "JSON.parse(fs.readFileSync('plugin.json'))"        # OK
ReadLints                                                   # 0 errors
```

- `marketplace.json` 의 `source: ./plugins/my-skills` 경로에 `.claude-plugin/plugin.json` 실재
- 양쪽 SKILL.md 모두 frontmatter `name`, `description` 정상 포함

## 변경된 파일

| 종류 | 경로 |
| --- | --- |
| 삭제 | `my-plugins/my-skills/` (전체) |
| 수정 | `.claude-plugin/marketplace.json` |
| 수정 | `plugins/my-skills/.claude-plugin/plugin.json` |
| 수정 | `plugins/my-skills/skills/emoji-summarizer/SKILL.md` |
| 수정 | `README.md` |
| 신규 | `plugins/my-skills/skills/blank-template/SKILL.md` |

## 학습 포인트

- **Claude Code 플러그인은 두 단계 매니페스트** (마켓플레이스 → 플러그인). 단일 파일이 아니라 `marketplace.json` → `plugin.json` 의 흐름을 정확히 따라가야 함.
- **`source` 경로가 단일 진실의 원천**. 중복 폴더가 있을 때 어느 쪽이 정식인지 가르는 기준은 항상 `marketplace.json` 의 `source`.
- **공식 스키마는 자주 바뀐다.** 작업 전 `https://docs.claude.com/en/docs/claude-code/plugin-marketplaces` 와 `.../plugins-reference` 를 직접 확인하는 것이 안전.
- **`description` 이 자동 호출의 핵심**. 트리거 키워드를 직접 인용 부호로 명시하면 매칭률이 크게 올라감.

## 다음 단계

[STEP 2 — 여러 도구에서 사용하기](02-multi-tool-distribution.md)
