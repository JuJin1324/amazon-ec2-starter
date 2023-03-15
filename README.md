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
> 인스턴스에 일정기간(1년~3년) 약정을 걸어서 사용한다.  
> 약정은 선수금 모두 지급, 선수금 부분 지급, 선수금 없음 옵션을 선택 가능하다.  
> 선수금을 모두 지급하는 옵션을 선택하면 On-demand 의 비용 기준 약 75% 를 절감할 수 있다.  
> 선수금 없음 옵션을 선택하면 On-demand 의 비용 기준 약 35~40% 를 절감할 수 있다.    
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

### SSH 접속
>

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

---

## AMIs(Amazon Machine Images)
### 개요
> AMI 는 EC2 instance 의 OS, preinstalled software, EBS 갯수 및 사이즈와 같은 설정 값을 미리 셋팅한 템플릿이다.  
> 하나의 AMI 로 하나의 EC2 인스턴스 혹은 여러개의 인스턴스를 생성할 수 있다.  

### 특성
> AMI 는 EC2 인스턴스 생성 시에 선택할 수 있으며 AMI 항목 아랫부분에 다음의 특성 2개를 확인할 수 있다.  
> Root device type, Virtualization type

### Root device type
> EC2 인스턴스의 부팅가능한 block device 를 root device 라고 명명된다.    
> Root device type 은 다음 2가지 이다.  
> Amazon EBS, Instance store  
> 
> Amazon EBS 는 인스턴스와 분리되어 존재하기 때문에 인스턴스 중지 시에도 데이터가 유지된다.  
> Instance store 는 인스턴스에 연결되어 존재하기 때문에 인스턴스 중지 시 데이터가 모두 사라진다.  
> 대부분 Amazon EBS 를 사용하는 듯하다.  

### Virtualization type
> EC2 인스턴스 가상화 타입으로 PV 가 옛날 버전의 가상화이고 HVM 이 최신 버전의 가상화임으로 되도록 HVM 사용이 권장된다.

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
> 서비스와 애플리케이션으로 분류: <큰 서비스 이름>-<애플리케이션 이름>-sg  
> 애플리케이션으로만 분류: <애플리케이션 이름>-sg
> 
> 예를 들어 `jujin` 이라는 web application 서비스를 호스팅하는 EC2 를 만들어서 해당 EC2 인스턴스에 SecurityGroup 을 붙이려고 할 때 
> SecurityGroup 의 이름을 `jujin-web-application-sg` 와 같은 식으로 혹은 `web-application-sg` 와 같이 범위를 더 늘려서 나눈다.
> 
> `jujin` 이라는 rdbms 서비스를 호스팅하는 RDS 의 경우는 해당 RDS 인스턴스에 붙일 SecurityGroup 이름을 `jujin-mysql-sg` 혹은 
> `mysql-sg` 와 같이 범위를 더 늘려서 나눈다.

### source IP 에 SecurityGroup ID 가 붙는 경우
> 해당 SecurityGroup 을 연결한 모든 인스턴스가 source 가 된다.  
> 인스턴스의 autoscaling 시에 유용하다.  

---

## Load balancer
### 개요
>
