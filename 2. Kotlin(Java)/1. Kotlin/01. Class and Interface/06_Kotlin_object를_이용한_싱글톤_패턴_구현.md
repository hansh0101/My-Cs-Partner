# Kotlin object를 이용한 싱글톤 패턴 구현


> 개발을 하다보면, 객체에 대한 하나의 인스턴스만 필요할 때, 하나의 인스턴스를 재사용하기 위해 싱글톤 패턴을 구현해야 할 일이 생긴다.
> * 싱글톤 패턴 - 객체의 인스턴스를 1개만 생성하여 계속 재사용하는 패턴

### Java에서의 싱글톤

- 기존 자바에서는 싱글톤 패턴을 구현하기 위해 많은 코드를 작성해야 했다. 보통은 다음과 같은 방식으로 싱글톤 패턴을 구현하였다.
    
    ```java
    public class SingletonClass {
    	// 1. static으로 선언된 객체를 담는 변수
    	private static SingletonClass instance;
    
    	public String sampleString = "Sample String"; // 싱글톤에 집중하기 위해 public으로 설정
    	
    	private SingletonClass() {} // 생성자
    
    	// instance를 가져오는 메서드
    	public static synchronized SingletonClass getInstance() {
    		// 2. 만약 기존에 instance가 생성되어 있다면 기존 instance 사용, 만약 초기화되지 않았다면 새로 생성
    		if (instance == null) {
    			instance = new SingletonClass();
    		}
    
    		// 3. instance 반환
    		return instance;
    	}
    }
    ```
    
    1. 클래스 내에 클래스의 인스턴스를 담는 변수 instance를 선언한다.
    2. synchronized 함수로 SingletonClass.getInstance()가 호출된다.
        1. 만약 instance가 초기화되지 않았다면(null) instance를 생성한다.
    3. instance를 반환한다.
- **싱글톤 패턴을 구현하기 위해 너무 많은 코드(보일러 플레이트)가 쓰여졌다. 이러한 방식은 코드 가독성을 떨어뜨리며, 오류를 발생시킬 가능성을 높인다.**
- 이를 해결하기 위해 Kotlin에서는 object 키워드를 이용해 싱글톤 패턴을 간편하게 구현할 수 있도록 돕는다.

### object 키워드를 이용한 싱글톤 패턴

- Kotlin에서는 이를 단 하나의 키워드로 구현한다. 바로 object 키워드이다. 싱글톤으로 구현되어야 하는 클래스를 object 키워드를 사용함으로써 싱글톤으로 구현할 수 있다.
    
    ```kotlin
    object SingletonClass {
    }
    ```
    
- SingletonObject는 한 번만 생성되는 클래스의 인스턴스로 내부의 모든 값들 역시 한 번만 생성된다. 예를 들어, 다음과 같이 인스턴스를 object 내에서 생성했다고 해보자.
    
    ```kotlin
    object SingletonClass {
    	val sampleString = "Sample String"
    }
    ```
    
- 여기서 sampleString은 한 번만 생성된다. 따라서 SingletonClass.sampleString은 어디서나 같은 인스턴스가 된다.
    
    ```kotlin
    object SingletonClass {
    	val sampleString = "Sample String"
    }
    
    fun main() {
    	if (SingletonClass.sampleString == SingletonClass.sampleString) {
    		println("동등성 비교 true")
    	}
    	if (singletonClass.sampleString === SingletonClass.sampleString) {
    		println("동일성 비교 true")
    	}
    }
    
    /*
    출력
    동등성 비교 true
    동일성 비교 true
    */
    ```
    

### object 코드의 문제점: 프로세스 시작 시 인스턴스가 생성된다.

- 위에서 Java로 구현한 싱글톤과 Kotlin object로 구현한 싱글톤 코드는 조금 다른 점이 있다.
- 위의 Java 코드에서 SingletonClass는 getInstance()가 처음 호출될 때 초기화되어 메모리 상에 올라간다.
- 아래의 Kotlin 코드는 SingletonClass를 프로세스가 메모리 상에 올라갈 때 곧바로 생성되어 올라간다. 이는 클래스가 사용되지 않을 때도 메모리 상에 인스턴스가 올라가 있다는 것을 뜻한다.
- 즉, Java 코드는 호출될 때 인스턴스가 생성되는 반면 Kotlin에서는 프로세스가 시작될 때 인스턴스가 생성된다.

### 해결책: 내부 변수들의 초기화 시점을 by lazy를 활용해 조정한다.

- object 자체가 프로세스 실행 시 메모리에 곧바로 올라가는 것은 막지 못하지만, 내부 변수들을 by lazy를 이용해 생성함으로써 호출될 때 초기화될 수 있게 만들 수 있다.
- 아래와 같이 object 내부의 변수값을 by lazy를 통해 지연 생성(호출될 때 메모리에 올라가도록 하는 방법)을 하도록 만들면 된다.
    
    ```kotlin
    object SingletonClass {
    	val sampleString by lazy { "Sample String" }
    }
    ```
    
- 그렇게 한다면 sampleString은 호출될 때 처음 메모리 상에 올라가도록 할 수 있다.
- 이 방법을 통해 SingletonClass 내부에 많은 변수가 있을 때 메모리를 많이 잡아먹는 변수들을 지연 생성을 하도록 함으로써 메모리를 최적화할 수 있다.