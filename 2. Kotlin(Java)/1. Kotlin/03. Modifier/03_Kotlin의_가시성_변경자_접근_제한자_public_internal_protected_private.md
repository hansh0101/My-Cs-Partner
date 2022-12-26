# Kotlin의 가시성 변경자(접근 제한자) - public, internal, protected, private

### 가시성 변경자

- 가시성 변경자는 클래스에 대한 외부 접근 권한을 제어한다. Kotlin에서는 4가지 가시성 변경자를 제공한다.
    - public
    - internal
    - protected
    - private
- Kotlin에서는 최상위 선언에서의 가시성 변경자와 클래스 멤버 선언에서의 가시성 변경자의 접근성이 달라진다. 먼저 공통적으로 적용되는 가시성 변경자인 public에 대해 알아보자.

### public

- public은 기본 가시성 변경자로, 어디에서나 접근 가능한 가시성 변경자이다. Kotlin에서는 아무것도 쓰지 않을 때 public이 기본 가시성 변경자로 설정된다. 이는 가시성 변경자를 쓰지 않을 때 default라는 별도의 가시성 변경자가 설정되는 Java와의 차이점을 가진다.
    
    ```kotlin
    public class Library {
    	public val testA = "A"
    }
    ```
    
- 위와 같이 코드를 작성하면, public 가시성 변경자는 회색으로 처리되어 생략 가능함을 나타내게 된다. 따라서 위 코드는 아래 코드와 같이 public 가시성 변경자를 생략하고 나타낼 수 있다.
    
    ```kotlin
    class Libaray {
    	val testA = "A"
    }
    ```
    
- 즉, 아무것도 쓰지 않을 때 기본 가시성 변경자는 public이 되며, public은 모든 곳에서 접근 가능하다.

### internal, protected, private

- 나머지 가시성 변경자는 최상위 선언과 클래스 멤버에 대해 적용되었을 때 어떻게 가시성이 변경되는지 각각 살펴본다.
- [최상위 선언에서 가시성 변경자](04_Kotlin_최상위_선언에서_가시성_변경자_public_internal_private.md)
- [클래스 멤버의 가시성 변경자](05_Kotlin_Class_Member_가시성_변경자_public_internal_protected_private의_차이.md)