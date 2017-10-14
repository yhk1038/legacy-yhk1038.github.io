---
layout: post
cover: 'assets/images/cover6.jpg'
title: 지킬 jasper 테마 적용시 json 버전 에러
subtitle: bundle install 실행 시 json-1.8.3 이 설치 안되는 문제 해결
date:   2017-10-13 19:10:00
tags: blog
subclass: 'post tag-test tag-content'
categories: 'Freddy'
navigation: True
logo: 'assets/images/docki-logo.png'
---

jasper 테마를 적용한 뒤 지킬을 실행하는 과정에서,
bundle install 이 중간에 멈추고 `check 'gem install json -v '1.8.3'`이라고 나왔다.

그래서 시키는대로 실행했다.

```
$ gem install json -v '1.8.3'
```

#### 에러
이번엔 다음과 같은 에러가 주욱 뜬다.

![gem install 과정에서 발생한 에러](assets/images/gem_json_1_8_3_install_error.png)

#### 난관

계속 시도해봤지만 같은 문제가 반복돼서 찾아보니 https://github.com/flori/json/issues/253 많은 사람들이 이미 겪고 있던 문제였다.(만세)

1. 나의 경우에는 맥을 쓰고 있으니 `sudo apt-get or yum` 을 실행할 수 없었고,
1. 그래서 `brew install coreutils` 을 실행해봤는데 동작하지 않는다.
1. gem json 자체를 언인스톨 하고 다시 설치해도 같은 문제가 발생한다.

#### 해결

문제는 Ruby 의 버전 이었다.
MacOS Sierra 에서 ruby version 2.4.0 은 json 1.8.3 과 호환되지 않는다고 한다.

의외로 다음과 같이 간단하게 모든걸 해결할 수 있다.


    ## Gemfile 에 json gem 을 3번 line 처럼 추가하면 끝난다. 

    1 source "https://rubygems.org"
    2
    3 gem 'json', github: 'flori/json', branch: 'v1.8'
    4 gem "jekyll", "~> 3.0.3"
    5 ...


그리고 나서

    $ bundle install


전부 해결됐다. 이제 다시 `jekyll serve`를 실행하면 4000 port 에서 블로그를 확인할 수 있다.