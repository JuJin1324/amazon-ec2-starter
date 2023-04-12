# amazon-ec2-starter

## Amazon EC2(Elastic Compute Cloud)
### 개요
> AWS 에서 제공하는 가상화 서버.  
> 원하는 스펙으로 구성하여 생성 가능하며 스펙 업그레이드(scale-up) 및 다운그레이드(scale-down) 모두 가능.

### 비용에 따른 인스턴스 분류
> 인스턴스 사용을 시간 당 얼마로 계산. 기본적으로 AWS 의 계산은 인스턴스 갯수 당 계산이 아닌 시간당 계산이다.
> 예를 들어서 동일한 스펙의 인스턴스 2개를 2시간 동안 사용했다고 하면 청구서에는 각 인스턴스 별 시간으로 나오지 않고
> 1개의 인스턴스가 4시간을 사용했다고 비용이 계산되어 나온다.
> 
> 다음은 비용에 따른 인스턴스 분류이다.  
> On-demand  
> 사용한 만큼 비용을 지불하는 방식이다. 새로 인프라를 구축하느라 얼마나 많은 인스턴스가 필요한지 가늠이 안될 경우 사용하는 것이 좋다.  
> 테스트 혹은 scale-out 과 같이 장기적으로 사용할 인스턴스가 아닌 경우에도 사용하기 좋다.
> 
> Spot instances  
> 일반적으로 On-demand 보다 약 90% 싸게 인스턴스를 사용할 수 있다.  
> 내가 설정한 경매 가격보다 인스턴스 비용의 가격이 낮으면 해당 인스턴스를 사용하는 방식이며, 내가 설정한 경매 가격보다 인스턴스
> 비용이 높아지면 2분의 텀을 두고 인스턴스가 종료된다.(AWS 에서 인스턴스가 종료되었다고 notification 을 발송한다.)  
> 애플리케이션이 실행 도중 중단되도 상관없는 경우 사용한다.  
> 
> Reserved instances  
> 인스턴스에 일정기간(1년에서 3년) 약정을 걸어서 사용한다.  
> 약정은 선수금 모두 지급, 선수금 부분 지급, 선수금 없음 옵션을 선택 가능하다.  
> 선수금을 모두 지급하는 옵션을 선택하면 On-demand 의 비용 기준 약 75% 를 절감할 수 있다.  
> 선수금 없음 옵션을 선택하면 On-demand 의 비용 기준 약 35% 에서 40% 를 절감할 수 있다.    
> Reserved instance 는 특정 인스턴스에 약정을 거는 것이 아닌 인스턴스 타입(스펙)에 약정을 거는 것이다.    
> AWS 의 EC2 가격이 지속적으로 떨어지고 있음으로 약정은 1년 정도씩 거는 것이 권장된다.  
> 참고로 약정을 걸게되면 약정건 인스턴스의 정확한 타입(스펙)으로 이용하지 않아도 약정 비용이 그대로 청구되기 때문에
> 인스턴스 타입(스펙)을 신중히 선택하는 것이 필요하다.

### Life cycle
> stop   
> 인스턴스의 OS 를 shutdown 한다. shutdown 완료 후 stopped 상태가 되며 해당 상태에서는 EC2 인스턴스 비용이 발생하지 않는다.
> (EBS 사용 비용은 발생한다.)  
> stopped 된 상태에서 인스턴스 타입(스펙)을 변경할 수 있다.    
> stopped 된 상태에서 EBS 볼륨(root volume)을 변경할 수 이다.    
>
> reboot  
> 아마존에서는 SSH 를 통해서 OS 에 접근해서 직접 reboot 하지않고 AWS Console 혹은 CLI, API 를 통해서 reboot 하는 것을 권장한다.

### Termination protection
> Amazon EC2 의 인스턴스는 종료 방지 기능이 존재한다.  
> aws-cli 를 통해서 의도치 않게 종료할 수 있기 때문에 종료 방지 기능을 걸어주면 콘솔 및 aws-cli 를 통해서도 종료되지 않는다.  
> 종료시키기 위해서는 종료 방지 기능을 해제한 후 종료하면 된다.

### DNS Hostname
> EC2 연결 시 DNS Hostname 을 사용하는 것이 IP 주소로 접근하는 것 보다 이점이 존재한다.  
> 외부 네트워크에서 DNS Hostname 을 사용하여 접속 시 퍼블릭 IP 를 제공하고,
> 내부 네트워크에서 DNS Hostname 을 사용하여 접속 시 프라이빗 IP 를 제공한다.

### Public IP
> 인스턴스가 중지 상태(Stopped)가 되거나 종료 상태(Terminated)가 되면 Public IP 가 Release 된다.    
> 인스턴스를 중지 후 재시작 시 Public IP 가 변경된다.  
> Elastic IP(고정 아이피) 사용 시에도 Public IP 가 Elastic IP 로 대체되기 때문에 Public IP 가 Release 된다.

### KeyPair
> SSH 를 통해서 EC2 인스턴스에 접근하기 위한 Key의 Pair(Private Key, Public Key)
> EC2 인스턴스 생성 시 인스턴스에 접근할 수 있도록 허용하는 KeyPair 등록이 필수.

### Amazon Linux 인스턴스에서 호스트 이름 변경
> 호스트 이름 업데이트를 유지하려면 preserve_hostname cloud-init 설정이 true로 설정되어 있는지 확인 필요.   
> 다음 명령을 실행하여 이 설정을 편집하거나 추가할 수 있다.  
> `sudo vi /etc/cloud/cloud.cfg`  
> 파일 끝에 `preserve_hostname: true` 를 추가한다.  
> 
> `sudo hostnamectl status` 를 통해 현재 설정된 호스트 이름을 확인한다.  
> `sudo hostnamectl set-hostname <변경할 호스트 이름>.localdomain` 을 통해 변경할 호스트 이름을 설정한다.  
> `sudo vi /etc/sysconfig/network` 에서 `HOSTNAME=<변경된 호스트 이름>.localdomain` 으로 변경되었음을 확인한다.  
> EC2 콘솔에서 재부팅 한 후 호스트 이름이 변경되었는지 확인한다.  
> 
> 참조사이트: [Amazon Linux 인스턴스에서 호스트 이름 변경](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/set-hostname.html)

### 인스턴스 타입 변경
> 인스턴스 타입을 변경하기 위해서는 먼저 인스턴스를 중지한다.  
> 타입을 변경하려는 인스턴스를 선택한 후 작업 버튼 클릭 -> 인스턴스 설정 -> 인스턴스 유형 변경을 클릭한다.

### Public instance SSH 접속
> 먼저 KeyPair 가 없다면 EC2 콘솔의 왼쪽 탭에서 네트워크 및 보안 섹션에 '키 페어' 를 클릭하여 접속 후 
> 키 페어를 생성한다.  
> SSH 접속을 원하는 클라이언트는 macOS 를 기준으로 설명한다.  
> 키 페어를 생성하면서 받은 .pem 파일을 ~/.ssh 에 위치시켰으며 이름은 test-keypair.pem 으로 가정하여 설명한다.  
> Public instance 에 접근하는 클라이언트의 IP 주소가 EC2 인스턴스의 보안 그룹에 22 포트와 함께 등록되었다고 가정한다.  
> Public instance 가 amazon linux 를 사용하여 계정명이 ec2-user 로 가정한다.   
> 다음 명령어를 통해서 ssh 접속이 가능하다
> ```shell
> export AWS_KEYPAIR=$HOME/.ssh/test-keypair.pem
> export PUBLIC_EC2_IP=<Public instance 의 Public IP address>
> ssh -i $AWS_KEYPAIR ec2-user@$PUBLIC_EC2_IP
> ```

### Private instance SSH 접속
> Private instance 는 외부망에서는 직접 접속을 할 수 없음으로 접속을 위해서 bastion host 가 필요하다.  
> bastion host 는 내부 접속 용도의 EC2 instance 로 Public instance 이며, 보안 그룹에는 
> 외부에서 접속할 클라이언트의 IP 와 22 포트를 등록해야 한다.  
> Private instance 의 보안 그룹에는 bastion host 의 IP Address 혹은 bastion host 의 보안그룹을 등록하여 
> bastion host 에서 Private instance 에 접속이 가능하도록 설정이 필요하다.  
> 외부망에서 Private instacne 의 접속은 ssh 터널링을 통해서 다음의 명령어로 접속한다.  
> ```shell
> export AWS_KEYPAIR=$HOME/.ssh/test-keypair.pem
> export BASTION_HOST_IP=<Bastion host 의 public IP address>
> export PRIVATE_EC2_IP=<Private instance 의 private IP address>
> ssh -t -o ProxyCommand="ssh -W %h:%p ec2-user@$BASTION_HOST_IP -i $AWS_KEYPAIR" ec2-user@$PRIVATE_EC2_IP -i $AWS_KEYPAIR'
> ```

---

## AMIs(Amazon Machine Images)
### 개요
> AMI 는 EC2 instance 의 OS, preinstalled software, EBS 갯수 및 사이즈와 같은 설정 값을 미리 셋팅한 템플릿이다.  
> 하나의 AMI 로 하나의 EC2 인스턴스 혹은 여러개의 인스턴스를 생성할 수 있다.  

### 특성
> AMI 는 EC2 인스턴스 생성 시에 선택할 수 있으며 AMI 항목 아랫부분에 다음의 특성 2개를 확인할 수 있다.  
> Root device type, Virtualization type

### Root device type
> EC2 인스턴스의 부팅가능한 block device(SSD와 같은 storage) 를 root device 라고 명명된다.    
> Root device type 은 다음 2가지 이다.  
> Amazon EBS, Instance store  
> 
> Amazon EBS 는 인스턴스와 분리되어 존재하기 때문에 인스턴스 중지 시에도 데이터가 유지된다.  
> Instance store 는 인스턴스에 연결되어 존재하기 때문에 인스턴스 중지 시 데이터가 모두 사라진다.  
> 대부분 Amazon EBS 를 사용하는 듯하다.  

### Virtualization type
> EC2 인스턴스 가상화 타입으로 PV 가 옛날 버전의 가상화이고 HVM 이 최신 버전의 가상화임으로 되도록 HVM 사용이 권장된다.

---

## EBS (Elastic Block Store)
### 개요
> EC2 인스턴스에 연결할 저장소(HDD 혹은 SDD)  
> EC2 인스턴스에 연결하기 위해서는 EC2 인스턴스와 EBS 가 동일한 가용 영역(AZ) 에 있어야 한다.  
> EC2 인스턴스와 독립되어 존재하기 때문에 EC2 인스턴스 재시작 혹은 중지(stop) 하더라도 데이터는 유지됨.   

### EC2 인스턴스 연결
> 비용은 설정한 볼륨 크기에 따라서 과금된다.  
> 하나의 EBS 볼륨은 하나의 EC2 인스턴스와만 연결 가능하다.  
> EC2 인스턴스는 여러개의 EBS 볼륨을 연결할 수 있다.  
> EBS 볼륨과 EC2 인스턴스는 직접 연결된 것이 아닌 네트워크를 통해서 연결된 것이다.  
> 
> EC2 인스턴스에서 EBS 설정 탭에서 '종료 시 삭제' 옵션이 '예' 로 설정되어 있으면 종료(terminated) 시에 EBS 도 자동 삭제되지만
> '종료 시 삭제' 옵션을 '아니오'로 설정하면 EC2 인스턴스 종료 후 EBS 볼륨이 어느 인스턴스와도 연결되어 있지 않더라도 그 상태 그대로 유지됨.

### General Purpose SSD(gp2)
> 33GB 이하는 최소 IOPS 로 100 으로 제공하며 33GB 를 초과하면 GB 당 3 IOPS 를 증가시켜서 제공한다.    
> gp2 에서 제공하는 IOPS 의 최대치는 16,000 이다. 

### Provisioned IOPS SSD(io1)
> I/O 처리량이 많이 필요한 RDB 와 같은 서비스 사용 시 사용한다.  
> io1 의 IOPS 설정은 최소 100 부터 최대 64,000 까지 가능하다.  

### Throughput optimized HDD(st1)
> Big data, Data warehouse, Log processing 등에 사용한다.   
> 볼륨 크기는 최소 500GB 부터 16TB 까지 제공한다.

### CloudWatch Monitoring
> EBS 의 적절한 volume 및 IOPS 크기 선택, 처리량 모니터링을 위해서 모니터링이 필요하다.  
> io1 만 1분 단위로 모니터링 데이터를 cloudwatch 에 전송하고 나머지는 5분 단위로 모니터링 데이터를 전송한다.  
> 추가 프로그램 설치 및 설정 없이 EBS 생성 시 기본적으로 read/write bandwidth 및 
> read/write throughput metric 을 cloudwatch 에서 확인할 수 있다.    
> 디스크 사용량은 cloudwatch agent 를 설치를 통해서 따로 모니터링이 필요하다.

### Snapshots
> EBS backup 으로 하나의 EBS 에 대해서 여러 시점 백업 시 변경 사항 versioning 을 통해서 각 백업 마다 데이터를
> 통째로 백업하여 관리하는 것이 아닌 변경 사항에 대해서 백업을 하여 저장 공간을 아껴 저장한다.

### EBS-optimized EC2 instance
> 일반적인 EC2 인스턴스는 외부에서 들어오는 네트워크 인터페이스와 EBS 볼륨과의 통신 네트워크 인터페이스가 동일하다.  
> 외부에서 들어오는 트래픽이 많아질 수록 EBS 볼륨과의 통신에도 악영향을 끼칠 수 있다.  
> 특히나 EBS 타입을 IOPS SSD 로 사용하는 경우에는 특히나 EBS-optimized instance 를 사용해야 효과를 볼 수 있다.   
> 일반적으로 만들 때 EBS 최적화가 활성화 되어 있다.  

---

## Auto scaling
### 개요
> 오토스케일링(Auto Scaling)은 클라우드의 유연성을 돋보이게 하는 핵심기술로 CPU, 메모리, 디스크, 네트워크 트래픽과 같은 
> 시스템 자원들의 메트릭(Metric) 값을 모니터링하여 서버 사이즈를 자동으로 조절 하는 서비스를 말한다.  

### 목표
> 1.정확한 수의 EC2 인스턴스를 보유하도록 보장  
> 지정한 수의 인스턴스 갯수 유지, 최대 인스턴스 갯수 유지, 최소 인스턴스 갯수 이상의 인스턴스가 실행되도록 유지 등
> 
> 2.다양한 스케일링 정책 적용 가능  
> CPU 부하에 따른 인스턴스 크기 늘리기/줄이기 및 시간에 따른 인스턴스 늘렸다 줄이기 등 
> 
> 3.가용영역에 인스턴스가 골고루 분산될 수 있도록 인스턴스를 분배  

### 구성 요소
> Auto scaling group: EC2 인스턴스를 조정 및 관리 목적의 논리 단위로 취급될 수 있도록 그룹으로 구성 
> 
> Minimum size
> 트래픽의 감소로 인하여 최소한으로 유지할 인스턴스의 양, CloudWatch 에서 scale down 이벤트 트리거로 인하여 
> 인스턴스를 줄여도 이 값 만큼의 인스턴스는 실행되어야 한다.
> 
> Desired capacity  
> 현재 사용 인스턴스의 양  
> 
> Maximum size  
> 트래픽의 증가로 인하여 인스턴스를 늘리지만 무한정으로 늘릴 수 없기 때문에 늘릴 수 있는 양의 최대치 
> 
> Instance template
> auto scaling 시 인스턴스를 생성하기 위한 AMI 이미지  

### 참조사이트
> [EC2-오토-스케일링-ELB-로드-밸런서-개념-구축-세팅-💯-정리](https://inpa.tistory.com/entry/AWS-%F0%9F%93%9A-EC2-%EC%98%A4%ED%86%A0-%EC%8A%A4%EC%BC%80%EC%9D%BC%EB%A7%81-ELB-%EB%A1%9C%EB%93%9C-%EB%B0%B8%EB%9F%B0%EC%84%9C-%EA%B0%9C%EB%85%90-%EA%B5%AC%EC%B6%95-%EC%84%B8%ED%8C%85-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC)

---

## SecurityGroup
### 개요
> 인스턴스(EC2, RDS 등) 단위의 virtual firewall.  
> SecurityGroup 은 Deny rule 이 없다. 허용 룰만 존재하며 허용 룰이 없으면 모두 Deny 이다.  
> Stateful 하다 => Inbound 룰에 추가된 포트 및 주소로 들어온 트래픽은 따로 Outbound 룰에 추가하지 않아도 응답을 반환할 수 있다.  

### 권장 사항(내 주관이 들어가있음 주의)
> 인스턴스가 호스팅하는 애플리케이션을 기준으로 한 SecurityGroup 생성을 권장.  
> 
> 이름 구성  
> 서비스와 애플리케이션으로 분류: <큰 서비스 이름>-<애플리케이션 이름> 
> 애플리케이션으로만 분류: <애플리케이션 이름>
> 
> 예를 들어 `jujin` 이라는 web application 서비스를 호스팅하는 EC2 를 만들어서 해당 EC2 인스턴스에 SecurityGroup 을 붙이려고 할 때 
> SecurityGroup 의 이름을 `jujin-web-app` 와 같은 식으로 혹은 `web-app` 와 같이 범위를 더 늘려서 나눈다.
> 
> `jujin` 이라는 rdbms 서비스를 호스팅하는 RDS 의 경우는 해당 RDS 인스턴스에 붙일 SecurityGroup 이름을 `jujin-mysql` 혹은 
> `mysql` 와 같이 범위를 더 늘려서 나눈다.

### source IP 에 SecurityGroup ID 가 붙는 경우
> 해당 SecurityGroup 을 연결한 모든 인스턴스가 source 가 된다.  
> 인스턴스의 autoscaling 시에 유용하다.  

---

## Load balancer
### 개요
> 로드 밸런서를 통해서 EC2 인스턴스들을 그룹화하여 단일 endpoint 를 제공한다.  
> 그룹화한 EC2 인스턴스들의 health check 를 주기적으로 하며 healthy 한 인스턴스에만 트래픽을 라우팅한다.  

### Application load balancer
> Application level(OSI 7)의 라우팅을 지원한다.  
> 지원하는 Protocol 은 HTTP, HTTPS 이다.    
> 고정 IP 는 지원하지 않는다.  

### Network load balancer
> TCP level(OSI 4) 의 라우팅을 지원한다.  
> 지원하는 Protocol 은 TCP 이다. (HTTP, HTTPS 도 포트로 지원하지만 Application level 의 지원은 없다)  
> 고정 IP 를 지원한다.  

### Access logs
> ELB 는 access log 를 생성하는 기능이 있다.  
> source IP address, latencies, request path, server response 등을 기록한다.  
> access log 는 S3 에 기록되며 버킷명은 사용자가 지정할 수 있다. 추가 비용은 없다.  
> access log 기록 옵션은 default 로 off 로 되어있으며 확인 및 수정을 위해서는 다음의 경로로 이동하면 된다.  
> EC2 콘솔 이동 -> 로드 밸런서 -> 로드 밸런서 클릭 -> 아래 Attributes 탭 클릭 -> Monitoring 섹션에 Access logs 확인  

### Connection draining
> ELB 는 unhealthy 하거나 deregister 하는 인스턴스의 라우팅을 멈추고 healthy 한 인스턴스에 라우팅한다.    
> 인스턴스를 deregister 시에 이미 해당 인스턴스에 들어온 요청을 처리하기 위해서 인스턴스의 deregister 를 딜레이한다.  
> deregistration delay 는 1초부터 3600초까지 설정할 수 있다.  
> deregistration delay 는 Load balancer 에 설정하는 것이 아닌 대상 그룹(Target group)에 설정한다.  
> 설정은 다음과 같다.  
> EC2 콘솔 -> 대상 그룹 -> 대상 그룹 클릭 -> Attributes 탭 클릭 -> Target configuration 섹션에서 Deregistration delay 확인  

### 삭제 방지
> EC2 인스턴스 뿐 아니라 로드밸런서도 삭제 방지 기능을 사용하여 삭제 방지를 할 수 있다.  

### Listener rules
> ELB Listener 의 rule 을 통해서 경로 기반 라우팅, domain 기반 라우팅 등을 설정할 수 있다.  
> rule 기능을 통해서 하나의 ELB 로 여러 domain 기반의 서비스를 하나의 ELB 를 통해서 서비스할 수 있다.  

### Cross-zone load balancing
> ELB 에서 라우팅을 2개의 AZ 에 나눠서 라운드 로빈으로 라우팅한다고 가정한다.  
> 2a 에는 10개의 인스턴스가 2c 에는 2개의 인스턴스가 있다고 가정하고 10개의 요청이 발생하면 ELB 는 기본적으로 
> 2a 와 2c 에 각각 5개의 요청을 배분한다.  
> 하지만 Cross-zone load balancing 을 enable 하면 각 AZ 의 인스턴스 갯수에 맞게 요청을 배분한다.  
> Application load balancer 에서는 항상 enable 되어 있으며 Network load balancer 에서만 enable/disable 설정이 가능하다.  
> 
> EC2 콘솔 이동 -> 로드 밸런서 -> 로드 밸런서 클릭 -> 아래 Attributes 탭 클릭 -> Target selection configuration 섹션에 
> Cross-zone load balancing 확인  

### SSL offloading
> HTTPS 트래픽을 받기 위해서 ELB 에 SSL cert 를 등록 후 HTTPS 포트로 Listener 를 설정하여 HTTPS 트래픽을 받는다. 
> 대상 그룹에는 HTTPS(443) 로 트래픽을 받지 않고 HTTP(80) 로 받으면 ELB 에서 트래픽에 대한 암호화/복호화 작업을 수행하고
> 서비스하는 EC2 인스턴스들은 암호화/복호화 작업을 수행하지 않고 복호화된 트래픽을 받아서 처리만 하면 되기 때문에 
> EC2 인스턴스들에 부하가 적어진다.  
> 
> SSL Cert 는 AWS Certificate Manager(ACM) 에서 발급받을 수 있다.  

### Proxy protocol
> ELB 로 요청이 들어오면 ELB 에서 본인의 IP 주소로 source IP 를 변경한다.  
> EC2 콘솔 이동 -> 로드 밸런서 -> 로드 밸런서 클릭 -> 아래 Attributes 탭 클릭 -> Traffic configuration 섹션에
> X-Forwarded-for header 에 Append 값을 통해서 애플리케이션에서 X-Forwarded-for header 를 통해 client 의 IP 를 알 수 있다.

### Slow start
> 대상 그룹(Target group)에 EC2 인스턴스가 추가된 후 InService 로 상태가 변하고 나서 로드밸런서에서 해당 인스턴스로
> 트래픽을 라우팅을 바로 하지 않고 기다리는 시간을 설정할 수 있다.  
> EC2 콘솔 -> 대상 그룹 -> 대상 그룹 클릭 -> Attributes 탭 클릭 -> Traffic configuration 섹션에서 Slow start duration 확인  
> 설정을 안할 거면 0초로 설정할 수 있고 설정 시 30초부터 15분까지로 설정할 수 있다.  


