---
title: S3 Web hosting 구성하기
weight: 1
pre: "<b>5.1 </b>"
---

1. AWS Cloud9의 터미널로 이동합니다. (**“beer-IDE”**)

{{% notice warning %}}
Fabric Client 에 접속되어 있어야 합니다. 
{{% /notice %}}

2. Web UI의 javascript 소스에서 **ELB endpoint** 정보를 업데이트합니다. 
```
cd ~/blockchain-supplychain/beer-supplychain-ui/src/assets/js/
vi demo.js
```

```
//customizing variables. 
var username = " " // insert your user name 
var OrderEndpoint = "http://[your ELB endpoint]" // insert your ELB endpoint
```

- username은 앞서 swagger에서 생성한 이름입니다. 이 값은 HTTP 요청 헤더에 추가되어 username을 생성한 사용자만 요청할 수 있도록 제한합니다.  

- ELB endpoint정보는 앞서 생성한 CloudFormation 스택 (“beer-env-setting”)의 Outputs 탭에서 확인할 수 있습니다. 다음과 유사한 값입니다. 

```
beer-env-Blockcha-1T1EEE1JN9X7O-283923670.us-east-1.elb.amazonaws.com	
```

3. S3 정적 웹 호스팅환경 구성을 위해CloudFormation 스택을 생성합니다. 
```
cd ~/blockchain-supplychain/beer-supplychain-ui
aws cloudformation deploy --stack-name $NETWORKNAME-s3-setting --template-file setting-webui.yaml --region $REGION
```

4. Stack 생성이 완료되는 데는 약 2-3분이 소요됩니다. 기다리는 동안 무슨 작업을 하는지 살펴보겠습니다. CloudFormation은 S3정적 웹사이트를 구성하며, 다음과 같은 자원이 포함되어있습니다. 

- S3
-	S3 bucket policy 

{{% notice info %}}
S3 의 정적 웹 사이트 호스팅에 대해 더 궁금하신 분들은 링크를 참고해주시기 바랍니다. 
https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/user-guide/static-website-hosting.html
{{% /notice %}}

5. 스택 배포가 완료되면, 생성된 S3 bucket에 소스를 복사합니다. 
```
cd src 
aws s3 cp . s3://$(aws s3 ls | grep beer-s3-setting | awk -F ' ' '{print $3}')/ --recursive             
aws s3 ls s3://$(aws s3 ls | grep beer-s3-setting | awk -F ' ' '{print $3}')/ --recursive                                                                                        
```

6. 다음과 같이 src 밑의 web UI 소스들이 버킷으로 업로드 되면 정상입니다. 

```
upload: assets/img/Amazon-Managed-Blockchain.png to s3://beer-s3-setting-s3bucket-1lat1jcha9r3j/assets/img/Amazon-Managed-Blockchain.png
upload: ./index-retailer.html to s3://beer-s3-setting-s3bucket-1lat1jcha9r3j/index-retailer.html
upload: assets/js/jquery.validate.min.js to s3://beer-s3-setting-s3bucket-1lat1jcha9r3j/assets/js/jquery.validate.min.js
upload: ./index-truck.html to s3://beer-s3-setting-s3bucket-1lat1jcha9r3j/index-truck.html
upload: ./index-device.html to s3://beer-s3-setting-s3bucket-1lat1jcha9r3j/index-device.html
……
```

7. 브라우저를 통해 web UI에 접근하여 정상적으로 뜨는지 확인합니다. 웹사이트 URL정보는 앞서 생성한 CloudFormation 스택 (“beer-s3-setting”)의 Outputs 탭에서 확인할 수 있습니다. 

![client](/lab6/images/s3_1.png)

8. Web UI의 기본 index 파일은 Manufacturer APP으로 설정해놓았습니다.  각 멤버의 APP에 들어가기 위해서는 다음과 같이 URL을 입력합니다. 

- Manufacturer app : [WebsiteURL]
- Shipping Company app : [WebsiteURL]/index-truck.html
- Retailer app : [WebsiteURL]/index-retailer.html



---
© 2020 Amazon Web Services, Inc. 또는 자회사, All rights reserved.
