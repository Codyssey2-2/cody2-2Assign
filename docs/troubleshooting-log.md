# Troubleshooting Log

> 네 가지 시나리오를 모두 실제로 재현하고, 참여자의 이름과 역할을 남깁니다. 공유 브랜치에는 `reset`이나 강제 push를 사용하지 않습니다.

## 1. `git commit --amend` — 완료

### 참여자

- 실행: 김현중 (Member 1)
- 확인: 김현중 — `git log -1 --oneline`으로 결과 확인

### 상황 / 재현 절차

실습 브랜치에서 임시 파일 `docs/.git-practice-temp.md`를 추가하고, 최근 로컬 커밋을 의도적으로 모호한 메시지로 만들었습니다. 이 커밋은 원격에 push하지 않았습니다.

### 명령과 결과

```bash
git add docs/.git-practice-temp.md
git commit -m "docs: temp git practice"
git commit --amend -m "docs: amend practice commit message"
git log -1 --oneline
```

처음 커밋 `f06c525`의 메시지가 `252535f docs: amend practice commit message`로 바뀐 것을 확인했습니다. amend는 새 커밋을 만들어 최근 커밋을 대체하므로 커밋 SHA도 변경됩니다.

### 왜 이 방법을 선택했는가

아직 공유하지 않은 가장 최근 커밋의 메시지만 바꾸는 상황이므로 amend가 적합합니다. 이미 push한 공유 커밋은 히스토리를 바꾸지 않도록 후속 커밋 또는 팀 합의 절차를 사용합니다.

## 2. `git reset --soft HEAD~1` — 완료

### 참여자

- 실행: 김현중 (Member 1)
- 확인: 김현중 — `git status --short`로 staging area 상태 확인

### 상황

amend한 임시 커밋을 원격에 공유하지 않은 상태에서 취소하되, 임시 파일 변경은 유지되는지 확인했습니다.

### 명령과 결과

```bash
git reset --soft HEAD~1
git status --short
```

HEAD가 이전 `4cf24e4`로 이동했고, `git status --short` 결과가 `A  docs/.git-practice-temp.md`로 표시되었습니다. 즉 커밋만 취소되고 파일 변경은 staging area에 유지됐습니다. 실습 후 `git restore --staged docs/.git-practice-temp.md`로 staging에서 제외하고 임시 파일을 삭제했습니다.

### 왜 이 방법을 선택했는가

커밋 단위를 다시 구성하고 싶지만 작업 내용은 보존해야 하는 로컬·미공유 상황이므로 `--soft`를 선택했습니다. 원격에 공유된 커밋은 팀원의 히스토리에 영향을 주지 않도록 `reset` 대신 `git revert`를 사용합니다.

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
