# flutter_screenutil
1. 초기화 `ScreenUtilInit`

   가장 먼저 `MaterialApp` 즉, 최상단 앱 부분을 `ScreenUtilnit`으로 감싸준다.
   기본적으로 `designSize`에는 기준이 될 사이즈 `Size(width, height)` 로 기준이 될 사이즈를 넣어주고, `builder`에는 위의 코드아 같이 `MaterailApp()` 을 반환할 수 있게 해주는 것이다.

2. .w, .h, .r, .sp
   이후 사용할 때에는 뒤에 속성들을 붙여서 사용한다
   - .w : 너비를 기준으로 배율을 적용한다.
   - .h : 높이를 기준으로 배율을 적용한다.
   - .r : 너비와 높이 중 작은 값으로 배율을 적용한다.
   - .sp : 화면 사이즈를 기준으로 font-size에 배율을 적용한다.
  
```dart
Container(
  width: 50.w,
  height:200.h
)

//If you want to display a square based on width:
Container(
  width: 300.w,
  height: 300.w,
),

//If you want to display a square based on height:
Container(
  width: 300.h,
  height: 300.h,
),

//If you want to display a square based on minimum(height, width):
Container(
  width: 300.r,
  height: 300.r,
),
```

### 단점
- 가로나 세로에 고정된 값을 사용해야 한다. 
- 즉, 화면 회전을 막거나 세로를 기준으로 UI를 구성해야 한다.
- 각 위젯의 크기를 개별적으로 조정하므로 코드가 길어질 수 있다.

### 장점
위젯 크기 조정이 단순해 성능에 큰 영향을 미치지 않으며 각 위젯의 크기를 개별적으로 조정하므로 효율적일 수 있다.
