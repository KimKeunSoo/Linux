# Linux

[리눅스 계열 명령어]

라즈베리안 - ifconfig
우분투 - ip addr
sudo apt purge : 패키지랑 설정파일도 함께 삭제
sudo apt remove : 패키지만 지우고 설정파일은 남겨둠
sudo apt autoremove : 불필요한 패키지 제거
sudo apt install -y "packageName" : 묻지말고 패키지 설치
sudo apt clean : 설치/업그레이드 후 다운로드된 패키지 청소(모두 삭제)
sudo apt autoclean설치/업그레이드 후 다운로드 패키지는 보관하되, 구 버전 패키지만 삭제

라즈베리안은 데비안 계열이며, 데비안 계열에는 nodejs말고 node라는 패키지가 잇음

ls -l /usr/bin | grep node
해당 파일 안에 node찾기

ln 명령어는 멀리 파일까지 안가고 실행하는 파일링크를 사용하기위해 사용함

ln -s /usr/bin/node node
해당파일의 node를 멀리서 실행

ln -s는 심볼릭링크(소프트 링크)
ln 은 하드링크

vim .bash_history : 모든 입력한거 다 볼수 있음

cd ~ubuntu : 물결은 그전 path 무시하고 들어갈수 있음

ls -l
권한문자열, 해당 디렉토리 내부의 파일과 디렉토리 갯수, 권한가진자, 소유주가 속한 그룹, 크기, 마지막으로 접근한 시각, 파일 및 dir 이름
chown : 소유권 변경
chmod : 파일권한 변경

dir1의 소유주를 root 그룹의 root로 바꾸려면
chown root:root dir1 (여기서 : 또는 .  둘다쓸수잇음)

root 소유자권한이 잇는 파일이나 디렉토리는 옮겨지지 않음.  따라서
chown ubuntu.ubuntu test.pcap 해줘야함

tshark -D : 모든 연결
tshark -i eth0 : eth0 선택
tshark -i eth0 -w test.pcap 파일 저장
