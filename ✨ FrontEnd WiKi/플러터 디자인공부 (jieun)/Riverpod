# 상태관리란 ?
상태란, 위젯이 활성화되고 데이터를 메모리에 저장한 것이다. 즉 , 변경되는 데이터를 관리하는 것을 상태관리라고 한다

Flutter 같은 선언형 framework는 UI를 변경하기 위해 위젯을 다시 build 해야 한다. 
1. `createState()` 를 호출해 현재 상태를 별도의 State 클래스에 저장한다
2. 마냑 상태를 수정해야 할 경우에는 `setState()` 를 호출하면 `build()` 가 호출되며 수정된 상태로 UI가 다시 그려진다.
	(`dispose()` 호출 시 위젯트리에서 제거할 수 있음)

- 하지만, `setState()` 를 호출해 상태관리를 하려 하면 해당 위젯과 하위 위젯의 `build()` 가 전부 호출되어 UI 가 렌더링 되기 때문에 불필요한 렌더링이 발생해 성능 저하의 문제가 발생함

### 모든 위젯이 데이터를 공유하기 위한 방법으로는 GetX, BLoC, Provider, Riverpod 등 대표적인 상태관리들이 있다



---

# Riverpod
- Provider의 단점을 개선한 라이브러리
- Flutter에 의존적이지 않는다
- 컴파일 타임 동안 안전하다

### riverpod 사용하기
1. Provider가 동작할 수 있도록 하기 위해서 앱의 가장 최상위 부모 위젯을 ProviderScope로 감싸준다. 
```dart
void main() {
	runApp (const ProviderScope(child: MyApp()));
}
```
- provider는 riverpod 에서 상태를 캡슐화하는 객체이자 상태의 변화를 감시하는 중요한 역할을 한다
	(즉, riverpod는 provider를 용도에 따라 세분화한 상태관리 라이브러리)

### 주로 많이 쓰이는 Provider
- `Provider` : 가장 기본이 되는 Provider로 단순히 값만 읽을 수 있음
- `StateProvider` : StateNotifierProvider 보다 단순한 상태를 관리한다
- `FutureProvider` : 일반적인 Provider와 역할은 같지만 미래에 데이터를 제공해주기 때문에 비동기 처리가 가능한 Provider
- `StreamProvider` : FutureProvider와 비슷하지만 Stream 처리에 유용하다
- `ChangeNotifierProvider` : Provider 라이브러리의 ChangeNotifierProvider를 사용하는 경우 riverpod 에서도 동일하게 사용가능
- `StateNotifierProvider` : `StateNotifier` 는 상태를 가지고 있ㄷ으며 상태가 변경되면 리스너들에게 알려준다

### - ConsumerWidget
ConsumerWidget을 상속하면 build함수의 파라미터로 받아 WidgetRef를 사용할 수 있다
```dart
class MyApp extends ConsumerWidget {
	const MyApp ({Key? key}): super(key:key) ;
    
    @override
    Widget build (BuildContext context, WidgetRef ref) {}
}
```
- 상태를 구독하기 위해 `ConsumerWidget` 을 상속한 클래스를 만든다
- `ConsumerWidget` 는 `Provider` 객체의 상태를 구독하고, 상태변경이 있을 때마다 `build` 를 호출해서 화면을 다시 그린다

## - WidgetRef
```dart
Widget build (BuildContext context, WidgetRef ref)
```
- `WidgetRef` 를 통해 하위 위젯에서 Provider 객체를 생성하거나 액세스 할 수 있다
- `WidgetRef`가 `Provider` 객체에 액세스하여 상태변경을 감지하면 현재 위젯에 다시 빌드를 요청한다
- `context` 같은 역할을 해서 다른 위젯에서도 상태를 공유하는 역할을 한다

**Provider 와 상호작용하기 위한 ref의 주요 사용 방법 **
1. ref.watch : Provider를 구독해 변화를 모니터링 하고, 값이 변경되면 위젯을 다시 빌드하거나 구독 하고 있는 위치에 상태를 전달한다 (상태 변화를 화면에 즉각 반영해야 할 때 사용)
```dart
// Provider를 초기에 한 번만 읽음 - 값이 변경되더라도 build를 요청하지 않음
final String value = ref.read(helloWorldProvider);
```

2. ref.read : Provider의 상태를 가져오고 값을 변경할 수 있다. 
```dart
//Provider를 반복해서 관찰하고 상태가 변하면 build를 호출함 !!!!
final String value = ref.watch(helloWorldProvider);
```

3. ref.listen : watch와 동일하게 변화를 모니터링 하지만, 위젯을 rebuild하거나 상태를 전달하지 않고 값ㄷ이 변경될 때 정의한 함수를 실행하는 것 
	ex_ ) SnackBar나 Dialog 를 처리할 때 유용하다
```dart
void _makeSnackBar(){
  ref.listen(
    keywordProvider,
    ((previousState, newState) {
      showSnackBar('new value is: $newState');
    }),
  );
}
```

    
위 코드를 통해 상태에 변화가 생기면, `build` 가 호출되며 화면의 데이터가 변하게 된다

`Provider` 는 다른 `Provider` 를 포함할 수 있다.
```dart
final byeWorldProvider = Provider<String>((ref) => 'bye');

final helloWorldProvider = Provider<String>((ref) {
  final String value = ref.read(byeWorldProvider); // 상태 조합이 가능하다
  return 'Hello world: '+value;
});
```
