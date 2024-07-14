
-  **`soutv`** 
	- System._out_.println()
    
- * **`Ctrl + Alt+ V`** 
	- Member findMember = memberService.findMember(1L);
    
- **`alt + insert`**
    generate 만들어주기.
    getter와 setter도 같은 방법으로 만들어주기.
    
- **`ctrl+shift+T`**
    - test 생성 시 따로 test를 작성하지 않고
    - create new test!!
    
- **`Alt + Enter`**
    - static import (자바 기본문법임.)
    - Assertions._assertThat_( … → _assertThat_( … 으로 변환해줌.


- **`Ctrl+Alt+M`**
    - 이전
```java
public MemberService memberService(){    
    //Ctrl+Alt+M 눌러서 duplication을 잡아줌.
    return new MemberServiceImpl(new **MemoryMemberRepository**());
}
        
public OrderService orderService(){
    return new OrderServiceImpl(new MemoryMemberRepository(), new RateDiscountPolicy());
}
```
        
		- 이후
```java
public MemberService memberService(){
    //Ctrl+Alt+M 눌러서 duplication을 잡아줌.
    return new MemberServiceImpl(memberRepository());
}
        
private MemberRepository memberRepository() {
    return new MemoryMemberRepository();
}
        
public OrderService orderService(){
    return new OrderServiceImpl(memberRepository(), new RateDiscountPolicy());
}
```
        
    - duplication을 잡아줌.
        
- **`iter + Enter`**
    - 리스트나 배열이 있으면 for문이 자동 완성됨.


- **`Ctrl+D`**
    - 드래그한 부분 복사
    - **`ctrl+C`** + **`ctrl+V`**와 같은 효과임.


- **`Ctrl+E`**
    - 이전 코드로 갈 수 있음.


- **`Shift + F6`**
    - 파일 이름 바꾸기


- **`Ctrl+shift+Enter`**
	    ![[notion.png]]
    - 바로 아래로 내려감.


- **`Ctrl+Shift+T`**
    - class 명을 잡고 위의 **`Ctrl+Shift+T`** 클릭 시 쉽게 test 파일 만들기 가능.


  
---
  
| @eunsoA, 2024.07.02.