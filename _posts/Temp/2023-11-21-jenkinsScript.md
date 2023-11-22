---
# 파일명, 카테고리명은 영문으로 작성해주세요.
# 글제목
title: "Jenkins 권한 자동화 Script"

# 헌재 글에 카테고리를 지정합니다.
# 카테고리 관리는 다음 위치에서 수행 <~/_pages/categories> 폴더내 <카테고리.md>파일
# 주의 > 포스트에 등록한 카테고리와 동일해야함, 카테고리 파일명과 카테고리를 동일하게 작성하여 관리하도록함
categories: 
  - DevSafetyOps

# 현재 글에 태그를 입력합니다.
# tags:
#   - tag1
#   - tag2
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

# Jenkins 권한 자동화

## 목차
- [사용 목적](#사용-목적)
- [사용 방법](#사용-방법)
  - [Project-based Matrix Authorization Strategy 플러그인 설치](#0-Project-based-Matrix-Authorization-Strategy-플러그인-설치)
  - [Jenkins 관리자 계정 확인](#1-Jenkins-관리자-계정-권한-확인)
  - [필요 파일 설치](#2-필요-파일-설치)
  - [권한 부여](#3-권한-부여)
- [결과](#결과)

---

## 사용 목적 

Jenkins에서 사용자에게 권한을 할당하지 않다면 모든 Job Item들을 보게 되거나 Overall/Read 권한 오류가 발생한다. "test"라는 ID를 가진 사용자는 "test_job"이라는 Job만 볼 수 있도록 권한을 할당하고 싶을 때, UI에서 번거롭게 설정하기보다 Script 실행으로 권한을 할당하려 한다.

---

## 사용 방법

### 0. Project-based Matrix Authorization Strategy 플러그인 설치 확인

- Jenkins 관리 > Plugin Manager > 설치된 플러그인 목록 > Matrix Authorization Strategy Plugin 확인

![plugin](https://github.com/SyneticsCorp/SyneticsCorp.github.io/assets/113246634/168fed8f-275e-4bee-a30f-d08780a6a658)

### 1. Jenkins 관리자 계정 권한 확인

- Project-based Matrix Authorization Strategy 설정이 아니라면 [UI](#ui) 혹은 [Jenkins Script Console](#script-console)에서 실행
  - #### UI
    - Jenkins 관리 > Configure Global Security > Project-based Matrix Authorization Strategy 설정 <br> ![01_authori-1](https://github.com/SyneticsCorp/SyneticsCorp.github.io/assets/113246634/1c5df159-3b25-4bdf-b780-25fc31e1e44c) 
    - Add user 클릭 <br> ![01_authori-2](https://github.com/SyneticsCorp/SyneticsCorp.github.io/assets/113246634/c485b363-a1c2-46fe-8d1f-1bf49652b8e0) 
    - 관리자 계정 User ID 설정 <br> ![01_authori-3](https://github.com/SyneticsCorp/SyneticsCorp.github.io/assets/113246634/c2676b74-e3f6-40fe-9e4a-c917ec2e2fe4) 
    - Overall/Administer 체크박스 클릭 후 Save <br> ![01_authori-4](https://github.com/SyneticsCorp/SyneticsCorp.github.io/assets/113246634/9c2b44f2-0b72-49c3-bb85-cda10b377090)
  - #### Script Console
    ```
    import hudson.model.*
    import jenkins.model.*
    import hudson.security.*

    def adminID = "admin"

    def jenkins = Jenkins.getInstance()

    def pmas = jenkins.getAuthorizationStrategy()
    if (!(pmas instanceof ProjectMatrixAuthorizationStrategy)) {
        pmas = new ProjectMatrixAuthorizationStrategy()
        jenkins.setAuthorizationStrategy(pmas)
    }

    // "admin"에게 모든 권한 추가
    pmas.add(Jenkins.ADMINISTER, "${adminID}")

    jenkins.save()

    ```

### 2. 필요 파일 설치

- jenkins-cli.jar
  - Ubuntu 사용 시
    ```
    sudo wget http://[Jenkins URL]/jnlpJars/jenkins-cli.jar
    ```
  - UI에서 다운로드 시 <br> Jenkins 관리 > Jenkins CLI > jenkins-cli.jar 하이퍼링크 클릭 <br> ![jenkins-cli.jar](https://github.com/SyneticsCorp/SyneticsCorp.github.io/assets/113246634/4c121de8-5753-4214-9178-05efa85352b9)

- groovy 파일 저장
  - 파일 이름 예시: **jenkinsAuto.groovy**
  - 코드 <br>
  ```
  import hudson.model.*
  import jenkins.model.*
  import hudson.security.*
      
  def grantReadPermission(String userID, String jobName) {
      // 초기값 설정
      def jenkins = Jenkins.getInstance()
      def user = User.get("${userID}", false, null)
      def job = jenkins.getItemByFullName("${jobName}", Job.class)

      // 플러그인 설정
      def pmas = jenkins.getAuthorizationStrategy()
      if (!(pmas instanceof ProjectMatrixAuthorizationStrategy)) {
          pmas = new ProjectMatrixAuthorizationStrategy()
          jenkins.setAuthorizationStrategy(pmas)
      }
      
      // Overall/READ 권한
      pmas.add(Jenkins.READ, "${userID}")
      
      jenkins.save()
      
      // 사용자별 Job 권한 설정
      AuthorizationMatrixProperty amp = job.getProperty(AuthorizationMatrixProperty.class)
      if (amp == null) {
          amp = new AuthorizationMatrixProperty()
          job.addProperty(amp)
      }
      
      amp.add(Job.READ, "${userID}")
      
      job.save()
  }


  // 값 받아오기
  def customUserID = args[0]
  def customJobName = args[1]
  grantReadPermission(customUserID, customJobName)
  ```

### 3. 권한 부여

- Terminal 창에서 실행
- jenkins-cli.jar && groovy 파일이 존재하는 위치에서 실행

```
java -jar jenkins-cli.jar -s http://[JENKINS_URL]/ -auth [USER_NAME]:[API_TOKEN] groovy =< [GROOVY_FILE_NAME].groovy "[NEW_USER_ID]" "[NEW_JOB_NAME]" 
```

```
JENKINS_URL: Jenkins URL 주소
USER_NAME: 관리자 계정 ID
API_TOKEN: Jenkins API Token
GROOVY_FILE_NAME: Groovy 파일 이름
NEW_USER_ID: 권한 할당할 사용자 ID
NEW_JOB_NAME: 사용자에게 권한 할당할 Job 이름
```

```
// 예시

java -jar jenkins-cli.jar -s http://localhost:18080/ -auth admin:jenkinsToken groovy =< jenkinsAuto.groovy "test" "test_job" 
```

---

## 결과

**실행 전** 

![03_test-1](https://github.com/SyneticsCorp/SyneticsCorp.github.io/assets/113246634/6893386b-3d4c-45ea-b8a3-e431e9043cf9)


**실행 후** 

![03_test-2](https://github.com/SyneticsCorp/SyneticsCorp.github.io/assets/113246634/773c8d44-fbfd-4458-9fec-488ef2a787f5)

