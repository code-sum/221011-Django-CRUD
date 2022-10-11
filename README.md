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
  - `createsuperuser` : 기본으로 내장된 기능(auth)을 활용했던 것임



- 사전 설정 [(link)](https://docs.djangoproject.com/en/4.1/ref/contrib/auth/)

1. 가상환경 및 Django 설치, 프로젝트 생성 및 추가 설정 👉 [(link)](https://github.com/code-sum/TIL/blob/master/notes/dj_modelform2.md) 

2. accounts app 생성 및 등록 (멀티 앱 활용)

   - accounts : 사용자 정보를 관리하는 기능의 앱

   2-1. app 생성

   ```bash
   $ python manage.py startapp accounts
   ```

   2-2. app 등록

   > auth 와 관련한 경로나 키워드들을 Django 내부적으로 accounts 라는 이름으로 사용하고 있기 때문에 되도록 accounts 로 지정하는 것을 권장
   >
   > 다른 이름으로 설정해도 되지만 나중에 추가 설정을 해야 할 일들이 생기게 됨

   ```python
   # pjt/settings.py 에서
   # INSTALLED_APPS = [] 괄호에 아래와 같이 생성한 앱 등록
   
   INSTALLED_APPS = [
       'accounts',
       ...,
   ]
   ```

   2-3. urls.py 설정 (url 분리 및 매핑)

   ```python
   # pjt/urls.py 에서 아래와 같이 include import 한 다음
   # 'accounts/' 경로로 향하는 path 추가
   
   from django.contrib import admin
   from django.urls import path, include
   
   urlpatterns = [
       path('admin/', admin.site.urls),
       path('accounts/', include('accounts.urls')),
   ]
   ```

   ```python
   # accounts/urls.py 생성하고, 아래와 같이 코드 계속 작성
   
   from django.urls import path
   from . import views
   
   app_name = 'accounts'
   # app_name 은 왜 쓸까요?
   # 우리는 기본적으로 URL을 모두 변수화해서 쓰고 있음
   # Template, View에서 직접 '/accounts/' X 
   # app_name과 path 이름으로 
   
   urlpatterns = [
   
   ]
   ```

   

---



## 2. User model 활용하기

> 🗂️[(참고자료1)](https://docs.djangoproject.com/en/3.2/ref/contrib/auth/#user-model) [(참고자료2)](https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#substituting-a-custom-user-model)



- Django User Model
  - "Custom User Model 로 대체하기"
  - Django 는 기본적인 인증 시스템과 여러가지 필드가 포함된 User Model 을 제공, 대부분의 개발 환경에서 기본 User Model 을 Custom User Model 로 대체
  - Django 는 새 프로젝트를 시작하는 경우 비록 기본 User 모델이 충분하더라도 커스텀 User 모델 설정하는 것을 강력하게 권장(highly recommended)
  - 커스텀 User 모델은 기본 User 모델과 동일하게 작동하면서도 필요한 경우 나중에 맞춤 설정할 수 있기 때문
    - 단, User 모델 대체 작업은 프로젝트의 모든 migrations 혹은 첫 migrate 를 실행하기 전에 이 작업을 마쳐야 함
    - 모델을 바꾼다는 것은 DB가 변경된다는 것과 동일한 말이기 때문에, 만약 Custom User Model 을 언제든지 변경할 수 있도록 미리 만들어두지 않으면 나중에 모델 하나 바꾸기 위해 DB를 복잡하게 변경해야 하는 이슈가 발생할 수 있음



- 모델 설정 해보기

1. 모델 정의

   1-1. 만약 accounts 앱을 만들고 등록하기 전에, 다른 앱을 만들면서 모델을 생성하고 마이그레이트 했다면, DB 열었을 때 `auth_user` 부분에 이미 admin 유저에 대한 정보가 입력되어 있는 것을 확인할 수 있음

   - table 의 기본적인 네이밍 컨벤션 자체가 '앱이름_모델이름' 이며, 이 모델을 Django 에서 **가지고 와서 활용**하는 것이 모델 정의의 핵심 (=클래스 상속)

   1-2. AUTH_USER_MODEL 정의하기

   ```python
   # pjt/settings.py 최하단에 아래와 같은 코드 추가
   # accounts 앱에 있는 User 가 이제부터 사용할 사용자 정보라고 입력하는 것
   
   # User Model
   AUTH_USER_MODEL = 'accounts.User'
   ```

   1-3. 내부에 있던 모델 상속 받아오기

   - `AbstractUser` 가 무엇인지 궁금하다면? 👉 [(link)](https://github.com/django/django/blob/main/django/contrib/auth/models.py#L405)

   ```python
   # accounts/models.py 에서 아래와 같이 내용 채우기
   
   '''
   아래 내용은 원래 프로젝트 시작할 때 작성해야 되는거니까, 
   혹시 이전에 작성되었던 DB(db.sqlite3) 존재하면 삭제하기
   migrations 풀더에 __init__.py 제외하고 번호 붙은 파일들도 삭제
   '''
   
   from django.db import models
   from django.contrib.auth.models import AbstractUser
   
   # Create your models here.
   class User(AbstractUser):
       pass
   ```

2. 모델 만들었으니까 DB에 반영하기

   ```bash
   $ python manage.py makemigrations
   $ python manage.py migrate
   
   # 이제 auth_user 테이블이 아니라 accounts_user 테이블 사용시작
   ```

3. createsuperuser 명령어로 회원(관리자) 정보 하나 만들기

   ```bash
   $ python manage.py createsuperuser
   
   # 이후 DB 새로고침하면, accounts_user 에서 admin 정보 확인 가능
   # 여기까지 작성하면 user model 사용할 준비 완료
   ```

   

---



## 3. 회원가입 기능 구현

> 2. 에서 만든 User Model 바탕으로 CRUD 기능 구현하기
>
> - 회원정보 생성 : CREATE
> - 회원정보 확인 : READ
> - 회원정보 프로필보기 : 상세보기(READ detail page) 
> - 회원정보 수정 : UPDATE
> - 회원정보 삭제 : DELETE 



암호화의 핵심 : 반대 방향으로 복호화가 불가능해야 합니다



1. 회원가입 기능 구현

2. User 모델 변경하기(11:35 ~)

