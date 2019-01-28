# tech-note
### OAuth

서비스의 인증 및 권한부여를 관리하는 프레임워크. 

서버와 클라이언트 사이에 인증을 완료하면 서버에서 권한부여의 결과로 access token을 전송.

클라이언트는 access token을 이용하여 서비스에 접근.

```
     +--------+                               +---------------+
     |        |--(A)- Authorization Request ->|   Resource    |
     |        |                               |     Owner     |
     |        |<-(B)-- Authorization Grant ---|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(C)-- Authorization Grant -->| Authorization |
     | Client |                               |     Server    |
     |        |<-(D)----- Access Token -------|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(E)----- Access Token ------>|    Resource   |
     |        |                               |     Server    |
     |        |<-(F)--- Protected Resource ---|               |
     +--------+                               +---------------+
```

(B) 단계에서 Authorization Grant에는 4가지 타입이 있음.

- Authorization Code: Resource Owner가 Authorization Server에서 인증을 받고 Authorization Code를 Client에게 발급. Client는 Authorization Code를 Authorization Server에 넘기고 access token을 넘겨받음.
- Implicit: Authorization Code를 간소화한 절차로, Authorization Code를 발급하지 않고 바로 access token을 발급.
- Resource Owner Password Credentials: Resource Owner의 아이디와 비밀번호가 Authorization Grant로 사용. access token 취득 후 Client에서 아이디, 비밀번호를 보관할 필요는 없음.
- Client Credentials: Resource Owner가 유저가 아닌, Client인 상황에서 활용. Client가 직접 인증정보를 Authorization Server에 보내 access token을 발급 받음.


### JWT

JSON Web Token으로 OAuth에 사용되는 토큰중 하나로 토큰 자체에 정보들을 갖고 있다.

#### **토큰 구성**

header.payload.signature

**header 구성**

- typ: 토큰의 타입을 지정(JWT)
- alg: 해싱 알고리즘을 지정. default(SHA256)

예) 

```
{
  "typ": "JWT",
  "alg": "HS256"
}
```

**payload 구성**

- 등록된 클레임
  - iss: 토큰 발급자
  - sub: 토큰 제목
  - aud: 토큰 대상자
  - exp: 토큰 만료시간, 시간은 NumericDate 형식
  - nbf: Not Before, 이 시간 이전에는 토큰을 처리하지 않아야함을 의미. NumericDate
  - iat: 토큰이 발급된 시간. NumericDate
  - jti: JWT 고유 식별자로, 중복적인 처리를 방지하기 위하여 사용됨. 일회용 토큰에 유용.
- 공개 클레임: IANA에 등록하여 사용하기에 naming이 중복되는것을 방지하기 위해 일반적으로 URI 형태로 구성
- 비공개 클레임: 클라이언트와 서버간의 협의하에 사용되는 네임들.

예) 

```
{
    "iss": "velopert.com",
    "exp": "1485270000000",
    "https://velopert.com/jwt_claims/is_admin": true,
    "userId": "11028373727102",
    "username": "velopert"
}
```

**signature 구성**

- SHA256(Base64Encode(header) + "." + Base64Encode(payload), secret)



결과 Token

Base64Encode(header)  + "." + 

Base64Encode(payload) + "." + 

Base64Encode(SHA256(Base64Encode(header) + "." + Base64Encode(payload), secret))



**※ base64 인코딩시 패딩문자(=)가 붙는 경우가 있는데 URL 파라미터로 token을 날릴경우 url-safe하지 않으므로 패딩문자를 제거해줘야 한다. 제거해도 디코딩시 전혀 문제가 되지 않음.**
