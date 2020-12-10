---
title: "Mobius와 nCube SSL Test"
date: 2019-10-28 20:00:00 -0900
categories: [mobius]
tags: 
- iot
- internet of things
- oneM2M
- iot 표준
- Mobius
- nCube
---

이 포스트는 Mobius(server)에서 openssl을 이용하여 SSC를 통해 자체 인증서를 생성하고 nCube(client)에서 측정되는 온,습도 값이 Mobius로 넘어갈때 SSL을 enable 했을때와 dissable 했을때의 패킷을 분석해 SSL이 정상적으로 적용이 되었는지 테스트하는 내용이다.<br>



# Disable

---

먼저 서버의 mobius.js 파일을 열어 52번째 라인을 disable로 수정한다.

![그림1](/assets/images/IOT/mobius/[disable]mobius2.png)



nCube의 온습도 센서가 측정 한 값을 보여주며 동시에 서버로 이 값을 보낸다.

![그림1](/assets/images/IOT/mobius/tas.png)


wireshark로 nCube에서 Mobius로 보낸 데이터의 패킷을 확인한다.

![그림1](/assets/images/IOT/mobius/[disable]nCube-_Mobius.png)



# Enable

---

nCube의 conf.js 파일을 열어 밑의 그림처럼 내용을 'enable'로 바꾼다

![그림1](/assets/images/IOT/mobius/nCube_enable.png)

똑같이 nCube에서 온습도 센서가 측정한 값을 서버로 보낼때 패킷을 분석해본다.

![그림1](/assets/images/IOT/mobius/[enable]nCube-Mobus1.png)

그리고 마지막으로 서버의 DB도 확인해본다.

![그림1](/assets/images/IOT/mobius/data 확인.png)





