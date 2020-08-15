---
title: cloud9 구성    
weight: 1
pre: "<b>0.1 </b>"
---


## 유의사항
본 Lab은 기본적으로 **미국 버지니아 북부(us-east-1)**을 사용합니다. 우측 상단의 리전 정보가 버지니아 북부 리전이 맞는지 반드시 확인 후 시작해 주시길 바랍니다. 콘솔은 영문 기준으로 작성되어 있습니다.

## 사전 준비 사항 

본 Lab에서 우리는 github에 저장된 소스를 가져와 수정하고 사용할 예정입니다. 개발을 위해 로컬 환경을 사용할 수도 있지만 필요한 권한 설정 작업을 줄이기 위해 AWS Cloud9을 사용하겠습니다. AWS Cloud9은 브라우저만으로 코드를 작성, 실행 및 디버깅할 수 있는 클라우드 기반 IDE(통합 개발 환경)입니다. PART 2, PART 3 에서 이 Cloud9을 통해 Fabric Client 및 API 서버를 구성할 예정입니다. 

1. AWS Management Console에서 **AWS Cloud9** 서비스로 [이동](https://us-east-1.console.aws.amazon.com/cloud9/home/product)합니다.

2. **[Create environment]** 를 클릭합니다. 

3. **Environment name and description** 에서 **Name**을 **“beer-IDE”**로 지정하고 **[Next Step]** 을 클릭합니다. 

![pre](/lab2/image/pre_1.png)

4. **Configure settings**의 **Instance type**을 **Other instance type – t2.medium**으로 변경하고 **Next Step]**을 클릭합니다. 

![pre](/lab2/image/pre_2.png)

5. Review 후 **[Create environment]** 를 클릭합니다. 환경 생성 완료는 2 ~ 3분 정도 소요됩니다. 

6. 생성이 완료되면 **Welcome** 창을 종료하고 Terminal창의 크기를 늘려줍니다. 

![pre](/lab2/image/pre_3.png)

7. Terminal에서 다음을 입력합니다. 
```
cd ~
git clone https://github.com/yijeong/awsblockchainsupplychain.git
```
{{% notice info %}}
해당 git repository에는 본 Lab에서 사용할 Fabric client구성 용 CloudFormation stack / Chain Code / Restful API / Web UI code가 포함되어있습니다. 
{{% /notice %}}





