# ❔ 질문
1. URLSession에 대해 설명하세요 <br />
2. RESTAPI에 대해 설명하세요 <br />

# ✏️ 작성자
- [고사리, 릴리](#고사리-릴리)
- [아샌, 제인](#아샌-제인)
<br />

---

# 고사리, 릴리
## URLSession에 대해서 설명하시오.
URL로 데이터를 다운로드, 업로드 할 수 있는, 애플에서 제공하는 통신 API 입니다.
HTTP 통신 프로토콜을 사용하기 때문에 동일하게 Request와 Response 구조로 되어있습니다.

- Request: `URL 객체를 통해 직접 통신`하거나 `URLRequest 객체에 옵션을 부여`하여 통신한다.
- Response: `설정된 Task의 Completion Handler 형태로 response를 받`거나, `URLSessionDelegate를 통해 지정된 메서드를 호출하는 형태로 response를 받`는다.
<br />
<br />

## URLSessionConfiguration의 종류
1. `Default Session`: 기본적인 Session으로, 디스크 기반 캐싱을 지원하고 인증서를 사용자의 key chain에 저장합니다. 또한 쿠키도 저장합니다,
2. `Ephemeral Session`: 어떠한 데이터(cache, cookie, credential) 저장하지 않는 형태의 세션입니다.
3. `Background Session`: 앱이 종료된 이후에도 통신이 이뤄지는 것을 지원하는 세션입니다.
<br />
<br />

## URLSessionTask의 종류
1. `URLSessionDataTask`: 기본적인 데이터를 받는 경우, response데이터를 메모리 상에서 처리
2. `URLSessionUploadTask`: 파일 업로드 시 사용, 사용하기 편한 request body 제공
3. `URLSessionDownloadTask`: 실제 파일을 다운받아 디스크에 사용될때 사용
<br />
<br />

## SessionDelegate 는 무엇인가요?
Session의 task들은 공통된 delegate를 공유합니다. Seesion 레벨의 이벤트, Session life cycle 변화시 호출되는 메서드를 정의하고 있습니다.

기본적으로는 설정된 Task의 Completion Handler 형태로 response를 처리하게 된다.
그렇지 않고 Delegate를 사용하는 경우는 아래와 같다.

- 앱이 background 상태로 들어갈 때에도 파일 다운로드를 지원하도록 설정하고 싶을 때
- 인증과 캐싱을 default 옵션으로 사용하지 않을 때
- HTTP redirect를 핸들링할 때

```
💡 🚨유의할 점
앱이 종료되거나 명시적으로 session을 무효화하기전까지 `URLSession`은 `delegate` 를 강하게 참조한다. 따라서 사용하지 않을시엔 session을 명시적으로 무효화(`[finishTasksAndInvalidate()](https://developer.apple.com/documentation/foundation/urlsession/1407428-finishtasksandinvalidate)`
 호출 ) 해야한다. 그렇지 않으면 메모리 누수가 발생한다.
```
<br />
<br />

## URLSession의 기본 Life Cycle
1. Session configuration을 결정하고, Session을 생성한다.
2. 통신할 URL과 Request 객체를 설정한다.
3. 사용할 Task를 결정하고, 그에 맞는 Completion Handler나 Delegate 메소드들을 작성한다.
4. 해당 Task를 실행한다.
5. Task 완료 후 Completion Handler가 실행된다.
<br />
<br />

## 어떤 Thread에서 Task를 진행해야 하나요?
URLSession API는 thread-safe하기 때문에 어떤 쓰레드를 사용하여도 무방합니다.
<br />
<br />

## REST API에 대해 설명하시오.
Representational State Transfer API.

REST 소프트웨어 아키텍쳐를 준수한 API를 REST API, Restful API라고 말합니다.  REST 아키텍쳐는 클라이언트, 서버간 정보를 안전하고 효율적으로 교환하기 위한 규칙을 정의하고 있습니다. 이를 구현한 RESTful API는 유연하고 확장가능한 API로 다른 애플리케이션, 서버와 통신시 많이 사용되고 있습니다.

RESTful API는 아래 6가지 기준을 준수해야합니다.

1. **균일한 인터페이스** :
표준 형식으로 정보를 송수신합니다. 균일한 인터페이스에는 4가지 아키텍처 제약 조건이 있습니다.
    1. **자원의 식별** : 요청은 리소스를 식별해야 합니다. 이를 위해 균일한 리소스 식별자(URI)를 사용합니다.
    2. **메시지를 통한 리소스의 조작** : 클라이언트는 원하는 경우 리소스를 수정하거나 삭제하기에 충분한 정보를 리소스 표현에서 가지고 있습니다. 서버는 리소스를 자세히 설명하는 메타데이터를 전송하여 이 조건을 충족합니다.
    3. **자기서술적 메시지** : 각 메시지는 자신을 어떻게 처리해야 하는지에 대한 충분한 정보를 포함해야 한다. 이를 위해 서버는 클라이언트가 리소스를 적절하게 사용할 수 있는 방법에 대한 메타데이터가 포함된 명확한 메시지를 전송합니다.
    4. **애플리케이션의 상태에 대한 엔진으로서 하이퍼미디어** : 클라이언트는 작업을 완료하는 데 필요한 다른 모든 관련 리소스에 대한 정보를 수신합니다. 이를 위해 서버는 클라이언트가 더 많은 리소스를 동적으로 검색할 수 있도록 표현에 하이퍼링크를 넣어 전송합니다.
2. **무상태** :
각 요청 간 클라이언트의 정보가 서버에 저장되지 않습니다. 따라서 각 요청이 분리되어 있고 서로 연결되어 있지 않습니다.
3. **계층화 시스템** :
요청된 정보를 검색하는 데 관련된 서버(보안, 로드 밸런싱 등을 담당)의 각 유형을 여러 계층으로 계층화하고, 클라이언트가 볼 수 없는 상태로 유지합니다.
4. **캐시 가능성**
서버 응답 시간을 개선하기 위해 클라이언트 또는 중개자에 일부 응답을 저장하는 캐싱을 지원합니다.
5. **온디맨드 코드** :
요청을 받으면 서버에서 클라이언트로 실행 가능한 코드를 전송하여 클라이언트 기능을 확장할 수 있는 기능.
<br />
<br />

## REST API의 장점은?
확장가능하고 유연한 API를 설계할 수 있습니다.

- 무상태는 서버가 과거 클라이언트 요청 정보를 유지할 필요가 없기 때문에 서버 로드를 제거합니다. 잘 관리된 캐싱은 일부 클라이언트-서버 상호 작용을 부분적으로 또는 완전히 제거해 효율적입니다. 이러한 모든 기능은 성능을 저하시키는 통신 병목 현상을 일으키지 않으면서 확장성을 지원합니다.
- RESTful 웹 서비스는 완전한 **클라이언트-서버 분리**를 지원합니다. 각 부분이 독립적으로 발전할 수 있도록 다양한 서버 구성 요소를 단순화하고 분리합니다. 서버 애플리케이션의 플랫폼 또는 기술 변경은 클라이언트 애플리케이션에 영향을 주지 않습니다.
- REST API는 사용되는 기술과 독립성을 유지합니다. API 설계에 영향을 주지 않고 다양한 프로그래밍 언어로 클라이언트 및 서버 애플리케이션을 모두 작성할 수 있습니다.
<br />
<br />

## RESTful API 클라이언트 요청에는 무엇이 포함되어 있나요?
RESTful API에는 다음과 같은 주요 구성요소를 포함하는 요청이 필요합니다.

**고유 리소스 식별자**
서버는 고유한 리소스 식별자로 각 리소스를 식별합니다. REST 서비스의 경우 서버는 일반적으로 URL(Uniform Resource Locator)을 사용하여 리소스 식별을 수행합니다. URL은 리소스에 대한 경로를 지정합니다. URL은 웹페이지를 방문하기 위해 브라우저에 입력하는 웹 사이트 주소와 유사합니다. URL은 요청 엔드포인트라고도 하며 클라이언트가 요구하는 사항을 서버에 명확하게 지정합니다.

**메서드**
개발자는 종종 Hypertext Transfer Protocol(HTTP)을 사용하여 RESTful API를 구현합니다. HTTP 메서드는 리소스에 수행해야 하는 작업을 서버에 알려줍니다. GET, POST, PUT, DELETE
<br />
<br />

## REST API의 기본 수행 기능
- Create: `POST` 요청을 사용하여 리소스를 생성
- Read: `GET` 요청을 사용하여 리소스를 검색
- Update: `PUT` 요청을 사용하여 리소스를 업데이트
- Delete: `DELETE` 요청을 사용하여 리소스를 삭제

**HTTP 헤더**
요청 헤더는 클라이언트와 서버 간에 교환되는 메타데이터입니다. 예를 들어, 요청 헤더는 요청 및 응답의 형식을 나타내고 요청 상태 등에 대한 정보를 제공합니다.

- 데이터
REST API 요청에는 POST, PUT 및 기타 HTTP 메서드가 성공적으로 작동하기 위한 데이터가 포함될 수 있습니다.
- 파라미터
RESTful API 요청에는 수행해야 할 작업에 대한 자세한 정보를 서버에 제공하는 파라미터가 포함될 수 있습니다. 다음은 몇 가지 파라미터 유형입니다.
URL 세부정보를 지정하는 경로 파라미터.
리소스에 대한 추가 정보를 요청하는 쿼리 파라미터.
클라이언트를 빠르게 인증하는 쿠키 파라미터.
<br />
<br />

## Response로 돌아오는 상태코드를 종류별로 설명해주세요
`200`: 일반 성공 응답
`201`: POST 메서드 성공 응답
`400`: 서버가 처리할 수 없는 잘못된 요청
`404`: 리소스를 찾을 수 없음
`5XX`: 서버 오류

참고링크: [https://aws.amazon.com/ko/what-is/restful-api/](https://aws.amazon.com/ko/what-is/restful-api/)

[https://www.redhat.com/ko/topics/api/what-is-a-rest-api](https://www.redhat.com/ko/topics/api/what-is-a-rest-api)
<br />
<br />
<br />

---

# 아샌, 제인
## URLSession에 대해서 설명하시오.
앱과 서버 간의 데이터를 주고받기 위해서는 HTTP 프로토콜을 이용해야 하는데, URLSession은 iOS 앱에서 서버와 통신하기 위해 애플이 제공하는 API이다.

HTTP를 포함한 몇 가지 프로토콜을 지원하고 인증, 쿠키 관리, 캐시 관리 등을 지원한다.

기본적으로 request, response 구조를 가지고 있다.
<br />
<br />

## URLSession의 종류에는 무엇이 있나요?
기본적인 request에 사용하는 shared session이 있다.

configuration이 없어서 커스터마이징이 불가능하지만, 기본적인 기능을 사용하기에 좋다. 

그 외에는 모두 ****Session Configuration으로 커스터마이징 가능하다.****

- default session
    - shared session과 비슷하지만 configure가 가능하다.
    - delegate를 사용할 수도 있다.
- Ephemeral sessions
    - shared session과 비슷하지만 캐시, 쿠키, 증명서 등을 디스크에 기록하지 않는다.
- Background sessions
    - 백그라운드에서 앱이 실행중이지 않을 때 컨텐츠를 업로드, 다운로드 할 수 있다.
 
![Untitle](https://user-images.githubusercontent.com/83864058/187936826-df8ea3c1-c5eb-4a0e-83c0-ac6b146fff6e.png)
<br />
<br />

## dataTask를 생성하는 메서드의 종류에는 어떤 것이 있나요?
URLSession 인스턴스를 이용해서 하나 이상의 URLSessionDataTask를 생성할 수 있다.

- dataTask(with:) → URLSessionDataTask
- uploadTask(with:from:) → URLSessionUploadDataTask
- downloadTask(with:) → URLSessionDownloadDataTask
- streamTask(withHostName:port:) → URLSessionStreamTask

하나의 세션당 여러개의 dataTask 생성이 가능하다.

![Untitle](https://user-images.githubusercontent.com/83864058/187937116-c2eb41af-d0ac-4db8-a14a-5a486314c8e7.png)

[https://developer.apple.com/documentation/foundation/urlsessiontask](https://developer.apple.com/documentation/foundation/urlsessiontask)
<br />
<br />

## session delegate는 어떤 경우에 사용하나요?
- Authentication 실패시
- 서버에서 데이터가 왔을 때
- 데이터가 캐싱이 가능해질 때 등

다양한 이벤트들을 처리할 수 있는 메서드를 제공한다.

업로드/다운로드 등의 진행상황을 실시간으로 받을 수 있다. 

![스크린샷 2022-05-30 오전 11.40.42.png](https://user-images.githubusercontent.com/83864058/187937291-d3430bb0-56e6-4f76-a50b-23df0be9e06d.png)

[https://developer.apple.com/documentation/foundation/urlsessiondatadelegate](https://developer.apple.com/documentation/foundation/urlsessiondatadelegate)
<br />
<br />

## REST API에 대해 설명하시오.
Representational State Transfer의 약자로서 두 컴퓨터 시스템이 인터넷을 통해 정보를 안전하게 교환하기 위해 사용하는 인터페이스이다.

- 균일한 인터페이스
    - 서버가 표준 형식으로 정보를 전송
- **무상태**
    - 서버가 이전의 모든 요청과 독립적으로 모든 클라이언트 요청을 완료하는 통신 방법
- 계층화 시스템
    - 클라이언트는 클라이언트와 서버 사이의 다른 승인된 중개자에게 연결할 수 있으며 여전히 서버로부터도 응답을 받습니다.
- 캐시 가능성
- 온디맨드 코드

[RESTful API란 무엇인가? - RESTful API 초보자 가이드 - AWS](https://aws.amazon.com/ko/what-is/restful-api/)
<br />
<br />

## REST 구성 요소에는 뭐가 있나요?
- 자원(Resource): URL
    
    > 모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재한다.
    자원을 구별하는 ID는 ‘/groups/:group_id’와 같은 HTTP URI 다.
    Client는 URI를 이용해서 자원을 지정하고 해당 자원의 상태(정보)에 대한 조작을 Server에 요청한다.
    > 
- 행위(Verb): HTTP Method
    
    > HTTP 프로토콜의 Method를 사용한다.
    HTTP 프로토콜은 GET, POST, PUT, DELETE 와 같은 메서드를 제공한다.
    > 
- 표현(Representation of Resource)
    
    > Client가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 **응답**(Representation)을 보낸다.
    REST에서 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation으로 나타내어 질 수 있다.
    JSON 혹은 XML를 통해 데이터를 주고 받는 것이 일반적이다.
    > 

[https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)

클라이언트 요청 - URL, 메서드, 헤더

서버 응답 - 응답(200, 201, 400, 404..), 메시지 본문, 헤더
<br />
<br />

## API란 무엇인가요?
> 애플리케이션 프로그래밍 인터페이스(API)는 다른 소프트웨어 시스템과 통신하기 위해 따라야 하는 규칙을 정의한다. 
개발자는 다른 애플리케이션이 프로그래밍 방식으로 애플리케이션과 통신할 수 있도록 API를 표시하거나 생성합니다.
> 

[API(애플리케이션 프로그래밍 인터페이스)란 - 개념, 기능, 장점](https://www.redhat.com/ko/topics/api/what-are-application-programming-interfaces)

웹 API는 클라이언트와 리소스 사이의 게이트웨이라고 생각할 수 있다.

API는 **내부의 구현사항을 숨겨놓고 외부에서 사용할 때 필요한 것만 노출해둔 것**을 의미한다.

API가 처음 사용된 이유는 하드웨어 독립성 때문인데, 

서로 다른 하드웨어 부품에 각각 프로그래밍을 하기 보다는 추상화 계층을 통해 한번만 코드를 작성하여 여러 곳에서 공통적으로 사용이 가능하도록 하기 위해서이다.

따라서 서버에서 제공하는 Web API를 통해 각기 다른 기기(iOS, Android)에서 서버의 데이터를 읽어오거나 업데이트가 가능하다. 

REST는 API를 설계하는 인터페이스의 한 종류로서 네트워크 요청과 반응을 어떻게 저장할지에 대한 데이터 포맷중 하나이다. (이전에는 REST가 아니라 SOAP라는 데이터 포맷을 통해 XML을 주고 받았었다.)
<br />
<br />
<br />
