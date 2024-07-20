

### 참고자료
---
* https://velog.io/@ichubtou/Spring-Boot-Web-Socket-%EA%B0%84%EB%8B%A8%ED%95%9C-%EC%B1%84%ED%8C%85-%EA%B8%B0%EB%8A%A5-%EA%B5%AC%ED%98%84
* https://yjkim-dev.tistory.com/65
* https://velog.io/@sunkyuj/Spring-%EC%9B%B9%EC%86%8C%EC%BC%93%EC%9C%BC%EB%A1%9C-%EC%8B%A4%EC%8B%9C%EA%B0%84-%EC%B1%84%ED%8C%85-%EA%B5%AC%ED%98%84



#### 웹소켓이란?
* 웹 소켓이란 두 프로그램 간의 메세지 교환을 위한 통신 방법
* 웹 소켓은 서버와 클라이언트간에 연결을 유지하여 언제든 양방향 통신 또는 데이터 전송이 가능하게 하는 기술
* Real-Time Web application 구현을 위해 널리 사용되어지고 있다.
	- 실시간 채팅, 주식 시세 업데이트, 실시간 통지 및 알림에 사용
	

#### WebSocket & HTTP의 특징과 차이점
- WebSocket은 **HTTP를 통해 최초 연결**되며, 이후 일정 시간이 지나면 HTTP 연결은 자동으로 끊어지고 webSocket connection은 유지된다.
- HTTP와 달리 WebSocket은 **stateful 프로토콜**이다.
- HTTP는 stateless하기 때문에 서버에 변경사항이 생겨도 클라이언트에서 요청을 하지 않으면 변경사항이 적용되지 않지만, WebSocket은 지속적으로 connection을 유지하기 때문에 **실시간으로 변경사항이 적용**된다.

- WebSocket은 **HTTP와 동일한 port(80)를 사용**한다.
- 웹어플리케이션에서 기존의 서버와 클라이언트 간의 통신은 대부분 HTTP를 통해 이루어 졌으며 HTTP는 Request/response기반의 **Stateless protocol**이다.  
    즉, 서버와 클라이언트 간의 Socket connection같은 영구적인 연결이 되어있지 않고 클라이언트 쪽에서 필요할 때 Request를 할 때만 서버가 Response를 하는 방식으로 통신이 진행되는 **한방향 통신**이다. 이럴 경우 서버 쪽 데이터가 업데이트 되더라도 클라이언트 쪽에는 화면은 Refresh하지 않는 한 변경된 데이터가 업데이트 되지 않는 문제가 발생한다.  
    
    대체 방법으로 Polling, Long Polling, Streaming 등이 있으나 이 모든 방법들이 HTTP를 통해 통신하기 때문에 Request, Response 둘 다 Header가 불필요하게 크다.
    
- Web Socket은 **Stateful protocol**이기 때문에 클라이언트와 한 번 연결이 되면 계속 같은 라인을 사용해서 통신하기 때문에 HTTP 사용 시 필요 없이 발생되는 HTTP와 TCP연결 트래픽을 피할 수 있다. 마지막으로 Web Socket은 HTTP와 같은 포트(80)을 사용하기에 기업용 어플리케이션에 적용할 때 방화벽은 재설정 하지 않아도 되는 장점이 있다.


#### WebSocket 사용 시 주의점

- WebSocket은 **stateful** 프로토콜로써 connection을 항상 유지해야하기 때문에 트래픽이 많은 경우 서버에 부담이 될 수 있다.
- 연결이 계속 유지되어야 하기 때문에, 연결이 끊어졌을 때 적절히 대응할 수 있어야 한다.


