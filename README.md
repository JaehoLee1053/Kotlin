# Kotlin
## INDEX
### [Operator](#operator)
### [Scope Function](#scope-function)

> 이용 목적별 구분

> 5개 함수

>> let()

>> with()

>> run()

>> apply()

>> also()

> takeIf와 takeUnless

## Operator
> safeCall(?.)
> elvis operator(?:)

## Scope Function

> 이용 목적별 구분

>>> Non-null object에 lambda를 사용하고자 할 경우 => let (return lambda)

>>> Object를 변경하고자 할 경우 => apply (return context object)

>>> 추가 효과 => also (return context object)

>>> Obhect를 변경하고 결과값을 계산할 때 => run (return lambda)

>>> Expression이 필요한 여러 줄의 code를 처리하고자 할 때 => run (return lambda)

>>> Object에 함수 호출들을 grouping할 때 => with (return lambda)

||context object|return|
|---|---|---|
|run|this|lambda result|
|with|this|lambda result|
|apply|this|context object|
|let|it|lambda result|
> let()

>> A. chain operation의 경우

>>> Chain operation에서 하나 이상의 함수를 사용하기 위해 사용할 수 있다.

```kotlin
class Student(var name:String, var age:Int){
    fun incrementAge() = age++
}

Student("Alice", 20).let{
    print("${it.age}, ")
    it.incrementAge()
    println(it.age)
}
```

>> B. non-null value로 code block을 실행시키는 경우

>>> ?.let{} code block 안에는 non-null인 값만 들어올 수 있으므로 non-null 값이 필요한 경우 사용 가능하다.

>>> let()은 lambda 값을 return 한다.

>>> * list에는 filterNotNull() method가 존재하기 때문에 예시와 같은 경우는 거의 없다.

```kotlin
listOf("one", null, "two")
    .forEach{
        it?.let{
            println("$it, ")
        }
    }
```

> with()

>>> with()은 parameter를 object로 받아 사용

>> A. lambda 식의 결과값을 필요로 하지 않고, 함수 호출을 grouping 해서 사용

```kotlin
fun main(){
    val numbers = mutableListOf("one", "two", "three")
    with(numbers){
        println("'with' is called with argument $this")
        println("It contains $size elements")
    }
}
```

>> B. Object가 필요로 하는 값을 계산해 주는 경우

>>> this를 context object로 받는 경우는 생략이 가능하다.

```kotlin
val numbers = mutableListOf(1, 2, 3)
with(numbers){
    print(first() + last())
}
```

> run()

>>> with과 비슷하지만 object를 받는 위치가 다르다.
>>> 필요에 따라 앞에 safeCall(?.)을 붙여 null이 아닌 값만 run을 실행할 수도 있다. (이 이유로 with보다 run을 많이 사용)

>> A. 객체를 초기화하고 return 값의 계산이 필요한 경우

```kotlin
val service = MultiportService("https://example.kotlinlang.org", 80)
val result - service.run{
        port = 8080
        query(prepareRequest() + " to port $port"
    }
```

>> B. expression이 필요한 여려 줄의 code를 한 번에 처리할 때

```kotlin
val hexNumberRegex = run{
    val digits = "0-9"
    val hexDigits = "A-Fa-f"
    val sign = "+-"
    Regex("[$sign]?[$digits$hexDigits]+"
}
```

>> C. let과 run의 조합

>>> let, run, 그리고 elvis operator(?:)를 조합하여 null이 아닐 경우 let block을, null인 경우 run block이 실행되도록 사용하기도 한다.
>>> block의 계산값을 사용하지 않는다면, apply와 함께 사용하기도 한다.

```kotlin
var mNullable:String? = null
mNullable?.let{
    print(it)
}?:run{
    throw NullPointerException("mNullable was Null")
}
```

> apply()

>>> 전달받는 객체의 멤버에 operate하는 것이 주 목적이다.

```kotlin
class Student(var name: String, var age: Int)
val adam = Student("Adam", 30).apply{
    age = 15
    name = "Adam Etchins"
}
print(adam.name)
```

>>> Block에 받는 object가 member에 접근하는 경우 this는 생량 가능하지만, this가 사라지면 member 변수인지 외부 변수인지 구분하기 어렵기 때문에 객체 멤버를 수정하는 일에 적합하다.

>>> 아래와 같은 Builder 패턴에도 사용할 수 있다.

```kotlin
AlertDialog.Builder(this).apply{
    setMessage("테스트")
    setPositiveButton("확인") {_, _ ->
        Toast.makeText(this@MainActivity,
                "확인함:, Toast.LENGTH_SHORT).show()
    }
}.create().show()
```

> also()

>>> Object를 수정하지 않고, debugging을 위한 logging을 하는 등의 부가적인 일을 할 때 상요한다.

```kotlin
val numbers = mutableListOf("one", "two", "three")
    numbers
        .also{println("기존 리스트: $it"}
        .add("four")
```

```kotlin
Intent(this, MainActivity"::class.java)
    .also{startActivity(it)}
```

> takeIf와 takeUnless

>>> takeIf와 takeUnless의 context object와 return은 it이다.

>>> 조건을 달아서 filtering할 수 있고, 그 값을 받아 사용할 수 있다.

>>> 조건이 만족되지 않을 경우 null을 return한다. (safe call(?.)과 함께 사용 권장)

```kotlin
val number = Random.nextInt(100)

val evenOrNull = number.takeIf { it % 2 == 0 }
val oddOrNull = number.takeUnless { it % 2 == 0 }
println("even: $evenOrNull, odd: $oddOrNull")
```
