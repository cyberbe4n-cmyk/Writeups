# 문제 설명
<img width="224" height="112" alt="image" src="https://github.com/user-attachments/assets/82c28276-dbfe-4c23-9358-a50c592166cd" />

# 목표
문제를 통해 Flag 획득

# 코드 분석
먼저 `.htaccesss` 파일을 분석했다.
<img width="233" height="60" alt="image" src="https://github.com/user-attachments/assets/df854bf2-4f24-479a-987c-a04d18457c07" />

```conf
<VirtualHost *:80>
  ServerAdmin webmaster@localhost
	DocumentRoot /var/www/html

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

   <Directory /var/www/html/>
     AllowOverride None
     Require all granted
   </Directory>

  <Directory /var/www/html/user/>
      DAV On
      Options Indexes
      AllowOverride All
      Require all granted
  </Directory>
</VirtualHost>
```
