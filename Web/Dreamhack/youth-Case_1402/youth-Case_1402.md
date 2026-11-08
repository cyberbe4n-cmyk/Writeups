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
`/shop`과 `/shop/` 경로로 오는 모든 요청을 거부한다.

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
