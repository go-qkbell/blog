---  
title: "[Study] Hololive Discord Bot 제작기 #1"  
date: 2022-04-05
categories: ["Study", "Go", "KR", "Hololive", "Discord"]  
tags: ["Study", "Go", "KR", "Hololive", "Discord"]

summary: 홀로라이브 디스코드 봇 제작기 #1

weight: 1
math: true
draft: false
---  

갑자기 든 생각인데, 취직하고 나서는 딱히 개인 프로젝트를 진행한 것도 없고 <br>
주말에 한 시간 이상 코딩을 해본적도 없는 것 같다.<br>
취준 할 당시만 해도, 취직하고 나서도 계속 자기계발을 해서 진취적인 개발자가 되겠다고 다짐했던거 같은데... ㅋㅋ<br>
<br>
딱히 게임을 많이 하는 것도 아니고, 해봤자 주말에 에이펙스 한두시간 하고 안하는편인데<br>
왜이럴까 생각해보니, 요즘 버튜버에 빠져 있어서 하루종일 유튜브만 보는 것 같다.<br>
<br>
이왕 보는거 코딩도 하면서 보려고 내가 좋아하는 취미랑 연관도 있고 학습도 할 겸 홀로라이브 디스코드 봇을 제작해보려고 한다.<br>
<br>
현재 생각중인 핵심 기능으로는
- 현재 라이브 방송 중인 모든 홀로라이브 멤버를 알려주는 기능
  - 채널명, 채널 링크, 썸네일 사진, 시청자수
- 라이브 방송 예정인 홀로라이브 멤버들을 알려주는 기능
  - 채널명, 채널 링크, 썸네일 사진, 시작 예정 시간
- 홀로라이브 멤버가 새 영상을 유튜브에 올리면 알림 해주는 기능
- 홀로라이브 멤버 정보를 출력 해주는 기능

이렇게 4 가지가 있다. 나머지는 개발하면서 생각 나면 실시간으로 추가하는걸로..
<br>
<br>
필요한 작업 으로는
- 디스코드 연동
- 유튜브 연동
- 홀로라이브 데이터 폴링 혹은 pubsub 구독
- 만약 폴링 구조로 갈 시, 홀로라이브 데이터 일정 시간동안 캐싱

등이 있는 것 같다.
<br>
<br>
인프라는 구글 클라우드 vm 인스턴스랑 cloud run 중에 고민중이다.<br>
어차피 이걸로 대형 디스코드 서버를 운영할 생각도 없고, 그냥 개인 서버에서 친구들이랑 쓸 용도라 cloud run 프리티어로 굴리면서 서버리스로 가도 될 것 같은데..<br>
우선 이부분은 어느정도 개발을 끝마치고 생각해 봐야겠다.<br>
<br>
오랜만에 재밌어보이는 프로젝트라 기대된다.