---
layout: post
title:  "깃 블로그 만들기 (테마 적용하기)"
date:   2025-04-11 00:47:38 +0900
categories: 깃블로그 루비설치 Jekyll설치 Jekyll테마모음
---

> Jekyll
>디자인부터 할 필요 없이 누군가 만들어둔 테마로 페이지 관리하는 데 사용할 수 있음
>
>Jekyll 테마 모음 사이트  
>[http://jekyllthemes.org/](http://jekyllthemes.org/)  
>[https://pages.github.com/themes/](https://pages.github.com/themes/)


# 루비 설치

Jekyll은 루비 언어로 구현되어 있다. 우리가 직접 루비를 다룰 일은 아직 없으니 Jekyll 테마를 이용한다면 반드시 설치해야 한다.

또, 루비를 설치하면 git에 push하기 전에 로컬에서도 확인할 수 있다.

루비 공식 홈페이지 : [https://www.ruby-lang.org/ko/documentation/installation/](https://www.ruby-lang.org/ko/documentation/installation/)

* ### Windows 설치 방법

    > 쉬운 설치
    > [https://rubyinstaller.org/downloads/](https://rubyinstaller.org/downloads/)
    > 
    1. Devkit 포함된 버전으로 설치
        - 설치파일을 실행해 설치 완료
    2. 루비 인코딩 (한글 인식 가능하도록)
        - 루비 커맨드창 열기 (Start Command Prompt with Ruby)
        - 블로그 관리용 폴더로 이동
        예) D:\git_blog
        - 인코딩 명령어 실행
            chcp 65001
        
        

* ### MacOS 설치 방법

    <aside>
    💡 선행 과제

    - brew 설치
    </aside>

    1. 루비 버전 관리 툴 설치 (버전이 안 맞을 수 있으니 공식 홈페이지에서 안정화 버전 확인 후 설치 권장)   
        *(루비 공식 사이트의 방식으로 했을 땐 오류가 발생해 버전 관리 툴 설치 방법으로 진행했음)*
        
        ```php
        brew update // 패키지 관리자 업데이트 brew 명령어 사용 전 실행하길 권장
        brew install rbenv // 루비 버전 관리 툴 설치
        rbenv --version // 루비 버전 관리 툴 설치 확인
        ```
        
    2. 루비 특정 버전 설치
        
        ```php
        rbenv install -l // 다운 가능한 루비 버전 확인
        rbenv install {version}
        ```
        
    3. 설치 완료 메세지를 확인하며 루비 global 설정  
        *(설치 완료 시 이 문구를 실행하라는 내용이 표시됩니다.)*
        ```php
        rbenv global {version}
        ```
        
    4. 루비 환경설정 적용
        
        ```php
        vi ~/.zshrc
        
        // vi 편집기에서 아래 내용 저장
        export PATH={$Home}/.rbenv/bin:${PATH} && \
        eval "$(rbenv init -)"
        
        ----
        
        source ~/.zshrc // 환경설정 적용
        ```

&nbsp;

&nbsp;
# Jekyll 설치

1. 명령어 실행  
    ```php
    gem install jekyll bundler
    gem install webrick // 로컬 서버 설치
    ```

    > MacOS에서 gem install jekyll bundler → __“Gem::FilePermissionError” 발생 시__   
    > 루비를 __rbenv로 버전을 지정해서 재설치__
    >

2. 로컬 서버 실행  
    ```php
    cd {블로그 관리용 폴더}

    bundle install
    bundle exec jekyll serve // 로컬 서버 실행
    ```

3. 로컬 서버 확인  
    bundle exec jekyll serve 까지 오류 없이 진행했다면 아래 주소에서 블로그 확인 가능
    - http://127.0.0.1:4000
    - http://localhost:4000

    ![image-center]({{ '/images/20250405_create_git_blog/check_local_url.png' | absolute_url }}){: .align-center}

&nbsp;

&nbsp;
# Jekyll 테마 적용
테마 모음 사이트에서 원하는 테마를 결정했으면 해당 테마의 homepage로 들어가보자. 친절한 개발자들이 자신의 테마를 적용하는 방법을 README에 정리해두었으니 이쪽 코드를 참고, 검색하면 활용할 수 있다.
&nbsp;

&nbsp;

&nbsp;

&nbsp;
* * *
&nbsp;

&nbsp;

&nbsp;
## 알게 된 내용

|용어|내용|
|---|---|
|gem|ruby에서 사용하는 외부 라이브러리|
|jekyll|블로그를 쉽게 만들 수 있게 해주는 gem|
|bundler|프로젝트에 필요한 gem과 그 버전을 GemFile에 적어두고 "bundle install" 명령어 실행 시 GemFile 안의 gem을 설치해주는 __의존성 관리 도구__|