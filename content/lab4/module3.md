---
title: Channel 구성 
weight: 3
pre: "<b>3.3 </b>"
---

Hyperledger Fabric에서 원장은 channel 범위 안에 있습니다. 모든 멤버가 공통 channel에서 작업하는 경우 원장을 전체 네트워크에서 공유할 수 있으며 특정 참여자만 포함하도록 구성할 수도 있습니다. 

1. Channel 구성을 위한 세부 정보가 담겨 있는 configtx.yml 파일을 편집합니다. 메모장에 저장해놓은 **MEMBERID**를 참고합니다. (예 : m-CE3YCGWODFG4DIT6NE47P2K5G4)

```
cp ~/blockchain-supplychain/beer-supplychain-fabric-client/configtx.yaml ~
vi configtx.yaml
```

```
  13      Name: <Insert your MEMBER_ID>
  14             # ID to load the MSP definition as
  15      ID: <Insert your MEMBER_ID>
```

2. Channel을 구성하기 위해 실행 파일을 수행합니다. 

```
# configtx peer block 생성
docker exec cli configtxgen -outputCreateChannelTx /opt/home/$CHANNEL.pb -profile OneOrgChannel -channelID $CHANNEL --configPath /opt/home/ 
```

- (결과 example)
2019-05-20 07:24:23.285 UTC [common/tools/configtxgen] main -> INFO 001 Loading configuration
2019-05-20 07:24:23.290 UTC [common/tools/configtxgen] doOutputChannelCreateTx -> INFO 002 Generating new channel configtx
2019-05-20 07:24:23.290 UTC [common/tools/configtxgen/encoder] NewApplicationGroup -> WARN 003 Default policy emission is deprecated, please include policy specificiations for the application group in configtx.yaml
2019-05-20 07:24:23.291 UTC [common/tools/configtxgen/encoder] NewApplicationOrgGroup -> WARN 004 Default policy emission is deprecated, please include policy specificiations for the application org group m-3OEGYAVR5RF5DOEUP4ICR7GZQI in configtx.yaml
2019-05-20 07:24:23.292 UTC [common/tools/configtxgen] doOutputChannelCreateTx -> INFO 005 Writing new channel tx

```
# Channel생성
docker exec -e "CORE_PEER_TLS_ENABLED=true" \
 -e "CORE_PEER_TLS_ROOTCERT_FILE=/opt/home/managedblockchain-tls-chain.pem" \
 -e "CORE_PEER_ADDRESS=$PEER" -e "CORE_PEER_LOCALMSPID=$MSP" \
 -e "CORE_PEER_MSPCONFIGPATH=$MSP_PATH" cli peer channel create \
 -c $CHANNEL -f /opt/home/$CHANNEL.pb -o $ORDERER --cafile $CAFILE --tls
```

- (결과 example)
2019-05-20 07:26:04.258 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2019-05-20 07:26:04.342 UTC [cli/common] readBlock -> INFO 002 Got status: &{NOT_FOUND}
2019-05-20 07:26:04.352 UTC [channelCmd] InitCmdFactory -> INFO 003 Endorser and orderer connections initialized
……
2019-05-20 07:26:06.697 UTC [cli/common] readBlock -> INFO 018 Received block: 0

```
# Channel에 peer node 참여 
docker exec -e "CORE_PEER_TLS_ENABLED=true" \
 -e "CORE_PEER_TLS_ROOTCERT_FILE=/opt/home/managedblockchain-tls-chain.pem" \
 -e "CORE_PEER_ADDRESS=$PEER" -e "CORE_PEER_LOCALMSPID=$MSP" \
 -e "CORE_PEER_MSPCONFIGPATH=$MSP_PATH" cli peer channel join -b $CHANNEL.block -o $ORDERER --cafile $CAFILE --tls
```

- (결과 example)
2019-05-20 07:29:55.821 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2019-05-20 07:29:56.140 UTC [channelCmd] executeJoin -> INFO 002 Successfully submitted proposal to join channel



3. 실행 결과 생성된 파일들을 확인합니다. 
```
# Channel Configuration 생성 확인 
ls -lt ~/$CHANNEL.pb 

# mychannel.block 생성 확인 
ls -lt /home/ec2-user/fabric-samples/chaincode/hyperledger/fabric/peer
```

4. 이제 다음과 같은 구성이 완성되었습니다. 

![client](/lab4/image/client_4.png)



---
© 2020 Amazon Web Services, Inc. 또는 자회사, All rights reserved.
