# blank-template

새 스킬을 만들 때 **복사해서 시작하는 빈 템플릿**입니다. 실제 작업에는 자동으로 사용되지 않습니다.

- 소스: [`plugins/my-skills/skills/blank-template/SKILL.md`](../../plugins/my-skills/skills/blank-template/SKILL.md)
- 자동 트리거: 아니오 (의도적 비활성)

## 왜 이 템플릿이 필요한가

스킬을 처음부터 매번 빈 파일에서 시작하면 다음을 자주 빠뜨리게 됩니다.

- frontmatter `name` / `description`
- 출력 형식 / 엣지 케이스 / 예시 섹션
- 안내용 인용 블록 삭제

`blank-template` 은 이 모든 체크포인트가 미리 박혀 있는 골격을 제공해, 새 스킬을 만들 때 빠짐없이 작성하도록 돕습니다.

## 자동 트리거가 안 되도록 설계된 이유

`description` 첫 줄에 **"실제 작업에는 자동으로 사용되지 않습니다"** 라고 명시되어 있고, 영문 안내(`Internal template — do not auto-trigger`)도 함께 들어 있습니다. 모델은 이 description 을 보고 일반 사용자 입력에서는 호출하지 않습니다.

더 강력하게 차단하려면 frontmatter 에 다음을 추가할 수 있습니다.

```yaml
---
name: blank-template
description: ...
disable-model-invocation: true
---
```

## 사용 절차 (요약)

자세한 단계와 체크리스트는 [docs/contributing.md](../contributing.md) 를 참고하세요.

```bash
cd plugins/my-skills/skills
cp -r blank-template <new-skill-name>
```

1. 폴더명을 새 스킬 이름(kebab-case)으로 변경
2. `SKILL.md` 의 frontmatter (`name`, `description`) 다시 작성
3. 본문의 `(TODO)` 표시 모두 채우기
4. 안내용 인용 블록(`>`) 통째 삭제
5. `plugin.json` / `marketplace.json` 의 `version` 올림
6. `/reload-plugins` 후 자동·슬래시 호출 확인
7. [docs/skills.md](../skills.md) 의 표에 새 줄 추가

## 템플릿 골격 한눈에 보기

`blank-template/SKILL.md` 가 제공하는 섹션은 다음과 같습니다.

| 섹션 | 무엇을 채우나 |
| --- | --- |
| frontmatter `name` | 폴더명과 동일한 kebab-case 이름 |
| frontmatter `description` | 한 문장 요약 + 트리거 키워드 + 사용 시나리오 |
| 개요 | 스킬이 무엇을 하는지 1~2문장 |
| 규칙 | 출력 길이 / 톤 / 금지 사항 |
| 출력 형식 | 사용자가 받게 될 결과물 형태 (코드펜스 사용 주의) |
| 엣지 케이스 | 빈 입력 / 모호한 입력 / 민감 정보 / 범위 외 요청 |
| 예시 | 최소 2개 이상, 다양한 톤 / 길이 |

## 흔한 실수

| 실수 | 결과 |
| --- | --- |
| 안내용 인용 블록(`>`)을 지우지 않음 | 모델이 그 내용까지 응답에 섞어 보냄 |
| frontmatter `name` 을 그대로 `blank-template` 으로 둠 | 새 스킬이 등록되지 않거나 충돌 발생 |
| `(TODO)` 를 일부만 채움 | 트리거는 되지만 동작이 일관되지 않음 |
| 예시를 1개만 작성 | 모델이 출력 패턴을 일반화하지 못함 |

## 관련 문서

- [docs/contributing.md](../contributing.md) — 새 스킬 추가 전체 절차 / 체크리스트
- [docs/architecture.md](../architecture.md) — SKILL.md 구조 / 자동 호출 원리
- [docs/skills.md](../skills.md) — 현재 등록된 스킬 목록
