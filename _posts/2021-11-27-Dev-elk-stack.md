---
title:  "[ELK] ELK stack"
excerpt: 로그 모니터링, 데이터 시각화 등에 자주 활용되는 스택
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

