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





## Typescript 명령어 정리



**npm init --y     package.json**



package.json에 scripts안에
"dev" : "ts-node src", 
"build" : "tsc && node dist"



**npm i -D typescript ts-node**



1. package.json에 devDependencies 항목등록  
2. node_modules 디렉토리 설치
npm i -D @types/node  node_modules에 @types\node 설치
tsc --init    tsconfig.json (ts compiler set file) [복사해서 넣으면 쉬움]

터미널-빌드작업실행-tsc:감시



**ts -> js -> 각각 방식 다름**
Web : AMD(asynchronous module definition)
Nodejs : CommonJS



**tsconfig.ts - compilerOptions**

1. module 키
Web : amd , Nodejs : commonjs
2. moduleResolution 키
amd : classic, commonjs : node
3. target
transfile할 js의 version, 대부분 es5
4. baseUrl
현재 디렉터리 : . 
5. OutDir
base에서 디렉터리 이름 (ex ./dist)
6. paths
import문에서 from 부분을 해석할때 찾아야하는 디렉터리 설정
외부패키지면 node_modules에 찾아야하므로 키값에 node_modules/* 도 포함
7. esModuleInterop
AMD방식으로만 동작한다는 가정인 라이브러리 존재때문에 (ex Chance) ture로 설정
8. sourceMap
true이면 .js 말고 .js.map 파일도 만들어짐
map파일은 변환된 js코드가 ts코드 어디에 해당하는지 알려줌. 주로 디버깅할때 사용
9. downlevelteration
generator 구문을 동작하려면 반드시 true로
10. nolmplicitAny 
f(a, b)일 경우 f(a: any, b:any)처럼 암시적으로 any타입 간주. 
true 일경우 f(a,b)오류 
false 일경우 f(a,b) 오류아님 



**키워드 let, const**
let 변수이름  // 값이 수시로 변결될수 있음
const 변수이름 = 초깃값 // 절대 변하지않음



let a : any = 1/ture/...
let num : number = 1
let bool : boolean = true
let str : string = 'Hello'
let obj : object = {}
obj = {name: 'Jack', age: 32} // any처럼 작동, name과 age 의 속성이름만 가짐
let u : undefined = undefined



타입추론
let n = 1
let b = true



템플릿 문자열
`${변수 이름}`



let count = 10, message = 'Your count'
let result = `${message} is ${count}`
console.log(result) // Your count is 10



interface Iperson{
  name : string
  age : number
  etc? : boolean
}



interface안쓰고 쓰는 anonymous interface



let ai : { 

 name : string
  age : number
  etc? : boolean
} = {name: 'Jack', age:32}



function printMe(me : {name : string, age : number, etc?: boolean}){
cosole.log(
  me.etc?
      `${me.name} ${me.age} ${me.etc}` :
      `${me.name} ${me.age}
)
}
printMe(ai)





생성자(constructor)
class Person{
  constructor (public name : string, public age? : number) {}
}
let jack : Person = new Person('Jack', 32)
console.log(jack2)     //Person{name: Jack, age: 32}



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

