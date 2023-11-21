---
# 파일명, 카테고리명은 영문으로 작성해주세요.
# 글제목
title: "Jenkins 플러그인 분석하기-Envinject"

# 헌재 글에 카테고리를 지정합니다.
# 카테고리 관리는 다음 위치에서 수행 <~/_pages/categories> 폴더내 <카테고리.md>파일
# 주의 > 포스트에 등록한 카테고리와 동일해야함, 카테고리 파일명과 카테고리를 동일하게 작성하여 관리하도록함
categories: 
  - DevSafetyOps

# 현재 글에 태그를 입력합니다.
#  tags:
#  - Jenkins
#  - plugins
#  - envinject
#  - envinject-api


#sidebar-right : true #오른쪽 사이드바 활성화 속성
#subtitle : "subtitle을 입력해주세요." #페이지 히어로에서 서브타이틀로 들어가는 부분 텍스트 입력


# ****현재 작성중인 글(post)을 <추천>과 <Best>글로 지정하기 위해 아래와 같이 속성을 지정합니다.****

# featuredPost: true #추천글 지정 (메인 페이지에 표출됨. 최대 3개까지만 목록에 표시됨)
#  mostRead: true #Best글 지정 (메인, 블로그 페이지에 표출됨, 가장 많이 본글 1개 목록에 표시)


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

# Jenkins 플러그인 분석하기-Envinject0.

> ## 목차    
>
> *  분석정보 
> *  기능
> *  설정


## 분석정보
 *  실행환경 : Ubuntu
 *  분석대상 plugin: envinject, envinject-api
 *  위치 : $JENKINS_HOME/plugins 
 *  분석대상 job: ESCAS-OSS-Template
 *  관련 사이트
    * envinject : <https://plugins.jenkins.io/envinject/>
    * envinject-api : <https://plugins.jenkins.io/envinject-api/>


## 기능 

1. Jenkins Java 프로세스에서 상속된 환경 변수를 제거합니다.

2. 노드(컨트롤러/에이전트) 시작 시 환경 변수를 주입합니다.

3. 실행을 위한 SCM 체크아웃 전후에 설정 스크립트를 실행합니다.

4. 실행을 위한 SCM 체크아웃 전후에 환경 변수를 주입합니다.

5. 실행을 위한 빌드 단계로 환경 변수를 주입합니다.

6. 실행을 위한 비밀번호 값을 안전하게 주입합니다.

7. 각 빌드에 사용되는 환경 변수 세트를 표시하기 위해 빌드가 끝날 때 환경 변수를 내보냅니다.


## 설정
###  General 
* 노드(컨트롤러/에이전트) 시작 시 환경 변수를 주입

*  설정방법 : Jenkins > <job> > 구성(configuration) > General > 'Prepare an envrionment for the run' 선택 >
  *  properties/script/groovy script를 통해 설정 가능

![image](https://github.com/SyneticsCorp/SyneticsCorp.github.io/assets/150226255/8a4e11b7-f563-4750-ae64-675c08441706)

*  설정확인
  *  URL로 확인하기: 웹 실행 > http://<jenkins_url>/job/<job_name>/<build_no>/injectedEnvVars/export
  *  설정 file로 확인하기: 터미널 실행 > cat $JENKINS_HOME/jobs/<job_name>/builds/<build_no>/inejctedEnvVars.txt


###  Build Environment
1. 실행을 위한 빌드 단계로 환경 변수를 주입

* 설정방법 : Jenkins > <job> > 구성(configuration) > Build Envrionment > 'Inject environment variables to the build process' 선택 >
  * properties/script/groovy script를 통해 설정 가능

![image](https://github.com/SyneticsCorp/SyneticsCorp.github.io/assets/150226255/a472b6c2-8c4d-49b8-81bb-a1d96780e805)

* 설정확인
  * URL로 확인하기: 웹 실행 > http://<jenkins_url>/job/<job_name>/<build_no>/injectedEnvVars/export
  * 설정 file로 확인하기: 터미널 실행 > cat $JENKINS_HOME/jobs/<job_name>/builds/<build_no>/inejctedEnvVars.txt

* 예제 : 

![image](https://github.com/SyneticsCorp/SyneticsCorp.github.io/assets/150226255/8b626771-7dd7-4188-b4ca-afba43d7024a)


2. 실행을 위한 비밀번호 값을 안전하게 주입

* 설정방법 : Jenkins > <job> > 구성(configuration) > Build Envrionment > 'Inject passwords to the build as envrionment variables' 선택 >
  * Global/job별 passwaord 설정 가능

![image](https://github.com/SyneticsCorp/SyneticsCorp.github.io/assets/150226255/4761cee3-df47-4191-84b9-a8f5d0163133)

* 설정확인: 터미널 실행 > cat $JENKINS_HOME/jobs/<job_name>/config.xml


###  Build steps
* 각 빌드에 사용되는 환경 변수 세트를 표시하기 위해 빌드가 끝날 때 환경 변수를 내보냅니다.

* 설정방법 : Jenkins > <job> > 구성(configuration) > Build steps > 'Add build step' 선택

![image](https://github.com/SyneticsCorp/SyneticsCorp.github.io/assets/150226255/2c7c9cac-c914-4402-af3f-31c73b0d0805)

* 예제 : 

![image](https://github.com/SyneticsCorp/SyneticsCorp.github.io/assets/150226255/020b9d09-ca4f-4325-b1a4-947699c1d210)
* 설정확인 : cat ./build.info.propertise




