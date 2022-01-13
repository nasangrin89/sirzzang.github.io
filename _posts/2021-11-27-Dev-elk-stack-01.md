---
title:  "[ELK] ELK stack - 1. 개요"
excerpt: 로그 모니터링, 빅데이터 분석 등에 자주 활용되는 스택
toc: true
categories:
  - Dev
header:
  teaser: /assets/images/blog-Dev.jpg
tags:
  - ELK
  - 시각화
  - 모니터링
---



 ELK stack이란, Elastic의 오픈소스 프로젝트 Elasticsearch, Logstash, Kibana를 같이 연동하여 사용하는 것을 의미한다. 같이 사용할 때 시너지 효과가 좋아 주로 빅데이터 분석, 로그 모니터링 시스템 구축에 많이 사용하곤 하는데, 최근 회사 업무에서 ELK stack을 이용해 로그 모니터링 시스템을 구축해 본 경험이 있어 해당 분야에서 ELK stack을 어떻게 활용할 수 있는지에 대해 공부한 내용을 정리해 보고자 한다.

<br>

# 로그 모니터링 시스템





# ELK stack 



 ELK stack을 이루는 Elasticsearch, Logstash, Kibana 각각을 간단하게 알아보면 다음과 같다.



## Elasticsearch











로그 데이터를 수집하고 정제하여 시각화함으로써 로그 모니터링 시스템을 구축할 때 사용한다. 







대시보드를 잘 만들면 아래와 같이도..







오픈소스



- 전체 과정

  - elastic search
  - logstash -> elastic search
  - kibana -> elastic search

- elastic search

  - 어떻게 저장되는지
  - 쿼리 방법

- logstash

  - input: file, tcp, stdin
  - filter: grok, json -> 로그 적절하게 파싱

- kibana: elasticsearch에 쿼리, Kibana Query Language ? 웹 서버??

  - discover, log stream
  - bar chart, donut chart 등 차트 활용해서 시각화 가능

  

---



각각을 연결해서 하나의 스택으로 구성해서 쓰는 게 편한데

- docker-elk
- 어떻게 설정되어 있는지 뜯어봐 보자



---







beat도 있다고 하는데



---



elastic search 자체를 좀 알아 보고 싶다. 인덱싱 방법 재밌는데. 특히 저장 시 형태소 분석, 불용어 처리 등 거치는 과정이 자연어 처리에서 배웠던 거랑 비슷해서 재밌어 보임

kibana에서 다양하게 시각화할 수 있는 방법?



쿼리하는 게