# Adaptive Design 이란? 
>다양한 화면 크기와 해상도에 대응할 수 있도록 디자인하는 방법이다. 플러터는 단일 코드로 안드로이드와 ios 앱을 동시 개발가능하므로 특히나 Adaptive Design 이 중요하다.

- Adaptive DeSign 은 디바이스의 화면 크기에 따라 레이아웃과 요소들이 유동적으로 변화한다.

---

# 화면의 사이즈를 읽는 방법
- MediaQuery
- MediaQueryData.fromWindow
- WidgetsBinding.instance.window.physicalSize

### 1. MediaQuery
```dart
MediaQuery.of(context).size.width
```

일반적으로 특정 위젯의 크기를 결정하기 위해 사용된다.

### 2. MediaQueryData.fromWindow
```dart
MediaQueryData.fromWindow(WidgetsBinding.instance.window).size.width
```

- 앱의 경우, 앱이 시작할 때 로딩화면에서 initialize작업을 하면서 해당 기기의 unitSize를 설정할 때 사용한다.

- 위젯트리와 상관없이 동작하기 때문에, 싱글톤처럼 어느 한 곳에 메모리로 잡아놓고 build 메서드 안에서 값을 할당하면, MediaQuery와 다르게 오동작을 막을 수 있다.

### 3. WidgetsBinding.instance.window.physicalSize
```dart
MediaQueryData.fromWindow(WidgetsBinding.instance.window).size.width;
```
현재 디바이스의 너비를 물리적 픽셀(Physical Pixel) 단위로 가져온다.
   

---

# SafeArea 
운영 체제 때문에 발생하는 침범을 피하기 위해 위젯의 자식을 충분한 `padding`을 준 뒤 끼워 넣는 위젯이다.

휴대폰 기기에 Notch 가 있다면 해당 부분이 가려지므로 `SafeArea Widget` 을 통해 해결할 수 있다.

-  자식이 스크린 상단의 상태바나 하단바 겹치지 않도록 충분한 공간을 준다
- Notch의 부분에 공간이 필요하지 않다면 방향에 false 입력! (기본값은 top/bottom/left/right 다 true)

---

# 플러터에서 기기별 화면 크기를 조절하는 방법

## 1. MediaQuery
- 화면 사이즈 측정
```dart
@override
Widget build(BuildContext context) {
  var screenSize = MediaQuery.of(context).size;

  return Scaffold(
    body: Center(
      child: Text('Screen width: ${screenSize.width} \nScreen height: ${screenSize.height}'),
    ),
  );
}
```
- 반응형 패딩
```dart
@override
Widget build(BuildContext context) {
  var screenWidth = MediaQuery.of(context).size.width;

  double padding = screenWidth > 600 ? 20.0 : 10.0;

  return Scaffold(
    body: Padding(
      padding: EdgeInsets.all(padding),
      child: Text('Responsive Padding'),
    ),
  );
}
```
- 방향별 레이아웃
```dart
@override
Widget build(BuildContext context) {
  var orientation = MediaQuery.of(context).orientation;

  return Scaffold(
    body: orientation == Orientation.portrait
        ? Text('Portrait mode')
        : Text('Landscape mode'),
  );
}
```
- SafeArea 계산
```dart
@override
Widget build(BuildContext context) {
  var padding = MediaQuery.of(context).padding;
  var safeHeight = MediaQuery.of(context).size.height - padding.top - padding.bottom;

  return Scaffold(
    body: Container(
      height: safeHeight,
      child: Text('This is a safe area'),
    ),
  );
}
```
- 동적 폰트 크기 조정
```dart
@override
Widget build(BuildContext context) {
  var screenWidth = MediaQuery.of(context).size.width;

  double fontSize = screenWidth > 600 ? 24.0 : 16.0;

  return Scaffold(
    body: Text(
      'Dynamic Font Size',
      style: TextStyle(fontSize: fontSize),
    ),
  );
}
```

### MediaQuery 이용해 화면 정보를 읽는 방법
```dart
MediaQuery.of(context).size             //앱 화면 크기 size  Ex) Size(360.0, 692.0)
MediaQuery.of(context).size.height      //앱 화면 높이 double Ex) 692.0 
MediaQuery.of(context).size.width       //앱 화면 넓이 double Ex) 360.0
MediaQuery.of(context).devicePixelRatio //화면 배율    double Ex) 4.0
MediaQuery.of(context).padding.top      //상단 상태 표시줄 높이 double Ex) 24.0

// 화면의 너비와 높이를 가져온 후 필요한 위젯에 해당 값 적용 가능
Container
  width: screenWidth * 0.5, 	// 화면의 50% 너비 사용
  height: screenHeight * 0.3, 	// 화면의 30% 높이 사용
  child: /* 위젯 구현 */,
)
```
- 실제 픽셀 값은 아래의 방식으로 구하기 가능
```dart
import 'dart:ui';

window.physicalSize                    //앱 화면 픽셀 크기 size Ex) Size(1440.0, 2768.0)
```

위와 같은 방법들 을 사용하면, 기기마다 다른 화면 크기에도 UI가 적절히 맞춰진다

---

## 2. flutter_screenutil 패키지 사용하기
`flutter_screenutil` 패키지 사용 시, 더 간단하게 기기별 화면 크기 조절이 가능하다. 

1. pubspec.yaml 파일에 의존성 추가
```
dependencies:
	flutter_screenutil: ^5.8.4
```

2. 패키지 import 후, 초기화하기
```dart
import 'package:flutter_screenutil/flutter_screenutil.dart';
 
@override
Widget build(BuildContext context) {
  // 화면 크기 초기화
  ScreenUtil.init(context, width: 360, height: 690, allowFontScaling: true); 
  // ... 나머지 코드
}
```
3. 화면 크기 계산 후, UI 적용하기
```dart
Container(
  // 화면의 50% 너비 사용
  width: 0.5.sw,
  // 화면의 30% 높이 사용
  height: 0.3.sh,
  child: /* 위젯 구현 */,
)
```

위의 방법대로 하면 `MediaQuery`를 사용한 방법보다 간결히 기기별로 화면 크기를 조절할 수 있다.

`ScreenUtilInit` 에 기본적으로 넣어야 하는 것은 2가지인데, 하나는 `designSize` 필드와 `builder` 이다. 
`designSize` 에는 Size(width, height) 로 기준이 될 사이즈가 넣어주고 `builder` 에는 위의 코드와 같이 `MaterialApp()` 을 반환할 수 있게 해주면 된다.

---

위의 두 가지 방법 중, 
안드로이드와 ios 에 두가지 다 적용가능하며 좀 더 간결하게 화면크기를 기기별로 조절가능한 `flutter_screenutil` 이용하려 한다.
