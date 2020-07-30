---
layout: post
title:  "[Flutter] Flutter 앱 개발기 4 : firebase login 연결하기"
date:   2020-07-14 11:38:31 +0900
---

안녕하세요, 김선진입니다.

한동안 블로그를 쉬었는데요,
블로그를 쉬었더니 개발도 같이 쉬어버려서 다시 시작하기로 했습니다.
flutter 앱 개발기도 점점 막바지로 치닫고 있네요.

이번에는 서버 개발자분과 저 모두 골머리를 앓게 했던 oauth2를 과감히 버리고
git reset으로 한 번 날렸다가(부들) 어찌 저찌 겨우 살려낸 좌충우돌
firebase auth 구현기입니다.


## 0. firebase 프로젝트 만들기
가장 먼저 해야 할 것은 firebase 프로젝트 만들기입니다.
아래 사진을 따라 다음, 다음, 다음을 눌러주세요.

(구글 계정이 없다면 구글 계정부터 만들어주세요.)
![firebase1](../../../img/posts/20201714-firebase1.png){: width="100"}
![firebase2](../../../img/posts/20200714-firebase2.png){: width="100"}
![firebase3](../../../img/posts/20200714-firebase3.png){: width="100"}
![firebase4](../../../img/posts/20200714-firebase4.png){: width="100"}


## 1. firebase ios 앱 추가하기
방금 만든 프로젝트로 들어가서 앱 추가를 해봅시다.
먼저 IOS 앱을 추가해봅시다.

### 1-1. 앱 등록
![ios1](../../../img/posts/20200714-ios1.png){: width="100"}
앱 주소와 앱 닉네임을 대강 정해주세요.
번들ID는 프로젝트 만드실 때 설정한 번들 ID로 해 주시면 됩니다.

### 1-2. 구성 파일 다운로드
앱 등록을 대충 해주시고 다음을 눌러 구성 파일을 다운받아주세요.
![ios2](../../../img/posts/20200714-ios2.png){: width="100"}
구성 파일은 xcode로 프로젝트를 열어서 추가해주세요.
(직접 추가하면 인식하지 못해서 빌드가 안 될 확률이 있습니다...)

### 1-3. Firebase SDK 추가
```
pod init
```
pod file을 열고 다음 코드를 추가해주세요.
```
# add the Firebase pod for Google Analytics
pod 'Firebase/Analytics'
# add pods for any other desired Firebase products
# https://firebase.google.com/docs/ios/setup#available-pods
```
pod file을 저장하고 다음 명령어를 실행합니다.
```
pod install
```

### 1-4. URL Scheme 추가
![ios3](../../../img/posts/20200714-ios3.png){: width="100"}
파일 추가가 끝났으면 URL 스키마를 추가해주세요.

## 2. firebase web 앱 추가하기
pubspec.yaml 파일을 열고 아래 라인을 추가해주세요.
```
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^0.2.5  # add dependency for Firebase Core
```

디펜던시 추가가 끝났으면 로그인 버튼 등에 아래 코드를 추가해주세요.

```
   FirebaseAuth auth = FirebaseAuth.instance;
    GoogleSignIn googleSignIn = GoogleSignIn();

    GoogleSignInAccount account = await googleSignIn.signIn();
    GoogleSignInAuthentication authentication = await account.authentication;
```

이제 후처리를 하시면 완료~.
다들 즐거운 플러터 앱 만드세요.

그럼 이만~
