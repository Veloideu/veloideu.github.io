---
title: ๐ Naver Weather Api
date: 2021-08-30 19:16:03 +07:00
# modified: 2021-08-30 19:16:03 +07:00
tags: [html, php, linux]
description: ๋ค์ด๋ฒ์์ ๋ ์จ๋ฅผ ๊ฒ์ํด ๋์ค๋ ๋ฐ์ดํฐ ์ ๋ณด๋ค์ ์ฝ๊ฒ(ํ์ฑ) ์ฌ์ฉํ  ์ ์๋๋ก ๊ฐ๋จํ๊ณ  ์ฝ๊ฒ ์ ๊ณตํด ์ฃผ๋ API๋ฅผ ๋ง๋ค์ด ๋ณด์์ต๋๋ค. "์ด API๋ JSON ํฌ๋งท์ ์๋ต์ ์ ์กํฉ๋๋ค."
image: "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcVNmm8%2FbtrdEjeFK6C%2FSb271UmoPaBwntvxpKDAOK%2Fimg.png"
---

# 1. ๊ฐ์

PHP, HTML, CSS ์ธ์ด ๊ณต๋ถ (ํ์ฑ)๐.

<hr>

## 1.1 API ๋ชฉ์ 

๋ค์ด๋ฒ์์ ๋ ์จ๋ฅผ ๊ฒ์ํด ๋์ค๋ ๋ฐ์ดํฐ ์ ๋ณด๋ค์ ์ฝ๊ฒ`(ํ์ฑ)` ์ฌ์ฉํ  ์ ์๋๋ก ๊ฐ๋จํ๊ณ  ์ฝ๊ฒ ์ ๊ณตํด ์ฃผ๋ API๋ฅผ ๋ง๋ค์ด ๋ณด์์ต๋๋ค. "์ด API๋ `JSON ํฌ๋งท`์ ์๋ต์ ์ ์กํฉ๋๋ค."

<hr>

# 2. ํ๊ฒฝ ๊ตฌ์ถ

<span style="#03f3b3;color: red;">LAMP</span>
<br>L: Linux (์ด๋ฒ wordpress๋ CentOS 7์์ ์งํํฉ๋๋ค.)
<br>A: Apache ์น ์๋ฒ
<br>M: MySQL ๋๋ MariaDB
<br>P: PHP
<br>Centos7 & Apache/2.4.6 & 5.5.68-MariaDB & PHP 5.4.16

<hr>

# 3. ์น ์๋น์ค ์์
<figure>
<img src="https://blog.kakaocdn.net/dn/V1g3h/btrduU2e4i9/hQ5NRi4lajIvCogJdDhQ11/img.gif" alt="Weather API Web Sites">
<figcaption>Fig 1. Check app service</figcaption>
</figure>

GIF๋ฅผ ํตํด ํํ์ด์ง๊ฐ ์ด๋ค์์ผ๋ก ์ด๋ฃจ์ด์ง๊ณ  API๋ฅผ ํตํด JSON ํฌ๋งท์ ์๋ต์ด ์ด๋ป๊ฒ ์ค๋์ง ํ์ธํ  ์ ์์ต๋๋ค. (๋ค์์ ์ฌ์ง์ผ๋ก ๋ณด์ฌ๋๋ฆฌ๊ฒ ์ต๋๋ค.)

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
$ curl "http://192.168.35.237/weather/weather.php?query=์ง์ญ"
 -H "Content-Type: application/json; charset=UTF-8"
```
curl<sup id="user">[[1]](#curl)</sup>์ ํตํ ์๋ฒ ๋์ ๋ฐ ์กฐํ ๊ฒฐ๊ณผ ํ์ธ

<hr>

# 4. ํ๋ก์ ํธ ๊ฒฐ๊ณผ
1. html, css ์ ์ฉ ๋ฐฉ๋ฒ ๋ฐ ์ฝ๋๋ฅผ ๋ ์ฝ๊ฒ ์ง๋ ๋ฒ์ ๋ฐฐ์ ๊ณ ,
2. php๋ฅผ ํตํด api๋ฅผ ๋ง๋ค์ด json์ผ๋ก ์ฝ๊ฒ ํฌ๋งท ์๋ต์ ํ๋ ๋ฒ๋ ์ ์ ์์ด์ ๋์ค์ ๋ ์จ api๋ฅผ ์จ์ ๊ฐํธํ๊ฒ ์กฐํํ  ์ ์์ ๊ฒ ๊ฐ์ต๋๋ค.
3. ํ์ด์ฌ, ์๋ฐ์คํฌ๋ฆฝํธ, ์๋ฐ๋ก ํ์ฑ์ ํ๋ค๊ฐ php๋ก ํด๋ณด๋ php๋ ์ข ๋ง์ผ์ด๋ผ๋ ๊ฑธ ์์์ต๋๋ค. (๊ทธ๋๋ php ๊ณต๋ถ๋ผ์ ์ข์๋ค๐)

<hr>

##### Notes

<small id="curl"><sup>[[1]](#user)</sup> curl์ ์คํ ์์ค๋ก ๊ฐ๋ฐ๋์ด ์๋์ฐ์ ๋ฆฌ๋์ค์ ๊ธฐ๋ณธ ์ค์น๋๊ณ  ์๋ ์น ๊ฐ๋ฐ ํด๋ก์จ http, https, ftp, sftps, smtp, telnet ๋ฑ์ ๋ค์ํ ํ๋กํ ์ฝ๊ณผ Proxy, Header, Cookie ๋ฑ์ ์ธ๋ถ ์ต์๊น์ง ์ฝ๊ฒ ์ค์ ํ  ์ ์์ต๋๋ค.<br>์ด๋ฌํ ์ฅ์  ๋๋ฌธ์ Client๋ฅผ ์ฝ๋ฉ์ ์์ํ๊ธฐ ์ ์ curl ๋ช๋ น์ด๋ก ์๋ฒ ๋์์ ๋จผ์  ํ์ธํจ์ผ๋ก์จ ์ข ๋ ๋น ๋ฅด๊ฒ ๊ฐ๋ฐ์ ์งํํ  ์ ์์ต๋๋ค.</small>

<hr>

# 5. ์ฐธ๊ณ ์๋ฃ
- https://html5up.net/read-only
- ์ฝ๋ : https://github.com/Veloideu/Weather-API-html-css-php- (์  ๊นํ์ ์ฌ๋ ค๋์์ต๋๋ค.)