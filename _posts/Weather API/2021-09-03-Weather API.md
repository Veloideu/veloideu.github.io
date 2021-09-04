---
title: 🔆 Weather Api (html, php)
date: 2021-08-30 19:16:03 +07:00
# modified: 2021-08-30 19:16:03 +07:00
tags: [html, php, linux]
description: 네이버에서 날씨를 검색해 나오는 데이터 정보들을 쉽게(파싱) 사용할 수 있도록 간단하고 쉽게 제공해 주는 API를 만들어 보았습니다. "이 API는 JSON 포맷의 응답을 전송합니다."
image: "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcVNmm8%2FbtrdEjeFK6C%2FSb271UmoPaBwntvxpKDAOK%2Fimg.png"
---

# 1. 개요

PHP, HTML, CSS 언어 공부 (파싱)👇.

<hr>

## 1.1 API 목적

네이버에서 날씨를 검색해 나오는 데이터 정보들을 쉽게`(파싱)` 사용할 수 있도록 간단하고 쉽게 제공해 주는 API를 만들어 보았습니다. "이 API는 `JSON 포맷`의 응답을 전송합니다."

<hr>

# 2. 환경 구축

<span style="#03f3b3;color: red;">LAMP</span>
<br>L: Linux (이번 wordpress는 CentOS 7에서 진행합니다.)
<br>A: Apache 웹 서버
<br>M: MySQL 또는 MariaDB
<br>P: PHP
<br>Centos7 & Apache/2.4.6 & 5.5.68-MariaDB & PHP 5.4.16

<hr>

# 3. 웹 서비스 시작
<figure>
<img src="https://blog.kakaocdn.net/dn/V1g3h/btrduU2e4i9/hQ5NRi4lajIvCogJdDhQ11/img.gif" alt="Weather API Web Sites">
<figcaption>Fig 1. Check app service</figcaption>
</figure>

GIF를 통해 홈페이지가 어떤식으로 이루어지고 API를 통해 JSON 포맷의 응답이 어떻게 오는지 확인할 수 있습니다. (다음은 사진으로 보여드리겠습니다.)

<!-- <sup id="user">[[1]](#user-ref)</sup> -->
<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcVNmm8%2FbtrdEjeFK6C%2FSb271UmoPaBwntvxpKDAOK%2Fimg.png" alt="Weather API Web Sites - Main">
<figcaption>Fig 2. Main.</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FefWqFt%2Fbtrdz7lRQUy%2FBSMGs7m6dIZIbrRgX0fAKk%2Fimg.png" alt="Weather API Web Sites - JSON">
<figcaption>Fig 3. Json</figcaption>
</figure>

<figure>
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FTzIRT%2Fbtrdsw035ez%2FsfRKLJRtfmiLBue4rFo4Gk%2Fimg.png" alt="Weather API Web Sites - JSON">
<figcaption>Fig 4. Search</figcaption>
</figure>

<figure>
<div style="border:1px dashed; padding:25px;"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdMNroT%2Fbtrdylyg1XN%2FXG2P5XyF0iw7lSM6k4ZLC1%2Fimg.png" alt="Weather API Web Sites - Result"></div>
<figcaption>Fig 5. Result</figcaption>
</figure>

<figure>
<div style="border:1px dashed; padding:6px;"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FboeojY%2FbtrdES82Dwy%2FqY8xZSKRAFAgSyTGLEcWBK%2Fimg.png" alt="Weather API Web Sites - Curl"></div>
<figcaption>Fig 6. Curl</figcaption>
</figure>
<!-- <mark>Shell adalah sebuah command-line interpreter; program yang berperan sebagai penerjemah perintah yang diinputkan oleh User yang melalui terminal</mark>, sehingga perintah tersebut bisa dimengerti oleh si Kernel. -->

```bash
$ curl "http://192.168.35.237/weather/weather.php?query=지역"
 -H "Content-Type: application/json; charset=UTF-8"
```
curl<sup id="user">[[1]](#curl)</sup>을 통한 서버 동작 및 조회 결과 확인

<hr>

# 4. 프로젝트 결과
1. html, css 적용 방법 및 코드를 더 쉽게 짜는 법을 배웠고,
2. php를 통해 api를 만들어 json으로 쉽게 포맷 응답을 하는 법도 알 수 있어서 나중에 날씨 api를 써서 간편하게 조회할 수 있을 것 같습니다.
3. 파이썬, 자바스크립트, 자바로 파싱을 하다가 php로 해보니 php는 좀 막일이라는 걸 알았습니다. (그래도 php 공부돼서 좋았다😅)

<hr>

##### Notes

<small id="curl"><sup>[[1]](#user)</sup> curl은 오픈 소스로 개발되어 윈도우와 리눅스에 기본 설치되고 있는 웹 개발 툴로써 http, https, ftp, sftps, smtp, telnet 등의 다양한 프로토콜과 Proxy, Header, Cookie 등의 세부 옵션까지 쉽게 설정할 수 있습니다.<br>이러한 장점 때문에 Client를 코딩을 시작하기 전에 curl 명령어로 서버 동작을 먼저 확인함으로써 좀 더 빠르게 개발을 진행할 수 있습니다.</small>

<hr>

# 5. 참고자료
- https://html5up.net/read-only
- 코드 : https://github.com/Veloideu/Weather-API-html-css-php- (제 깃헙에 올려두었습니다.)