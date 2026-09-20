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

## 4. `git stash` / `git stash pop` — 작성 예정

### 참여자 / 상황 / 명령 / 결과 / 선택 이유

```bash
git stash push -m "wip: member profile draft"
git switch <other-branch>
git switch <original-branch>
git stash pop
```
