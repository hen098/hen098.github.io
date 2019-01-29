---
title: ENOSPC 에러
tags: ["programming"]
---
개발환경 : Ubuntu
`yarn serve`에 ENOSPC 관련 에러 발생

### 해결
```bash bash
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU3NTU3Mjc4OCwtNDgyMDQ4MzQyXX0=
-->