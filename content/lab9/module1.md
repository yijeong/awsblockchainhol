---
title: 리소스 정리 
weight: 1
pre: "<b>8.1 </b>"
---

1. S3 버킷을 삭제합니다. 

- AWS Management Console에서 **S3** 서비스로 [이동](https://console.aws.amazon.com/s3) 합니다. 

- **“beer-s3-setting-“**으로 시작되는 Bucket을 선택하고 [Delete]를 클릭합니다. 

- 확인을 위해 버킷이름을 기입하고 [Confirm]을 클릭합니다.

![delete](/lab9/images/delete_1.png)

2. CloudFormation 스택을 삭제합니다. 

- AWS Management Console에서 **CloudFormation** 서비스로 [이동](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks?filter=active) 합니다. 

- **“beer-s3-setting”** > **“beer-env-setting”** 순으로 stack을 삭제합니다. 

3. Fabric 네트워크를 삭제합니다. 

- AWS Management Console에서 **Amazon Managed Blockchain** 서비스로 [이동](https://console.aws.amazon.com/managedblockchain/home?region=us-east-1#networks) 합니다. 

- Networks에서 **“beer”**를 클릭합니다. 

- 상단에서 Member 탭을 클릭합니다. 

- Member owned by you에서 **“org1”** 을 선택하고 Delete member합니다. 이전에 초대했던 org2도 해당 AWS account에서 삭제가 되어야 정상적으로 Fabric 네트워크가 삭제됩니다. 

4. Cloud9 의 IDE를 삭제합니다. 

- AWS Management Console에서AWS **Cloud9** 서비스로 [이동](https://us-east-1.console.aws.amazon.com/cloud9/home?region=us-east-1) 합니다. 

- Your environments에서 **“beer-IDE”**를 선택하고 Delete 합니다. 


5. KeyPair를 삭제합니다. 

- AWS Management Console에서AWS **EC2 > Keypair** 로 [이동](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#KeyPairs:sort=keyName) 합니다.

- **“beer-keypair”** 를 선택하고 Delete를 클릭합니다. 



수고하셨습니다.


---
© 2020 Amazon Web Services, Inc. 또는 자회사, All rights reserved.
