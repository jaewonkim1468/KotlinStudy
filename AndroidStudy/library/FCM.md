# FirebaseCloudingMessage
* Firebase에서 제공하는 서비스
* 메시지 및 알림을 위한 크로스 플랫폼 클라우드 솔루션
## 필요성
* 소켓 프로그램을 통해 서버와 연결을 지속한 상태에서 데이터를 송수신하면 앱이 백그라운드 생태일때 자원낭비가 심함
* FCM을 사용하면 서버와 연결하고 있지 않더라도 서버의 데이터를 수신할 수 있음
***
## 동작원리
### 앱을 위한 키를 FCM 서버를 통해 얻는 단계
1. 스마트폰에 앱이 설치되는 순간 시스템에서 Firebase 서버에 키 획득을 위한 요청을 보냄.(개발자가 구현할 로직은 X)
2. Firebase 서버에서 키를 만들어 스마트폰에 전달함. 키가 전달되면 시스템은 인텐트를 발생해 앱의 서비스를 구동함. 그러면 개발자가 작성한 서비스가 실행되고 서비스 내에서 키값을 획득함.
3. 앱에 전달된 키를 서버에 전송. 실시간 데이터 푸시 기능이 필요한 곳은 서버이며, 서버에서 전달받은 키를 이용하여 데이터를 전송
4. 서버에서는 전달받은 키값을 영속화함(보통 DB에 저장). 이렇게 하면 서DB에는 앱이 깔린 모든 클라이언트 스마트폰의 키가 저장됨
### 서버에서 데이터를 스마트폰에 전달하는 절차
1. 서버에서 데이터를 스마트폰에 전송하기위한 키(스마트폰에 설치된 앱을 식별할 수 있는 유일성이 확보된 값)를 DB에서 획득함.
2. DB의 키와 실제 앱에 전송하고자 하는 데이터를 Firebase 서버에 전달함. Firebase 서버와는 HTTP 통신이 이용되며 Firebase 서버에서 원하는 방식대로 데이터를 구성하여 요청이 이루어짐
3. Firebase 서버에서는 전달받은 키값을 식별해 어떤 스마트폰의 어떤 앱인지 식별함. 그리고  식별해낸 스마트폰으로 데이터를 전달하면 개발자가 만들어둔 데이터를 받기 위한 서비스가 실행되어 데이터를 획득함.
*** 
## 메시지 유형
>FCM을 통해 2가지 유형의 메시지를 받을 수 있음
>- 알림 메시지(notification) "표시 메시지"로 간주됨. FCM SDK에서 자동으로 처리함
>   >* 사용자에게 표시되는 키 모음이 사전 
정의되어 있음
>   >* 데이터 페이로드(전송의 근본적인 목적이 되는 데이터의 일부분, 헤더&메타데이터와 같은 데이터는 제외)를 추가할 수 있음.
>- 데이터 메시지(data) 클라이언트 앱에서 처리함
>   >* 사용자가 정의한 키-값 쌍만 포함됨
>   >