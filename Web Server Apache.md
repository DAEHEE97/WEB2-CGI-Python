#  MacOS에서 Python, 내장 웹 서버 Apache를 연동해 웹 애플리케이션을 구동

---


## Web Server Apache httpd

Apache HTTP Server, also known as Apache httpd, is a popular open-source web server software that powers millions of websites around the world. It is developed and maintained by the Apache Software Foundation and is available for free. 




- httpd.conf 아파치 설정 파일 경로

    > The default ports have been set in`/usr/local/etc/httpd/httpd.conf` to 8080

    > DocumentRoot `/usr/local/var/www`




---


## Apache httpd 설치

1. openssl 설치
> `brew install openssl`


2. Brew에서 제공하는 Apache HTTPD (오픈 소스 기반 웹 서버)의 새 버전을 설치해야 합니다.
> `brew install httpd`


3. Apache 서버가 시작
> `brew services start httpd`


4. http://localhost:8080 브라우저에 접속하면 "It works!"라는 간단한 헤더가 표시되어야 합니다.
> <Directory "/usr/local/var/www"> index.html 실행 된 것

---

##  Apache와 Python을 CGI로 연동


5. httpd.conf (아파치 설정 파일), httpd.bak.conf 백업 파일 생성


6. cgid, cgi 활성화
```
<IfModule !mpm_prefork_module>
	LoadModule cgid_module lib/httpd/modules/mod_cgid.so
</IfModule>
<IfModule mpm_prefork_module>
	LoadModule cgi_module lib/httpd/modules/mod_cgi.so
</IfModule>
```


7. python 파일 에 관해서 CGI 실행

```
<Files *.py>
    Options ExecCGI
    AddHandler cgi-script .py
</Files>

```


8. Apache 서버 재시작 
> `brew services restart httpd` / `brew services stop httpd`



9. 이때 실행시키려는 .py 조건
     - `#!/usr/bin/env python3` 쉬뱅
     
     - `print("content-type:text/html; charset=utf-8\n")` 헤더 content type
     
     - `sudo chmod a+x .py`
     
     

---

## CLI (command-line Interface)

`/`
- 최상위 경로


`sudo chmod a+x helloworld.py`
- 실행 모드

