---
title: RESTful API서버 구성
weight: 1
pre: "<b>4.1 </b>"
---

1. Fabric Client에 접속해 있는 터미널에서 Node.js를 설치합니다. 

```
cd ~
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash
. ~/.nvm/nvm.sh 
nvm install lts/carbon
sudo yum install gcc-c++ -y
```

2. package.json에 명시되어있는 의존 패키지들을 설치합니다. 
```
cd ~/blockchain-supplychain/beer-supplychain-rest-api/ 
npm install
```

{{% notice info %}}
npm install 명령어를 사용하면 package.json에 명시된 모든 의존 패키지를 한번에 설치할 수 있습니다.
{{% /notice %}}

3. REST API를Fabric 네트워크에 연결하기 위한 connection profile을 생성합니다. connection profile에는 Fabric네트워크에 대한 설명이 포함됩니다.
```
cd ~/blockchain-supplychain/beer-supplychain-rest-api/connection-profile/
./gen-connection-profile.sh
```

4. 마지막 명령어 수행 결과로 다음과 같이 출력되면 정상입니다. 

```
/home/ec2-user/blockchain-supplychain/tmp/connection-profile:
total 8
-rw-rw-r-- 1 ec2-user ec2-user 2302 May 15 06:09 beer-supplychain-connection-profile.yaml
drwxrwxr-x 2 ec2-user ec2-user 4096 May 15 06:09 org1

/home/ec2-user/blockchain-supplychain/tmp/connection-profile/org1:
total 4
-rw-rw-r-- 1 ec2-user ec2-user 817 May 15 06:09 client-org1.yaml
```

5. 3번 수행 결과로 생성된 connection-profile 을 확인합니다. 
```
more ~/blockchain-supplychain/tmp/connection-profile/beer-supplychain-connection-profile.yaml
```

{{% notice info %}}
모듈 2 에서 만든 Fabric 네트워크와 다른 정보가 있다면 수정 해주어야 합니다. (ex. Network name, CA registrar enrollID, ecrollSecret 등) 
{{% /notice %}}

6. app.js에서 사용하는 구성파일인 config.json 도 확인합니다. 
```
cd ~/blockchain-supplychain/beer-supplychain-rest-api
vi config.json
```

{{% notice warning %}}
** config.json의 피어 이름( 'peers :' 아래)이 connection profile의 피어 이름과 동일한 지 확인하십시오. 또한 admin 아이디와 secret이 올바른지 확인하고 연결 프로필에서 업데이트 한 값과 일치하는지 확인합니다. 본 Lab의 가이드를 그대로 따라왔다면 수정할 것이 없습니다. 
{{% /notice %}}

7. REST API를 실행합니다. 
```
nvm use lts/carbon
node app.js > temp.log & 
ps -ef | grep app.js
```

8. app.js에는 swagger구성에 대한 설정이 들어있습니다. 따라서 생성된 API 명세서를 swagger로 확인할 수 있습니다. 

** Swagger는 간단한 설정으로 프로젝트에서 지정한 URL들을 HTML화면으로 확인할 수 있게 해주는 프로젝트입니다.  

- CloudFormation으로 생성한 ELB endpoint 를 확인합니다. 스택의 **Outputs**에서 **ELBDNS**를 확인합니다. 

![client](/lab5/images/rest_1.png)

- 브라우저 주소창에 **<ELBDNS>/api-docs** 를 입력합니다. (예: http://amblab-en-blockcha-xxxxxxxxxxx.us-east-1.elb.amazonaws.com/api-docs/)

- Swagger 화면이 나오는 것을 확인합니다. 

![client](/lab5/images/rest_2.png)

- 우리는 CloudFormation 스택을 통해 Fabric Client용 EC2와 그 EC2를 target 으로 갖는 ELB를 생성했습니다. 해당 ELB의 port configuration은 **80 (TCP) forwarding to 3000(TCP)**입니다. 따라서 80포트로 받은 요청을 실제 node.js application이 listen하는 3000 포트로 포워딩하여 전달해줍니다. 
자세한 설정은 **EC2 콘솔 > Load Balancer > amblab-xxx..으로 시작하는 ELB선택 > Description**에서 확인해 볼 수 있습니다.

- Swagger에 명시된 API들과 앞서 배포한 Chaincode의 함수(app.js)의 내용과 일치하는 것을 확인합니다. 

9. Swagger를 통해 Fabric 네트워크의 원장을 쿼리하고 업데이트 할 사용자를 생성합니다. Swagger 상단의 **User** 밑에  **Post /user**를 클릭합니다. 

- 본 Lab의 모듈 2 에서 Fabric 네트워크를 생성할 때 첫번째 member(Organization)인 **“org1”**과 그에 속한 CA, 그리고 관리자인 **“admin1”**을 만들었습니다. User 신청(register)과 등록(enroll)은 admin의 cert를 이용해서 **“org1”** 의 새 사용자를 등록하는 과정입니다. 여기서 생성될 새로운 **“username”**은 원장을 쿼리하고 업데이트 할 때 사용하는 ID가 됩니다. 

![client](/lab5/images/rest_3.png)

10. 우측의  **Try it out** 을 클릭합니다. 

11. **Edit Value** 에서 username 과 orgName을 하기와 같이 수정합니다. 

```
{
  "username": "<your-name>",
  "orgName": "org1"
}
```
{{% notice warning %}}
Fabric 네트워크 생성 시 첫번째 member(Organization)을 **“org1”**으로 지정하신 것을 기억하실 겁니다. 따라서 orgName은 **“org1”**로 고정입니다. 혹시 Fabric 네트워크 구성시 다른 이름으로 생성하셨다면 그 이름을 쓰셔야 합니다. 
{{% /notice %}}

12. 하기의 **Execute** 를 클릭하여 수행합니다. Response에서 code가 200 이고 Details가 다음과 같으면 정상입니다. 

```
{
  "success": true,
  "secret": "",
  "message": "vanessa enrolled Successfully"
}
```

13. Swagger를 통해 트랜젝션이 정상적으로 생성되는지 테스트합니다. Swagger 하단의 **Transfer** 밑에  **Post /transfer/init** 를 클릭합니다. 

![client](/lab5/images/rest_6.png)

14. 우측의  **Try it out**  을 클릭합니다. <your-name>은 앞의 8.3에서 생성한 username 입니다.

15. **Parameter** 에서 **X-username**과 **X-orgName**을 하기와 같이 수정하여 HTTP Header 값을 추가합니다. 

```
    X-username: <your-name> 
    X-orgname: org1
```

16. 하기의 **Execute** 를 클릭하여 수행합니다. Response에서 code가 200 이고 Details가 다음과 같으면 정상입니다.

```
{
  "transactionId": "a6b324c177ae2fef78b74ddbf243395574c09909331ac2e2df5e4646af8b0e64"
}
```

17. 이제 다음과 같은 구성이 완성되었습니다.

![client](/lab5/images/rest_8.png)


---
© 2020 Amazon Web Services, Inc. 또는 자회사, All rights reserved.
