---

# CGI (Common Gateway Interface)

- 언어 와 상관없이, 웹서버와 언어들 사이에 약속, 규약


공용 게이트웨이 인터페이스(영어: Common Gateway Interface; CGI)는 웹 서버 상에서 사용자 프로그램을 동작시키기 위한 조합이다.

존재하는 많은 웹 서버 프로그램은 CGI의 기능을 이용할 수 있다.

웹 서버 프로그램의 기능의 주체는 미리 준비된 정보를 이용자(클라이언트)의 요구에 응답해 보내는 것이다. 

그 때문에 서버 프로그램 그룹에서는 정보를 그 장소에서 동적으로 생성하고 클라이언트에 송신하려하는 조합을 작성하는 것이 불가능했다. 

서버 프로그램에서 다른 프로그램을 불러내고, 그 처리 결과를 클라이언트에 송신하는 방법이 고안되었다. 


- 이를 실현하기 위한 **서버 프로그램과 외부 프로그램과의 연계법을 정한 것이 CGI**이다.



---

<img src="https://upload.wikimedia.org/wikipedia/commons/6/61/CGI_common_gateway_interface.svg" width = 500 height = 500> 


---

![image.png](attachment:image.png)

# WEB2 - CGI - Python


- 기존 고정적인 HTML 수정 

- 효율적인 자원 관리, 다이나믹, 상호작용

- index.py Application 파일이 Web 에서 구동되고 있으므로,  Web Application 개발

 

---

## CGI로 홈페이지를 출력


Web Browser - Web Server - CGI - Python - file


- 출력 헤더 : `print("content-type:text/html; charset=utf-8\n")`

- 웹 브라우저 가 웹 서버에 요청할 때, 웹 서버는 웹 페이지를 응답하면서, 웹 페이지가 어떤 데이터 인지 헤더를 통해 알려준다.

- 웹 브라우저는 응답 받은 데이터를 html 로 처리 합니다.



![image.png](attachment:image.png)

---

##  URL query string을 가져오는 방법


QUERY_STRING 

- URL의 query string을 입력 값으로, 웹 애플리케이션으로 끌어오기


- 접속 URL에 ?id가 포함되어 있어야 오류없이 동작


---

### `?id`


```.py


# form["id"].value 명령 시, ?id를 포함되도록 웹 애플리케이션 개발, 접속


import cgi
form = cgi.FieldStorage()
pageId = form["id"].value 


# hypertext reference를 ?id 로 변경

<body>
  <h1><a href="index.py">WEB</a></h1>
  <ol>
    <li><a href="index.py?id=HTML">HTML</a></li>
    <li><a href="index.py?id=CSS">CSS</a></li>
    <li><a href="index.py?id=JavaScript">JavaScript</a></li>
  </ol>
  
  <h2>WEB</h2>
  
  <p>The World Wide Web (abbreviated WWW or the Web) is an information space where documents and other web resources are identified by Uniform Resource Locators (URLs), interlinked by hypertext links, and can be accessed via the Internet.[1] English scientist Tim Berners-Lee invented the World Wide Web in 1989. He wrote the first web browser computer program in 1990 while employed at CERN in Switzerland.[2][3] The Web browser was released outside of CERN in 1991, first to other research institutions starting in January 1991 and to the general public on the Internet in August 1991.
  </p>
</body>
```



---

## title format

- URL query string에 id 값 여부에 따라서 동작하는 웹 앱을 구현

```.py
import cgi

form = cgi.FieldStorage()

if 'id' in form:
    pageId = form['id'].value
else:
    pageId = 'Welcome'


<h2>{title}</h2>


.format(title=pageId)

```

---

## body format

- 본문 내용을 태그로 변경, 파일 제어


- 본문의 내용을 별도의 파일로 저장하고, 파이썬의 파일 제어 기능을 이용해서, 파일을 읽어서 본문의 내용을 자동으로 생성하는 기능을 구현


```index.py


# data 파일에서 'pageId' 에 해당하는 파일을 읽기 모드로 오픈
# read 파일 전체 한꺼번에 read 

description = open('data/'+pageId, 'r').read()



<p> "문단 내용" </p>

```


```.py 

<p> {desc} </p>
 
.format(desc = description)

```


---

## dir 내 파일 목록 리스트로 변경,  \<li> 태그로 연결 하여 글 목록 구현


- `os.listdir()` : dir 내 데이터 목록 리스트화 
 
 
- 태그로 하나씩 단순 구현 했던 기존 글 목록

- 폴더 명에 맞춰 웹 앱에 맞게 구현

- index.py 파일 하나로 모든 웹페이지 데이터 포멧을 처리할수 있습니다.
    - data 폴더명에 맞춰 개발 하여, 글 을 추가 삭제할 때, data 폴더 안에서 해결 가능하게 하였다.

---

---

```.py

# dir내 파일 목록 리스트화 

import os
files = os.listdir('data')

print(files) # ['CSS', 'HTML', 'JavaScript'] 


```


---

```.py

# dir내 파일 목록들을 연결 해서 문자열로 출력


import os
files = os.listdir('data')

lst_str = ''

for i in files: # ['CSS', 'HTML', 'JavaScript'] 
  lst_str = lst_str + i

print(lst_str) # CSSHTMLJavaScript

```

---

```.py


# 반복문으로, 리스트 값을 꺼내, <li> 태그 로 만들기

import os
files = os.listdir('data')

lst_str = ''

for item in files:
  lst_str = lst_str + '<li><a href="index.py?id={name}">{name}</a></li>'.format(name=item)


# print(lst_str) print 해줘야 웹에 출력 됨


```

---



```.py

# 기존 목록 태그 

<ol>
    <li><a href="index.py?id=HTML">HTML</a></li>
    <li><a href="index.py?id=CSS">CSS</a></li>
    <li><a href="index.py?id=JavaScript">JavaScript</a></li>
</ol>
  
```

```.py


# 동적 목록 태그

<ol>

    {lst}

</ol>

,format(lst = lst_str)
```

---

## Create file

- Python 웹 애플리케이션에서 파일 생성 기능 구현


- 입력 데이터를 input 하기 위해서 create.py 작성, 해당하는 href를 이동 후, 처리



1. `index.py?id=` 에서 `create.py` url 이동

2. form 생성해서, 입력 데이터 `process_update.py`로 action


2. `create.py` 에서 form 생성해서, 입력 데이터 `process_create.py` 로 전송 (action)


3. `process_create.py`에서 write 모드로 파일 오픈 후, 입력 데이터를 불러와서, 파일 write 


4. `Redirction` 로 새로 생성한 `index.py?id=title` url 이동



### Form


- 사용자로 부터 정보를 입력 받는 양식 form


### method = 'get'

- 페이지 접속시 사용하는 default GET, url QUERY_STRING 생성자로, submit 시

`http://localhost:8080/process_create.py?title=CSI&description=CSI+is...`


---

### method = 'post'

- 사용자가 정보를 서버로 전송하는 하려면 method를 Post로 form 태그 작성 해야 함


```.py
  
# <form >태그 : 사용자 입력 데이터들을 특정한 CGI 애플리케이션(python)으로 전송하고 싶다면 
# - action = 데이터를 어디로 보낼 것 인가.

  <form action="process_create.py" method="post">
        
      <p><input type="text" name="title" placeholder="title"></p> # input - text 타입 
      
      <p><textarea rows="4" name="description" placeholder="description"></textarea></p>  # input text 여러줄
      
      <p><input type="submit"></p>       # input - submit

  </form>

```


- `name` : 전송시 데이터 name title, description


- `placeholder` : hint


- `submit` : 전송 버튼

---

![image.png](attachment:image.png)

---

### redirection


- html form을 이용해서 전송한 데이터를 Python 애플리케이션이 받아서 파일 생성 처리 후

- 생성한 파일로 url 로 이동 하려면 Redirection 해서, 이동


```.py
# Redirection Head
print("Location: index.py?id="+title)
print()


# 기존 헤드, 웹 서버가  웹 브라우저 한테 response 하면서  
# 받는 데이터는 text로 html로 작성된거야 알려주는 헤더
# print("content-type:text/html; charset=utf-8\n") 


```




---

## Update file

- Python 웹 애플리케이션에서 파일 수정 기능을 구현


- 업데이트를 위한 입력 데이터를 input 하기 위해서 update.py 작성, 해당하는 href를 이동 후, 처리


1. `http://localhost:8080/index.py?id=CGI` > `http://localhost:8080/update.py?id=CGI` 이동


2. form 생성해서, 입력 데이터 `process_update.py`로 전송 (action)
    - 이때 업데이트를 위한 pageId를 hidden 으로 포함하여, 전송

3. pageId에 해당하는 파일을, write 모드로 파일 오픈 후, 입력 데이터를 불러와서, 파일 write 
    - 이떄 file 제목 까지 변경 하도록 rename 

4. `Redirction` 로 새로 생성한 `index.py?id=title` url 이동


5. `http://localhost:8080/update.py?id=CGI2` 로 이동





---





1. 파일 수정을 하려면, 우선 해당 파일이 존재해야 합니다.


2. `pageId` ( `form['id'].value` ) 존재 여부에 따라 href 생성, 없으면 공백 처리 
  


```.py

# 파일이 존재 했을 때, link 클릭시 update.py 로 이동 ?id 에 따라

if 'id' in form:
    pageId = form['id'].value
    description = open('data/'+pageId, 'r').read()
    update_link = '<a href="update.py?id={}">update</a>'.format(pageId)

else:
    pageId = 'Welcome'
    description = 'Hello, web'
    update_link = ''
    

```


```.py

# 본문에 href 를 format 처리하여 삽입



<body>
  <h1><a href="index.py?id=WEB">WEB</a></h1>
  <ol>
  {lst}
  </ol>
  <a href="create.py">Create</a>
  
  {update_link}
  
  <h2>{title}</h2>
  <p>{desc}</p>
</body>
</html>
'''.format(title=pageId, desc = description, lst = lst_str, update_link = update_link))


```

---

## Delete file

- Python 웹 애플리케이션에서 파일 삭제 기능을 구현

1. form 생성하여, 바로 `process_delete.py`로 전송 (action)
    - pageId를 hidden 으로 전송
    - 기존 href를 이용하여, 입력을 위한 .py 링크 접속하여 처리 하는 것이 아니라

2. 파일 remove 후 Redirection 하여, `index.py` 홈 화면으로 이동


```index.py

form = cgi.FieldStorage()

if 'id' in form:
    pageId = form['id'].value
    description = open('data/'+pageId, 'r').read()
    update_link = '<a href="update.py?id={}">Update</a>'.format(pageId)
    
    # form 생성하여, 바로 process_delete.py로 액션 
    
    delete_action = '''
        <form action="process_delete.py" method="post">
            
            <input type="hidden" name="pageId" value="{}">
            
            <input type="submit" value="delete">
        </form>
        '''.format(pageId)


```
        

---

## 함수

Refactoring

- Python의 함수를 이용해서 서로 연관된 코드를 그룹핑 하여 간결성, 재사용성 높이기

- index.py 글 목록 리스트 코드 함수화 처리


```.index.py

# 기존 코드

import os

files = os.listdir('data')

listStr = ''
for item in files:
    listStr = listStr + '<li><a href="index.py?id={name}">{name}</a></li>'.format(name=item)
    

```


---

```.index.py


# 함수화


def getList():

    files = os.listdir('data')
    listStr = ''
    for item in files:
        listStr = listStr + '<li><a href="index.py?id={name}">{name}</a></li>'.format(name=item)
    return listStr
    

```





---

## 모듈


- Python의 변수,함수를 파일로 분리해서 모듈화

- index.py, create.py, update.py `getList()`함수 모듈화


view.py 생성 후 `import view` 하여, `view.getList()`로 refactorig



---

## 보안 (XSS), python HTML sanitizer

- 보안 공격의 사례 중 하나인 XSS(Cross-Site Scripting)
    - 입력 데이터를 코드 해석 하여, error 발생

-  '<' '>' 부분 문자로 해석하도록 replace 처리

```

description = description.replace('<', '&lt;')
description = description.replace('>', '&gt;')

```

---

## PyPI ( the Python Package Index ), 패키지 매니저 PIP

- 타인이 만든 소프트웨어를 자신의 소프트웨어에 부품으로서, 사용하기 위한 방법으로서 패키지 매니저 Pypi와 PIP


`pip3 install html-sanitizer`

- 설치 위치 : Requirement already satisfied: beautifulsoup4 in /Users/daeheehan/opt/anaconda3/lib/python3.8/site-packages (from html-sanitizer) (4.6.0)


---

## API(Applicatoin Programming Interface)


---

## Web Framework

우리는 웹서버와 파이썬를 연동하기 위해서 CGI라는 기술을 통로로 사용했습니다.


안타깝게도 CGI는 느리기 때문에 오늘날은 거의 사용하지 않습니다.
 
그런데 CGI는 뿐 아니고, FastCGI, WSGI를 이용해서, 직접 웹애플리케이션을 만드는 것을 여러가지 고충이 있습니다.

 

이런 고충을 극복할 수 있도록 도와주는 기술들을 Famework라고 합니다.

 

- 일군의 컴퓨터 공학자들은 웹개발 작업에서 **공통적으로 필요한 작업들** 만을 예리하게 도려내서, Famework(프래임웍크) 라는 것을 만들었습니다.
 

---

## DataBase

한편 우리는 정보를 파일에 저장하고 있습니다.

- 데이터베이스를 이용하면, 복잡한 데이터를 편리하게 다룰 수 있습니다.

많은 데이터를 빠르게 검색할 수 있습니다.

파일을 읽고 쓰는 코드만, 데이터베이스를 읽고 쓰는 코드로 바꾸면, 우리의 웹애플리케이션은

천재적인 엔지니어들이 인생을 갈아서 만든 정보 시스템인, 데이터베이스의 강력한 성능을 엔진으로 손쉽게 가질 수 있습니다.



---

## Crawling

크롤링(Crawling) 또는 스크래핑(Scraping)으로 혼동해서 쓰는 경우가 많이 있습니다. 

크롤링은 개인 혹은 단체에서 필요한 데이터가 있는 웹(Web)페이지의 구조를 분석하고 파악하여 긁어옵니다

- 필요한 것은, **웹페이지를 다운로드 하는 방법** 과 **다운로드 한 웹페이지를 분석하는 기술** 입니다.


Urllib과 같은 라이브러리를 이용하면 파일을 다운로드 받을 수 있습니다.

또 Beautifulsoup와 같은 라이브러리를 이용하면 HTML을 손쉽게 분석할 수 있습니다.


---
