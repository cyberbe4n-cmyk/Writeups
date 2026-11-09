# 문제 설명
<img width="365" height="146" alt="image" src="https://github.com/user-attachments/assets/f3dcae2e-eecc-464f-9129-cabd28177dde" /><br>
### 목표
문제를 통해 **플래그(FLAG)** 획득<br>
# 코드 분석
## app.js
<img width="489" height="669" alt="image" src="https://github.com/user-attachments/assets/eb5376cf-1e93-45fd-8819-3aa783febbfc" /><br>
서버에서는 `/`와 `/shop` 두 개의 라우터가 존재하며, app.set('case sensitive routing', true)설정을 통해 URL 경로의 대소문자를 구분한다.<br>
`/shop`라우터는 POST 요청만 처리하며, `req.body.leg`를 통해 클라이언트가 전달한 leg 파라미터 값을 받는다
## nginx.conf
<img width="319" height="480" alt="image" src="https://github.com/user-attachments/assets/cc3012f9-fc1c-4d6b-963c-c674ee641152" /><br>
nginx 프록시가 `/shop`과 `/shop/` 경로로 오는 모든 요청을 거부한다.<br>
## ag.js
```js
module.exports = [
    {
        "id": 1,
        "name": "FLAG",
        "description": "DH{fake_flag}"
    },
    {
        "id": 2,
        "name": "DRAG",
        "description": "To pull something along forcefully, often on the ground or another surface, causing friction or resistance. It also refers to the delay in performance or response time."
    },
    {
        "id": 3,
        "name": "SLAG",
        "description": "The waste material produced by the smelting process, which involves separating metal from its ore. Slag is typically a mixture of metal oxides and silicon dioxide."
    },
    {
        "id": 4,
        "name": "SWAG",
        "description": "Refers to stylish confidence in one's appearance or demeanor. It can also mean promotional goods or items given away for free as a form of advertising."
    }
]
```
name이 "FLAG"이면 플래그를 획득한다.
# 공격 표면 분석
## ngnix 프록시 우회 방법
먼저 /shop으로 가는 모든 요청이 거부되므로, nginx 프록시부터 우회해야될 거 같다.
## docker-compose.yml
<img width="458" height="229" alt="image" src="https://github.com/user-attachments/assets/e58baa69-2ee7-48bb-aea1-ee893495538b" /><br>
도커 파일에서 nginx의 버전`1.22.0`을 확인할 수 있으므로 적절한 우회법을 찾아 요청을 보낼 것이다.

nginx는 `location = /shop { deny all; }` 로 `/shop` 경로를 차단하고 있었다. 이 설정은 요청 URI를 바이트 단위로 완전히 일치하는 경우에만 차단하는 방식이다
반면 백엔드는 내부적으로 `url.parse()`를 사용해 요청 경로를 파싱하는데, 이 함수는 레거시 호환성을 위해 NBSP(\xA0) 등 일부 공백류 문자를 경로 끝에서 자동으로 제거하는 특성이 있다.

<img width="368" height="227" alt="image" src="https://github.com/user-attachments/assets/e0dba7e2-0614-4db2-affb-84b7ac715b75" /><br>
참고 : https://hacktricks.wiki/pentesting-web/proxy-waf-protections-bypass.html<br>
위 사진은 nginx 버전에 정규화 과정에서 제거되지 않는 문자들에 대한 표이다.
위 문제는 nginx 1.22.0v 이므로 \xa0을 입력해서 우회할 수 있다.
## 서버 필터링 우회 방법
<img width="402" height="125" alt="image" src="https://github.com/user-attachments/assets/d6cde921-81f9-4408-b831-d6bc2c31847f" /><br>
이제 위 조건문만 우회하면 된다. 서버는 leg 파라미터 값을 `req.body.leg.toLowerCase()` 코드를 사용해 모두 소문자로 변경하기 때문에 이 코드를 우회할 거다.

<img width="311" height="700" alt="image" src="https://github.com/user-attachments/assets/8e69dbb3-32ac-408e-be0e-f14cee8b13d7" /><br>
참고 : https://dev.to/jagracey/hacking-github-s-auth-with-unicode-s-turkish-dotless-i-460n<br>
위 사진은 `toUppercase` 또는 `toLowercase` 시 유니코드에 따라 아스키 문자를 리턴하는 데이터들이며
이 방법을 사용해 우회할 수 있다.
`ﬂ`이랑 똑같이 생긴 유니코드가 `Fl`이라는 아스키 문자를 리턴하기 때문에 `ﬂ`이라는 문자로 우회할 수 있다.
# 익스플로잇
## nginx 우회
<img width="699" height="363" alt="image" src="https://github.com/user-attachments/assets/44fc2328-c3c5-4de5-9f0a-ad126fb7aca4" />
<img width="1106" height="368" alt="image" src="https://github.com/user-attachments/assets/f1ad7d6b-d818-4d6b-b614-609cc28dbe18" /><br>
Burp Suite에 Request-Hex에서 /shop뒤에 \xA0을 붙여 POST 요청을 보내주면 Access Denied가 뜨며, /shop 라우터에 성공적으로 요청을 보낸 것을 확인할 수 있다.
## 서버 필터링 우회
`ﬂ`유니코드 문자가 toLowerCase과정을 거치고도 `Fl`이라는 아스키 문자를 리턴하기 때문에 `ﬂ`이라는 문자로 우회했다.
<img width="1395" height="366" alt="image" src="https://github.com/user-attachments/assets/164ad1e8-91cf-40b0-9756-40de156343fc" />

