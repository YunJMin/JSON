# **1. REST API의 탄생**
REST는 Representational State Transfer라는 용어의 약자
# **2. REST의 특징**
- 자원(RESOURCE) - URI
- 행위(Verb) - HTTP METHOD
- 표현(Representations)

##    2-1 Uniform (유니폼 인터페이스)
        - URI로 지정한 리소스에 대한 조작을 통일
        - 한정적인 인터페이스로 수행하는 아키텍쳐 스타일을 말함
##    2-2 Stateless (무상태성)
        - 작업을 위한 상태정보를 따로 저장, 관리X
        - 서비스의 자유도가 ↑, 서버에서 불필요한 정보를 관리X -> 구현이 단순
##    2-3 Cacheable (캐시 가능)
        - HTTP라는 기존 웹표준을 그대로 사용
        - 캐싱 기능 적용 가능
            (캐싱 - 일시적인 특징이 있는 데이터 하위 집합을 저장하는 고속 데이터 스토리지)

##    2-4 Self-descriptiveness (자체 표현 구조)
        - REST API 메세지만 보고도 쉽게 이해 할 수 있는 자체 표현구조

##    2-5 Client - Server 구조
        - REST 서버 -> API 제공
        - 사용자 인증 or 컨텍스트(세션, 로그인 정보)를 직접 관리하는 구조
        - 역할이 확실히 구분되있음

##    2-6 계층형 구조
        - REST 서버는 다중 계층으로 구성될 수 있음
        - 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성↑
        - 네트웨크 기반의 중간매체를 사용할 수 있음
            ex) PROXY, 게이트웨이
# **3. REST API 디자인 가이드**
  1) URI는 정보의 자원을 표현(리소스명은 동사보다는 명사를 사용)
<br>2) 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELEDTE) 표현

        
##    3-1 REST API 중심 규칙
### 1-1) GET /members/delete/1
- 위 방식은 REST를 제대로 적용하지 않은 URI
- URI는 자원을 표현하는데 중점을 두어야함
- delete는 X
### 2-2) DELETE /members/1 
<br>(위 잘못된 URI를 HTTP Method를 통해 수정)
- 회원정보를 가져올때 GET
- 회원 추가 시의 행위를 표현하고자 할 때는 POST METHOD 사용
**1. 회원정보를 가져오는 URI**
- GET /members/show/1   (x)
- GET /members/1        (o)

**2. 회원을 추가할 때**
- GET /members/insert/2 (x)  - GET 메서드는 리소스 생성에 맞지 않습니다.
- POST /members/2       (o)
1. POST 
        - POST를 통해 해당 URI를 요청하면 리소스를 생성
2. GET 
        - GET를 통해 해당 리소스를 조회
        - 리소스를 조회, 해당 도큐먼트에 대한 자세한 정보를 가져옴
3. PUT
        - PUT를 통해 해당 리소스를 수정
4. DELEDTE
        - DELEDTE를 통해 리소스를 삭제
    
    => URI는 자원을 표현하는데 집중, 행위에 대한 정의는 HTTP METHOD를 통해 하는 것이 REST한 API설계 중심 규칙

   
## 3-2 URI 설계 시 주의할 점
### 1) 슬래시 구분자(/)는 계층 관계를 나타내는데 사용
- http://restapi.example.com/houses/apartments
- http://restapi.example.com/animals/mammals/whales
### 2) URI 마지막 문자로 슬래시(/)를 포함하지 않음
- URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야함
- URI가 다르다는 것은 리소스가 다르다는 것, 역으로 리소스가 다르면 URI도 달라져야함
  - http://restapi.example.com/houses/apartments/ (X)
  - http://restapi.example.com/houses/apartments  (0)
> URI를 만들어 통신을 해야 하기 때문에 혼동을 주지 않도록 URI 경로의 마지막에는 슬래시(/) 사용X

        
### 3) 하이픈(-)은 URI 가독성을 높이는 사용
- 불가피하게 긴 URI경로를 사용하게 된다면 하이픈을 사용해 가독성을 높일 수 있음


### 4) 밑줄(_)은 URI에 사용하지X
- 밑줄은 문자를 가림 -> 하이픈 사용(가독성↑)
### 5) URI 경로에는 소문자가 적합
- URI 경로에는 대문자 사용 피하기

        
### 6) 파일 확장자는 URI에 포함시키지X
- http://restapi.example.com/members/soccer/345/photo.jpg (X)
## 3-3 리소스 간의 관계를 표현하는 방법
- REST 리소스 간에는 연관 관계가 있을 수 있음
- /리소스명/리소스 ID/관계가 있는 다른 리소스명
> ex) GET : /users/{userid}/devices (일반적으로 소유 ‘has’의 관계를 표현할 때)
## 3-4 자원을 표현하는 Collection과 Document
        - DOCUMENT는 문서로 이해해도 됨 -> 한 객체
        - COLLECTION은 객체들의 집합
            ex) http:// restapi.example.com/sports/soccer
                -> sports라는 컬렉션과 soccer라는 도큐먼트로 표현
                    ex) http:// restapi.example.com/sports/soccer/players/13
                        -> sports, players 컬렉션과 soccer, 13번 선수를 의미하는 도큐먼트로 URI가 이루어짐
  => 컬렉션 복수로 사용하고 있음
<br>=> 컬렉션과 도큐먼트를 사용할 때 단수 복수도 지켜준다면 URI를 설계할 수 있음
# **4. HTTP 응답 상태 코드**

> 200: 클라이언트의 요청을 정상적으로 수행함

> 201: 클라이언트가 어떠한 리소스 생성을 요청, 해당 리소스가 성공적으로 생상됨
<br>(POST를 통한 리소스 생성 작업 시)

> 400: 클라이언트의 요청이 부적절 할 경우 사용

> 401: 클라이언트가 인증되지 않은 상태에서 보호된 리소스를 요청했을때
<br>(로그인 하지 않은 유저가 로그인 했을 때, 요청 가능항 리소스를 요청했을 떄)

> 403: 유저 인증상태와 관계 없이 응답하고 싶지 않은 리소스를 클라이언트가 요청했을때

> 405: 클라이언트가 요청한 리소스에서 사용 불가능한 Method를 이용했을 경우
<br>(응답 시 Location header에 변경된 URI를 적어야함)

> 500: 서버에 문제가 있을 경우