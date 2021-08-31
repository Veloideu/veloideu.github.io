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

LAMP를 기반으로 웹 모의해킹 진단을 위하여 가상의 커뮤니티 사이트 구축 하여, 모의 침투 테스트를 통해 취약점를 진단합니다.

<!-- CIA Web Sites는 php, mariadb의 취약점을 고려해야 할 측면에 대해 설명합니다. -->

## 1.1 사이트 목적

웹 사이트를 직접 개발하여 취약한 코드를 더 자세히 알 수 있으며,
<br>웹 개발에 취약점이 어디서 어떤 방식으로 취약점이 발생하는지 세밀하게 관찰하며, 관찰한 내용을 토대로 점검할 수 있습니다.


# 2. 환경 구축

`LAMP` / `Centos7` `Apache/2.4.6` `5.5.68-MariaDB` `PHP 5.4.16`
<br>L: Linux (이번 wordpress는 CentOS 7에서 진행합니다.)
<br>A: Apache 웹 서버
<br>M: MySQL 또는 MariaDB
<br>P: PHP


<style>
.zoom {
  padding: 25px;
  width: 600px;
  height: 340px;
}

.zoom:hover {
  transform: scale(1.75);
  transition: .5s; /* 부드럽게 */
}
</style>

# 3. 웹 서비스 시작
<figure>
<div style="border:1px dashed; padding:25px;" class="zoom"><img src="https://blog.kakaocdn.net/dn/zQSSl/btrdDLDbTUF/ATgAF375H6kwcUo6uLVCx1/img.gif" alt="Weather API Web Sites"></div>
<figcaption>Fig 1. Check app service</figcaption>
</figure>

## 3-1 취약점 분석
<figure>
<div style="border:1px dashed; padding:25px;" class="zoom"><img src="https://blog.kakaocdn.net/dn/zQSSl/btrdDLDbTUF/ATgAF375H6kwcUo6uLVCx1/img.gif" alt="Weather API Web Sites"></div>
<figcaption>Fig 1. Check app service</figcaption>
</figure>

# 4. 프로젝트 결과

# 5. 대응방안

##### Notes

# 6. 참고자료