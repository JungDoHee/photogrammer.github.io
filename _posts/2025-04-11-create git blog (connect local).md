---
layout: post
title:  "깃 블로그 만들기 (로컬과 연결하기)"
date:   2025-04-11 00:47:38 +0900
categories: 깃블로그 gitclone
---

> 선행 과제  
> git 설치 또는 GitHub Desktop 설치   
> 
> __본문에서는 git 명령어로 설명함__
    
&nbsp;
# 로컬 환경에 저장소 복제

## 1. PC의 원하는 경로에 블로그 관리용 폴더 생성

- Windows 예) D:\git_blog
- MacOS 예) /Users/user_name/git_blog

1. 저장소 복제
- 1에서 생성한 블로그 관리용 폴더로 이동
- 저장소 주소 복사 (저장소 → Code 클릭 → Local 탭)
    ![image-center]({{ '/images/20250405_create_git_blog/git_clone.png' | absolute_url }}){: .align-center}
    
- 터미널에서 git 명령어 실행 (1에서 생성한 블로그 관리용 폴더에서 실행해야 함)

```php
git clone HTTPS주소
```

&nbsp;
## 2. 페이지 생성 확인
- index.html 파일 생성 후 “hello” 입력 (>> 로컬에만 저장된 상태)
- 생성한 파일을 git에 적용

```php
git add -A // 수정된 파일을 대상으로
git commit -m "Create index file" // 저장소에 임시로 적용한 후
git push // 저장소에 적용
```

- url 확인  
    url 확인 방법 : 저장소 → Settings → Pages → GitHub Pages 영역에 표시됨  
    (만약 여기에 주소가 없다면 최대 1시간을 기다리거나, 도메인 형식에 맞지 않는 저장소 명칭을 사용했을 수 있음)
    ![image-center]({{ '/images/20250405_create_git_blog/settings.png' | absolute_url }}){: .align-center}
    
- 브라우저에서 url 확인
    ![image-center]({{ '/images/20250405_create_git_blog/check_url.png' | absolute_url }}){: .align-center}