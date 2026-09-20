# Conflict Resolution Log

> 실제 충돌을 해결한 직후 아래 형식을 복사해 기록합니다. 최소 2건을 남기며, 그중 1건은 같은 파일의 같은 hunk를 서로 다르게 수정한 비자명 충돌이어야 합니다.

## 충돌 기록 #1

### 참여자

- 작성자: 오철호 (Member 2)
- 상대: 김현중 (Member 1, PR #5)

### 상황

- 브랜치 / 파일: `feature/8-cheolho-readme` / `README.md`
- `README.md`의 Member 2 표기(`<이름 / 역할>` → `오철호 / 팀원`)를 수정해 PR #9를 올린 뒤, 같은 파일의 바로 위 줄(Member 1)을 수정한 김현중의 PR #5가 먼저 `main`에 merge됨. 이후 `feature/8-cheolho-readme`를 `origin/main`에 rebase하는 과정에서 인접 줄 수정이 겹쳐 충돌 발생.

### 충돌 내용

```text
<<<<<<< HEAD
- [김현중](team/member-1.md) — 팀 소개 문서 및 협업 규칙 담당
- [Member 2](team/member-2.md) — `<이름 / 역할>`
=======
- [Member 1](team/member-1.md) — `<이름 / 역할>`
- [Member 2](team/member-2.md) — 오철호 / 팀원
>>>>>>> 5572f6d (docs: update member-2 label in readme)
```

### 해결 과정

```bash
git fetch origin
git rebase origin/main
# README.md 충돌 마커를 열어 Member 1(김현중 변경분)과 Member 2(오철호 변경분)를 모두 반영
git add README.md
git rebase --continue
git push --force-with-lease origin feature/8-cheolho-readme
```

- 선택한 해결 전략과 이유: 두 변경 모두 같은 목록의 서로 다른 항목을 갱신한 것이라 한쪽을 버릴 이유가 없었음. 따라서 두 줄을 모두 남기는 방향으로 병합함(김현중 줄 + 오철호 줄).

### 결과와 배운 점

- 관련 PR / 커밋: PR #9, 커밋 `a8bba2f`
- 최종 결과: `README.md`에 두 팀원의 최신 소개 표기가 모두 반영된 상태로 rebase 완료 후 push.
- 예방 방법: 여러 팀원이 같은 목록형 파일(`README.md`)의 항목을 동시에 갱신할 때는 작업 전 `git fetch && git rebase origin/main`으로 최신 상태를 자주 반영하면 충돌을 더 빨리, 더 작은 범위에서 발견할 수 있음.

## 충돌 기록 #2 — 작성 예정

위 형식을 복사해 두 번째 충돌과 해결 기록을 추가합니다.

## 충돌 기록 #2 — 작성 예정

위 형식을 복사해 두 번째 충돌과 해결 기록을 추가합니다.
