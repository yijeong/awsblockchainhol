---
title: Chaincode 배포 
weight: 4
pre: "<b>3.4 </b>"
---

Chaincode 는 블록체인 네트워크상에서 멤버들이 동의한 비즈니스 로직을 처리하며 smart contract이라고 부르기도 합니다. 이번 모듈에서는 supply chain구성을 위한 Chaincode를 install / instanticate 하고 잘 배포되었는지 query 및 invoke합니다. 

배포하기에 앞서, 본 Lab의 Chaincode는 어떤 로직을 담고 있는지 알아봅니다. 
본 Lab의 시나리오는 맥주 공급을 위한 supply chain이고 블록체인에 참여하는 멤버는 **Manufacturer / ShippingCompany / Retailer** 가 있다고 앞서 설명을 했습니다. Chaincode에는 주문을 발생시키고 각 멤버에게 요청을 전달하고 트랜젝션을 발생시키는 함수가 포함되어 있습니다. 

![client](/lab4/images/client_5.png)

우선, **initLedger**는 주문 상태를 초기화합니다. 그리고 주문이 필요한 시점에 **startTransfer**를 호출하며 주문 수량을 원장에 전달합니다. 주문이 발생하면 “Manufacturer” 가 **acceptTransfer**와 **requestTransfer** 를 통해 “ShippingCompany”에 상태를 넘겨주게 되고 마찬가지로 “ShippingCompany”는 Retailer에게 전달하기 위해 **acceptTransfer**와 **requestTransfer**를 호출합니다. (같은 함수를 사용하므로 함수 파라미터로 들어있는 owner로 구분을 하게됩니다.) 
맥주 배송이 완료되면 Retailer는 **acceptTransfer**를 호출하고 최종적으로 전달 된 맥주 병 수를 확인합니다. 그리고 나서, **Complete**를 통해 주문을 완료합니다. 이후 최초 주문 수량과 최종 배송 수량을 비교하여 같으면 *'Completed'* 상태가 반환되고, 그렇지 않은 경우에는 *‘OutOfCompliance’*가 반환됩니다. 

코드의 자세한 내용이 궁금하다면 ```/home/ec2-user//blockchain-supplychain/beer-supplychain-chaincode/go/beer.go``` 의 내용을 확인해보시기 바랍니다. 
이 시나리오는 PART 6. Supply Chain 작동 확인에서 Web UI를 통해 직관적으로 확인해볼 예정입니다. 

그럼, Chaincode배포를 시작해봅니다. 

1. CLI 컨테이너 상에 Fabric Client폴더가 마운트 된 것을 확인합니다. 

```
docker inspect cli
```
```
      "Mounts": [
            …….
            {
                "Type": "bind",
                "Source": "/home/ec2-user/fabric-samples/chaincode",
                "Destination": "/opt/gopath/src/github.com",
                "Mode": "rw",
                "RW": true,
                "Propagation": "rprivate"
            },
            {
                "Type": "bind",
                "Source": "/home/ec2-user",
                "Destination": "/opt/home",
                "Mode": "rw",
                "RW": true,
                "Propagation": "rprivate"
            }

```
{{% notice info %}}
/home/ec2-user/fabric-samples/chaincode 이 위치에 chaincode를 복사하면 cli 컨테이너에서 접근할 수 있게 됩니다. 
{{% /notice %}}

2. 이 폴더에 **beersupplychain** 폴더를 만들고 git에서 clone해온 chaincode를 복사합니다. 
```
cd ~
mkdir -p ./fabric-samples/chaincode/beersupplychain
cp ./blockchain-supplychain/beer-supplychain-chaincode/go/* ./fabric-samples/chaincode/beersupplychain
```

3. Peer에 chaincode 를 install 합니다.
```
docker exec -e "CORE_PEER_TLS_ENABLED=true" \
 -e "CORE_PEER_TLS_ROOTCERT_FILE=$CAFILE" \
 -e "CORE_PEER_LOCALMSPID=$MSP" -e "CORE_PEER_MSPCONFIGPATH=$MSP_PATH" \
 -e "CORE_PEER_ADDRESS=$PEER" cli peer chaincode install -n beer -v v0 \
 -p $CHAINCODEDIR
```

{{% notice info %}}
docker cli에서 쓰이는 환경 변수들은 모두 13번에서 수행한 ~/peer-export.sh 로 설정된 peer node의 환경 변수입니다. 
{{% /notice %}}

4. 결과가 다음과 같이 나오면 정상입니다. 

```
2019-05-14 06:15:15.990 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2019-05-14 06:15:15.991 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
2019-05-14 06:15:16.122 UTC [chaincodeCmd] install -> INFO 003 Installed remotely response:<status:200 payload:"OK" >
```

5. Peer에 chaincode 를instantiate합니다. 
```
docker exec -e "CORE_PEER_TLS_ENABLED=true" \
 -e "CORE_PEER_TLS_ROOTCERT_FILE=$CAFILE" \
 -e "CORE_PEER_LOCALMSPID=$MSP" -e "CORE_PEER_MSPCONFIGPATH=$MSP_PATH" \
 -e "CORE_PEER_ADDRESS=$PEER" cli peer chaincode instantiate \
 -o $ORDERER -C $CHANNEL \
 -n beer -v v0 -c '{"Args":["init"]}' \
 --cafile $CAFILE --tls
```
{{% notice info %}}
docker cli에서 쓰이는 환경 변수들은 모두 13번에서 수행한 ~/peer-export.sh 로 설정된 peer node의 환경 변수입니다. 
{{% /notice %}}


6. 결과가 다음과 같이 나오면 정상입니다. 
```
2019-05-14 06:23:03.467 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2019-05-14 06:23:03.468 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
```

7. Peer 에 Chaincode를 query하여 정상적으로 결과가 나오는지 확인합니다. 현재는 아무런 주문이 없으니 [ ] 가 반환되면 정상입니다. 
```
docker exec -e "CORE_PEER_TLS_ENABLED=true" \
-e "CORE_PEER_TLS_ROOTCERT_FILE=/opt/home/managedblockchain-tls-chain.pem" \
-e "CORE_PEER_LOCALMSPID=$MSP" \
-e "CORE_PEER_MSPCONFIGPATH=$MSP_PATH" \
-e "CORE_PEER_ADDRESS=$PEER" cli peer chaincode query \
-C $CHANNEL -n beer \
-c '{"Args":["queryAllOrder"]}'
```

8. Peer 에 Chaincode를 invoke하여 정상적으로 업데이트가 되는지 확인합니다. 

```
# Chaincode invoke 
docker exec -e "CORE_PEER_TLS_ENABLED=true" \
-e "CORE_PEER_TLS_ROOTCERT_FILE=$CAFILE" \
-e "CORE_PEER_LOCALMSPID=$MSP" \
-e "CORE_PEER_MSPCONFIGPATH=$MSP_PATH" \
-e "CORE_PEER_ADDRESS=$PEER" cli peer chaincode invoke \
-C mychannel -n beer -c '{"Args":["initLedger"]}' \
-o $ORDERER --cafile $CAFILE --tls 
 
# Chaincode query (invoke된 것 확인)
docker exec -e "CORE_PEER_TLS_ENABLED=true" \
-e "CORE_PEER_TLS_ROOTCERT_FILE=$CAFILE" \
-e "CORE_PEER_LOCALMSPID=$MSP" \
-e "CORE_PEER_MSPCONFIGPATH=$MSP_PATH" \
-e "CORE_PEER_ADDRESS=$PEER" cli peer chaincode query \
-C $CHANNEL -n beer \
-c '{"Args":["queryAllOrder"]}'

```

9. 하기와 같이 업데이트 내역이 출력되면 정상입니다. 

```
[{"Key":"ORDER0", "Record":{"State":"1","count":"0","owner":"Genesis","ctime":"Wed, 15 May 2019 04:27:04 UTC","utime":"Wed, 15 May 2019 04:27:04 UTC"}}]
```

10. 이제 다음과 같은 구성이 완성되었습니다.

![client](/lab4/images/client_6.png)


---
© 2020 Amazon Web Services, Inc. 또는 자회사, All rights reserved.
