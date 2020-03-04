---
layout: post
title:  "[Flutter] Flutter 앱 개발기 2 : 플러터에서 Axios 통신하기"
date:   2020-02-18 11:38:31 +0900
---

안녕하세요, 김선진입니다.

1편에서 말한 대로 이번 포스팅에서는 flutter로 axios통신 하는 방법에 대해 설명해보겠습니다.
그 전에 flutter의 Stateful Widget에 대해 조금 알아봅시다.

stateful widget에 대한 공식 flutter 문서의 설명은 아래와 같습니다.

```cassandraql
A widget that has mutable state.

State is information that (1) can be read synchronously when the widget is built and (2) might change during the lifetime of the widget. It is the responsibility of the widget implementer to ensure that the State is promptly notified when such state changes, using State.setState.

A stateful widget is a widget that describes part of the user interface by building a constellation of other widgets that describe the user interface more concretely. The building process continues recursively until the description of the user interface is fully concrete (e.g., consists entirely of RenderObjectWidgets, which describe concrete RenderObjects).

Stateful widgets are useful when the part of the user interface you are describing can change dynamically, e.g. due to having an internal clock-driven state, or depending on some system state. For compositions that depend only on the configuration information in the object itself and the BuildContext in which the widget is inflated, consider using StatelessWidget.
```
출처: [flutter dev][stateful]

stateful widfet은 stateless widget과 달리 변경 가능한 State를 가집니다.

그렇기 떄문에 비동기 통신 등으로 데이터를 받아와 저장하거나, 함수 등을 이용해서 값을 변경해줘야 하는 화면에서 유용하게 쓰입니다.

저는 이번에 http 통신으로 값을 받아와 채워넣는 goods_list 파일과 함께 stateful widget을 사용한 개발을 진행해보았습니다.

# goods_list.dart 작성하기
## 1.StatefulWidget 만들기
가장 먼저 stateful widget 클래스를 상속받는 GoodsList Class를 만들어주세요.

```
class GoodsList extends StatefulWidget {
  @override
  _State createState() => _State();

  const GoodsList({Key key}) : super(key: key);
}
```

GoodsList Class는 State를 실제로 생성해주는 역할을 담당합니다.

저는 별도 클래스로 GoodsList를 받아오고 있기 때문에 이 클래스 내부에서 초기화만 진행하고 있는데요,
flutter 공식 홈페이지의 다른 StatefulWidget 코드를 보시면 위젯 내부에 더 많은 생성 인자 및 필드를 가질 수 있습니다.

```
class Bird extends StatefulWidget {
  const Bird({
    Key key,
    this.color = const Color(0xFFFFE306),
    this.child,
  }) : super(key: key);

  final Color color;
  final Widget child;

  _BirdState createState() => _BirdState();
}
```
출처: [flutter dev][stateful]

## 2. State 만들기
먼저 만든 위젯에서 사용할 State를 상속받은 _State Calss를 만들어주세요.

State Class에서는 실제 위젯의 골격을 빌드하고, initState() 함수를 통해 State초기화를 진행합니다.
위젯에서 사용할 데이터 또한 이 곳에서 관리할 수 있습니다. (또는 위젯 내부에서 관리 해도 무관합니다. 저는 여기서만 사용하도록 했습니다.)

골격은 원하시는 대로 작성해주시면 됩니다. 제가 만든 예시 코드는 아래와 같습니다.

```
class _State extends State<GoodsList> {
  Future<List<Goods>> goodsList;

  @override
  void initState() {
    super.initState();
    goodsList = getGoodsList();
  }

  @override
  Widget build(BuildContext context) {
    return
      FutureBuilder<List<Goods>>(
        future: getGoodsList(),
        builder: (context, snapshot) {
          if (snapshot.hasError) print(snapshot.error);

          return snapshot.hasData
              ? GridView.builder(
              physics: ScrollPhysics(),
              shrinkWrap: true,
              gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
                crossAxisCount: 2,
              ),
              itemCount: snapshot.data.length,
              itemBuilder: (context, index) {
                return GoodsItem(snapshot.data[index].name, snapshot.data[index].price);
              }
          )
              : Center(child: CircularProgressIndicator());
        },
      );
  }
}
```

## 3.Goods Class 만들기
이제 통신 결과를 Jsom파싱해서 넣어줄 Goods Class를 만들어주세요. 사용하시는 api의 단위 모델처럼 만드시면 됩니다.

```

class Goods {
  final int id;
  final String name;
  final int price;

  Goods({this.id, this.name, this.price});

  factory Goods.fromJson(Map<String, dynamic> json) {
    return Goods(
      id: json['id'],
      name: json['name'],
      price: json['price'],
    );
  }
}

```
Goods Class내부에서는 Json 파싱해서 생성하는 factory를 만들어 주시면 됩니다.
factory 지정자는 인스턴스를 직접 생성하는 방식이 아니라 함수가 대신 생성해주는 방식입니다. 생성자 대신 사용할 수 있어요.

클래스가 간단해서 좋네요.

## 4.Http 통신 구현하기
### 4-1.http get 함수 만들기
```
Future<http.Response> get(url) async {
  return await http.get('baseurl');
}
```
http call을 날려 Future를 반환할 base url을 가진 아이를 만들어주세요.
Future는 dart 클래스로, 말 그대로 미래를 상징합니다. 비동기 통신의 결과인 미래를 반환하고, 미완료/완료 두가지의 상태를 갖습니다.

Future 타입을 반환하는 경우 await 키워드를 써서 값을 반환받아줘야 합니다.

### 4-2.JSON parsing 함수 만들기
response로 날아올 json을 파싱할 함수를 만들어주세요.

아까 만든 factory를 이용해서 인스턴스를 생성해주시면 됩니다.
```
List<Goods> parseGoodsList(String responseBody) {
  final parsed = json.decode(responseBody).cast<Map<String, dynamic>>();
  return parsed.map<Goods>((json) => Goods.fromJson(json)).toList();
}
```

### 4-3.getGoodsList 함수 만들기
마지막으로 위에서 만든 함수들을 이용 getGoodsList한 함수를 만들어주세요.
```
Future<List<Goods>> getGoodsList() async {
  try {
    var result = await get('path');
    if (result.statusCode == 200) {
      // If server returns an OK response, parse the JSON.
      return parseGoodsList(result.body);
    } else {
      // If that response was not OK, throw an error.
      throw Exception('Failed to load post');
    }
  } catch (e) {
    print(e);
  }
}
```
아까 만든 get 함수로 path를 넘겨주시고, stateCode를 이용해서 성공을 가리는 부분을 작성해주세요.
(api에서 예외처리가 잘 되어있다면 try로 감싸는 것 만으로 충분합니다.) 실패할 경우에 대한 부분도 잘 작성해주시면 드디어 !
오늘 목표로 했던 http 통신을 하는 flutter 위젯 만들기가 끝이 납니다.ㅎㅎ


다음 시간에는 custom text field 만들기로 찾아오겠습니다.
그럼 이만~!

[stateful]: https://api.flutter.dev/flutter/widgets/StatefulWidget-class.html