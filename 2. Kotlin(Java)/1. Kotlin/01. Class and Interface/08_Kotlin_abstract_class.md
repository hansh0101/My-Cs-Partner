# Kotlin abstract class

### abstract class란 무엇인가?

- **abstract class란 그 자체로 인스턴스화될 수 없는 클래스**이다.
- **abstract class는 abstract class를 상속받는 여러 클래스에서 공통으로 쓰는 프로퍼티와 메서드를 모아놓는 용도로 쓴다.**
- 즉, abstract class란 여러 클래스의 추상적인 부분(공통적인 부분)을 모아놓은 클래스인데 그 자체로는 인스턴스화 할 수 없는 클래스이다.
    - 프로그래밍 언어들에서 abstract class를 abstract type을 가진다고 하고 abstract type은 그 자체로 인스턴스화를 할 수 없는 타입이다. 반대의 단어로는 concrete type이 있는데 concrete type은 그 자체로 인스턴스화할 수 있는 타입을 말한다.

### Kotlin의 abstract class의 특징

- abstract class는 여러 클래스의 공통적인 부분을 모아놓은 아직 미완성된 클래스이다.
- 그 클래스 내부의 일부 변수나 함수도 미완성됐을 수 있다. 따라서 인스턴스화가 불가능하다.
- 미완성된 변수나 함수에는 abstract modifier가 앞쪽에 붙는다. abstract로 만들어진 것들은 미완성된 것들이므로 인터페이스처럼 무조건 abstract class를 상속받는 곳에서 구현해야 한다.

### abstract class 만들면서 살펴보기

- abstract class를 직접 생성해보면서 abstract class가 어떻게 동작하는지 살펴보자. Rectangle 클래스를 abstract class로 만들 것이다.
- Rectangle abstract class는 정해진 4개의 꼭짓점(vertex)을 가지고 있고 정해지지 않은 width(너비)와 height(높이)를 가지고 있다. 너비와 높이를 곱한 값이 사이즈로 정해지며, Rectangle의 이름은 정사각형인지 직사각형인지 정해지지 않았다. 이를 코드로 작성하면 다음과 같다.
    
    ```kotlin
    abstract class RectangleAbstract() {
    	val numVertex = 4 // Rectangle의 꼭짓점 개수는 변하지 않는다. 확정됨.
    	abstract val width: Float // width는 Rectangle마다 변화할 수 있다. 미정됨.
    	abstract val height: Float // height 또한 Rectangle마다 변화할 수 있다. 미정됨.
    
    	fun getSize(): Float = width * height // 사이즈는 width와 height의 곱이다. 확정됨.
    	abstract fun getRectangleTypeName() // Rectangle은 정사각형일 수도 직사각형일 수도 있다. 미정됨.
    }
    ```
    
- 위에서 볼 수 있듯이 **abstract class는 내부 변수나 함수 또한 정해지지 않으면 abstract로 만들 수 있다. abstract로 만들어진 것들은 미완성된 것들이므로 인터페이스처럼 무조건 abstract class를 상속받는 곳에서 구현해야 한다.**
    - 이 방법보다는 interface를 따로 쓰는 방법이 권장되긴 한다.
- 또한 abstract class는 그 자체로 미완성됐기 때문에 인스턴스화가 불가능하다. abstract class RectangleAbstract를 인스턴스화하려고 하면 `Cannot create an instance of an abstract class`라는 경고 메시지가 뜨는 것을 확인할 수 있다.

### abstract class의 구현

- 위 abstract class를 상속해 정사각형 클래스를 구현한다고 해보자.
- 구현을 하면서 알아두어야 할 것은 3가지이다.
    - abstract 프로퍼티들은 부모에서 무조건 구현되어야 한다. 구현을 하는 방법은 생성자에 넣는 방법과 custom get을 통해 구현하는 방법이다.
    - abstract fun은 override되어 구현되어야 한다.
    - 나머지 abstract가 붙지 않은 것들은 구현이 필수적이지 않다.
- 이 규칙에 따라 구현한 Square은 다음과 같다.
    
    ```kotlin
    class Square(override val width: Float) : RectangleAbstract() {
    	override val height: Float
    		get() = width
    	
    	override fun getRectangleTypeName(): String {
    		return "Square"
    	}
    }
    ```
    
- Square은 width와 height가 같다. 따라서 width만 인자로 받고 height는 width의 값을 가져오도록 custom get을 사용하는 방법을 취했다.
- abstract fun getRectangleTypeName()을 구현해 “Square”라는 이름을 반환하도록 했다.
    - 물론 open과 같은 modifier가 붙었을 때 override를 할 수 있다.
    

### abstract class의 한계점

- abstract class는 절대로 바뀌지 않는 개념에 대한 공통 부분을 만들 때는 유용한 도구이다.
- 하지만 바뀌지 않는 도구는 거의 없고, 특히 클라이언트 개발 시에는 모든 것이 바뀔 수 있어 사용할 일이 거의 없다.
- 변화에 대한 유연성을 가져가려면 abstract fun보다는 interface를 쓰는 것이 합리적이다.
- abstract fun처럼 내부에 구현체(정해진 변수나 함수)가 있으면 구현체에 의존적이게 되어 변화에 대한 유연성이 감소되기 때문이다.