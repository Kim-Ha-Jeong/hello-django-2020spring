# hello-django-2020spring

## part1
### 개발환경 구축 및 url 연결하기


    1. 개발 서버
	    * python manage.py runserver : 개발 서버 작동
    2. 첫번째 뷰 작성하기
	    * def (request): return HttpResponse : 가장 기본적인 형태의 뷰
		* path('url',파일명.함수명,이름) : url에 연결
		* include()  : 다른 URLconf 참고용으로 사용
    3. path(route, view, name, kwargs)
		* route : url 패턴을 가진 문자열(요청 처리할 때 필요한 패턴)
		* view : 특정한 view 함수를 호출
		* kwargs : view에 사전형으로 전달
		* name : url의 이름 
	
## part2
### Database 연결하기(SQLite)


	1. 데이터베이스 설치
		* python manage.py migrate 
		  : settings.py파일의 데이터베이스 설정, 데이터베이스 테이블을 만듦
	2. 모델 만들기
		* class 이름(models.Model):
			인스턴스 이름 = models.데이터 타입
		* 모델 만들기의 기본(객체지향형)
		* 타입에 따라 필수 인수가 다름
		* CharField : max_length
		* ForeignKey를 통해 question과 choice가 관계있음
	3. 모델의 활성화
		* mysite/settings.py에 polls.apps.PollsConfig를 추가하고 
		  python manage.py makemigrations polls를 하면
		  polls/migrations/0001_initial.py에 db모델이 추가됨
	4. API 가지고 놀기
		* python manage.py shell : python shell 실행
		* .objects.all() : 안에 있는 모든 tuple 볼 수 있음
		* .question_text(column명) = " " : 각 coulmn에 있는 데이터 수정 
		* .objects.get(조건) : 조건에 따른 데이터베이스 볼 수 있음
		* .create( ) : 데이터베이스 생성
		* .objects.filter(조건) : 조건에 따른 데이터베이스 볼 수 있음
		* .delete : 데이터베이스 삭제
		* __str__ : 오버라이딩해서 객체를 표현하는데 사용
		* import datetime/ from django.utils import timezone  
		  : 시간에 관련된 모듈, 유틸리티
	5. 관리자 생성하기
		* python manage.py createsuperuser 
		  : 관리 사이트에 로그인 할 수 있는 유저 생성
		* 내가 만든 테이블을 admin.py에 설정해줘야 
		  localhost:8000/admin에 출력이 됨
		* 관리자로 로그인한 후 데이터베이스를 편리하게 관리할 수 있음
    
## part3
### 인터페이스 만들기


    1. 뷰 추가하기
		* views.py에 def함수로 뷰를 만듦
		* <int:question_id> : db에 있는 데이터를 가져와서 url에 출력
	2. 뷰가 실제로 뭔가를 하도록 만들기
		* loader.get_template('') : '' 안에 있는 template을 불러옴
		* template.render()해서 알맞은 url에 index.html이 나타나게 함
	3. 지름길: render()
		* HttpResponse, loader 대신에 render를 사용함
	4. 404 에러 일으키기
		* raise Http404('') : 오류 발생시 ''안에 있는 문자 출력
	5. 지름길: get_object_or_404()
		* 모델을 첫번째 인자로 받고 객체가 존재하지 않을 경우에 
		  http404가 발생함
	7. 템플릿 시스템 이용하기
		* {% for ~ %} ~ {% endfor %} : html에서 for문을 사용할 수 있음
	8. 템플릿에서 하드코딩된 URL 제거하기
		* {% url %} : URL 경로들의 의존성을 제거할 수 있음
	9. URL의 이름공간 정하기
		* 여러가지 프로젝트 앱을 사용하기 위해 app_name을 urls.py에 선언함
	
## part4
###  상세 페이지 구현하기

	
    1. write a minimal form
		* 기존의 html에 있는 폼 형태를 이용함
		* forloop.counter : for태그가 반복한 횟수
		* {% csrf_token %} :  csrf 공격을 방지하기 위한 태그
		* request.post : request.post의 값은 항상 문자열임
		* HttpResponseRedirect : 버튼 클릭 시 이 url로 넘어감
		* pluralize : 복수형을 만들어주는 역할 (vote --> votes)
	2. views 수정
		* ListView : 개체 목록 표시
		* DetailView : 세부 정보 페이지 표시 
		/ URL의 기본키 값이 pk이기 때문에 pk로 바꿔야함
