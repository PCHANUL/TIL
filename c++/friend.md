# friend 키워드

`friend`키워드는 접근 지정자를 무시하는 예외 기능을 가진 키워드이다. 이는 객체 지향 개념의 정보 은닉 개념에 정면으로 위배된다.  

- friend로 선언된 대상에게 `private` 또는 `protected`가 `public`으로 작용한다. 
- friend 지정은 단방향이며 명시적으로 지정한 대상에게만 작용된다.
- friend 관계는 상속되지 않는다.

friend 키워드는 3가지 경우에 사용된다.

1. friend 클래스 : 특정 클래스가 다른 클래스를 friend로 선언
2. friend 멤버 함수 : 클래스의 특정 멤버 함수를 friend로 선언
3. friend 전역 함수 : 접근 지정자를 무시하고 클래스 내부의 멤버에 접근
   
[friend 키워드](https://genesis8.tistory.com/98)  
