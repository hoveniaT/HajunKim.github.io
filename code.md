미세먼지와 HUE 연동하는법 
===================
Introduction
------------
[1.라즈베리 파이를 통해 실시간 미세먼지 농도 측정한 값을 HUE 와 연동하는 법 설명서](#2.-라즈베리-파이를-통해-실시간-미세먼지-농도-측정한-값을-HUE-와-연동하는-법-설명서)	 

[2.미세먼지 공공 데이터를 HUE 로 가져오는 방법 설명서](#1.-미세먼지-공공-데이터를-HUE-로-가져오는-방법-설명서) 

이 설명서는 미세먼지 공공데이터를 google sheet로 받아오는 법을 먼저 숙지해야 한다. 마찬가지로 라즈베리 파이를 통해 실기간 미세먼지 농도를 측정하는 방법도 먼저 숙지해야 한다. 불빛을 통해 미세먼지 데이터를 사람들에게 시각적으로 전달하기 위하여 시도한 방법이다.   
어려운 점, 혹은 오류가 있을 경우에는  ha_jun-_-v@hanmail.net으로 메일을 공손히 보내주길 

just in case 
모든 코드는 cmd (terminal)창에서 진행된다. 
파일이 설치된 위치에 들어가서 python3 파일이름 하면 cmd에서 파이썬을 실행 가능한 것은 안다고 가정한다. 

<img src="https://user-images.githubusercontent.com/37058246/59561648-ab313d80-905d-11e9-8718-1b6ee2c25bbd.jpg" width=60% height=60%>

1. 라즈베리 파이를 통해 실시간 미세먼지 농도 측정한 값을 HUE 와 연동하는 법 설명서
---------------------------------------------------------------
라즈베리 파이를 통해 실기간 미세먼지 농도를 측정하는 방법은 이곳에서 설명하지 않는다. 

따라서 이 곳에서 중점적으로 설명할 것은 HUE와 인터넷과의 연동이다.

이 과정이 있어야 HUE가 연동이 되어, 데이터를 받아올 수 있으므로 필수로 참고해야 한다. 

python 코드와 HUE를 연동하기 위해서는 2가지의 과정이 필요하다. 

1. 인터넷과 HUE 연동하여 어플로 조작 가능하도록 하기 

2. Username 과 Hue 전구의 번호를 받아와 python과 연동하기 

아래 영상은 연동 전 setting에 관한 유튜브 영상이다. 
영상을 참고하면 충분히 파이썬 코드와 연동에 필요한 과정들을 수행 가능하다. 

<iframe width="948" height="533" src="https://www.youtube.com/watch?v=TL-K4Gm0fis" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

인터넷과 HUE 연동하여 어플로 조작 가능하도록 하기 
1. 근처 wifi 위치를 확인한다. 
2. wifi에 아래의 사진과 같은 philps bridge를 연결한다. (philips hue 전구 박스를 확인하면 있다.)
<img src= "https://user-images.githubusercontent.com/37058246/59563613-26531d80-9077-11e9-95f0-3aab7080d782.jpg" width= 60% height=60%>
3. 플레이 스토어, app store에서 philips hue를 다운 받는다. 
4. 어플의 화면을 켜 연동을 확인한다. 
5. bridge 본체의 버튼을 누르라는 안내문이 어플에 나타나면 동그란 버튼을 눌러준다. 
6. 전구를 HUE에 등록한다. 

위 과정은 동영상을 첨부하였다. 참고하면 정확하게 어떻게 수행되는지 확인 가능하다. 


Username 과 Hue 전구의 번호를 받아와 python과 연동하기 
```python3
b = Bridge("192.168.0.9", "fwVQGMv9TZ0bx1lXms6TVrgKs9-6YEGze3kEn42g")
lights = b.lights
light_num1 = 8
light_num2= 9

```
파이썬과 hue를 연동하기 위하여서는 총 ip, username, light number가 필요하다. 

ip를 받아오는 과정은 단순 공유기의 ip를 받아오는 것이 아니다. 
hue 어플 내에서 따로 생성된 ip를 받아와야한다. 

어플 내 하단 바의 설정 > Hue bridge 선택 하여 확인할 수 있다. 
아래의 사진을 참고하면 된다. 


<img src= "https://user-images.githubusercontent.com/37058246/59563760-19372e00-9079-11e9-86fa-d38012f03e18.jpeg" width=30% height=30%>

<img src= "https://user-images.githubusercontent.com/37058246/59563750-01f84080-9079-11e9-8aca-92c2d8bea8de.jpeg" width=30% height=30%>


username 을 받아오는 과정은 단순하다. 

```python3
from qhue import create_new_username
username = create_new_username("192.168.0.8")
print(username)

```
아래 코드를 수행하면 username 이 자동으로 프린트 된다. 프린트 된 유저네임을 복사하여 파이썬 코드의 username 위치에 작성하여주면 된다. 

3. light number 받아오는 법 
light number을 받아오는 법은 생각보다 까다롭다. 
어떠한 page에 들어가서 그곳에서 사용하는 문법으로 확인해봐야하는데 어렵지는 않지만, 
이렇게까지 할 필요없이 0(혹은1)~ 10 까지의 숫자를 넣어서 시도해보면 어렵지 않게 찾을 수 있을것이고
대부분 처음 생성하는 light number은 1로, 그 후 추가되는 것은 숫자가 1 씩 증가할 것이다. 



2. 미세먼지 공공 데이터를 HUE 로 가져오는 방법 설명서
------------------------------------------

미세먼지 공공 데이터를 구글 sheet로 업데이트 하는 과정은 이곳에서 설명하지 않는다. 

아래사진과 같이 Sheet1에 미세먼지 데이터를 업데이트 했다고 가정해보자. 

준비물 
1. 공공 데이터가 업데이트 된 구글 sheet

2. 숫자 수치에 따라 빛의 밝기가 변하는 hue 코드

<img src="https://user-images.githubusercontent.com/37058246/59561975-54c5fe00-9061-11e9-8986-2e55a793f696.png" width=60% height= 60%>

이 중 가장 최근의 데이터만을 받아오기 위하여 한 줄만 새로운 Sheet에 업데이트 한다. 

<img src= "https://user-images.githubusercontent.com/37058246/59562009-fb120380-9061-11e9-99e4-e1f1c643d82c.png" width=60%
height=60%>

이 새롭게 업데이트 된 한 줄을 "준비된 숫자 수치에 따라 빛의 밝기가 변하는 hue 코드"에 업로드 한다.


```python3

import urllib.request 

url = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vT8zy7UI5D1Y-cvo5-19HVyOSapGmDa4S8Vt0H1v_tMPT2ZcQrfEubkQR8bGJkwMJHbECFqNMSyGvpn/pub?gid=1192547099&single=true&output=csv'   

file_name = 'Hajun.csv'

urllib.request.urlretrieve(url, file_name)

csvfile = open(file_name, 'r')
lineList = csvfile.read().splitlines()
csvfile.close()

tmp1,tmp2,tmp3,tmp4,dust10,tmp5,tmp6,tmp7,tmp8 = lineList[2].split(",")

print("dust10", dust10)
 
dust10 = int(dust10)

```

업로드 된 코드에 대하여 설명하겠다. 

urllib library에 대한 설명은 https://blog.naver.com/is_king/221461183877 을 참고하라. 
구동자체와는 큰 관계가 없으니 이해를 생략해도 상관없다. 

친절하게 설명을 하자면 

1. url = '주소값' 은 google sheet에서 본인의 주소값을 작성하면 된다. 단순한 변수이다. 이어지는 코드에서 url 변수는 모두 이 변수선언에 해당된 주소값이다. 

2. file_name = 파일이름 은 설정하고자 하는 본인의 파일 이름이다. 마찬가지로 단순한 변수이다.

3. urllib.request.urlretrieve(url, file_name), 즉 urlretrieve 함수는 설정한 url 주소(선언된 변수)에서 파일명대로 파일을 다운로드(ex.웹상 jpg 파일 다운로드 등)하는 것이다. 해당에서는 파일 이름의 확장자명이 csv이므로 csv로 다운로드한다.

4. csvfile 변수에 'r', 즉 read 모드로 연 파일을 저장한다. 

5. 그 후 linelist에 read 모드로 받아온 파일의 정보를 line 별로 나눠서(splitlines) 리스트화한다. 
위의 경우에는 linelist에 [[latest data],[Y,N,D,H,미세먼지10,온도,습도,강수량,풍향], [2019,6,16,17,3,19.4,77,0,41]] 로 저장되어있다. 

6. 이 중 lineList[2].split(",") 즉 3번째 줄의 데이터를 , 를 기준으로 나눠서 선언된 tmp1, tmp2, tmp3, tmp4, dust10, tmp5, tmp6, tmp7, tmp8 에 저장한다. 

이 중, 미세먼지 데이터는 5번째로 '3' 이 dust10 에 저장되었다. 저장된 정보는 string 이다. 

9. 따라서 숫자로 변환하여 주기 위하여 int(dust10)을 하여 인트형으로 변환한다. 

그 후, 
```python3
if dust10 < 1:
   b.lights(light_num1, 'state', bri=255, hue=45000, sat=255) #blue
   b.lights(light_num2, 'state', bri=255, hue=45000, sat=255) #blue
```
"준비된 숫자 수치에 따라 빛의 밝기가 변하는 hue 코드" 의 변수 대입 정보를 변경하여 반영시킨다. 


<img src ="https://user-images.githubusercontent.com/37058246/59562314-1b43c180-9066-11e9-8193-7b9e5ab346ad.png" width=60% height=60%>





