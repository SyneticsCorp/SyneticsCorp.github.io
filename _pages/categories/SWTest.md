---
title: "SW Test 모아보기"
layout: archive
permalink: categories/SWTest
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.SWTest %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}

<!-- testct1 라는 카테고리를 가진 포스트들만 모아서 한번에 보여줄 수 있는 이지를 만든다. 📂pages 폴더> 📂categories 라는 폴더> 이 안에 런 카테고리들만 가진 포스트들을 나열하여 보여 줄 수 있는 마크다운 파일을 만들어 정리. permalink의 주소로 접근가능함-->

<!-- !!! site.categories.카테고리명 // 포스트에 등록한 카테고리와 동일해야함 // 카테고리 파일명과 카테고리를 동일하게 작성하여 관리하도록함-->