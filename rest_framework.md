# django REST framework란
<div align = "center">
    <img src = "http://www.django-rest-framework.org/img/logo.png"alt = "logo">
</ div>

 - Django REST framework는 Web APIs 구축 툴킷
 - Django REST framwwork를 사용하는 메리트
    - 개발자에 [Web browsable API (http://restframework.herokuapp.com/)가 제공되며, 개발 된 API를 브라우저에서 테스트용으로 사용할 수 있다.
    - OAuth1a 및 OAuth2를위한 추가 패키지가 Authentication policies ( http://www.django-rest-framework.org/api-guide/authentication/ )에 포함되어있다. 이를 통해 사용자 인증을 통해 API를 사용할 수 있다.
    - Serialization이 ORM과 non-ORM 데이터 소스를 지원하고 있다.
    - Django의 based-views를 상속/오버라이드하여 커스터마이징 할 수 있다.

# Requirements/Installation
다음 링크를 참조
 - [Django REST framework - Requirements](http://www.django-rest-framework.org/#requirements)
 - [Django REST framework - Installation](http://www.django-rest-framework.org/#installation)

# RESTful한 API란
djnago REST framework는 RESTful API에 기반한 프레임 워크이다. 때문에 RESTful이 무엇인지를 아는 것은 개발에 중요한 것이다.
 - [RESTful API 란 무엇인가 - Qiita (http://qiita.com/NagaokaKenichi/items/0647c30ef596cedf4bf2)
 - [Web API 란 무엇인가 - Qiita (http://qiita.com/NagaokaKenichi/items/df4c8455ab527aeacf02#%E8%A8%AD%E8%A8%88%E3%81%AE%E7%BE%8E%E3%81%97%E3%81%84web-api%E3%81%AF%E4%BD%BF%E3%81%84%E3%82%84%E3%81%99%E3%81%84)
 - [REST API 란? - API 설계의 포인트 - 사용하면 안되는 코드 (http://wp.tech-style.info/archives/683)
 - [Restful api 설계 입문 - SlideShare (https://www.slideshare.net/MonstarLabInc/rest-ful-api)


# REST API의 구축 방법
※ 이미 app 폴더가 존재하고, model의 정의, setting파일 설정 migrate도 끝나고 있다고 하자.
 1. Serializer 작성
 2. ViewSet 작성
 3. URL pattern 설정

## 1. Serializer 작성
### Serializer란?
Serializer는 QuerySet 같은 복잡한 데이터와 모델 인스턴스가`JSON``XML` 및 기타 콘텐츠 형식으로 전환하는 것을 가능하게 하는 것이다. 또한 Serializer는 파싱된 데이터 (`JSON``XML` 등)를 모델 인스턴스나 QuerySet같은 복잡한 유형으로 전환할 수 있다.

![Serializer](https://image.slidesharecdn.com/djangorestframework-150428023227-conversion-gate02/95/introduction-to-django-rest-framework-an-easy-way-to-build-rest-framework-in-django-15-638.jpg?cb=1430720405)


### Serializer 클래스 작성
`serializers.py`을 app 폴더 아래에 작성하고 다음과 같이 Serializer 클래스를 정의한다.
```py
from rest_framework import serializers


class ModelNameSerializer(serializers.Serializer):
    model_field_name1 = serializers.CharField(max_length=200)
    model_field_name2 = serializers.IntegerField(read_only=True)
    model_field_name3 = serializers.DateTimeField()
```
 - [Tutorial 1: Serialization - Django REST framework](http://www.django-rest-framework.org/tutorial/1-serialization/)
 - [Serializers - Django REST framework](http://www.django-rest-framework.org/api-guide/serializers/)
 - [Serializer fields - Django REST framework](http://www.django-rest-framework.org/api-guide/fields/)
 - [Serializer relations - Django REST framework](http://www.django-rest-framework.org/api-guide/relations/)

## 2. ViewSet 만들기
`views.py` 파일에 Class-based Views를 사용하여 API의 클래스를 정의한다. Class-based Views 대신 함수를 정의하고 API를 개발할 수 있지만, DRY 코드를 달성하기 위해 전자를 사용하여 API를 작성한다.
```py
from app.models import Model1
from app.serializers import ModelNameSerializer

from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status


class ModelList(APIView):
    def get(self, request, format=None):
        model1s = Model1.objects.all()
        serializer = ModelNameSerializer(model1s, many=True)
        return Response(serializer.data)

    def post(self, request, format=None):
        serializer = ModelNameSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```
 - [Tutorial 3: Class-based Views - Django REST framework](http://www.django-rest-framework.org/tutorial/3-class-based-views/)
 - [Views - Django REST framework](http://www.django-rest-framework.org/api-guide/views/)
 - [Generic views - Django REST framework](http://www.django-rest-framework.org/api-guide/generic-views/)
 - [Viewsets - Django REST framework](http://www.django-rest-framework.org/api-guide/viewsets/)

## 3. URL pattern 설정
```py
from django.conf.urls import url, include


urlpatterns = [
    url(r'^api/', include('app.urls'))
]
```

---
# 참고 사이트
 - http://www.django-rest-framework.org/#
 - https://github.com/encode/django-rest-framework/tree/master
