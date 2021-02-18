# 각종 명령어 정리



## 리눅스 계열 명령어



1. ##### 내 ip 주소 탐색

   라즈베리안 - ifconfig
   우분투 - ip addr

   참고 : 라즈베리안은 데비안 계열이며, 데비안 계열에는 nodejs말고 node라는 패키지가 잇음



2. ##### 수도 명령어 정리

   sudo apt **purge** 	: 	패키지랑 설정파일도 함께 삭제
   sudo apt **remove**	 :	 패키지만 지우고 설정파일은 남겨둠
   sudo apt **autoremove**	 : 	불필요한 패키지 제거
   sudo apt install **-y** "packageName" 	: 	묻지말고 패키지 설치
   sudo apt **clean** 	: 	설치/업그레이드 후 다운로드된 패키지 청소(모두 삭제)
   sudo apt **autoclean** 	: 	설치/업그레이드 후 다운로드 패키지는 보관하되, 구 버전 패키지만 삭제





3. ##### ls, ln 명령어 사용

   ls -l /usr/bin | grep node	:	해당 파일 안에 node찾기

   ls -l	:	권한문자열, 해당 디렉토리 내부의 파일과 디렉토리 갯수, 권한가진자, 소유주가 속한 그룹, 크기, 마지막으로 접근한 시각, 파일 및 dir 이름

   

   ln 명령어는 멀리 파일까지 안가고 실행하는 파일링크를 사용하기위해 사용함

   ln -s /usr/bin/node node	:	해당파일의 node를 멀리서 실행

   ln -s는 심볼릭링크(소프트 링크)		vs		ln 은 하드링크



4. ##### 잡기술

   vim .bash_history 	:	 모든 입력한거 다 볼수 있음

   cd ~ubuntu 	:	 물결은 그전 path 무시하고 들어갈수 있음

   

   chown 	:	 소유권 변경
   chmod 	:	 파일권한 변경

   chown root:root dir1	 :	 dir1의 소유주를 root 그룹의 root로 바꾸려면  (여기서 : 또는 .  둘다쓸수잇음)

   

   root 소유자권한이 잇는 파일이나 디렉토리는 옮겨지지 않음.  따라서,
   chown ubuntu.ubuntu test.pcap 해줘야함

   

   

5. ##### TShark 사용

   tshark -D : 모든 연결 표시
   tshark -i eth0 : eth0 선택
   tshark -i eth0 -w test.pcap 파일 저장



6. network interface 관련

   sudo service dhcpcd restart/stop/start : dhcpcd 서비스를 조작하겠다.

   iwconfig : 무선랜 인터페이스 출력

   ifup wlan0(인터페이스 이름) : 인터페이스 키고 닫고

   ifdown wlan0

   sudo service --status-all : 모든 서비스 구동 상황 출력

   sudo iwlist wlan0 scan : 주변 AP 검색

   sudo nano /etc/wpa_supplicant/wpa_supplicant.conf 설정 파일을 수정하여 접속할 AP 접속시 사용할 보안 정책을 설정 가능

   하지만, 보안상의 이유로 wpa_supplicant.conf 설정 파일에 PW를 직접 입력하지는 않음. 대신 명령어

   wpa_passphrase KSPi-AP password 를 치면 나오는 암호에 대한 PSK가 생성됨. 따라서 이걸 긁어와서 원래의 PW를 뺀 문구를 저 위의 설정 파일에 수정한다.

   

7. 와이파이 관련 명령어

   iw event : 이벤트 모니터링

   iw dev : 무섲 장치들의 정보 확인 (현재 설정된 채널 등을 확인)

   ​		>iw dev wlan0 info 명령으로 하나의 장치(wlan0)만 확인 가능

   iw reg get : 현재 설정된 도메인 정보 확인 (국가 설정값 확인 가능)

   ​		>iw reg set US : US국가로 설정

   iw phy : 현재 설정된 채널 정보 확인 가능

   

   

   
   

   

## RaspberryPi rsa로 보안하여 SSH key를 발급 및 vscode 연결



### [Pi] ssh 설정

```bash
sudo raspi-config
```

Interface Options - P2 SSH - Yes 로 ssh를 사용할 수 있게 설정한다

Pi 보안 높이기의 보안을 높이기 위해

```shell
passwd pi
```

명령어로 바꿔주던가 아니면 위의 `raspi-config`명령어로 들어가서 System Options - Password - 새비밀번호 엔터로 비밀번호를 바꿔주자



### [Desktop] ssh-key 생성

윈도우 키 클릭 후 '선택적 기능'을 입력하여 OpenSSH 클라이언트를 설치함

`%USERPROFILE%\.ssh` 경로에 사용자의 ssh key가 있다면 `id_rsa`, `id_rsa.pub` 파일이 있을거임. 하지만 없다면 다음 명령어로 만들어야함

```shell
ssh-keygen -t rsa -b 4096
```

 4096비트의 rsa방법으로 ssh key 생성함

이렇게 생성한 키에서 공개키 파일(`id_rsa.pub`)을 pi 즉 리눅스 머신 측에 등록하는데 이를 위해 scp.exe로 머신측에 `id_rsa.pub`를 복사함



### [Desktop] ssh-key를 Pi로 복사

```bash
scp "%USERPROFILE%\.ssh\id_rsa.pub" pi@192.168.50.1:~/tmp.pub
```

사용자명 pi, 뒤의 ip주소는 해당 주소 :~/ 이후는 저장 위치이며 password를 입력하면 최종 보내기 완료

scp <유저홈디렉터리패스>/.ssh/id_rsa.pub 서버계정명@서버주소

### [Pi] ssh-key 저장

```shell
mkdir ~/.ssh
chmod 700 ~/.ssh
cat ~/tmp.pub >> ~/.ssh/authorized_keys
rm -f ~/tmp.pub
```

ssh파일을 만들어 주고 권한 변경, `tmp.pub`를 ssh파일에 인증키로 넣어주고 `tmp.pub` 삭제



### [Desktop] vscode에서 ssh연결

vscode를 실행해서 Remote - SSH 확장 설치

이후 Ctrl+Shift+P를 눌러 "Remote-SSH : Add New SSH Host" 엔터

```
ssh pi@192.168.50.1
```

엔터

```shell
C:\Users\USERPROFILE\.ssh\config
```

엔터

```
Host [저장하고자 하는 별칭]
  HostName 192.168.50.1
  User pi
```

저장! 끝!







 

## RaspberryPi AP mode설정하는방법(라즈비안)

우선 첫 location config 할때 US로 설정. 이후 모든 설정에 KR이아닌 US로 해야 대역폭이 겹치지도 않고 5G wifi를 검색할 수 있음

### 시작하기에 앞서,

```
$ sudo apt-get update
$ sudo apt-get upgrade
```

그 다음 필요한 모든 소프트웨어를 한 번에 설치한다.

```
$ sudo apt-get install dnsmasq hostapd
```

이 소프트웨어들은 설치되면 바로 실행이 저절로 되기 때문에 구성 파일이 준비되지 않았으므로, 다음과 같이 새 소프트웨어를 꺼준다.

```
$ sudo systemctl stop dnsmasq
$ sudo systemctl stop hostapd
```

설치가 다 완료되었고, 소프트웨어도 껐다면 올바르게 사용하기 위해 리부트한다.

```
$ sudo reboot
```



### 고정 IP 구성

독립형 네트워크를 구성하기 위해 라즈베리 파이에는 무선 포트에 **static IP 주소가 할당** 되어야 한다.

따라서 무선 네트워크 표준인 192.168.xx IP 주소를 사용하고 있다고 가정하고, 서버에 192.168.4.1을 할당한다.

```
$ sudo nano /etc/dhcpcd.conf
```

이 파일의 끝에 다음줄을 첨가한다. 대역폭 충돌을 피하기 위해 168.130 ~ 대역은 사용 금지! 192.168~ 대역을 사용하자

올바르게 작성했으면 다시 시작하여 새로운 wlan0 구성을 설정한다.

```
$ sudo service dhcpcd restart
```

 

### DHCP 서버 구성(dnsmasq)

DHCP 서비스는 dnsmasq에서 제공한다. 이 때, 기본적인 구성 파일에는 필요하지 않은 정보가 많이 포함되어 있기 때문에 처음부터 시작하기 위해서 파일을 옮기고, 새 파일을 만들어 수정한다.

```
$ sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig  
$ sudo nano /etc/dnsmasq.conf
```

다음 줄들을 dnsmasq 파일에 입력한다.

다음 코드를 통해 192.168.4.2에서 192.168.4.20 사이의 IP 주소를 24시간 동안 제공해주는 것이다.

```
interface=wlan0      # Use the require wireless interface - usually wlan0
  dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h
```



### 액세스 포인트 호스트 소프트웨어 구성(hostapd)

이 다음은 **무선 네트워크의 다양한 매개변수를 추가**하는 것에 대한 내용이다.

/etc/hostapd/hostapd.conf에 있는 hostapd 구성 파일을 편집하면 된다.

```
$ sudo nano /etc/hostapd/hostapd.conf
```

다음 파일을 열어서 아래 내용을 첨가해주면 된다.

이 때, **이름과 암호는 ''로 묶으면 안되고, 암호는 8자에서 64자 사이**여야한다. 5GHz 대역을 사용하려면 작동모드를 hw_mode=g에서 hw_mode=a로 변경하면 된다.

```
interface=wlan0
driver=nl80211
ssid=설정할 이름
hw_mode=g
channel=7
wmm_enabled=0
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=암호
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

이 때, 혹시 무선 네트워크를 숨기고 싶다면, ignore_broadcast_ssid=1로 바꿔주면 된다.

이렇게 작성을 완료했다면 이 위치를 알려야 하기 때문에 다음과 같은 명령을 적어준다.

```
$ sudo nano /etc/default/hostapd
```



이 파일에서 **#DAEMON_CONF** 로 시작하는 줄을 찾아 다음과 같이 작성하고, **주석을 해제**한다.

```
DAEMON_CONF="/etc/hostapd/hostapd.conf"
```



### 시작하기

이제 환경을 다 갖추었고, 시작하면 된다. hostapd는 masking이 되어있기 때문에 unmask를 해주고 시작한다.

```
$ sudo systemctl unmask hostapd
$ sudo systemctl enabel hostapd
$ sudo systemctl start hostapd
$ sudo systemctl start dnsmasq
$ sudo nano /etc/sysctl.conf
```

로 가서, 다음 행의 **주석을 해제**한다.

```
net.ipv4.ip_forward=1
```

또한, 다음 명령을 추가한다.

```
$ sudo iptables -t nat -A  POSTROUTING -o eth0 -j MASQUERADE
$ sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
```



그리고, 다음 파일로 가서 **exit0 바로 위**에 다음 행을 추가한다.

```
$ sudo nano /etc/rc.local
iptables-restore < /etc/iptables.ipv4.nat
```



이제 모든 작업이 완료되었고, **리부트**를 해준다.

```
$ sudo reboot
```



라즈베리파이를 reboot했을 때, 자꾸 hostapd와 dnsmasq가 꺼져 다시 켜야하는 상황이 있었다.

이 때는

```
$ sudo update-rc.d hostapd enable
$ sudo update-rc.d dnsmasq enable
```

를 치면 해결된다. 자동실행을 해지하고 싶다면 마지막을 **disable**로 바꿔주면 된다.



## Ubuntu IoT환경에서 Docker를 활용한 Mosquitto MQTT 브로커 서버 구현 방법  

```bash
cd /etc/netplan      

sudo nano 50-cloud-init.yaml  

sudo netplan apply  

sudo init 0  

sudo apt update
sudo apt upgrade

sudo apt-get install     apt-transport-https     ca-certificates     curl     gnupg-agent     software-properties-common  

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo apt-key fingerprint 0EBFCD88

sudo add-apt-repository    "deb [arch=arm64] https://download.docker.com/linux/ubuntu \    $(lsb_release -cs) \ stable"   

sudo apt-get update                                                                                                             

sudo apt-get install docker-ce docker-ce-cli containerd.io                                                                                                         
sudo apt upgrade                                                                                                                                                   
sudo docker run -it -p 1883:1883 -p 9001:9001 eclipse-mosquitto                                                                                                    
sudo docker container ls -a                                                                                                      sudo docker container ls                                                                                                         

sudo docker container stop <Container ID 앞 두글자만>                                                                                                                                         
sudo docker container rm 19                                                                                                                                        
sudo docker container ls -a                                                                                                     

sudo docker container ls                                                                                                         sudo docker container ls -a                                                                                                                                        
sudo docker container rm 73 ce                                                                                                                                     
sudo docker run -it -p 1883:1883 -p 9001:9001 --name k-mosquitto eclipse-mosquitto                                                                                 
sudo docker container ls                                                                                                                                           
sudo docker container stop k-mosquitto                                                                                                                             
sudo docker container ls                                                                                                         sudo docker container ls -a                                                                                                                                        
sudo docker start k-mosquitto                                                                                                                                      
sudo docker container ls                                                                                                                                           
```

