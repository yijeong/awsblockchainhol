---
title: Supply Chain 작동 확인
weight: 1
pre: "<b>6.1 </b>"
---

우선, 정상적으로 맥주가 배송되는 시나리오를 진행합니다.
최초 주문은 냉장고에 장착된 사물 인식 장비와 AWS IoT, Lambda와 같은 서비스를 통해 자동으로 발생되므로 그 작업을 시뮬레이션 해주어야 합니다. 


1. **AWS Cloud9**의 터미널로 이동합니다. (**“beer-IDE”**)

2. 주문을 발생 시키기 위해 **initLedger**및 **startTransfer** 를 합니다. 이 요청을 통해 Fabric 네트워크 원장에 새롭게 맥주 10명을 주문하는 트랜잭션이 생성됩니다. 
```
# initLedger – Ledger 초기화
curl -X POST "http://<ELBDNS>/transfer/init" -H "accept: */*" -H "X-username: <your-name>" -H "X-orgName: org1"

# startTransfer – 주문 발생 
curl -X POST "http://<ELBDNS>/transfer/start" -H "accept: */*" -H "X-username: <your-name>" -H "X-orgName: org1" -H "Content-Type: application/json" -d "{\"Owner\":\"Manufacturer\",\"Count\":\"10\"}"

```

- **username**은 앞서 swagger에서 생성한 이름입니다. 이 값은 HTTP 요청 헤더에 추가되어 **username**을 생성한 사용자만 요청할 수 있도록 제한합니다.  
- **ELB endpoint**정보는 앞서 생성한 CloudFormation 스택 (**“beer-env-setting”**)의 **Outputs** 탭에서 확인할 수 있습니다. 

3. 각각의 요청에 대해 다음과 같은 출력이 나오면 정상입니다. 

```
{"transactionId":"f95d02584a080f505daa17572251cf4f6ba73782a6c4a175e02b399ab65c7e8a"} 
```

4. Web UI중에서 **Manufacturer APP**이 active된 것을 확인합니다. 

![chain](/lab7/images/chain_2.png)

- Web UI는 트랜젝션의 owner와 status 으로 주문의 상태를 판단하며, 명시적으로 보이기 위해 현재 owner 및 현재 가능한 요청 버튼만 활성화되도록 구성되었습니다. 

5. Manufacturer App화면에서 **Accept Transfer** 버튼을 눌러 **acceptTransfer**요청을 합니다. 

- Web은 다음과 같은 요청을 API 서버를 통해 Fabric 네트워크에 하게 됩니다. 

```
curl -X POST "http://beer-supplychain-alb-2109300575.ap-northeast-2.elb.amazonaws.com/transfer/accept" -H "accept: */*" -H "X-username: client1" -H "X-orgName: org1"
```

6. 다음과 같이 alert 창이 뜨면 정상입니다. 

![chain](/lab7/images/chain_3.png)

7. Web UI화면에서 **Request Transfer** 을 클릭하여 **requestTransfer**요청을 합니다.

8. Request Amount 에 관한 modal이 뜨면 기본값을 유지하고 **Send message** 을 클릭합니다. 

![chain](/lab7/images/chain_4.png)

- Web은 다음과 같은 요청을 API 서버를 통해 Fabric 네트워크에 하게 됩니다. 

```
curl -X POST "http://beer-supplychain-alb-2109300575.ap-northeast-2.elb.amazonaws.com/transfer/request" -H "accept: */*" -H "X-username: client1" -H "X-orgName: org1" -H "Content-Type: application/json" -d "{\"Owner\":\"ShippingCompany\",\"Count\":\"10\"}"
```

9. TruckDriver App화면이 활성화 되었을 겁니다. 이전과 동일하게 **acceptTransfer**와 **requestTransfer** 요청을 합니다. 

- Web은 다음과 같은 요청을 API 서버를 통해 Fabric 네트워크에 하게 됩니다. 

```
curl -X POST "http://beer-supplychain-alb-2109300575.ap-northeast-2.elb.amazonaws.com/transfer/accept" -H "accept: */*" -H "X-username: client1" -H "X-orgName: org1"

curl -X POST "http://beer-supplychain-alb-2109300575.ap-northeast-2.elb.amazonaws.com/transfer/request" -H "accept: */*" -H "X-username: client1" -H "X-orgName: org1" -H "Content-Type: application/json" -d "{\"Owner\":\"Retailer\",\"Count\":\"10\"}"
```

10. Status 가 Retailer로 넘어오면 다시 **acceptTransfer** 요청을 하고 **complete** 버튼을 눌러 배송을 완료합니다. 

- Web은 다음과 같은 요청을 API 서버를 통해 Fabric 네트워크에 하게 됩니다. 

```
curl -X POST "http://beer-supplychain-alb-2109300575.ap-northeast-2.elb.amazonaws.com/transfer/accept" -H "accept: */*" -H "X-username: client1" -H "X-orgName: org1"

curl -X POST "http://beer-supplychain-alb-2109300575.ap-northeast-2.elb.amazonaws.com/transfer/complete" -H "accept: */*" -H "X-username: client1" -H "X-orgName: org1"
```

11. Web UI 각 멤버들의 APP에 **well-completed**라는 공지가 가는 것을 확인할 수 있습니다. 

![chain](/lab7/images/chain_5.png)

- **completed**가 되면 원장에 적인 최초 주문 수량과 마지막에 도착한 수량을 비교해서 그 결과를 각 멤버에게 공지하게 됩니다. 만약 수량이 다르면 다음과 같이 **out of compliance** 에러가 발생합니다. 

![chain](/lab7/images/chain_6.png)


---
© 2020 Amazon Web Services, Inc. 또는 자회사, All rights reserved.
