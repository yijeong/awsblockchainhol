---
title: (옵션) Blockchain network에 member추가하기 
weight: 140
pre: "<b>모듈 7. </b>"
chapter: true
---

# (옵션) Blockchain network에 member추가하기 

- 자, 생각해봅시다. 지금 우리는 이 Lab에서 하나의 member(organization)와 하나의 peer node를 생성하고 사용했습니다. 실제 운영 환경을 생각해본다면 다양한 AWS 계정의 member 가 참여할 수 있습니다. 해당 시나리오로 생각해본다면 Manufacturer / ShippingCompany / Retailer라는 각 멤버가 모두 다른 AWS계정을 갖고 Fabric 네트워크에 참가할 가능성이 높습니다. 따라서 이 파트에서는 옆에서 같이 Lab을 진행하고 있는 동료의 계정, 혹은 내가 가지고 있는 두번째 계정으로 member 추가하는 실습을 진행합니다. 

- 본 Lab의 시나리오에서는 처음부터 다른 member로 구성하면 peer node를 만들고 각각에 chaincode를 배포하는 동일한 과정이 3번 반복되어야 해서 생략했습니다. 실제 상황이라면 모든 멤버가 고유한 CA와 peer node 및 user를 가지고 적절한 권한이 있어야 한다는 걸 이해하시면 됩니다. 



---
© 2020 Amazon Web Services, Inc. 또는 자회사, All rights reserved.
