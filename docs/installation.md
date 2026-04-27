# 설치 가이드

`my-plugins` 마켓플레이스를 Claude Code 에 추가하고 `my-skills` 플러그인을 설치하는 방법을 설명합니다.

## 사전 요구사항

- [Claude Code](https://docs.claude.com/en/docs/claude-code) 가 설치되어 있고 인증이 완료된 상태.
- `/plugin` 명령이 보이지 않는다면 Claude Code 를 최신 버전으로 업데이트하세요.

## 1) 로컬 경로로 설치 (현재 PC에서 바로 사용)

Claude Code 세션 안에서 다음 슬래시 명령을 실행합니다.

```
/plugin marketplace add E:\apps\claude\my-plugins
/plugin install my-skills@my-plugins
```

설치되면 `emoji-summarizer`, `blank-template` 두 스킬이 사용 가능 상태가 됩니다.

## 2) GitHub 저장소로 설치 (다른 PC · 팀 공유)

이 저장소를 GitHub 에 올렸다면 어떤 PC에서든 한 줄로 설치할 수 있습니다.

```
/plugin marketplace add <github-username>/my-plugins
/plugin install my-skills@my-plugins
```

Private 저장소면 사전에 `gh auth login` 으로 GitHub 인증을 마쳐 두세요.

## 3) 로컬 개발 모드 (마켓플레이스 등록 없이 즉시 테스트)

매니페스트나 `SKILL.md` 를 수정하면서 빠르게 확인할 때는 `--plugin-dir` 플래그가 가장 편리합니다.

```bash
claude --plugin-dir ./plugins/my-skills
```

세션 도중 변경 사항을 다시 불러오려면 슬래시 명령으로 재로딩합니다.

```
/reload-plugins
```

## 업데이트

마켓플레이스 카탈로그(`marketplace.json`) 또는 플러그인 내부(`plugin.json`, 각 `SKILL.md`) 를 수정한 뒤에는 다음 중 하나를 사용합니다.

| 변경 위치 | 명령 |
| --- | --- |
| 스킬 본문 / 매니페스트 (현재 세션에 즉시 반영) | `/reload-plugins` |
| 마켓플레이스 카탈로그 변경 (버전 업, 새 플러그인 추가) | `/plugin marketplace update my-plugins` |

`plugin.json` 의 `version` 을 올렸을 때만 사용자에게 업데이트가 통지됩니다 (semver 권장).

## 제거

설치된 플러그인 / 마켓플레이스를 제거하려면:

```
/plugin uninstall my-skills@my-plugins
/plugin marketplace remove my-plugins
```

## 동작 확인

설치 후 다음을 확인하세요.

- `/help` 또는 `/plugin` 출력에 `my-skills` 가 표시되는지
- 자연어로 "이모지로 요약해줘" 같은 표현을 썼을 때 `emoji-summarizer` 스킬이 호출되는지
- `blank-template` 은 자동 트리거되지 않아야 정상입니다 (의도적으로 비활성 트리거)

문제가 생기면 [architecture.md](architecture.md) 의 폴더 구조와 매니페스트 필드를 다시 확인하세요.
