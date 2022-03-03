# Flutter Development Guide

###### written by Doohyeon Kim,   First written on 2021.05.17, last updated date on 2022.03.02

<br>

# Development
**Flutter를 Android나 Web 개발처럼 하지 않도록 주의!!**<br>
**프로세스를 위한 개발이나 개발 방법론을 위한 개발이 아닌, 제품/서비스를 위한 개발을 한다.**<br><br>
개발방법론은 어디까지나 개발자의 실수를 줄이고, 생산성과 유지보수를 개선하기 위한 방법일 뿐이다.<br>
방법론에 얽매기이 보다는, 해당 방법론이 왜 나오게 되었는지를 고민하며 개발한다.<br>
주어진 상황에 맞는 방법을 선택한다.<br>

<br>

## Architecture
이해를 돕기 위해 Clean Architecture와 MVVM 디자인 패턴과 비교했다.

회색으로 칠해져있는 부분은 상황에 따라 생략 가능하다.

![doolumi_flutter_architecture](https://user-images.githubusercontent.com/92246475/154798139-66b8e5d5-17c6-45fd-857c-792876226501.png)


<br>

## Philosophy
1. Client-Side는 가독성을 우선으로 개발한다.
2. 불필요한 기능을 구현하는 것을 피한다.
3. 기능을 구현했으면 테스트 코드를 작성하여 테스트한다.
4. TDD는 상황과 리소스를 고려하여 적용한다.
   - 상황을 고려하지 않은 TDD는 overengineering일 수 있으니 상황에 맞게 도입한다.
   - TDD를 하게 되면 기본적으로 유닛 테스트, 통합 테스트를 실시하며, 위젯 테스트까지 하면 좋지만 simulation과 emulatior로 대체해도 좋다.
   - 유닛 테스트에 너무 많은 시간 쓰지 말고, 통합 테스트를 중점으로 한다.<br>
5. 비용이 많이드는 작업은 비동기처리 한다.
6. 소통시 용어는 가능한 영어로 통일
   - 한국어 용어는 이중번역 되었거나 영어, 일본어에서 각각 번역 된 용어들이 있다.
   - 코드도 영어, 레퍼런스도 영어, 검색도 영어로 해서 더 익숙하다. 


<br><br>    

# Policies
1. print를 사용하지 않는다. assert 또는 logger를 사용한다.
2. global const를 사용하지 않는다. 가능한 local const 또는 static const를 사용하며, global const가 많아지면 따로 빼서 작성한다.
3. enum을 사용할 때는 if문과 ternary operator 사용하지 않는다. switch문을 사용한다.
   -- switch문을 사용하면 값 중 하나라도 놓칠 경우 analayzer가 경고를 표시한다.
4. var, dynamic을 사용하지 않는다.
    - 가능한 항상 generic으로 구현한다.
    - List 또는 Map을 명시적으로 구현한다.
    - Object도 되도록 피할 것을 권장하며, 차라리 casting을 사용한다.
    - 이렇게 구현하면 컴파일 시 개발자의 의도와 다른 부분을 컴파일러가 캐치해서 출력해줄 수 있다.
5. 개발한 파일에는 상단에 Author, Date, LastEditor, LastEditTime, Description을 comment한다. 이 때 주석은 block comments가 아닌 doc comments로 작성한다.
6. component 코드 작성시, animation을 제외한 정적인 view는 캡쳐해서 같은 폴더 안에 넣는다. (배포 시에는 삭제)
7. textStyle, font, theme, color 등은 직접 사용하지 않고, 미리 정의된 것만 import해서 사용한다. textStyle에서 color 변경은 copyWith를 사용한다.
8. Exception이 필요한 이유가 명확한게 아니면 if, else를 사용하여 예외처리를 한다.
    - 코드가 직관적이고 성능상의 이점도 있다.
9. 위젯에서 다른 위젯의 state를 참조하여 rebuild 할 때, property drilling 하지 않고 InheritedWidget이나 상태관리 패키지를 사용한다.
10. Class와 Widget은 상속 보다는 조합을 사용한다.
11. extension 사용하지 말고 관련 클래스 안에 메소드를 직접 구현한다.
12. 주석에 질문 금지, 주석으로 코드설명 금지, 주석은 구현에 대한 정보와 의도 또는 중요성에 대한 설명이 필요할 때만 작성한다.
13. loop/conditional statement 등 모든 코드에 bracket을 사용한다.
14. underscore(_)를 prefix로 사용하면 안 된다. dart에선 _가 private을 뜻한다.
15. View는 method -> build widget -> UI widget 순으로 작성한다.
16. import는 dart - package - 그 외 코드 순으로 import 하고, export는 import 뒤에 한다.
17. 80자 이하로 처리 가능한 코드가 아니면 bracket마다 comma(,) 무조건 사용
18. interleaving을 피한다.
    - child 타입에 따라 다른 action을 하기 위한 check 코드 또는 is를 사용하는 방식은 피한다.
    - 각각의 render 객체는 하나의 문제만 해결한다. 예를들어, 하나의 렌더 객체가 shape와 opacity에 대한 처리를 하는 대신, shape에 대한 객체와 opacity에 대한 객체를 각각 작성한다.
    - mutable 객체보다 immutable 객체를 선호한다. immutable 객체는 downstream customer가 데이터를 변경하는 리스크 없이 안전하게 값을 전달한다.
19. global state를 피한다. 함수는 해당하는 argument에 대해서만 작동해야 한다. instance 메소드라면 저장된 데이터에 대해서만 작동해야 한다.
20. log는 사용자가 무시할 수 있고 안전하게 사용 가능한 부분에만 작성한다. 특히 어떤 정보를 담는 logging 피한다. 디버깅 할 때는 해당 디버깅에서 필요한 부분만 임시로 logging 한다. 디버깅 메시지를 작성할 때는 메세지가 아닌 플래그 형태로 작성한다.
21. json_serializable을 사용한다. 프로젝트가 커지고 코드가 많아졌을 때 개발자의 실수를 줄여준다.
22. Widget은 가능한 const 키워들 붙여서 immutable하게 만든다. 불필요한 rebuild를 막아 성능에 이점이 있다.


<br>

## Naming
기본적인 네이밍 룰은 Style guide for Flutter를 참조한다.

https://github.com/flutter/flutter/wiki/Style-guide-for-Flutter-repo

그 외에는 effective dart를 참조한다.

https://dart.dev/guides/language/effective-dart/style

1. 미국식 영어를 사용한다.
2. 네이밍은 길어지더라도 최대한 이름만 보고 뜻을 알 수 있게 작성한다.
3. 모든 네이밍은 형태/종류, 의미/속성, 숫자, 상태로 조합하여 작성하고, 숫자는 01, 02, 03 형태로 작성한다.
   |Bad 😞|Good 😊|
   |:---:|:---:|
   |```on_01green_btn.dart```|**button_green01_on.dart**|
4. global constant에는 prefix k를 붙인다.
5. class, enum, extension, typedef 등에는 UpperCamelCase를 사용한다.
6. library, package, directory, source file, import prefix에는 snake_case를 사용한다.
7. 그 외 변수/상수 이름 등에는 lowerCamelCase를 사용한다.
8. 경로는 끝에 /를 붙이지 않으며, 띄어쓰기가 필요할 때는 -를 붙인다.
   |Bad 😞|Good 😊|
   |:---:|:---:|
   |``` /path/path/ ```|**/path/path-path**|
9. 다른 dart 파일을 import 할 때는 'package:'가 포함된 형태로 import 한다.

   **Bad** 😞
    ```
    import 'services/network_service.dart'; 
    ``` 
   **Good** 😊
    ```
    import 'package:flutter_project/lib/services/network_service.dart';
    ```
10. 관용적인 용어가 아니면 약어 사용을 피한다.
    |Bad 😞|Good 😊|
    |:---:|:---:|
    |```i```|**index**|
    |```horizontalPosition```|**x**|

11. double negative(이중 부정문) 사용 금지
    |Bad 😞|Good 😊|
    |:---:|:---:|
    |```disabled = false```|**enabled = true**|
    |```hidden = false```|**visible = true**|
12. 디버깅을 위한 변수나 클래스는 prefix로 debug, Debug를 붙인다. 프로덕선, 릴리즈에선 해당 코드는 전부 삭제한다.
13. 테스트를 위한 임시 디렉토리 또는 임시 파일을 만드는 경우, 다음과 같은 규칙으로 만든다.
    |Bad 😞|Good 😊|
    |:---:|:---:|
    |```testDir...```|**tempDir...**|
    |```testCode...```|**tempCode...**|

<br>

## Project File & Directory Structure
###### 목적 없는 지나친 정규화는 피한다.

### Directory Structure
``` bash
.
├── README.md
├── analysis_options.yaml
├── android
├── assets
│   ├── fonts
│   ├── icons
│   └── images
├── ios
├── lib
│   ├── components
│   │   └── component.dart
│   ├── config
│   │   ├── config.dart
│   │   ├── palette.dart
│   │   └── routes.dart
│   ├── constants
│   │   ├── api_path.dart
│   │   └── app_constants.dart
│   ├── features
│   │   ├── feature01
│   │   │   ├── data
│   │   │   │   ├── data_sources
│   │   │   │   │   ├── local
│   │   │   │   │   │   └── sample_local.dart
│   │   │   │   │   └── remote
│   │   │   │   │       └── sample_remote.dart
│   │   │   │   ├── entities
│   │   │   │   │   └── sample_entity.dart
│   │   │   │   └── repositories
│   │   │   │       └── sample_repository_impl.dart
│   │   │   ├── domain
│   │   │   │   ├── models
│   │   │   │   │   └── sample_model.dart
│   │   │   │   ├── repositories
│   │   │   │   │   └── sample_repository.dart
│   │   │   │   ├── translater
│   │   │   │   │   └── sample_translater.dart
│   │   │   │   └── usecases
│   │   │   │       └── sample_usecase.dart
│   │   │   └── presentation
│   │   │       ├── screens
│   │   │       │   └── sample_screen.dart
│   │   │       ├── view_models
│   │   │       │   └── sample_view_model.dart
│   │   │       └── views
│   │   │           └── sample_view.dart
│   ├── main.dart
│   ├── services
│   │   ├── network_service
│   │   ├── firebase
│   │   └── native_api
│   │       ├── local_notifivation.dart
│   │       └── shared_preference.dart
│   └── utilities
│       ├── error_message_handler.dart
│       └── logger.dart
├── pubspec.lock
├── pubspec.yaml
└── test
    └── widget_test.dart
```

<br>

## 패키지

패키지를 pubspec.yaml에 추가할 때 ^를 사용한다.

패키지는 가능한 Flutter Favorite에 있는 것으로 사용한다.

아래 목록에서 없는 패키지는 추가하기 전에 Flutter 개발 관리자에게 승인을 받은 후 추가한다.

패키지 목록
  - logger
  - json_serializable
  - shared_preferences
  - provider
  - flutter_local_notifications
  - http
  - url_launcher
  - flutter_native_splash
  - path_provider
  - cached_network_image
  - go_router
  - video_player
  - flutter_inappwebview
  - Flutter Community Plus Plugins
    - battery_plus
    - connectivity_plus
    - device_info_plus
    - network_info_plus
    - package_info_plus
    - sensors_plus
    - share_plus
  - uuid
  - easy_localization
  - mockito
  - build_runner
  - image_picker
  - geolocator
  - flutter_lints
  - permission_handler
  - flutter_svg
  - font_awesome_flutter
  - flutter_slidable
  - tutorial_coach_mark
  - fluttertoast
  - animations


## etc
1. 패키지를 pubspec.yaml에 추가할 때 ^를 사용한다.
2. 네트워크 이미지를 사용할 땐 cached_network_image를 사용한다.
3. 이미지 네이밍에 특정 패턴을 적용하면 Flutter에서 해상도별 이미지를 만들어준다.
    - 참고: https://flutter.dev/docs/development/ui/assets-and-images#resolution-aware
4. json_serializable 관련 명령어
json serializable 코드가 담인 파일 생성
```
flutter pub run build_runner build
```

watcher를 추가하여 필요할 때마다 자동으로 파일 생성
```
flutter pub run build_runner watch
```

pub finished with exit code 78 에러 발생 시
```
flutter clean
flutter pub get
flutter packages pub run build_runner build --delete-conflicting-outputs
```
