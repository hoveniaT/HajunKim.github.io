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


2. 라즈베리 파이를 통해 실시간 미세먼지 농도 측정한 값을 HUE 와 연동하는 법 설명서
------------------------------------------


