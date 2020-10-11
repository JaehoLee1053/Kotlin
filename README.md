# Kotlin
## INDEX
### [Scope Function](#scope-function)
> let()

> run()

> with()

> apply()

> also()


## Scope Function
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
            println("$it, "
        }
    }
```

> with()

> also()

> run()
