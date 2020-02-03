---
layout: post
title:  "[Ruby On Rails] aws S3 + My SQL 이미지 저장"
date:   2020-02-03 11:38:31 +0900
---



안녕하세요, 김선진입니다.

이번에 새로운 토이프로젝트를 진행하면서 백엔드 개발에도 참여하게 되었습니다! (짝짝!)

오랜만에 백엔드 개발을 해보니 재미가 있네요.



환경 세팅부터 아주아주 힘들었네요.


이 짜증나는 루비 버전 에러...
저놈의 젬파일 버전과 제 루비 버전이 다른 오류 때문에 설치에만 엄청난 시간을 소모했는데요, 어찌 저찌 해결하고 나니까 homeprew 설치 한 것 때문에 npm이 충돌나서 본업인 웹개발을 못할 뻔 했지 뭐예요? 어이가 없었습니다.



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

이제 서버가 실행될 것입니다. 서버 실행이 정상적으로 되었다면 다음 화면이 표시됩니다.




# Main.erb 생성하기




