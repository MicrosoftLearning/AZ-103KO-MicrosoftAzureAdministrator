---
title: Online Hosted Instructions
permalink: index.html
layout: home
---

# Content Directory

각 실습에 대한 연습 및 데모에 대한 하이퍼링크를 확인하실 수 있습니다.

## 실습

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| 모듈 | 실습 |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
