---
title: Fabric Client 구성 
weight: 2
pre: "<b>3.2 </b>"
---

## CloudFormation 스택 삭제 

1. 다시 **AWS Cloud9**의 터미널로 이동합니다. (**“beer-IDE”**)

2. 앞서 생성한 Fabric Client에 접속합니다. *<your EC2 Endpoint>*는 앞서 생성한 스택의 **Outputs**에서 **EC2URL**을 참고합니다. 
```
cd ~
ssh ec2-user@<your EC2 Endpoint> -i ~/beer-keypair.pem
```

3. Fabric Client에서 소스를 내려 받습니다.
```
git clone https://github.com/awslego/blockchain-supplychain.git
```

4. Fabric Network에 필요한 환경 변수 값을 수정하고 반영합니다. 
```
cd ~/blockchain-supplychain/beer-supplychain-fabric-client/
vi fabric-export.sh
```

```

  5 export REGION=us-east-1
  6 export NETWORKNAME=beer
  7 export MEMBERNAME=org1
  8 export NETWORKVERSION=1.2
  9 export ADMINUSER=admin1
  10 export ADMINPWD=Passwd123
  11 export NETWORKID=<your network ID, from the AWS Console>
  12 export MEMBERID=<your member ID, from the AWS Console>

```

{{% notice warning %}}
5~10라인의 정보도 앞서 생성한 것과 다르다면 수정을 해주어야 합니다. 
{{% /notice %}}

```
source fabric-export.sh
```

{{% notice warning %}}
ssh 세션을 재연결할 경우 다시 source 해주어야 합니다. 
{{% /notice %}}

5. 명령어 수행으로 마지막에 다음과 같은 값들이 출력됩니다. 다음 과정들을 위해 메모장에 기록해둡니다. 

![client](/lab4/images/client_2.png)

6. **fabric-export.sh** 실행으로 Peer node에 필요한 환경 변수를 위한 실행 파일도 생성이 되었습니다. 이 파일을 실행하여 적용합니다. 
```
echo $PEERSERVICEENDPOINT
cat ~/peer-exports.sh  
source ~/peer-exports.sh 
```
 
7. 앞서 **모듈 2**에서 Fabric Network 생성 시 인증기관(CA)를 구성 했습니다. 인증을 위한 퍼블릭 키를 복사해 온 뒤 관리자 아이디(bootstrap identity)를 등록합니다.  
```
aws s3 cp s3://us-east-1.managedblockchain/etc/managedblockchain-tls-chain.pem  /home/ec2-user/managedblockchain-tls-chain.pem

fabric-ca-client enroll -u https://$ADMINUSER:$ADMINPWD@$CASERVICEENDPOINT --tls.certfiles /home/ec2-user/managedblockchain-tls-chain.pem -M /home/ec2-user/admin-msp 
```

{{% notice info %}}
Fabric에서 MSP (Membership Service Provider)는 트러스트 도메인의 멤버를 정의하기 위해 루트 CA와 중간 CA를 식별합니다. **fabric-ca-client** enroll을 수행하면 관리자의 MSP에 대한 인증서가 /home/ec2-user/admin-msp ($FABRIC_CA_CLIENT_HOME)에 생성됩니다. 
{{% /notice %}}

8. 다음과 같이 결과가 나오면 정상입니다. 

```
2019/05/17 06:25:44 [INFO] Created a default configuration file at /home/ec2-user/.fabric-ca-client/fabric-ca-client-config.yaml
2019/05/17 06:25:44 [INFO] TLS Enabled
2019/05/17 06:25:44 [INFO] generating key: &{A:ecdsa S:256}
2019/05/17 06:25:44 [INFO] encoded CSR
2019/05/17 06:25:45 [INFO] Stored client certificate at /home/ec2-user/admin-msp/signcerts/cert.pem
2019/05/17 06:25:45 [INFO] Stored root CA certificate at /home/ec2-user/admin-msp/cacerts/ca-m-ce3ycgwodfg4dit6ne47p2k5g4-n-4erqhktvvjeudn2wlwf5eziiuq-managedblockchain-us-east-1-amazonaws-com-30002.pem
```

9. 생성된MSP는 멤버 관리자 용이므로 signcerts에서 admincerts로 인증서를 복사합니다.
```
cp -r admin-msp/signcerts admin-msp/admincerts
```


---
© 2020 Amazon Web Services, Inc. 또는 자회사, All rights reserved.
