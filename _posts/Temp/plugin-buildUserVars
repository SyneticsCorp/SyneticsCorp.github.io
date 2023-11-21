---
# 파일명, 카테고리명은 영문으로 작성해주세요.
# 글제목
title: "Jenkins 플러그인 분석하기-build-user-vars"

# 헌재 글에 카테고리를 지정합니다.
# 카테고리 관리는 다음 위치에서 수행 <~/_pages/categories> 폴더내 <카테고리.md>파일
# 주의 > 포스트에 등록한 카테고리와 동일해야함, 카테고리 파일명과 카테고리를 동일하게 작성하여 관리하도록함
categories: 
  - DevSafetyOps

# 현재 글에 태그를 입력합니다.
#  tags:
#  - Jenkins
#  - plugins
#  - build-user-vars-plugin


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

# Jenkins 플러그인 분석하기-build-user-vars-plugin.

> ## 목차    
>
> *  분석정보 
> *  기능
> *  동작

This is a normal paragraph: 
## 분석정보
 *  실행환경 : Ubuntu
 *  분석대상 plugin: build-user-vars-plugin
 *  위치 : $JENKINS_HOME/plugins 
 *  분석대상 job: ESCAS-OSS-Template
 *  관련 사이트 : https://www.jenkins.io/doc/pipeline/steps/build-user-vars-plugin/
 
end code block.


This is a normal paragraph: 
## 기능 

빌드를 시작한 사용자를 설명하는 환경 변수 세트

|변수명|설명|
|------|---|---|
|BUILD_USER|전체 이름(이름+성)|
|BUILD_USER_FIRST_NAME|이름|
|BUILD_USER_LAST_NAME|성|
|BUILD_USER_ID|젠킨스 사용자 id|
|BUILD_USER_GROUPS|Jenkins 사용자 그룹|
|BUILD_USER_EMAIL|이메일 주소|

end code block.

This is a normal paragraph: 

## 동작

* 설명: 해당 사용자에 대한 분석결과 URL 생성 시 사용 

* 예시 : 
protex_userid="${BUILD_USER_ID}@protexip.etri
..
echo "SUCCESS: 정적분석 부분 실패, Console Output 확인, 오픈소스 분석 성공, <a href="https://protexip.etri.re.kr" target="_blank"> 오픈소스 분석결과 확인</a> 후에 <a href='https://ola.etri.re.kr/api/v1/create_project?prjName=${JOB_NAME}&requestUserEmail=${PRI_EMAIL}&protexUserId=${BUILD_USER_ID}@protexip.etri&protexUserPw=${PROTEX_PWD}' target='_blank'>최종보고서신청</a>" | tee $WORKSPACE/build_descr.txt


end code block.
