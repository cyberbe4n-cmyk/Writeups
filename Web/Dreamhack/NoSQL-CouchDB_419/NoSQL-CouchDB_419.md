# 문제 설명
<img width="340" height="127" alt="image" src="https://github.com/user-attachments/assets/97cce044-d293-4c42-9a77-bf72241cd958" /><br>
# 목표
문제를 통해 Flag 획득<br>
# 공격 표면 분석
```js
app.post('/auth', function(req, res) {
    users.get(req.body.uid, function(err, result) {
        if (err) {
            console.log(err); 
            res.send('error');
            return;
        }
        if (result.upw === req.body.upw) {
            res.send(`FLAG: ${process.env.FLAG}`);
        } else {
            res.send('fail');
        }
    });
});
```
사용자가 입력한 uid 값이 CouchDB에서 그대로 사용되는 것을 확인할 수 있으며 여기에 대한 검증이 전혀 없다.<br>

CouchDB에는 다양한 문서가 존재하며 대표적으로는 `_all_docs` 같은 특수 문서가 있다.<br>
참고 : https://docs.couchdb.org/en/stable/api/index.html
## 인증 로직
```js
if (result.upw === req.body.upw) {
    res.send(`FLAG: ${process.env.FLAG}`);
} else {
    res.send('fail');
```
`_all_docs` 는 일반 사용자 계정 문서가 아니기 때문에 upw가 필요하지 않으며, JS에서 존재하지 않는 프로퍼티에 접근하면 에러가 아니라 undefined가 나온다.
```js
result.upw  // undefined
```
그러면 조건식은 아래와 같이 축소된다.
```js
undefined === req.body.upw
```
위 조건식이 `true`가 되려면 `req.body.upw`의 값도 `undefined`가 되여야한다.
```js
result.upw        // undefined (응답 JSON에 upw가 없어서)
req.body.upw      // undefined (요청에 upw를 안 보내서)
undefined === undefined   // true
```
조건이 참이 되어 `res.send('FLAG: ...')`가 실행된다.

# 익스플로잇
F12(관리자 도구)로 쉽게 요청을 보낼 수 있지만 보기 쉽게 Burp Suite 도구를 사용했다.<br>
<img width="1187" height="401" alt="image" src="https://github.com/user-attachments/assets/ab93b866-21a3-49e5-8d5b-2da46e77acc8" /><br>
### 결과 
Flag : DH{f350aad835d053891385b1bb9cfbc1c318ab29f0}
