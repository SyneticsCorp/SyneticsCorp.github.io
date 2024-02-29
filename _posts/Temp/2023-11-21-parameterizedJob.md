---
# 파일명, 카테고리명은 영문으로 작성해주세요.
# 글제목
title: "Jenkins Parameterized Job 실행 시 Response 예제"

# 헌재 글에 카테고리를 지정합니다.
# 카테고리 관리는 다음 위치에서 수행 <~/_pages/categories> 폴더내 <카테고리.md>파일
# 주의 > 포스트에 등록한 카테고리와 동일해야함, 카테고리 파일명과 카테고리를 동일하게 작성하여 관리하도록함
categories: 
  - DevSafetyOps

# 현재 글에 태그를 입력합니다.
tags:
  - Jenkins
  - Build
#   - tag3
#   - tag4
#   - tag5
#   - tag6
#   - tag7


#sidebar-right : true #오른쪽 사이드바 활성화 속성
#subtitle : "subtitle을 입력해주세요." #페이지 히어로에서 서브타이틀로 들어가는 부분 텍스트 입력


# ****현재 작성중인 글(post)을 <추천>과 <Best>글로 지정하기 위해 아래와 같이 속성을 지정합니다.****

# featuredPost: true #추천글 지정 (메인 페이지에 표출됨. 최대 3개까지만 목록에 표시됨)
# mostRead: true #Best글 지정 (메인, 블로그 페이지에 표출됨, 가장 많이 본글 1개 목록에 표시)


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

# Jenkins Parameterized Job 실행 시 Response 예제

## 목차

- [사용 목적](#사용-목적)
- [사용 방법](#사용-방법)
- [결과](#결과)

---

## 사용 목적

Parameterized Job Build 실행 시 어떤 명령어를 작성해야 하는지, 어떤 결과를 확인할 수 있는지 알아보려 한다.

---

## 사용 방법

- 파라미터가 1개일 때
  ```
  curl -X POST http://[JENKINS_URL]/job/[JOB_NAME]/buildWithParameters --data [PARAM1]=[VALUE1] --user [USER_NAME]:[API_TOKEN] -v
  ```

- 파라미터가 2개일 때
  ```
  curl -X POST http://[JENKINS_URL]/job/[JOB_NAME]/buildWithParameters --data [PARAM1]=[VALUE1] --data [PARAM2]=[VALUE2] --user [USER_NAME]:[API_TOKEN] -v
  ```

  ```
  JENKINS_URL: Jenkins URL 주소
  JOB_NAME: Build 실행할 Job 이름
  PARAM1, PARAM2: 파라미터 이름
  VALUE1, VALUE2: 파라미터의 결과값
  USER_NAME: 관리자 계정 ID
  API_TOKEN: Jenkins API Token
  ```

---

## 결과

```
Note: Unnecessary use of -X or --request, POST is already inferred.
*   Trying [JENKINS_URL]...
* Connected to [JENKINS_URL] ([JENKINS_URL]) port [JENKINS_URL_PORT] (#0)
* Server auth using Basic with user '[USER_ID]'
> POST /job/[JOB_NAME]/buildWithParameters HTTP/1.1
> Host: [JENKINS_URL]
> Authorization: Basic [ ]
> User-Agent: curl/7.81.0
> Accept: */*
> Content-Length: 9
> Content-Type: application/x-www-form-urlencoded
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 201 Created
< Date: [ ]
< X-Content-Type-Options: nosniff
< Location: http://[JENKINS_URL]/queue/item/12/
< Content-Length: 0
< Server: Jetty(10.0.11)
< 
* Connection #0 to host [JENKINS_URL] left intact

```

![result](https://github.com/SyneticsCorp/SyneticsCorp.github.io/assets/113246634/86292e8e-0f60-45c7-b721-ef05e1d9f0e7)
