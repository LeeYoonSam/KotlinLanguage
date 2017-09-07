# Kotlin Language

[Kotlin lang Hompage](https://kotlinlang.org)

[Kotlin lang test](https://try.kotlinlang.org/#/Examples/Hello,%20world!/Simplest%20version/Simplest%20version.kt)

[Kotlin lang document](https://kotlinlang.org/docs/reference/)

[Kotlin coding-conventions](https://kotlinlang.org/docs/reference/coding-conventions.html)

[참고 사이트](http://thdev.tech/kotlin/2016/08/02/Basic-Kotlin-01.html)


## 코틀린 기본 문법 - 변수

* 코틀린은 ;을 사용하지 않고, 아래와 같이 변수 타입이 뒤에 붙는 형태
* 변수 타입을 지정하지 않을 수 있다.

```
// java
int temp = 10;

// Kotlin
val temp: Int = 10
var temp = 15
```

### val, var
> val : 변할 수 없는 상수 Java : final, C/C++ 등에서는 const

> var : 일반적인 변수에 해당

**val**
* val은 read-only이면서 로컬 변수
* java에서는 final, c/c++에서는 const에 해당
* 초기화 이후 변할 수 없는 값

```
val a: Int = 1
val b = 1  // Int를 추론할 수 있다.
val c: Int  // Int를 초기화해주어야 하는데 생성자에서 초기화해야 한다.
c = 100 // 생성자 시점에서 초기화해주지 않으면 문법상 오류가 발생한다.
```

**var**
* var은 일반적인 변수에 해당

```
var x = 5 // Int를 추론할 수 있음
x += 10

// 추론은 가능하지만 실제 값이 Int가 아니더라도 오류가 발생하지는 않음
var x: Int = 1 // Int를 고정
x = 15
```

* 두가지 모두 기본적으로 타입을 지정해도 되고 안해도 된다.
* 지정하지 않아도 추론을 통해서 Int 인지 String인지 확인이 가능하지만 실수를 대비해서 지정하는게 좋다.

## 코틀린 기본 문법 - 함수
### 자바 기본 함수 생성

```
public int sum(int a, int b) {
  return a + b;
}
```

> 리턴타입 함수명(변수타입 변수명) { return 값; }

### 코틀린 기본 함수 생성

```
fun sum(a: Int, b: Int): Int {
	return a + b
}
```

> fun 함수명(변수명: 변수타입): 리턴타입 { return 값 }

### 코틀린으로 조금 간단하게 만들기
* 한줄로 표현 가능
* 변수 타입 명시하거나 하지 않을 수도 있다.
* =을 이용하여 단순히 이 함수가 리턴을 의미하도록 함축적으로 표현가능

```
// Type을 지정해서 return을 정의
fun sum(a: Int, b: Int): Int = a + b

// 바로 return도 가능
fun sum(a: Int, b: Int) = a + b

// 조건식 추가
fun max(a: Int, b: Int): Int {
	if(a > b) return a
	else return b
}

// 한줄로 변환
fun max(a: Int, b: Int) = if(a > b) a else b
```

## 코틀린 기본 문법 - null
* 코틀린의 기본 변수는 null을 가질 수 없다.
* ?(물음표)를 추가해야 null 명시 가능
* ?(물음표)와 !!(느낌표 2개)를 사용
	* nullable: ?
	* nullable 이면 오류 발생 : !!

```
var a: Int = 15
a = null // 문법상 오류 발생

var b: Int? = null
b = null // 정상 수행
```

### 함수에서 null 정의
* 아래 함수는 ABC객체의 a를 return
* 이때 a는 Int이면서 null이 될 수 있도록 return type에 ?(물음표)를 추가

```
// ABC 객체의 .a를 return 하지만 abc가 null이면 null을 리턴한다.
fun abc(abc: ABC?): Int? {
	return abc?.a
}
```


## 코틀린 기본 문법 - Any

### Any를 사용해서 함수 구현
* type을 Any라는 키워드로 지정가능

```
fun getStringLength(obj: Any): Int? {
	if(obj is String) {
		// 'obj' is automatically cast to 'String' int this branch
	    return obj.length
	}
	// 'obj' is still of type 'Any'
  	// Type이 String이 아니면 null을 리턴
	return null;
}
```
> Any는 Java의 Object에 해당하며 is는 instanceof와 같은 의미다.

### '!'를 사용해서 부정하기

```
// Java
int getStringLength(Object obj) {
  if (!(obj instanceof String)) return 0;
  return obj.length;
}

// Kotlin
fun getStringLength(obj: Any): Int? {
  if (obj !is String) return null
  return obj.length
}
```

## 코틀린 기본 문법 - loop
### 코틀린 기본 loop 구현

```
// Java
ArrayList<String> arrayList = new ArrayList<>();
for (String s : arrayList) {
    Log.d("TAG", "string : " + s);
}

// Kotlin
val arrayList = ArrayList<String>()
for (s in arrayList) {
    Log.d("TAG", "string : " + s)
}
```

### Using Collections

```
fun main(args: Array<String>) {
    val fruits = listOf("banana", "avocado", "apple", "kiwi")
    fruits
    .filter { it.startsWith("a") }
    .sortedBy { it }
    .map { it.toUpperCase() }
    .forEach { println(it) }
}
```

**chain 방식으로 객체를 리리턴한다.**

* 결과

> APPLE

> AVOCADO



## 코틀린 기본 문법 - when
### 코틀린 기본 when 구현
* when은 if문을 중첩하여 사용하지 않고 간단하게 Any와 함께 구현

```
fun main(args: Array<String>) {
	cases("Hello")	// String
	cases(1) 		// Int
	cases(System.currentTimeMillis()) // Long
	cases(MyClass())// Not a String
	cases("hello")	// Unknown
}

fun cases(obj: Any) {
	when(obj) {
		1 -> println("One")
		"Hello" -> println("Greeting")
		is Long -> println("Long")
		!is String -> println("Not a string")
		else -> println("Unknown")
	}
}
```

* 결과값

> Greeting

> One

> Long

> Not a string

> Unknown


* when 활용

    * when {} 안에서는 제일 처음 만나는 부분을 표시하고 종료한다.
    
```
fun main(args: Array<String>) {
    val items = setOf("apple", "banana", "kiwi")
    when {
        "orange" in items -> println("juicy")
        "apple" in items -> println("apple is fine too")
    }
}
```

* 결과값

> apple is fine too


## 코틀린 기본 문법 - ranges
* 숫자의 범위를 지정하여 사용하는 방식

### 코틀린 기본 ranges 구현

```
// java
for (int i = 1; i <= 5; i++) {
  Log.i("TAG", "index : " + i);
}

// Kotlin
for (x in 1..5) {
	println(x)
}
```

### ranges 형태를 if 문에서 사용

```
val array = ArrayList<String>
array.add("aaa")
array.add("bbb")

val x = 3

// x <= array.size -1 과 같음
if (x !in 0..array.size -1)
	println("Out: array 사이즈는 ${array.size} 요청한 x = ${x}")

//위의 출력 값은
// Out: array 사이즈는 2 요청한 x = 3
```

## 마무리
* 코틀린은 안전한 null을 처리할 수 있도록 지원하고 있다.
* 협업 하다보면 각자 다른 스타일로 코딩을 하게 될 가능성이 크므로 Kotlin 공식문서에 나와있는 코딩규칙을 기본으로 사용하는것이 좋을것 같다.

### 느낀점
1. java보다 간단하게 사용할 수 있고 유용한 함수들이 많은것 같다.
2. ${array.size}, ${x} 이런 코드는 React를 할때 Text 작성할때 써봤는데 아주 유용했다.
3. 코드에서 ;(세미콜론)이 빠지므로 실수할 여지가 많을것 같고 java와 Kotlin 같이 사용한다면 두가지의 혼선이 예상되므로 신경써서 사용 해야할것 같다.
4. 지속해서 스터디후 실제 프로젝트로 사용시 소스코드가 많이 줄어들것으로 예상되며, 코드 복잡도도 줄어들어 속도면에서도 향상될것 같다.
