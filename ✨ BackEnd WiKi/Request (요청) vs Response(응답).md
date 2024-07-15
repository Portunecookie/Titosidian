Request(요청)와 Response(응답)는 웹 애플리케이션에서 클라이언트와 서버 간의 상호작용을 설명하는 중요한 개념입니다. 다음은 이 두 개념에 대한 자세한 설명입니다.

### Request (요청)

#### 정의

Request는 클라이언트(보통 웹 브라우저나 모바일 앱)가 서버에게 어떤 작업을 해달라고 보내는 메시지입니다. 클라이언트는 필요한 데이터를 서버에게 보내고, 서버는 요청을 처리하여 응답을 보냅니다.

#### 구성 요소

1. **HTTP 메서드:** 요청의 종류를 나타냅니다.
    
    - **GET:** 서버에서 데이터를 가져옵니다.
    - **POST:** 서버에 데이터를 보냅니다.
    - **PUT:** 서버의 데이터를 업데이트합니다.
    - **DELETE:** 서버의 데이터를 삭제합니다.
2. **URL:** 요청이 전달될 서버 자원의 주소입니다.
    
3. **Headers:** 요청에 대한 추가 정보를 포함합니다.
    
    - 예: `Content-Type`, `Authorization`, `User-Agent` 등.
4. **Body:** (POST, PUT 요청에 주로 사용됨) 클라이언트가 서버로 보내는 데이터입니다.
    
    - 예: JSON 형식의 데이터, 폼 데이터 등.

#### 예시

http

코드 복사

`POST /login HTTP/1.1 Host: www.example.com Content-Type: application/json {   "username": "user123",   "password": "pass123" }`

- **메서드:** POST
- **URL:** /login
- **Headers:** Content-Type: application/json
- **Body:** {"username": "user123", "password": "pass123"}

### Response (응답)

#### 정의

Response는 서버가 클라이언트의 요청을 처리한 후 보내는 메시지입니다. 응답은 요청에 대한 처리 결과와 함께 클라이언트에게 전달됩니다.

#### 구성 요소

1. **Status Code:** 요청 처리 결과를 나타내는 숫자 코드입니다.
    
    - **200 OK:** 요청이 성공적으로 처리되었습니다.
    - **201 Created:** 새로운 리소스가 생성되었습니다.
    - **400 Bad Request:** 잘못된 요청입니다.
    - **401 Unauthorized:** 인증이 필요합니다.
    - **404 Not Found:** 요청한 리소스를 찾을 수 없습니다.
    - **500 Internal Server Error:** 서버에서 오류가 발생했습니다.
2. **Headers:** 응답에 대한 추가 정보를 포함합니다.
    
    - 예: `Content-Type`, `Set-Cookie` 등.
3. **Body:** 서버가 클라이언트에게 보내는 데이터입니다.
    
    - 예: JSON 형식의 데이터, HTML 문서 등.

#### 예시

http

코드 복사

`HTTP/1.1 200 OK Content-Type: application/json {   "message": "Login successful",   "token": "abc123" }`

- **Status Code:** 200 OK
- **Headers:** Content-Type: application/json
- **Body:** {"message": "Login successful", "token": "abc123"}

### 예시 시나리오

1. **클라이언트 요청 (Request)**
    
    - 사용자가 로그인 폼에서 아이디와 비밀번호를 입력하고 "로그인" 버튼을 누릅니다.
    - 클라이언트는 다음과 같은 요청을 서버로 보냅니다.
        
        http
        
        코드 복사
        
        `POST /login HTTP/1.1 Host: www.example.com Content-Type: application/json {   "username": "user123",   "password": "pass123" }`
        
2. **서버 응답 (Response)**
    
    - 서버는 요청을 받아 사용자의 아이디와 비밀번호를 검증합니다.
    - 검증이 성공하면, 서버는 다음과 같은 응답을 클라이언트에게 보냅니다.
        
        http
        
        코드 복사
        
        `HTTP/1.1 200 OK Content-Type: application/json {   "message": "Login successful",   "token": "abc123" }`
        

### 요약

- **Request:** 클라이언트가 서버에 작업을 요청하는 메시지입니다. 주로 HTTP 메서드, URL, 헤더, 바디로 구성됩니다.
- **Response:** 서버가 클라이언트의 요청에 응답하는 메시지입니다. 주로 상태 코드, 헤더, 바디로 구성됩니다.

이 두 가지 개념을 이해하면 클라이언트와 서버 간의 상호작용을 더 잘 이해할 수 있습니다.

---
| @eunsoA, 2024.07.