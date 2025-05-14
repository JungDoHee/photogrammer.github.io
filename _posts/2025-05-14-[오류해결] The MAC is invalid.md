---
layout: post
title:  "[오류해결] The MAC is invalid"
date:   2025-05-14 23:08:00 +0900
categories: Laravel MAC APP_KEY
---

# DecryptException: The MAC is invalid

&nbsp;

새로운 회사에서 프로젝트를 세팅하고 분석하는데 이 오류가 발생했다. 

> The MAC is invalid
> 

라라벨에서 문자열 복호화를 위해 Crypt 파사드를 사용하는 곳에서 발생하는 오류이다.

&nbsp;

---

&nbsp;
## 🚫 오류 원인

라라벨 문서의 **Security > 암호화** 페이지에 따르면 해당 오류는 메시지 인증 코드 MAC(Message Authentication Code) 가 유효하지 않다는 의미이다.

🔗 [라라벨 > Security > 암호화](https://laravel.kr/docs/9.x/encryption)

암호화된 문자열이 MAC을 적용한 것과 일치하지 않아 복호화를 중단하고 DecryptException 예외를 발생시킨 것이다.

&nbsp;

---

&nbsp;
## 🔐 라라벨 MAC, 암호화 키의 역할

- 참고 : 라라벨 > Security > 암호화 > 설정하기

라라벨은 암복호화를 위해 config/app.php의 key를 사용해 암복호화문을 만든다.

```php
# config/app.php

/*
|--------------------------------------------------------------------------
| Encryption Key
|--------------------------------------------------------------------------
|
| This key is used by the Illuminate encrypter service and should be set
| to a random, 32 character string, otherwise these encrypted strings
| will not be safe. Please do this before deploying an application!
|
*/

'key' => env('APP_KEY'),
```

나는 .env의 APP_KEY 값을 참고하도록 설정했기 때문에 .env를 더 확인해야 한다.

```php
# .env
APP_NAME=로컬프로젝트
APP_ENV=local
APP_KEY=여기에 암복호화에 사용되는 키가 적용되어야 합니다
```

&nbsp;

APP_KEY 생성에는 두 가지 방법이 있다.

- php artisan key:generate
- 라라벨 설치 중 자동으로 적용

&nbsp;

공식 문서에 의하면 아래 흐름대로 암복호화 될 것을 기대할 수 있다.

```php
# 예시, 실제 코드와 다릅니다
apple => (암호화 with APP_KEY) => sjsk2CssZ90
sjsk2CssZ90 => (복호화 with APP_KEY) => apple
```

만약 복호화에 사용된 APP_KEY와 암호화에 사용된 APP_KEY가 다르면 값도 다르다.

&nbsp;

---

&nbsp;

## ✅ 해결 방법

프로젝트에 참여한 모든 개발자가 동일한 APP_KEY를 사용해야 한다. 나의 경우, 아래 내용을 순서대로 적용했을 때 문제가 해결되었다.

- APP_KEY 적용
- (로컬에서 두 개 이상의 브라우저 사용 시) 브라우저의 쿠키, 캐시 삭제
- 라라벨 재실행