# Kotlin data class

### 개요

- Kotlin에서는 모든 클래스는 Any 클래스를 상속받는다.
    - Any 클래스에는 equals, hashCode, toString 메서드가 정의되어 있고, Kotlin의 클래스는 그렇기 때문에 위 메서드들을 재정의해 사용해야 한다.
- equals, hashCode, toString 메서드들을 재정의하는 것은 무척이나 귀찮은 작업이고, IDE에서 제공하는 자동 생성을 사용하더라도 보일러 플레이트 코드가 만들어져서 코드를 깔끔하지 못하게 만든다.
- 이를 위해 Kotlin에서는 equals, hashCode, toString을 자동으로 생성해주는 클래스인 data class를 제공한다.

### data class란?

- data class란 컴파일러에서 컴파일 시에 클래스에 공통적으로 필요한 메서드(equals, hashCode, toString)들을 재정의해주고 유틸성 메서드(copy)를 만들어주는 클래스를 뜻한다.

### equals()와 hashCode() 재정의를 통한 값 구분

- 이는 값 객체에 매우 유용하게 사용할 수 있다. 예를 들어 먼저 동등성 연산에 대한 것을 보자.
    
    ```kotlin
    class GalaxyTab(val modelName: String, val size: Int)
    
    val tab1 = GalaxyTab("S7", 11)
    val tab2 = GalaxyTab("S7", 11)
    
    println(tab1 == tab2)
    ```
    
- 위 코드에서 tab1과 tab2는 같은 프로퍼티를 갖는 동등한 인스턴스임에도 동등성 연산에서 false가 나왔다. 이 이유는 **equals 메서드가 재정의되지 않았기 때문이다.**
- **equals 메서드를 재정의하는 대신 단순히 클래스를 data class로 바꾸면 equals가 컴파일 시에 재정의되기 때문에 해결이 가능하다.**
    
    ```kotlin
    data class GalaxyTab(val modelName: String, val size: Int)
    
    val tab1 = GalaxyTab("S7", 11)
    val tab2 = GalaxyTab("S7", 11)
    
    println(tab1 == tab2) // true
    ```
    
- equals 동등성 연산에 대해 정의를 잘 했더라도, hash를 이용한 자료구조인 hashMap, hashSet에서는 문제가 생긴다.
- 이들 객체에서는 객체의 동등성 연산을 수행하기 전에 먼저 hashCode에 대한 비교 연산을 수행한다.
- hashCode 값이 달라지면 equals가 같더라도 다른 객체로 인식한다.
    
    ```kotlin
    class GalaxyTab(val modelName: String, val size: Int) {
    	override fun equals(other: Any?): Boolean {
    		return if (other is GalaxyTab) {
    			this.modelName == other.modelName && this.size == other.size
    		} else {
    			false
    		}
    	}
    }
    
    val tab1 = GalaxyTab("S7", 11)
    val tab2 = GalaxyTab("S7", 11)
    
    val mutableMap = mutableMapOf<GalaxyTab, Int>()
    mutableMap[tab1] = 10
    mutableMap[tab2] = 20
    
    println(mutableMap[tab1]) // 10
    println(mutableMap[tab2]) // 20
    ```
    
- 같은 key 값을 참조하는데 다른 값이 나왔다. 이는 hashCode()를 먼저 비교하고 hashCode가 달라 동등성 연산(equals)가 수행되지 않아 그렇다.
    
    ```kotlin
    println(tab1.hashCode()) // 1151020327
    println(tab2.hashCode()) // 2080166188
    ```
    
- 이를 해결하기 위해서는 **동등한 객체에 대해서는 hashCode를 같게 받을 수 있도록 hashCode()를 재정의해야 한다. 이 또한 data class로 바꾸면 hashCode가 재정의되기 때문에 간단히 해결 가능하다.**
    
    ```kotlin
    data class GalaxyTab(val modelName: String, val size: Int)
    
    val tab1 = GalaxyTab("S7", 11)
    val tab2 = GalaxyTab("S7", 11)
    
    val mutableMap = mutableMapOf<GalaxyTab, Int>()
    mutableMap[tab1] = 10
    mutableMap[tab2] = 20
    
    println(mutableMap[tab1]) // 20
    println(mutableMap[tab2]) // 20
    println(tab1.hashCode()) // 81479
    println(tab2.hashCode()) // 81479
    ```
    

### toString() 재정의를 통한 내부 프로퍼티 출력

- toString()은 객체의 상태를 문자열로 출력해주는 메서드이다.
- print 같은 문자열이 필요한 메서드에 객체가 들어가게 되면 toString()이 자동으로 수행되어 모든 객체에 필수적인 메서드이다. 하지만 재정의하지 않으면 의미 없는 값이 나오게 된다.
    
    ```kotlin
    class GalaxyTab(val modelName: String, val size: Int)
    
    val tab = GalaxyTab("S7", 11)
    println(tab.toString()) // GalaxyTab@610455d6
    ```
    
- data class로 정의하는 것만으로 내부 모든 프로퍼티를 포함한 toString()을 반환받을 수 있다.
    
    ```kotlin
    data class GalaxyTab(val modelName: String, val size: Int)
    
    val tab = GalaxyTab("S7", 11)
    println(tab.toString()) // GalaxyTab(modelName=S7, size=11)
    ```
    

### copy()

- data class에서는 copy()를 제공한다. 내부 프로퍼티 일부를 바꿀 수 있는 연산 또한 같이 제공한다.
- 아래와 같이 tabS7에서 이름만 S6로 바꾸어 tabS6 객체를 만들어낼 수 있다.
    
    ```kotlin
    data class GalaxyTab(val modelName: String, val size: Int)
    
    val tabS7 = GalaxyTab("S7", 11)
    val tabS6 = tabS7.copy(modelName = "S6")
    
    println(tabS7) // GalaxyTab(modelName=S7, size=11)
    println(tabS6) // GalaxyTab(modelName=S6, size=11)
    ```
    
    이 때 copy()는 shallow copy이다. shallow copy(얕은 복사)란 내부 프로퍼티에 대한 참조값을 유지하면서 copy를 진행하는 것을 뜻한다.