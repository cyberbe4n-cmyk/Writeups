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
