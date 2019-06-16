미세먼지와 HUE 연동하는법 
===================
Introduction
------------
[1.미세먼지 공공 데이터를 HUE 로 가져오는 방법 설명서](#1.-미세먼지-공공-데이터를-HUE-로-가져오는-방법-설명서)  


[2.라즈베리 파이를 통해 실시간 미세먼지 농도 측정한 값을 HUE 와 연동하는 법 설명서](#2.-라즈베리-파이를-통해-실시간-미세먼지-농도-측정한-값을-HUE-와-연동하는-법-설명서)	

이 설명서는 미세먼지 공공데이터를 google sheet로 받아오는 법을 먼저 숙지해야 한다. 마찬가지로 라즈베리 파이를 통해 실기간 미세먼지 농도를 측정하는 방법도 먼저 숙지해야 한다. 불빛을 통해 미세먼지 데이터를 사람들에게 시각적으로 전달하기 위하여 시도한 방법이다.   
어려운 점, 혹은 오류가 있을 경우에는  ha_jun-_-v@hanmail.net으로 메일을 공손히 보내주길 

<img src="https://user-images.githubusercontent.com/37058246/59561648-ab313d80-905d-11e9-8718-1b6ee2c25bbd.jpg" width=60% height=60%>


1. 미세먼지 공공 데이터를 HUE 로 가져오는 방법 설명서
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

'''python3
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

'''

업로드 된 코드에 대하여 설명하겠다. 

urllib library에 대한 설명은 https://blog.naver.com/is_king/221461183877 을 참고하라. 
구동자체와는 큰 관계가 없으니 이해를 생략해도 상관없다. 

친절하게 설명을 하자면 

url = '주소값' 은 google sheet에서 본인의 주소값을 작성하면 된다. 단순한 변수이다. 이어지는 코드에서 url 변수는 모두 이 변수선언에 해당된 주소값이다. 

file_name = 파일이름 은 설정하고자 하는 본인의 파일 이름이다. 마찬가지로 단순한 변수이다.

urllib.request.urlretrieve(url, file_name), 즉 urlretrieve 함수는 
설정한 url 주소(선언된 변수)에서 파일명대로 파일을 다운로드(ex.웹상 jpg 파일 다운로드 등)하는 것이다. 
해당에서는 파일 이름의 확장자명이 csv이므로 csv로 다운로드한다.

csvfile 변수에 'r', 즉 read 모드로 연 파일을 저장한다. 

그 후 linelist에 read 모드로 받아온 파일의 정보를 line 별로 나눠서(splitlines) 리스트화한다. 

위의 경우에는 linelist에 [[latest data],[Y,N,D,H,미세먼지10,온도,습도,강수량,풍향], [2019,6,16,17,3,19.4,77,0,41]] 로 저장되어있다. 

이 중 lineList[2].split(",") 즉 3번째 줄의 데이터를 , 를 기준으로 나눠서 선언된 tmp1, tmp2, tmp3, tmp4, dust10, tmp5, tmp6, tmp7, tmp8 에 저장한다. 

이 중, 미세먼지 데이터는 5번째로 '3' 이 dust10 에 저장되었다. 저장된 정보는 string 이다. 

따라서 숫자로 변환하여 주기 위하여 int(dust10)을 하여 인트형으로 변환한다. 

그 후, 
'''
if dust10 < 1:
   b.lights(light_num1, 'state', bri=255, hue=45000, sat=255) #blue
   b.lights(light_num2, 'state', bri=255, hue=45000, sat=255) #blue
'''

"준비된 숫자 수치에 따라 빛의 밝기가 변하는 hue 코드" 의 변수 대입 정보를 변경하여 반영시킨다. 


<img src ="https://user-images.githubusercontent.com/37058246/59562314-1b43c180-9066-11e9-8193-7b9e5ab346ad.png" width=60% height=60%>






2. 라즈베리 파이를 통해 실시간 미세먼지 농도 측정한 값을 HUE 와 연동하는 법 설명서
------------------------------------------


