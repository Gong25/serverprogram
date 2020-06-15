# 1 수행형 과제

## 1.1 목표 시스템의 환경 요구 사항에 대한 분석 능력 [p.5]
* 목표 시스템을 정하고, 그에 해당하는 환경을 분석해 보자.
  - 목표 시스템 : 관리자 웹 어플리케이션 시스템
  - 환경
    - 개발 언어 : Python, SQL, HTML5, CSS3
    - 개발 인원 및 기간 : 개발인원 - 1명, 개발 기간 - 1개월
    - 개발 HW 사양 : 프로세서 - AMD Ryzen 7 3700U 2.30 GHz / 메모리(RAM) - 16.0GB

## 1.2 개발 도구들의 역할에 대한 파악도 [p.7]
* 목표 시스템 개발에 활용한 도구들을 나열하고 역할에 설명해 보자.
```
  - Microsoft Visual Studio : 텍스트 편집기
  - Python3
  - Flask : Python 기반 웹 개발 프레임워크
```
## 1.3 형상관리 지침의 이해와 환경 구축 능력 [p.22]
* 형상관리(SCM)의 정의를 설명해 보자.
```
형상관리란 소프트웨어 개발과정에서 발생하는 변경사항을 버전별로 관리하기 위한 활동이다.
  1. 소프트웨어 변경사항을 파악하고 제어하며, 적절히 변경되고 있는지 확인하여 담당자에게 통보
  2. 프로젝트 생명주기 전 단계에서 수행하는 활동이며, 유지 보수단계에서도 수행된다.
  3. 형상관리를 함으로써, 개발 전체 비용을 줄이고, 개발 과정에서 발생하는 여러가지 문제점 발생요인이 최소화하도록 한다.
```

* 각자 형상관리툴에 이름과 이메일을 등록해 보자.(등록된 내용은 출력해서 붙여 넣기)
```console
hkit07@ubuntu:~/tinypetshop$ git config --list
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
remote.origin.url=https://github.com/luibelstudy/tinypetshop.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.master.remote=origin
branch.master.merge=refs/heads/master
```


# 2 실행형 과제

## 2.1 만들고자 하는 서비스의 서버프로그램 구현
![](https://github.com/Gong25/serverprogram/blob/master/index.png?raw=true)

## 2.2 서버프로그램에 대한 unit test 수행

### unit test 라이브러리 설치
* 설치
```console
$ pip3 install pytest
```

* 설치 결과
```console
Collecting pytest
  Downloading pytest-5.4.3-py3-none-any.whl (248 kB)
     |████████████████████████████████| 248 kB 244 kB/s
Collecting packaging
  Downloading packaging-20.4-py2.py3-none-any.whl (37 kB)
Collecting py>=1.5.0
  Downloading py-1.8.1-py2.py3-none-any.whl (83 kB)
     |████████████████████████████████| 83 kB 728 kB/s
Requirement already satisfied: attrs>=17.4.0 in /usr/lib/python3/dist-packages (from pytest) (19.3.0)
Collecting pluggy<1.0,>=0.12
  Downloading pluggy-0.13.1-py2.py3-none-any.whl (18 kB)
Collecting wcwidth
  Downloading wcwidth-0.2.4-py2.py3-none-any.whl (30 kB)
Requirement already satisfied: more-itertools>=4.0.0 in /usr/lib/python3/dist-packages (from pytest) (4.2.0)
Collecting pyparsing>=2.0.2
  Downloading pyparsing-2.4.7-py2.py3-none-any.whl (67 kB)
     |████████████████████████████████| 67 kB 1.2 MB/s
Requirement already satisfied: six in /usr/lib/python3/dist-packages (from packaging->pytest) (1.14.0)
Installing collected packages: pyparsing, packaging, py, pluggy, wcwidth, pytest
Successfully installed packaging-20.4 pluggy-0.13.1 py-1.8.1 pyparsing-2.4.7 pytest-5.4.3 wcwidth-0.2.4
```

### 테스트 파일 추가
* test_app.py
```python
import flask

app = flask.Flask(__name__)

with app.test_request_context('/'):
    assert flask.request.path == '/'
```

### unit test 실행

* 실행 방법
```console
$ pytest
```

* 실행 결과
```console
================================================= test session starts ==================================================
platform linux -- Python 3.8.2, pytest-5.4.3, py-1.8.1, pluggy-0.13.1
rootdir: /home/hkit07/tinypetshop
collected 0 items

================================================ no tests ran in 0.41s =================================================

```

## 2.3 수행형 과제에서 선정한 서비스에 대한 공통 모듈 구현
![](https://github.com/Gong25/serverprogram/blob/master/board.PNG?raw=true)

## 2.4 공통 모듈에 대한 unit test 수행

### unit test 라이브러리 설치

### 테스트 파일 추가
* test_app.py
```python
import flask

app = flask.Flask(__name__)

with app.test_request_context('/'):
    assert flask.request.path == '/'

with app.test_request_context('/getPosts.html?no=1'):
    assert flask.request.path == '/getPosts.html'
    assert flask.request.args['no'] == '1'
```

### unit test 실행

* 실행 방법
```console
$ pytest
```

* 실행 결과
```console
================================================= test session starts ==================================================
platform linux -- Python 3.8.2, pytest-5.4.3, py-1.8.1, pluggy-0.13.1
rootdir: /home/hkit07/tinypetshop
collected 0 items

================================================ no tests ran in 0.52s =================================================
```


## 2.5 잠재적 보안 취약성 제거를 위한 secret_key or CSRF Protection 적용

### csrf 라이브러리 설치
* 설치
```console
$ pip3 install Flask-WTF
```
* 설치 결과 
```console
Collecting Flask-WTF
  Downloading Flask_WTF-0.14.3-py2.py3-none-any.whl (13 kB)
Requirement already satisfied: Flask in /home/hkit07/.local/lib/python3.8/site-packages (from Flask-WTF) (1.1.2)
Collecting WTForms
  Downloading WTForms-2.3.1-py2.py3-none-any.whl (169 kB)
     |████████████████████████████████| 169 kB 360 kB/s
Requirement already satisfied: itsdangerous in /home/hkit07/.local/lib/python3.8/site-packages (from Flask-WTF) (1.1.0)
Requirement already satisfied: click>=5.1 in /usr/lib/python3/dist-packages (from Flask->Flask-WTF) (7.0)
Requirement already satisfied: Jinja2>=2.10.1 in /usr/lib/python3/dist-packages (from Flask->Flask-WTF) (2.10.1)
Requirement already satisfied: Werkzeug>=0.15 in /home/hkit07/.local/lib/python3.8/site-packages (from Flask->Flask-WTF) (1.0.1)
Requirement already satisfied: MarkupSafe in /usr/lib/python3/dist-packages (from WTForms->Flask-WTF) (1.1.0)
Installing collected packages: WTForms, Flask-WTF
Successfully installed Flask-WTF-0.14.3 WTForms-2.3.1
```

### CSRF 적용
* app.py
```python
    :
    :
from flask_wtf.csrf import CSRFProtect

app = Flask(__name__)
app.secret_key = 'very secret'
csrf = CSRFProtect(app)
    :
    :
```
* getPosts.html
```html
{% extends "layout.html" %}

{% block section %}
<h2>게시물 리스트</h2>
<ul>
    {% for post in posts %}
    <li><a href="getPost.html?csrf_token={{ csrf_token() }}&no={{post.no}}">{{post.subject}}</a> - {{post.content}}</li>    {% endfor %}

</ul>
<a href="addPost.html">추가</a>
{% endblock %}
```

### 작동 확인
* 콘솔에서 확인
```console
 * Serving Flask app "app" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Running on http://0.0.0.0:10007/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 290-386-189
218.51.230.89 - - [15/Jun/2020 08:57:08] "GET / HTTP/1.1" 200 -
218.51.230.89 - - [15/Jun/2020 08:57:09] "GET /static/css/common.css HTTP/1.1" 200 -
218.51.230.89 - - [15/Jun/2020 08:57:09] "GET /static/img/kongpicture.jpg HTTP/1.1" 200 -
218.51.230.89 - - [15/Jun/2020 08:57:09] "GET /favicon.ico HTTP/1.1" 404 -
218.51.230.89 - - [15/Jun/2020 08:57:10] "GET /getPosts.html HTTP/1.1" 200 -
218.51.230.89 - - [15/Jun/2020 08:57:12] "GET /getPost.html?csrf_token=IjM1YmFlZGEzODVmMjFiZmFjODFjM2FkYzVjMGNkMWMyNTIyZDljZjYi.Xuc35g.qXKaIcbfs6GDTDmZ_b4Kvf5TxAI&no=1 HTTP/1.1" 200 -
```
* 브라우저에서 확인
[본인 화면 복사]
[샘플]
![](https://github.com/luibelstudy/hkit_2020_gonggong/blob/master/3.%20%EC%84%9C%EB%B2%84%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8%20%EA%B5%AC%ED%98%84/%ED%8F%89%EA%B0%80/hkit01.PNG?raw=true)
