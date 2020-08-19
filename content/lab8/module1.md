---
title: Blockchain network에 member추가하기 
weight: 1
pre: "<b>7.1 </b>"
---

1. AWS Management Console에서 **Amazon Managed Blockchain** 서비스로 이동합니다.

2. Networks에서 **“beer”** 를 클릭합니다.

3. 상위 tab에서 Proposals 를 선택합니다. 

4. Create proposals에서 Propose invitation을 선택합니다. 

![member](/lab8/images/member_1.png)

5. Proposal configuration에서 제안 주체로 앞서 생성한 **“org1”**을 선택합니다. 

6. 초대할 AWS account 번호를 입력하고 create 를 클릭합니다. 

![member](/lab8/images/member_2.png)

7.  Active 에서 방금 생성한 proposals을 확인할 수 있습니다. 

![member](/lab8/images/member_3.png)

8. 해당 proposal을 클릭합니다. 

9. vote as member 로 **“org1”**를 선택하고, Yes 를 클릭합니다. 

![member](/lab8/images/member_4.png)

![member](/lab8/images/member_5.png)

10. 초대를 받은 AWS Account의 **Amazon Managed Blockchain** 서비스로 이동합니다. 

11. **Invitations** 에서 앞서 6번에서 생성한 **invitation** 내역을 확인합니다. 

![member](/lab8/images/member_6.png)
![member](/lab8/images/member_7.png)

- 앞서 모듈 2에서 Fabric 네트워크를 구성할 때 **voting policy**를 default 값으로 설정한 것을 기억하실 겁니다. Default policy는 다음과 같습니다. 

![member](/lab8/images/member_8.png)

- Managed Blockchain 는 분산 네트워크이기 때문에 네트워크를 생성한 AWS 계정이나 다른 AWS 계정이 소유하지 않습니다. 
- 따라서 네트워크에 참가하는 각 member들은 네트워크를 변경하기 위해 다른 모든 member들에게 투표(vote)를 의뢰합니다. 
- 예를 들어 다른 AWS 계정을 네트워크에 가입 시키려면 기존 member가 이 계정을 초대할 proposal를 만들어 보내고 다른 member들은 찬성 또는 반대 투표를 합니다. 
- Proposal이 승인되어 초대장이 해당 AWS 계정으로 전송되면 해당 계정은 초대를 수락하고 네트워크에 가입 할 member을 만듭니다. 
- 다른 AWS 계정에 있는 member를 삭제하기 위해서도 비슷한 절차가 필요합니다. 
- 다만 충분한 권한을 가진 AWS 계정의 보안주체는 제안서를 제출하지 않고 언제든지 해당 계정의 회원을 삭제할 수 있습니다. 

12. **invitations** 에서 해당 network 이름 (**“beer”**) 을 선택하고 **Accept invitation**을 클릭합니다. 

![member](/lab8/images/member_9.png)

13. **Create member and join network** 에서 Fabric 네트워크에 참여할 멤버를 구성하고 네트워크에 참여합니다. 

- Member name: org2
- Admin username : admin1
- Admin password : Passwd123

![member](/lab8/images/member_10.png)

14. **member** 생성이 완료 되는 것을 확인합니다. 

![member](/lab8/images/member_11.png)

15. 초대받은 AWS account의 Networks에 **“beer”**가 추가된 것을 확인합니다. 

![member](/lab8/images/member_12.png)

16. 초대받은 AWS account 와 초대한 AWS account 의 **Members**를 확인합니다. 

- 초대한 AWS account의 Members
![member](/lab8/images/member_13.png)

- 초대받은 AWS account의 Members
![member](/lab8/images/member_14.png)




---
© 2020 Amazon Web Services, Inc. 또는 자회사, All rights reserved.
