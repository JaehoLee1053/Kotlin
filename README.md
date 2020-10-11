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
'''
val numbers = mutableListOf("one", "two", "three", "four", "five")
numbers.map {it.length}.filter{it > 3}
    .let{print(it}
'''


>> B. non-null value로 code block을 실행시키는 경우

> run()

> with()

> also()
