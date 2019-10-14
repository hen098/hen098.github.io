---


---

<h2 id="title--jpa---where-어노테이션tags-jpa">–<br>
title : “JPA - @Where 어노테이션”<br>
tags: [“jpa”]</h2>
<h2 id="where-어노테이션"><code>@Where</code> 어노테이션</h2>
<p>테이블 정의부분에 정의하면 쿼리메소드를 사용할 때 기본으로 where조건으로 들어간다.</p>
<h3 id="사용">사용</h3>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@Entity</span>
<span class="token annotation punctuation">@Table</span><span class="token punctuation">(</span>name <span class="token operator">=</span> <span class="token string">"USER"</span><span class="token punctuation">)</span>
<span class="token annotation punctuation">@NoArgsConstructor</span><span class="token punctuation">(</span>access <span class="token operator">=</span> AccessLevel<span class="token punctuation">.</span>PROTECTED<span class="token punctuation">)</span>
<span class="token annotation punctuation">@Where</span><span class="token punctuation">(</span>clause <span class="token operator">=</span> <span class="token string">"STATUS &lt;&gt; 'WITHDRAW'"</span><span class="token punctuation">)</span>
<span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">User</span> <span class="token keyword">extends</span> <span class="token class-name">AbstractBaseEntity</span> <span class="token punctuation">{</span>
	<span class="token punctuation">.</span><span class="token punctuation">.</span>	
<span class="token punctuation">}</span>
</code></pre>

