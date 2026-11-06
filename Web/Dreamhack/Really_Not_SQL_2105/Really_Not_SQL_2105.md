<img width="556" height="20" alt="image" src="https://github.com/user-attachments/assets/855d4a03-8eba-49ad-9216-ac4625eea1c0" /># 문제 설명
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

<img width="556" height="20" alt="image" src="https://github.com/user-attachments/assets/46b4b13b-94cb-498b-b5c4-69b1b5033faa" />
