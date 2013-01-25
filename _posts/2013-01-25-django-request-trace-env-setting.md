---
layout: post
title: "Django request trace 환경 만들기"
description: ""
category: django
tags: [lesson, django, study, ipdb]
---
{% include JB/setup %}

Django webframework의 소스 분석 스터디를 진행하기 위해서 간편하게 ipdb를 사용한 트레이싱 방법을 소개한다.

#Virtualenv, Virtualenvwrapper 설치

먼저 시스템 기본 환경과 별도의 격리된 별도의 파이썬 인터프리터 환경을 만들어주면 좋다.

예를 들어 시스템 설치된 파이썬환경에 최신 버전의 django 패키지를 설치하게 되면 전체 시스템에 영향을 주게 된다.

그러면 예전 버전의 django에서 돌아가던 프로그램들이 갑작이 안돌게 되거나 예상치 못한 동작을 하는 문제가 발생할수 밖에 없다.

그래서 프로젝트별로 격리된 환경을 만들어서 특정 버전의 파이썬 인터프리터, 패키지를 설치 해도 시스템 전체에는 영향을 주지 않도록 하는것이다.

설명이 불친절하기는 했지만 여기까지 읽어도 필요성이 와닿지가 않는다면 일단 설치하지 않아도 상관 없다.

virtualenv의 설치에 관해서는 친절하게 설명해 주신 분들이 많기때문에 추천하는 문서를 링크하겠다.

[virtualenvwrapper tutorial](http://blog.fruiapps.com/2012/06/An-introductory-tutorial-to-python-virtualenv-and-virtualenvwrapper)

위의 링크 외에도 virtualenvwrapper로 검색하면 많은 글들이 있다.
추가로 pythonbrew라는 툴을 사용하면 파이썬인터프리터의 버전별 설치까지 관리할수 있다고 한다.

#Django 설치
Django는 간편하게 pip를 사용해서 설치하면 되지만 이건 stable버전에만 해당하는 이야기이다.

스터디에 사용하려는 1.5버전은 현재 RC버전으로 아직 정식 버전이 아닌상태라 pip로는 설치할수가 없다.

이럴때는 소스를 직접 받아서 설치를 해야하는데 가장 간단하면서도 활용도가 높은 git repository를 clone 하는 방식을 사용하자.

일단 git은 설치되어 있다고 가정하겠다(-_-;)

    $ git clone git://github.com/django/django.git

위의 명령어를 실행하면 django라는 디렉토리에 django 저장소가 복제된다.
복제가 완료되면 복제된 디렉토리로 가서 태그를 확인하자

    $ cd django
    $ git tag
    ...
    ...
    1.5b1
    1.5b2

1.5버전의 최신 태그로 체크아웃을 하면 소스가 해당 버전의 상태가 된다. 그러면 그상태로 인스톨하면 끝.

    $ git checkout 1.5b2
    $ python setup.py install

#ipdb 의 활용
소스코드의 실행 과정을 추적하기 위해서는 브레이크 포인트를 걸고 라인단위로 실행할 방법이 필요한데 eclipse pydev같은 IDE를 사용할수도 있지만 개인적으로는 간단하게 사용할수 있는 ipdb를 사용한다.

설치는 pip 를 사용해서 간단하게 된다.

    $ pip install ipdb

그리고 브레이크 포인트를 걸 특정 지점에 다음의 코드만 넣으면 끝~

{% highlight python %}
import ipdb;ipdb.set_trace()
{% endhighlight %}

코드가 실행되다가 위의 코드를 만나면 거기서 실행이 중단되고 디버깅 쉘이 뜨게 된다.

