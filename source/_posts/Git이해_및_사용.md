---
title : "Git 이해 및 사용"
tags: ["Git"]
---

`.git` 파일에 git의 db를 가지고 있으므로 해당 폴더를 날리면 복원할 수 없다.

## Storage
- key-value 형태의 db 사용 
- 파일을 해쉬로 변경하여 저장
- CR 만 가능

## Commit
- meta 데이터 (이메일, 이름 등) 
- tree
- 부모 커밋만 알고, 자식 커밋은 모른다.

### staged 
수정 파일을 곧 커밋할 것이라고 표시한 상태 

어떤 작업을 하던 중 다른 브랜치로 잠시 이동해야 할 경우, 아직 완료하지 않은 일을 commit 하지 않고 브랜치를 이동하는 방법

- `git stash` 나 `git stash save`를 실행하면 스택에 새로운 stash가 만들어진다. working directory는 깨끗해진다. 
- 다른 브랜치로 이동 
- `git stash list` 여러번 stash를 했다면 목록 확인 
- stash 작업 가져오기 
	- `git stash apply` : 가장 최근의 stash를 가져와 적용
	- `git stash apply [stash 이름]` : 해당 stash를 가져와 적용
	- 복원할 땐 그전과 같은 브랜치일 필요는 없다. 

### reset
변경 이력 되돌리는 명령어 
- mixed : 변경 이력은 삭제하지만 변경 이력은 남아있다. unstage 상태 
- hard : git head를 옮기고 디렉토리 전체를 옮김 
- HEAD : 현재 위치 
- ORIG_HEAD : reset 전의 커밋 참조 

### rebase 
merge 시에 rebase merge를 하면, 합쳐진 이력이 보이지 않고 master에서 깔끔하게 작업한 것처럼 보인다.
```
> git rebase master dev
```
master와 dev 브랜치의 공통 조상 커밋부터 dev 브랜치까지 모든 커밋의 base를 master 브랜치의 위치로 바꾸어라.
