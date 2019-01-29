### Ubuntu에서 vscode 권한문제로 저장실패
우분투에서 `vscode`를 사용하는데, 최초 저장마다 
`Failed to save 'filename':Insufficient permissions. Select 'Retry as Admin' to retry as administrator.`
라는 에러 팝업이 나타났다. 

### 해결법
```bash bash
sudo chmod -R 777 <project_dir_name>
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA1NjU5NDkyXX0=
-->