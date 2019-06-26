---
title: "브라우저에서 AWS S3 다운로드 url추출"
tags: ["s3"]
---
파일 업로드는 서버에서 (트랜잭션처리)
파일 다운로드는 브라우저에서 구현하기로 했다. 

```javascript
import  AWS  from  'aws-sdk'

const  config  = {
accessKeyId: 'AWS_ACCESS_KEY_ID',
secretAccessKey: 'AWS_SECRET_ACCESS_KEY',
region: 'AWS_REGION'
}

const  AWSS3  =  new  AWS.S3(config)

export const s3 = {
	down (fileName, callback) {
		AWSS3.getSignedUrl('getObject', {
		Key: fileName,
		Expires: 60	// URL 만료시간
		}, (err, url) => {
			callback(err, url)
		})
	}	
}
```

기존에 사용했던 `getObject()` 메소드를 사용하면 반환값이 object로 온다.
`getSinedUrl()`메소드를 사용하면, 업로드시에 `private`으로 업로드했어도 접근할수 있는 url이 생성된다. 

참고자료 
AWS S3 (pre siging url) : 
https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getSignedUrl-property 

<!--stackedit_data:
eyJoaXN0b3J5IjpbNjI0MDkwOTYyXX0=
-->