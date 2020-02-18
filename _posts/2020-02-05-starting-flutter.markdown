---
layout: post
title:  "[Flutter] Flutter 앱 개발기 1 : 플러터 시작해보기"
date:   2020-02-18 11:38:31 +0900
---

안녕하세요, 김선진입니다.

서버 프로젝트를 개설했으니 프론트 프로젝트도 개설해보려 합니다.

우선 플러터에 대해 간단히 설명하자면 Google이 만든 오픈소스 모바일 애플리케이션 개발 프레임워크로 1.2버전부터는 웹 애플리케이션 개발도 지원하기 시작했습니다.
웹개발을 얼마나 잘 지원해 줄 지는 모르겠지만, 세 가지 플랫폼에서 한 가지 코드로 구동 가능하다는 점이 아주 매력적입니다.

처음 플러터와 가장 고민했던 프레임워크는 Facebook이 만든 프레임워크인 React Native인데요, 리액트가 조금 더 일찍 나온 만큼 개발자 풀도 넓고 정보도 많은데다
JS를 이용한 개발이 가능하다는 큰 장점이 있죠. 하지만 성능이 플러터에 비해 떨어진단 단점이 있습니다.
RN은 겨우 네이티브 성능에 가까운 성능을 내고 있는 반면,
플러터는 네이티브와 비교해도 손색 없는 빠른 속도를 자랑합니다.

RN이 JS 레이어를 한 층 끼고 있기 때문에 개발은 편하지만 속도는 느린 묻더 단점을 가진 것입니다...

플러터는 1.0버전을 출시한 반면 RN은 아직도 0.61 버전에 머물러있다는 불안정성또한 남아있었습니다. (페이스북이 만들다 만 프레임워크...)
여튼 쨌든 그러햐여 플러터의 도입을 결정하게 되었네요.

# 플러터 개발하기
## 1.플러터 설치
https://flutter-ko.dev/docs/get-started/install
먼저 플러터를 설치해야 합니다. 설치 가이드는 위 링크에 굉장히 잘 작성이 되어 있어요.

설치 도중에 요상한 에러가 발생합니다.
```
ache/dart-sdk/bin/dart: No such file or directory
```
요상한 에러가 발생합니다. 환경 설정은 언제나 에러가 따르는 것 같죠^^?

설치한 폴더로 가서 bin/cache 폴더를 삭제해주세요. 그 후에 실행하시면 해당 에러가 사라집니다.

## 2. IDE 설치
IOS 개발용 Xcode 및 Android 개발용 Android studio를 설치해주세요.

## 3. 플러터 프로젝트 만들기

```
1) IDE를 켜고 Start a new Flutter project를 선택하세요.

2) 프로젝트 타입으로 Flutter Application을 선택하세요. Next을 누르세요.

3) SDK의 위치에 맞게 경로가 설정되어 있는지 확인하세요. (텍스트 필드가 비어 있으면 Install SDK…를 선택하세요).

4) 프로젝트 이름을 입력하세요 (예, myapp). 그런 다음 Next를 누르세요.

5) Finish를 클릭하세요.

6) Android 스튜디오가 SDK를 설치하고 프로젝트를 생성할 때까지 기다리세요.
```

저는 IDE로 IntelliJ를 사용하고 있어 해당 IDE에서 시작하는 법을 홈페이지에서 가져왔습니다.
하지만, Web 서비스를 생각하고 있다면 이렇게 프로젝트를 생성하시면 안됩니다.

(플러터는 현재 웹 응용 프로그램에 대한 버전이 1을 넘지 않았고 프로덕션 용도로 권장하고 있지 않습니다.)

```
 flutter channel master
 flutter upgrade
 flutter config --enable-web
 flutter create myapp //프로젝트를 처음 생성할 경우
 cd <into project directory> //프로젝트가 이미 생성된 경우
 flutter create .
```

아까 만든, 또는 새로 시작할 프로젝트에 대해 위 명령어를 입력해주시면 웹 개발이 가능하게 세팅됩니다.

플러터는 기본적으로 크롬 위에서 빌드되므로 꼭 크롬을 설치해주세요. 

## 4. 소스코드 작성하기
### 4-1. Package 나누기
dart 언어와 flutter 문서가 아주 상냥하고 상세한 부분까지 설명이 되어있으므로 
자세한 설명은 문서를 참고해주세요.

저는 이번 개발 프로젝트에서 dart언어를 처음 사용해 보았는데 횡스크롤이 길어져서 썩 마음에 드는 언어는 아니었습니다.
원래 취향대로 탭을 4칸 썼다면 너무 넓게 느껴졌을 것 같네요.

처음 만든 앱은 main.dart 파일이 미리 만들어져 있을 겁니다.
이 파일을 지우고 고쳐서 마음에 드는 방식으로 코드를 바꿔주시면 됩니다.

저는 패키지를 나누어서 작성했습니다.

![프로젝트 구조](../img/posts/project.png)

views
- home

widgets
- commons
- goodsList
- homeInfo
- navigationBar

제 방식이 정답은 아니기 때문에 디렉토리 구조는 원하는 대로 나누시면 될 것 같습니다.

플러터에서는 위젯이라는 이름으로 화면 구성요소를 분리합니다.
state를 가지냐 가지지 않느냐에 따라 위젯의 속성이 달라집니다.

저는 정적 화면인 경우 StatelessWidget, 동적 화면인 경우 StatefulWidget 으로 만들었습니다.

### 4-2. main.dart
메인 화면을 전부 지우고 원하는 앱 코드를 작성해주세요.

```
void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: '원하는 타이틀',
      theme: ThemeData(
        primarySwatch: 원하는 색,
      ),
      home: LoginView(),
      initialRoute: '/Login',
      routes: {
        '/Login' : (context) => new LoginView(),
        '/Home': (context) => new HomeView(),
      },
    );
  }
}
```

저는 이렇게 작성했습니다. 로그인 기능을 포함하고 있기 때문에 초기 라우트를 Login으로 설정했고,
Home 화면도 Route에 추가해 주었습니다.


### 4-3. home_view.dart
이미 flutter 자체에서 디자인 된 위젯을 많이 제공해주고 있어 별 디자인이 없어도 개발이 가능하지만,
이 프로젝트에는 신원미상의 실력자 디자이너분이 참여해주셨기 때문에 디자인과 함꼐 개발하는 방법에 대해서도
작성해보도록 하겠습니다.

```

class HomeView extends StatelessWidget {
  const HomeView({Key key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.white,
      body: ListView(
        children: <Widget>[
          Container(
            child : NavigationBar(),
          ),
          Container(
            child: HomeInfo(),
          ),
          Container(
            child:  GoodsList(),
          ),
        ],
      )
    );
  }
}

```
홈은 전부 위젯 단위로 쪼개서 개발했습니다.

위젯 단위로 쪼개지 않으면 탭이 너무 많아서 보기가 힘드니 가능한 위젯 단위로 작업하시길 추천드립니다.

flutter Class는 아주 세세한 단위로 나누어져있습니다.
저는 세로 스크롤이 되는 화면을 만들어야 했기 때문에 바디에 ListView를 넣고 children 을 Widget 배열로
명시한 후 각각의 Widget들을 Container 로 감싸 주었습니다. 


상단 네비게이션을 담당할 Navigation Bar를 만들었고, 중앙의 유저 소개를 보여줄 HomeInfo 위젯과
무한 스크롤을 통해 상품 목록을 불러올 GoodsList 위젯으로 나누어서 개발했습니다.

다음 개발기에서는 axios 통신에 대하여 작성해보도록 하겠습니다.
그럼 이만!
