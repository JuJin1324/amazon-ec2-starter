# amazon-ec2-starter

## AWS VPC(Virtual Private Cloud)
### 개요
> AWS VPC 는 클라우드의 가상의 `데이터 센터`라고 생각하면 된다.  

### 특징
> 하나의 VPC 는 하나의 Region 에 국한된다.  

### CIDR 
> `CIDR` 는 할당할 수 있는 IP Range 를 나타낸다.  
> 예시) 10.0.0.0/16 인 CIDR 가 있으면 마지막 숫자(16)을 8로 나눈 몫(2)으로 IP Address 왼쪽 앞부터 . 기준 2개를 고정한다.
> 여기서는 . 기준 2개는 10, 0 이며 이 2자리는 고정하고 뒤의 2개의 자리는 범위로 사용한다.
> 범위: 10.0.0.0 ~ 10.0.255.255  
> 
> 예시 2) 10.0.1.1/32 이면 마지막 숫자(32)를 8로 나눈 몫 4 자리를 고정함으로 IP 범위는 10.0.1.1 한 개이다.  

---

## Subnet
### 개요
> 서버 중 내부망 안에서만 접속이 가능해야하는 서버들과 내부망과 외부망 모두에서 접속이 가능해야하는 서버들이 존재한다.  
> 내부망 안에서만 접속이 가능해야하는 그룹은 private 세그먼트으로 묶으며 내부망 및 외부망 모두에서 접속이 가능해야하는 서버들은 
> public 세그먼트로 묶는다. 여기서 세그먼트는 네트워크 용어로 subnet(서브넷)으로 불린다.  
> 즉, 네트워크에서는 private 서브넷과 public 서브넷으로 구분할 수 있다.

### 특징
> subnet 은 VPC 의 CIDR block 안에서만 CIDR block 을 설정할 수 있다.  

### Public subnet
> Public subnet 의 외부 트래픽은 IGW(Internet GateWay) 를 통해서 외부로 나간다.

### Private subnet
> Private subnet 의 외부 트래픽은 NAT(Network Address Translation) Gateway 를 통해서 외부로 나간다. 

---

## EC2
### 개요
> 

### KeyPair
> 

### SSH 접속
> 

---

## Load balancer
### 개요
>
