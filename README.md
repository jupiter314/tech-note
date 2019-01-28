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
