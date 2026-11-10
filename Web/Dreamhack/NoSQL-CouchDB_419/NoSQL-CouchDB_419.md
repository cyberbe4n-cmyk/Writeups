# 문제 설명
<img width="340" height="127" alt="image" src="https://github.com/user-attachments/assets/97cce044-d293-4c42-9a77-bf72241cd958" /><br>
# 목표
문제를 통해 Flag 획득<br>
# 코드 분석
```js
app.post('/auth', function(req, res) {
    users.get(req.body.uid, function(err, result) {
        if (err) { // err 값을 없앤다
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
