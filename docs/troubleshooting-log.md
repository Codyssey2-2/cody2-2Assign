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

## 3. `git revert`

### 참여자

- 실행: 오철호 (Member 2)
- 확인: 오철호 (Member 2)

### 상황 / 재현 절차

`feature/21-cheolho-git-revert` 브랜치에서 `team/member-2.md`의 "한마디" 문구에 오타(`중요핟.`)를 낸 커밋을 만들어 원격에 push했습니다. 이미 원격에 공유된 커밋이라 `reset`이나 강제 push 대신, 새 취소 커밋을 만드는 `git revert`로 되돌렸습니다.

### 명령과 결과

```bash
$ git add team/member-2.md
$ git commit -m "docs: update member-2 comment"
[feature/21-cheolho-git-revert dd40cfa] docs: update member-2 comment
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git push -u origin feature/21-cheolho-git-revert
 * [new branch]      feature/21-cheolho-git-revert -> feature/21-cheolho-git-revert

$ git revert --no-edit dd40cfac8be511a8d5b2e46de8d3def1d0fc2c53
[feature/21-cheolho-git-revert 35fd318] Revert "docs: update member-2 comment"
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git push origin feature/21-cheolho-git-revert
   dd40cfa..35fd318  feature/21-cheolho-git-revert -> feature/21-cheolho-git-revert
```

### 왜 이 방법을 선택했는가

잘못된 커밋(`dd40cfa`)이 이미 원격 브랜치에 push되어 다른 사람이 내려받았을 수 있으므로, 히스토리를 지우는 `reset --hard`나 강제 push 대신 새로운 취소 커밋을 추가하는 `git revert`를 사용했습니다. 이렇게 하면 원본 실수와 수정 이력이 모두 남아 추적이 가능하고, 공유 브랜치의 히스토리를 깨뜨리지 않습니다.

## 4. `git stash` / `git stash pop` — 작성 예정

### 참여자 / 상황 / 명령 / 결과 / 선택 이유

```bash
git stash push -m "wip: member profile draft"
git switch <other-branch>
git switch <original-branch>
git stash pop
```
