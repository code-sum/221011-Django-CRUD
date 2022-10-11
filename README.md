# ✅Django Auth

>1. Django Auth
>2. User model 활용하기
>3. 회원가입 기능 구현
>
>
>
>🗂️ 참고자료
>
>- Django Auth 기본 [(link)](https://docs.djangoproject.com/en/4.1/topics/auth/default/)
>- Django User Model [(link)](https://docs.djangoproject.com/en/4.1/ref/contrib/auth/)
>- Customizing [(link)](https://docs.djangoproject.com/en/4.1/topics/auth/customizing/)
>- 비밀번호 암호화 [(link)](https://docs.djangoproject.com/en/3.2/topics/auth/passwords/) [(link2)](https://d2.naver.com/helloworld/318732)



## 1. Django Auth

- Django Auth 개요

  - 장고 인증 시스템(Django authentication system)은 인증과 권한 부여를 함께 제공(처리)하고 있음
    - User
    
    - 권한 및 그룹

    - 암호 해시 시스템
    
    - 기타 적용 가능한 시스템
  - 필수 구성은 settings.py / `INSTALLED_APPS = []` 에 기본 내장
    - django.contrib.auth
    - **contrib** 이 뭘까요? [(link)](https://docs.djangoproject.com/en/4.1/ref/contrib/)
  
  - 인증(Authentication)
    - 신원 확인
    - 사용자가 자신이 누구인지 확인하는 것
    - 이 사람이 해당 사이트에 인증된 사람인지 여부, 로그인
  - 권한/허가(Authorization)
    - 권한 부여
    - 인증된 사용자가 수행할 수 있는 작업을 결정
    - 인증 받은 사람들 가운데 스태프와 회원처럼 서로 다른 권한을 부여 받음
  - `create superuser` : 기본으로 내장된 기능(auth)을 활용했던 것임



- 사전 설정 (요약)

  - accounts app 생성 및 등록 (멀티 앱 활용)
  
  ```python
  0) 가상환경 꼭 켜기
  1) 기존 게시판 앱 만들었던 프로젝트에서 accounts 앱 시작/등록
  2) url 분리하기 메인 문지기에서 include 로 분리하고,
     accounts 앱에 서브 문지기 만들어주기
  3) 사용자 관리를 본격적으로 시작하기 위해, 모델 정의
     (근데 DB auth_user 확인해보면 이미 정의된 모델이 있음,
     테이블의 네이밍 컨벤션도 '앱이름_모델이름')
  4) 따라서 클래스 상속받아 기존 모델을 '커스텀으로' 가져오기
  5) settings.py 에 맨 하단 # User Model 부분 새롭게 작성
  6) models.py 에 상속 관련 내용 작성
    (이 부분은 원래 프로젝트 시작할 때 작성해야 되는거니까, 
     이전에 작성되었던 DB 존재하면 삭제하기)
  7) 모델 만들었으니까 마이그레이션 ~ 마이그레이트
  8) 여기까지 작성하면 user model 사용할 준비 완료!
  ```
  
  🗂️ [(참고자료)](https://docs.djangoproject.com/en/4.1/ref/contrib/auth/)



- 사전 설정 (조금 더 자세히)
  - 가상환경 및 Django 설치, 프로젝트 생성 및 추가 설정 👉 [(link)](https://github.com/code-sum/TIL/blob/master/notes/dj_modelform2.md) 
  - 




---



## 2. User model 활용하기

User Model 바탕으로 CRUD

> 회원정보 생성 : CREATE
>
> 회원정보 확인 : READ
>
> 회원정보 프로필 : 상세보기(detail page) 
>
> 회원정보 수정 : UPDATE
>
> 회원정보 삭제 : DELETE 

암호화의 핵심 : 반대 방향으로 복호화가 불가능해야 합니다



---



## 3. 회원가입 기능 구현



1. 회원가입 기능 구현

2. User 모델 변경하기(11:35 ~)

