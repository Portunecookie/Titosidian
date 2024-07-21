# setState() 을 사용한 상태 변경
`StatefulWidget`을 이용해 상태를 변경 할 때, `setState()`를 사용해야 한다.

즉, `setState()`는 `StatefulWidget`에서 특정 오브젝트의 값을 변경하기 위해 사용하는 메서드로, 	`setState()`안에서 상태를 변경하면 변경된 상태를 기반으로 `build()` 메소드를 실행시킨다. 

**단점** : widget에서 build가 호출될 때마다 위젯 전체를 다시 렌더링하기 때문에 성능저하가 일어날 수 있다. 또한, 하나의 페이지 안에서만 유효한 상태관리이다.

---
# Get X 란 ?
Flutter 개발을 위한 매우 가볍고 강력한 라이브러리

GetX를 이용한 상태관리 방식
1. 단순 상태 관리 : 기존 데이터의 변경여부와 관계없이 재렌더링 하는 것
2. 반응형 상태 관리 : 데이터 변화가 있을 때에만 재렌더링 하는 것

### 첫번째 : 단순 상태 관리
GetX를 사용해서 상태관리를 하기 위해서는 controller를 등록해서 사용해주어야 한다. Controller를 등록해줌으로써 하위의 모든 위젯은 controller에서 변경되는 데이터들을 실시간 반영 가능해지게 된다. 

### 두번째 : 반응형 상태 관리
반응형 상태 관리를 위한 Controller를 만들어준다. 단순 상태관리와 달리 변수의 타입을 RxInt, RxString 등 Rx{타입}의 방식으로 선언하고 변수의 값은 '.obs'를 붙인다. 
(만약 .obs를 사용한다면, 사용방식은 거의 동일하지만 GetX()와 달리 controller의 이름을 지정할 수 없다)

*- 업데이트를 해도 `update()`를 따로 호출할 필요가 없다*

- 반응형 변수의 선언 : 위젯에 변경사항이 있을 때만 재렌더링 해주는 것
	- 변수의 맨 뒤에 .obs 붙이기
    

### `BuildContext context` 의 무쓸모
- Get X 를 사용하면 context 없이 controller에 접근가능하다. (반면, provider은 context를 넘겨줘야만 한다)
- Get X 를 사용하면 위젯이 중첩되더라도 오로지 변경된 위젯만 다시 build가 된다
ex_) 한 Obx가 Listview를 보고 있고, 또다른 Obx는 그 Listview 안의 Checkbox를 보고 있다면, Checkbox 값이 변경된다 하더라도, Listview는 가만히 있고, Checkbox만 다시 빌드되는 것

### Get X 의 장점
1. 변경사항이 있는 위젯만 렌더링하는 것
2. 모든 위젯을 statelessWidget으로 만들고, 하나의 위젯만 실시간 업데이트가 필요하면, GetBuilder로 해당 위젯을 감싸 해당상태를 가져올 수 있다
	(statefulWidget이 필요없다)
3. Controller로부터 직접적인 이벤트 호출이 가능하다

### Route 를 사용하며 예전에 사용한 controller 데이터가 필요한 경우

GetBuilder 사용하거나 Get.find 이용해 데이터 불러오기


