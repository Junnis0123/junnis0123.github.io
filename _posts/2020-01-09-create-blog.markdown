---
layout: post
title:  "Window 환경에서 Git Blog 만들기"
date:   2020-01-09 01:47:31 +0900
subtitle: "알 수 없는 3 try"
tag: "jekyll"
---

안녕하세요, 김선진입니다. 

Window => Mac => Window에 걸친 제 블로그 이전이 드디어 끝난 기념으로 찍어뒀던 스크린샷들과 함께 포스팅을 해보려 합니다. 거의 애증이네요.
config, gemfile수정으로 7커밋을 했으나 되살리지 못한 두 번째 블로그... :innocent:

![2nd](./../img/posts/second-git-branch.png)

설치 방법은 [여기][jekyll]를 참고해서 작성되었습니다.


# 블로그 시작하기
Widnow는 Mac과 달리 ruby가 기본사양이 아니기 때문에 별도 설치를 해주어야 합니다.

## 1. ruby

[루비 다운로드 페이지 바로가기][ruby-install] 

위 링크에서 DevTool 포함된 인스톨러를 설치해주세요.
환경변수는 자동으로 생성해주기 때문에 따로 환경변수 설정은 하지 않았습니다.

## 2. jekyll

command를 열고 아래 명령어를 입력해주세요.
```
gem install jekyll bundler
```

지킬 번들을 인스톨합니다. 여기서 ```jekyll new "projectName"```를 이용해서 새 프로젝트를 생성해도 되지만, 테마를 적용하기 위해 편법을 사용해봅시다.

왜냐하면 지킬 기본 테마가 넘나 구리기 때문입니다.
![구린 지킬 기본 테마](./img/../../img/posts/jekyll-default-theme.png)

예쁘고 인기 있는 테마들을 둘러보는데 맘에 드는 게 두 개 있어서 그 중 지금 테마로 결정하게 되었습니다.
현재 제 블로그 테마는 Clean Blog Jekyll 입니다.

[클린 블로그 지킬  테마 깃 바로가기][theme]

원하는 테마를 찾아서 해당 깃을 ZIP로 다운로드 받아주세요.
![download](./img/../../img/posts/download-git.png)
다운로드가 완료되면 깃에 블로그용 르포를 하나 만들어주세요.

그 후 repo를 원하는 디렉토리에 clone한 뒤, 해당 디렉토리에 압축을 풀어줍니다. (테마 샘플을 그대로 베끼는 것과 같습니다.) 이렇게 쓰게 되면 gem으로 설치해서 사용하는 것보다 커스텀 테마를 사용할 수 있게 됩니다.

압축풀기가 끝났다면 디렉토리에서 command를 열고 아래 명령어를 입력해주세요.

```
bundle exec jekyll serve
```
이제 블로그 샘플 만들기가 끝이났습니다!

http://localhost:4000/

지킬 서버는 4000포트에서 실행됩니다.
이제 클론 한 페이지를 수정하기만 하면 되는데요, 저는 이 글 쓰기와 3번의 블로그 이사로 넘나 피곤한 상태가 되었기 때문에 나머지 커스텀 및 댓글 등에 대한 포스트를 다음에 진행해보려 합니다.

블로그를 새로 만드실 분이라면 gem으로 테마를 다운받는 것은 비추합니다. 뭔지 모를 에러가 와장창 나는 바람에 새벽 2시 21분에 분노의 글을 쓰는 일이 생겼기 때문이죠... 하여튼, 윈도우에서 개발하시는 모든 개발자분들을 응원하며 이번 포스팅을 마치겠습니다. 안녕!

[jekyll]: https://jekyllrb-ko.github.io/docs/usage/
[ruby-install]: https://rubyinstaller.org/downloads/
[theme]: https://github.com/BlackrockDigital/startbootstrap-clean-blog-jekyll