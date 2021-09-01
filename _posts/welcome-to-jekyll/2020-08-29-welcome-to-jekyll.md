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

1. <sup id="user">[[1]](#user-ref)</sup><kbd>Blind SQL</kbd>인젝션은 쿼리의 결과를 참과 거짓으로만 출력하는 페이지에서 사용하는 공격이다.
<br>출력 내용이 참 / 거짓 밖에 없어서 그것을 이용하여 데이터베이스의 내용을 추측하여 쿼리를 조작한다.

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fx1H93%2FbtqG9ST9EIX%2FX85GJhTbLbt2mkNoXKDm90%2Fimg.png
" alt="Weather API Web Sites - Main">
<figcaption>Fig 1. 공격 흐름도</figcaption>
</figure>


이제 저는 Blind SQL Injection 중에서 <sup id="user">[[2]](#user-ref)</sup>`Bollean Based`를 이용하여 게시판에서 공격해보도록 하겠습니다. 

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




<hr>

# 4. 프로젝트 결과

<hr>

# 5. 대응방안

<hr>

##### Notes
<small id="user-ref"><sup>[[1]](#user)</sup> Blind SQL 인젝션의 쿼리문에 사용하는 함수는 substring,ascii,limit 등이 있다.<br>
substring함수는 첫 번째 인자로 받은 문자열을 지정한 길이만큼 출력하는데, 주로 문자 하나씩 출력 하여 이름을 알아내는데에 사용한다.
<br>ascii함수는 문자를 아스키코드로 변환하는데, 작은따옴표(')를 우회하는 변수일 때 문자를 입력하기 위하여 사용한다.
limit 함수는 문자열의 길이를 반환한다. 문장열의 길이를 알아내면 substring 함수로 문자열을 추측하기 쉬워진다.  세 함수를 통해 임의의 값을 입력하며 데이터베이스 내용을 알아낼 때까지 계속 비교한다.</small>
<br>
<br>
<small id="user-ref"><sup>[[2]](#user)</sup> 참과 거짓만 출력하는 페이지에 공격자가 조작한 쿼리로 인해 데이터베이스 내용을 노출하는 취약점이다.  쿼리는 데이터베이스 내용이 일치하여 웹 페이지에서 참을 출력시킬 때까지 임의의 값을 대입한다.</small>

<hr>

# 6. 참고자료
https://lucete1230-cyberpolice.tistory.com/94