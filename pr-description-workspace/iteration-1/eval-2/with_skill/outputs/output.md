## Summary

사용자 프로필 업데이트 API(`PATCH /users/{id}/profile`)를 추가합니다. 허용된 필드(`display_name`, `bio`, `avatar_url`)만 선택적으로 업데이트할 수 있도록 화이트리스트 방식을 적용해, `email` 등 민감한 필드가 외부에서 임의로 변경되는 보안 위험을 방지합니다.

## Changes

- **`src/api/users.py` — `UserAPI.update_profile` 메서드 추가**
  - `user_id`와 업데이트할 `data` dict를 받아 허용된 필드(`display_name`, `bio`, `avatar_url`)만 반영
  - 존재하지 않는 사용자 요청 시 `NotFoundException` raise
  - 허용되지 않은 필드는 묵시적으로 무시 (화이트리스트 필터링)

- **`tests/test_users.py` — 프로필 업데이트 테스트 2건 추가**
  - `test_update_profile_success`: 정상 필드 업데이트 후 응답값 검증
  - `test_update_profile_invalid_field`: 허용되지 않은 `email` 필드 전송 시 무시 여부 검증

## How to Test

1. 서버 실행: `uvicorn main:app --reload` (또는 기존 실행 방법)
2. 정상 업데이트 확인:
   ```bash
   curl -X PATCH http://localhost:8000/users/1/profile \
     -H "Content-Type: application/json" \
     -d '{"display_name": "New Name", "bio": "Hello"}'
   ```
   - 응답 200, `display_name`이 `"New Name"`으로 반영되는지 확인
3. 허용되지 않은 필드 무시 확인:
   ```bash
   curl -X PATCH http://localhost:8000/users/1/profile \
     -H "Content-Type: application/json" \
     -d '{"email": "hack@evil.com"}'
   ```
   - 응답 200이지만 `GET /users/1` 조회 시 `email`이 변경되지 않았는지 확인
4. 존재하지 않는 사용자 요청:
   ```bash
   curl -X PATCH http://localhost:8000/users/9999/profile \
     -d '{}'
   ```
   - 응답 404 확인
5. 테스트 전체 실행: `pytest tests/test_users.py -v`

## Notes for Reviewers

- 허용 필드(`allowed_fields`)가 현재 코드에 하드코딩되어 있습니다. 필드 목록이 확장될 경우 별도 상수 또는 스키마 레이어로 분리하는 것을 고려해주세요.
- `update_profile`은 존재하지 않는 필드를 조용히 무시하는 방식이므로, 클라이언트가 오타를 낸 경우 피드백을 받지 못합니다. 엄격한 검증이 필요하다면 알 수 없는 필드에 대해 400 응답을 반환하는 방향도 검토 부탁드립니다.
