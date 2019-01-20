---
title: hexo 블로그 배포 팁
tags: ["hexo"]
---

주의사항

### 에러 발생시 

에러 내용 > `ERROR Deployer not found:git`

```bash bash
$ npm install hexo-deployer-git --save
```

`hexo-deployer-git`을 설치하지 않으면 위와 같은 에러가 발생한다. 

## 브랜치 관리

#### _config.yml

```yaml yml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: your repository url
  branch: master
```

`master` 브랜치를 넣은 경우, 다른 브랜치에서 `hexo d -g` 로 배포해야한다. 

정확한 이유를 모르겠다. 

그래서, 리모트에 `source` 브랜치를 따로둬서 거기에 소스관리를 하고, 배포는 master에서만 하는걸로 정의했다.