---
title: Fabric Client 환경 설치 
weight: 1
pre: "<b>3.1 </b>"
---

1. 모듈 1. 사전 준비 사항에서 설정한 **AWS Cloud9**의 터미널로 이동합니다. 

2. Terminal에서 다음과 같이 환경 변수 파일을 편집합니다. **NETWORKID**엔 Amazon Managed Blockchain 콘솔의 Networks에서 확인할 수 있는 **“beer”** 의 network ID를 입력합니다. 
```
cd ~/blockchain-supplychain
vi cloud9_env_setting.sh
```
```
export REGION=us-east-1 
export NETWORKID= #<your blockchain network ID>
export NETWORKNAME=beer
export VPCENDPOINTSERVICENAME=$(aws managedblockchain get-network --region $REGION --network-id $NETWORKID --query 'Network.VpcEndpointServiceName' --output text) 
echo $VPCENDPOINTSERVICENAME
```

3. 쉘스크립트를 실행하여 환경변수를 적용합니다. 
```
source cloud9_env_setting.sh
```

4. 마지막 명령어의 결과로 다음과 같은 형태의 endpoint가 발급되는지 확인합니다. 
```
com.amazonaws.us-east-1.managedblockchain.n-xxxxxxxxxxxxxxxxxxxxxxxxxxx
```

5. 이제 Fabric Client환경 구성을 위해CloudFormation 스택을 생성합니다. 
```
cd ~/blockchain-supplychain/beer-supplychain-fabric-client
sh env_setting.sh  
```

{{% notice warning %}}
처음에 **An error occurred (InvalidKeyPair.NotFound)** 에러가 나타날 수 있습니다. EC2 에 접속하기 위한Keypair가 없다는 것을 확인하고 새로 만드는 과정이니 정상적인 과정입니다. 
{{% /notice %}}

{{% notice warning %}}
**Keypair beer-keypair already exists.** 라는 결과가 나오면 EC2 콘솔 > Key Pairs 에서beer-keypair를 삭제하고 다시 수행해주시기 바랍니다.
{{% /notice %}}

6. Stack 생성이 완료되는 데는 약 5-6분이 소요됩니다. 기다리는 동안 무슨 작업을 하는지 살펴보겠습니다. CloudFormation은 Fabric Client 환경을 구성하며, 다음과 같은 자원들이 생성됩니다. 

- VPC
- VPC Endpoint 
- IAM Role
- Security Group
- EC2 (Fabric client) : Fabric CLI, docker, go 등이 설치되어있는 AMI로 생성 / Hyperledger에서 제공하는 fabric-samples 가 포함되어 테스트 가능 
- ELB 

7. 이제 다음과 같은 구성이 완성되었습니다. 

![client](/lab4/images/client_1.png)

7. AWS Management Console에서 **CloudFormation** 서비스로 [이동](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks?filter=active)합니다.
 
8. **“beer-env-setting”** 스택이 **CREATE_COMPLETE** 상태인 것을 확인합니다. 

9. **Outputs** 탭에서 어떤 자원들이 생성되었는지 확인합니다.  

![client](/lab4/images/client_2.png)






---
© 2020 Amazon Web Services, Inc. 또는 자회사, All rights reserved.
