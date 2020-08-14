---
title: 블록체인 네트워크 및 멤버 생성 
weight: 1
pre: "<b>2.1 </b>"
---

1. AWS Management Console에서 Amazon Managed Blockchain 서비스로 [이동](https://console.aws.amazon.com/managedblockchain/home?region=us-east-1#firstRun)합니다.

2. [Create a network] 를 클릭합니다.

![amb](/lab3/image/amb_1.png)

3. **Create blockchain network**에서 블록체인 네트워크를 생성합니다. Blockchain Framework에서 **Hyperledger Fabric 1.2** 을 선택하고 **network edition 은 **default** 로 둡니다.

{{% notice info %}}
네트워크 에디션은 네트워크의 속성을 결정합니다. 여기에는 member수, member 당 node 수 등이 포함됩니다.
{{% /notice %}}

![amb](/lab3/image/amb_2.png)

4. Network name and description 에서Network name 을 **“beer”** 으로 지정합니다. 

5. Voting policy는 그대로 두고 [Next]를 클릭합니다. 마지막 PART 6에서 member 를 추가하는 실습을 진행하며 설명을 하도록 하겠습니다. 

![amb](/lab3/image/amb_3.png)

6. Create Member 에서 블록체인 네트워크에 참가할 first member를 생성합니다. Member Configuration에서 Member name에 **“org1”**이라고 지정하고 Description에 적절한 설명을 넣어줍니다.

{{% notice info %}}
member (혹은 organization이라고도 합니다.)는 네트워크 내에서 고유한 정체성을 가지고 있으며 네트워크 당 최소 한 명의 member가 필요하므로 첫 번째 member를 설정한 뒤에 피어 노드를 추가할 수 있습니다. 
{{% /notice %}}

7. Hyperledger Fabric certificate authority (CA) configuration 에서 다음과 같이 지정합니다. 
	- Admin name: “admin1”
  	- Admin Password: “Passwd123”

8. [Next]를 선택합니다. 

9. Review 후, [Create network and member]를 선택합니다. 

10. Creating 에서 Available 상태까지는 약 15 ~ 20 분이 소요됩니다. 

![amb](/lab3/image/amb_4.png)



 
---
© 2020 Amazon Web Services, Inc. 또는 자회사, All rights reserved.
