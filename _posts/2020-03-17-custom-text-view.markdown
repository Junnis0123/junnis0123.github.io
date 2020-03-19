---
layout: post
title:  "[Flutter] Flutter 앱 개발기 3 : custom text field 만들기"
date:   2020-02-18 11:38:31 +0900
---

안녕하세요, 김선진입니다.
이번에는 디자인 된 텍스트뷰가 필요한 사람들을 위한 커스텀 텍스트뷰 제작기 및 텍스트뷰의 내용을
가져다 쓰는 방법을 소개해보겠습니다.

## 0. stateful widget class 작성하기
먼저 stateful한 text field view를 만들기 위해 클래스를 선언해주세요.

내부의 필요한 변수들은 용도에 맞게 생성해주세요.

저는 필요한 변수를 다음과 같이 정의했습니다.

다트에서는 _로 시작하는 변수를 private로 설정한다는 점 알아주세요.

```dart
class CustomTFV extends StatefulWidget{

  BorderRadius radius;
  Color textColor;
  BorderSide borderSide;
  String hintText;
  TextStyle hintTextStyle;
  Color boxColor;
  bool isPassword;
  bool isComment;
  bool isFocus = false;
  bool isEditing = false;
  bool isEnable;
  Function onPressed;
  final FocusNode _focusNode = FocusNode();
  final TextEditingController _editingController = TextEditingController();
  CustomTFV({
    Key globalkey,
    this.radius = const BorderRadius.all(Radius.circular(8.0)),
    this.textColor = const Color(0xFFFFFFFF),
    this.borderSide = const BorderSide(color: Color(0xFFF6F6F6)),
    this.hintText = "",
    this.hintTextStyle = const TextStyle(fontSize: 16),
    this.boxColor = const Color(0xFFF6F6F6),
    this.isPassword = false,
    this.isComment = false,
    this.isEnable = true,
    @required this.onPressed
  }):super(key:globalkey);
  @override
  State<StatefulWidget> createState() {
    return CustomTFVState();
  }
  
}
```
## 1. state class 작성하기
state를 이어서 만들어주세요.

initState()에서 위젯에 필요한 리스너 및 스테이트를 설정해주시고,
필요하다면 dispose 함수를 오버라이드해주세요.

그리고 빌드할 Widget의 모습까지 코드로 작성해주시면 됩니다.

initState() 코드부분만 예시로 첨부하겠습니다.

```dart
class CustomTFVState extends State<CustomTFV>{
  String get currentText {
    return widget._editingController.text;
  }
  @override
  void initState(){
    super.initState();
    widget._focusNode.addListener((){
      if(widget._focusNode.hasFocus){
        setState((){
          widget.isFocus = true;
        });
      }else{
        setState(() {
          widget.isFocus=false;
        });
      }
    });
    widget._editingController.addListener((){
      if(widget._editingController.text.length == 0){
        setState((){widget.isEditing = false;});
      }else{
        setState((){widget.isEditing = true;});
      }
    });
  }
```

## 2. 다른곳에서 사용하기
다른 곳에서 text field를 사용하기 위해서는 TextEditingController를 선언해서 넘겨주어야 합니다.

아까 위에서 선언한 변수중 TextEditingController가 있었죠?
그부분에 들어갈 private한 객체를 생성해줍니다.
```dart
class DetailProduct extends StatelessWidget{
  final myController = TextEditingController();
```

클래스의 선언부에서는 이렇게 볼 수 있습니다.
변수를 이렇게 선언해서 사용하시면 됩니다.
dispose()를 오버라이딩하지 않았다면 여기서 생성한 녀석을
어디선가 dispose()해주어야 한다는 사실을 잊지 마세요.

child로 넘어가봅시다.

```dart
  child: CustomTFV(
              isComment: true,
              hintText: "댓글을 입력해주세요.(100자)",
              editingController: myController,
              commitFunction: () {
                return showDialog(
                  context: context,
                  builder: (context) {
                    return AlertDialog(
                      // Retrieve the text the user has entered by using the
                      // TextEditingController.
                      content: Text(myController.text),
                    );
                  },
                );
              },
            ),
          ),
```

차일드에서는 필요한 변수들을 전부 넘겨주어 사용하시면 됩니다.
컨트롤러의 text에 접근해서 내용물을 가져올 수 있습니다.

다들 즐거운 플러터 앱 만드세요.

그럼 이만~