---
layout: post
title:  "[Ruby On Rails] Jets 프로젝트 만들"
date:   2020-02-03 11:38:31 +0900
---



안녕하세요, 김선진입니다.

이번에 새로운 토이프로젝트를 진행하면서 백엔드 개발에도 참여하게 되었습니다! (짝짝!)


역시 새로운 도전을 하는 건 환경 세팅이 제일 문제인 것 같습니다.

환경만 이틀째 세팅중이네요.

이 짜증나는 루비 버전 에러...
저놈의 젬파일 버전과 제 루비 버전이 다른 오류 때문에 설치에만 엄청난 시간을 소모했는데요,
어찌 저찌 해결하고 나니까 homeprew 설치 한 것 때문에 npm이 충돌나서 본업인 웹개발을 못할 뻔 해서 어이가 없었습니다.



# 프로젝트 세팅하기

현재는 private git repo를 이용해서 개발중입니다.

새로 프로젝트를 생성하시고 개발을 진행할 폴더에서 git clone [repourl] 을 해주세요.

루비마인을 열고 해당 프로젝트를 열어주신 뒤 yarn install 을 실행해줍니다.

```
gem install bundle
bundle intsall
bundle update
```

기본 세팅을 위해 순서대로 실행해줍시다. 여기서 만약 rbenv 버전이 맞지 않다는 에러가 발생하면

``` 
rbenv install 2.6.3
rbenv local 2.6.3
```

하신 후 위의  명령어 3개를 입력해주시면 됩니다. 번들 다운로드가 모두 끝났다면 이제 프로젝트 환경설정은 끝입니다!



## 1. DB 마이그레이션

우선 brew install mysql 로 db를 설치해주세요.

그리고 mysql.server start 명령어를 입력하면 서버가 잘 동작해야 하는데 그렇지 못합니다.

```
ERROR! MySQL server PID file could not be found!
```

잘못된 PID를 물고 실행중인 mysql process를 죽여주고 다시 실행해야합니다. 다음 명령어를 입력해서 실행중인 mysql 서버를 찾아주세요. 맨 처음 나오는 숫자가 PID입니다.

```
ps aux | grep mysql
sudo kill -15 [PID]
mysql.server start
```


순서대로 입력하면 드디어 SUCCESS! 메시지가 출력되는 것을 볼 수 있습니다.

감격적이네요.

이제 레일즈의 마이그레이션 방법대로 db를 마이그레이션 해줍시다. 아래 명령어를 순서대로 입력해주세요.

저는 db를 토이프로젝트 함께 작업하시는 혜리님께서 미리 작업해주셨기 때문에 별도로 테이블을 만들지는 않았습니다.



rake db:create
rake db:migrate



## 2. 웹 서버 시작하기

```
rails server
```

이제 서버가 실행될 것입니다. 서버 실행이 정상적으로 되었다면 다음 화면이 표시됩니다.기

이렇게 rails 프로젝트로 개발을 하나 했더니 프로젝트 사양이 엎어져서 api로 개발 노선을 틀기로 했어요!

바뀐 환경을 새로 세팅해주는 김에 git repository 까지 만들어보기로 했습니다.



# Jet 설치하기

하루 걸러 하루 새로운 환경을 세팅해야한다니.
사실 어제는 또 Windows에 Ruby 세팅을 했거든요. 너무 노답이라 해당 내용은 패스하겠습니다.

Jet 설치는 https://haereeroo.tistory.com/10 포스트를 참고 해서 진행했습니다.
그런데 포스트랑 제 설치 환경이 조금 많이 달랐아요. 처음에는 webpack 충돌인가 싶었는데 아니었습니다.

## 알 수 없는 에러 해결하기.
![error](../../../img/posts/스크린샷%202020-02-04%20오후%2011.59.57.png)
스크린샷을 보시면 아시다시피 bundler가 jets를 로드하는데 실패했다고 하고 초록색으로 어떤 행위를 하면 좋을지 설명해주네요.
그대로 한 번 실행해봅시다.


```
vi ~/.aws/config
```
![vi](../../../img/posts/스크린샷%202020-02-05%20오전%201.25.23.png)
아무 세팅이 없는 config 파일을 확인할 수 있습니다.

i를 눌러 insert모드로 바꿔주시고 region을 작성해주세요.


![vi-edit](../../../img/posts/스크린샷%202020-02-05%20오전%201.25.45.png)
작성이 끝났다면 :wq를 입력해서 저장해줍니다.
이제 다시 콘솔로 돌아와 원하는 위치로 간 다음

```
jets new [projectName]

or

jets new [projectName] -mode api
```
저는 api 서버를 만들기 떄문에 mode api 파라매터와 함께 프로젝트를 시작했습니다.

```
cd [projectName]
bundle install
jets server
```
명령어를 입력하시고 localhost:8888 으로 접속하시면 짠.


![vi-edit](../../../img/posts/스크린샷%202020-02-05%20오전%201.33.27.png)
Congrats 🎉 You have successfully created a Jets project.


여러분도 이제 API 개발을 시작하실 수 있습니다!
