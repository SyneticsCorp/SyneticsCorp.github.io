---
# 파일명, 카테고리명은 영문으로 작성해주세요.
# 글제목
title: "이걸 우리가 공유하는게 맞나 싶지만, Jenkins에서 Sparrow 실행하기"

# 헌재 글에 카테고리를 지정합니다.
# 카테고리 관리는 다음 위치에서 수행 <~/_pages/categories> 폴더내 <카테고리.md>파일
# 주의 > 포스트에 등록한 카테고리와 동일해야함, 카테고리 파일명과 카테고리를 동일하게 작성하여 관리하도록함
categories: 
  - DevSecOps

# 현재 글에 태그를 입력합니다.
tags:
  - DevSafetyOps
  - DevOps
  - Jenkins
  - Sparrow
  - 정적분석


#sidebar-right : true #오른쪽 사이드바 활성화 속성
#subtitle : "subtitle을 입력해주세요." #페이지 히어로에서 서브타이틀로 들어가는 부분 텍스트 입력


# ****현재 작성중인 글(post)을 <추천>과 <Best>글로 지정하기 위해 아래와 같이 속성을 지정합니다.****

# featuredPost: true #추천글 지정 (메인 페이지에 표출됨. 최대 3개까지만 목록에 표시됨)
mostRead: true #Best글 지정 (메인, 블로그 페이지에 표출됨, 가장 많이 본글 1개 목록에 표시)


# # 배너 이미지 설정 (page__hero.html)
# header:
#   overlay_color: "#000"
#   overlay_filter: "0.5"
#   #배너 이미지 > 아래 폴더 위치에 이미지를 넣고, 경로작성
#   overlay_image: /assets/images/main_home.png
#   #배너 타이틀 아래 표시되는 설명 작성
#   image_description: "시네틱스는 Automotive, 철도, 항공 SW 개발을 위한 지속적 통합과 빌드 가상화 컨설팅과 교육을 제공합니다."


layout: single #페이지 레이아웃을 설정함
author_profile: false #작성자 정보 표시 여부

---

<!-- **** 아래 부분부터 본문 영역입니다.*** -->

# Jenkins에서 Sparrow 연동하기

> **요약**    
>
> *  제조사에서 해야되는걸 왜 우리가 해야하나 싶지만 메뉴얼에 없어서요
> *  Jenkins는 Command-Line 실행이 가능하면 뭐든 적용 가능
> *  Sparrow는 Code Soanr, SonarQube처럼 분석은 내 PC(Jenkins)에서, 결과는 Web에 게시

자동차, 철도, 항공, 로봇 등 안전 필수 시스템(Safety Critical System) 들은 사고 발생시 큰 인명 및 재산 피해로 이어지기 때문에 개발 과정에 서 철저한 안전 활동이 요구된다. 특히, 시스템 내 SW의 역할이 중요해지 고 그 비중이 높아짐에 따라, 사고의 잠재적 원인인 해저드(Hazard)를 분 석하고 안전 요구사항을 도출하기 위한 노력이 필요하다. 이에 따라, 안 전성 확보가 필수적인 산업분야에서는 도메인별 특성에 맞는 기능 안전 표준을 제정하여 SW 안전 기준을 강화하고, 국제 표준 준수를 의무화하 고 있다.  

한편, 초지능, 초연결 시대에 진입하면서 머신러닝, 딥러닝 등 AI 기 술이 산업 전반에 활용되고 있으며, 개인지원 로봇 (Personal Care Robot) 의 자율 기능(autonomous function)에도 적용되고 있다. AI 기술은 자율성 을 갖추어 큰 편리함을 제공하지만, 전통적인 알고리즘 기반의 SW가 아 닌 데이터를 기반으로 학습을 통해 개발되는 SW이기 때문에, 기존과는 다른 안전성 확보 방안이 필요하다. AI 안전성 분야에서는 AI 실패를 예 방하기 위한 연구들을 수행해오고 있다. OECD와 EU 등 국제기구는 신뢰 가능(Trust worthy) AI 시스템 구축을 위한 지침을 제공하고 있으며, 자동 차 분야에서도 자율 주행 관련 안전성 표준인 SOTIF(Safety Of The Intended Function)를 개발 중이다.

하지만, 개인 지원 로봇의 자율 기능을 위한 안전성 연구는 많이 이 루어지지 않고 있다. 개인지원 로봇의 안전성 확보를 위해 개발된 표준인 ISO 13482는 Hazard 분석과 리스크 완화 방안을 위한 기본 틀을 제시하 고 있지만, 자율 의사결정 지침에서는 상위 수준의 가이드만 제시하고 있 다. 그러므로, 개인지원 로봇 분야의 실무자들을 지원하기 위해서는 기존 안전분석 기법과 더불어 자율 기능에 적용하기 위한 안전성 분석 프로세 스 및 가이드라인이 필요하다.  

<hr>


## 이미지
> 1. [깃허브 이슈를 활용한 이미지 쉽게 넣는 팁](https://ahribori.com/article/5a03bcfd6c9eef13d882e29a)
> 2. [깃허브에 특정 폴더 위치에 이미지를 업로드해서 마크다운 링크로 작성하는 방법](https://theorydb.github.io/envops/2019/05/22/envops-blog-how-to-use-md/)


<!-- 이미지 정렬 ![대체텍스트](이미지주소 "이미지제목") -->
### 블랙박스 테스팅
![블랙박스 테스팅](https://user-images.githubusercontent.com/92907581/141237421-2f48053f-4ce7-4bec-8e60-7169f2619433.png)

### 화이트박스테스팅

![화이트 박스 테스팅](https://user-images.githubusercontent.com/92907581/141237421-2f48053f-4ce7-4bec-8e60-7169f2619433.png)




<br><br><br> 

### 발표자료 영상

<iframe width="100%" src="https://www.youtube.com/embed/qcrS6poY-OY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


## 표

| 블랙박스 테스팅                                       | 화이트박스 테스팅                                          |
| ------------------------------------------- | ----------------------------------------------------- |
|  텍스트 및 이미지 입력 | 텍스트 및 이미지 입력 |
| ![블랙박스 테스팅](https://user-images.githubusercontent.com/92907581/141237421-2f48053f-4ce7-4bec-8e60-7169f2619433.png)
 | 입 출력값을 중심으로 테스팅하는 방법 |
| 화이트박스 테스팅 | 논리적 구조를 기반으로 소스코드를 실행시켜 테스트하는 방법 |
| [Post with a Gallery][gallery-post] | A post showing several images wrapped in `<figure>` elements. |
| [Sample Collection Page][sample-collection] | Single page from a collection. |
| [Categories Archive][categories-archive] | Posts grouped by category. |
| [Tags Archive][tags-archive] | Posts grouped by tag. |





