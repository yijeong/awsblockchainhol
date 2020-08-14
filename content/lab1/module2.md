---
title: 실습 흐름 
weight: 2
pre: "<b>0.2 </b>"
---


## Lab 시나리오 설명 

- Supply chain에 참여하는 각 멤버들이 있습니다. (Manufacturer / Shipping Company / Retailer) 
- 각 멤버가 맥주 배송을 위해 서로에게 보내는 요청들(API)이 블록체인 원장에 적힙니다. (해당 Lab에서는 맥주의 주문 수량에 대한 내용을 기록합니다.)
- 배송이 완료되면 처음에 발생한 주문 수량과 최종적으로 배송된 맥주 수량을 비교하여 일치하는 경우에는 ***well-completed*** 공지를, 불일치하는 경우에는 ***out-of-compliance*** 공지를 각 멤버들에게 전달합니다.  
- 결과적으로 블록체인을 기반으로 조작이 불가능한 무결한 배송을 보장할 수 있습니다. 

![scenario](/lab1/image/scenario.png)


## Lab 아키텍처 설명 

![archi](/lab1/image/architecture.png)

본 Lab은 크게 3가지의 기능 구현에 초점이 맞춰져 있습니다.

1. Amazon Managed Blockchain을 이용해 각 멤버 및 피어 노드가 있는 Hyperledger Fabric 프레임워크 네트워크를 구성합니다. 
2. 객체 탐지 알고리즘이 내장된 AWS Deeplens가 ML 추론 및 객체를 카운팅합니다. 그리고 IoT rule 정책에 따라IoT Core가Lambda를 트리거 하고 주문이 담긴 트랜젝션을  Blockchain 노드로 전송합니다.  
3. Fabric Client 및 RESTful API 서버가 chaincode를 배포하고 Fabric 네트워크를 관리하며, RESTful API로 노출하여Fabric 네트워크 사이의 gateway 역할을 합니다. 

{{% notice warning %}}
본 Lab은 AWS Seoul summit 2019에서 시연한 Blockchain Pub을 바탕으로 만들어졌습니다. 
시연에서는 냉장고(Device) 안에서 맥주의 수량을 체크하고 자동으로 주문을 발생시키는 AWS DeepLens, IoT Core, Lambda 를 활용했지만 AWS DeepLens 나 냉장고와 같은 실물이 없는 경우 구현이 어려울 수 있습니다. 따라서 본 Lab에서는 위의 3가지 기능 구현 중 2번은 명시적인 API 요청으로 대체하겠습니다. 2번 구현이 궁금하신 분들은 55페이지의 Appendix를 참고해주시기 바랍니다. 
{{% /notice %}}
 
AWS Seoul summit 2019에서 선보인 시연이 궁금하신 분들은 하기 링크를 참고해주시기 바랍니다. 
https://m.facebook.com/1563378127252216/posts/2282618825328139?sfns=mo


---
© 2020 Amazon Web Services, Inc. 또는 자회사, All rights reserved.
