# Troubleshooting Log

> 네 가지 시나리오를 모두 실제로 재현하고, 참여자의 이름과 역할을 남깁니다. 공유 브랜치에는 `reset`이나 강제 push를 사용하지 않습니다.

## 1. `git commit --amend` — 작성 예정

### 참여자

- 실행: `<member>`
- 확인: `<reviewer>`

### 상황 / 재현 절차

최근 로컬 커밋의 메시지에 오타를 낸 뒤, 아직 push하지 않은 상태에서 수정합니다.

### 명령과 결과

```bash
git commit --amend -m "docs: correct team profile title"
git log -1 --oneline
```

### 왜 이 방법을 선택했는가

아직 공유하지 않은 가장 최근 커밋의 메시지만 바꾸므로 amend가 적합합니다. 이미 push한 공유 커밋은 reword 대신 후속 커밋 또는 합의된 절차를 사용합니다.

## 2. `git reset --soft HEAD~1` — 작성 예정

### 참여자 / 상황 / 명령 / 결과 / 선택 이유

```bash
# push하지 않은 로컬 커밋만 대상으로 실행
git reset --soft HEAD~1
git status
```

## 3. `git revert` — 작성 예정

### 참여자 / 상황 / 명령 / 결과 / 선택 이유

```bash
# 원격에 공유된 잘못된 커밋을 새 취소 커밋으로 되돌림
git revert <commit-sha>
git push origin <feature-branch>
```

## 4. `git stash` / `git stash pop` — 완료

### 참여자

- 실행: `김수정 (Member 3)`
- 관련 Issue: `#20`

### 상황 / 재현 절차

`feature/20-stash-troubleshooting` 브랜치에서 `team/member-3.md`를 수정하던 중, 커밋하지 않은 작업을 유지한 채 다른 브랜치로 전환해야 하는 상황을 재현했습니다.

### 명령과 결과

```bash
git stash push -m "stash practice member 3"
git status
git stash list

git switch main
git switch feature/20-stash-troubleshooting

git stash pop
git status
```

`git stash` 실행 후 working tree가 clean 상태가 되었고, `git stash list`에서 임시 보관된 작업을 확인했습니다.

`main` 브랜치로 전환한 뒤 다시 작업 브랜치로 돌아와 `git stash pop`을 실행하자 `team/member-3.md`의 수정사항이 정상적으로 복원되었습니다.

### 왜 이 방법을 선택했는가

아직 커밋하지 않은 작업을 잃지 않고 잠시 보관한 상태에서 다른 브랜치로 전환하기 위해 `git stash`를 사용했습니다. 이후 원래 작업 브랜치에서 `git stash pop`을 사용해 보관했던 변경사항을 다시 복원했습니다.