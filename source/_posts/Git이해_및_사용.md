---


---

<hr>
<h2 id="title--git-이해-및-사용tags-git">title : “Git 이해 및 사용”<br>
tags: [“Git”]</h2>
<p><code>.git</code> 파일에 git의 db를 가지고 있으므로 해당 폴더를 날리면 복원할 수 없다.</p>
<h2 id="storage">Storage</h2>
<ul>
<li>key-value 형태의 db 사용</li>
<li>파일을 해쉬로 변경하여 저장</li>
<li>CR 만 가능</li>
</ul>
<h2 id="commit">Commit</h2>
<ul>
<li>meta 데이터 (이메일, 이름 등)</li>
<li>tree</li>
<li>부모 커밋만 알고, 자식 커밋은 모른다.</li>
</ul>
<h3 id="staged">staged</h3>
<p>수정 파일을 곧 커밋할 것이라고 표시한 상태</p>
<p>어떤 작업을 하던 중 다른 브랜치로 잠시 이동해야 할 경우, 아직 완료하지 않은 일을 commit 하지 않고 브랜치를 이동하는 방법</p>
<ul>
<li><code>git stash</code> 나 <code>git stash save</code>를 실행하면 스택에 새로운 stash가 만들어진다. working directory는 깨끗해진다.</li>
<li>다른 브랜치로 이동</li>
<li><code>git stash list</code> 여러번 stash를 했다면 목록 확인</li>
<li>stash 작업 가져오기
<ul>
<li><code>git stash apply</code> : 가장 최근의 stash를 가져와 적용</li>
<li><code>git stash apply [stash 이름]</code> : 해당 stash를 가져와 적용</li>
<li>복원할 땐 그전과 같은 브랜치일 필요는 없다.</li>
</ul>
</li>
</ul>
<h3 id="reset">reset</h3>
<p>변경 이력 되돌리는 명령어</p>
<ul>
<li>mixed : 변경 이력은 삭제하지만 변경 이력은 남아있다. unstage 상태</li>
<li>hard : git head를 옮기고 디렉토리 전체를 옮김</li>
<li>HEAD : 현재 위치</li>
<li>ORIG_HEAD : reset 전의 커밋 참조</li>
</ul>
<h3 id="rebase">rebase</h3>
<p>merge 시에 rebase merge를 하면, 합쳐진 이력이 보이지 않고 master에서 깔끔하게 작업한 것처럼 보인다.</p>
<pre><code>&gt; git rebase master dev
</code></pre>
<p>master와 dev 브랜치의 공통 조상 커밋부터 dev 브랜치까지 모든 커밋의 base를 master 브랜치의 위치로 바꾸어라.</p>

