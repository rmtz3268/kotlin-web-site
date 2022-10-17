[//]: # (title: Arrays)

Arrays in Kotlin are represented by the `Array` class. Arrays have a fixed size and their elements **must** all be of the same type. If you need to be able to change the size or have elements of different types, use [collections](collections-overview.md) instead.

## Create arrays

To create an array, use the function [`arrayOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/array-of.html) and pass the item values to it, so that `arrayOf(1, 2, 3)` creates an array `[1, 2, 3]`. For example:

```kotlin
fun main() {
//sampleStart
    val simpleArray = arrayOf(1, 2, 3)
    println(simpleArray.joinToString(" "))
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="simple-array-kotlin"}

> When creating arrays, you can use a [trailing comma](coding-conventions.md#trailing-commas).
>
{type="note"}

Alternatively, the [`arrayOfNulls()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/array-of-nulls.html#kotlin$arrayOfNulls(kotlin.Int)) function can be used to create an array of a given size filled with `null` elements.

```kotlin
fun main() {
//sampleStart
    val nullArray = arrayOfNulls(3)
    println(nullArray.joinToString(" "))
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="null-array-kotlin"}

Another option is to use the `Array` constructor that takes the array size and a function that returns values
for array elements given its index:

```kotlin
fun main() {
//sampleStart
    // Creates an Array<Int> with default values
    val defaultArray = Array<Int>(3)
    println(defaultArray.joinToString(" "))
    
    // Creates an Array<String> with values ["0", "1", "4", "9", "16"]
    val asc = Array(5) { i -> (i * i).toString() }
    asc.forEach { println(it) }
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3"}

> Indices start at 0 in Kotlin.
>
{type="note"}

You can assign an array to a variable. With this approach it may seem that you can dynamically change the size of the array but Kotlin is actually creating a new array each time. We recommend that you use [mutable collections](collections-overview.md) instead, which are designed to make it easy to add and remove elements.

```kotlin
fun main() {
//sampleStart
    var riversArray = arrayOf("Nile", "Amazon", "Yangtze")
    riversArray += "Mississippi"
    println(riversArray.joinToString(" "))
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="rivers-array-kotlin"}

### Empty arrays

To create an empty array, use [`emptyArray()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/empty-array.html):

```kotlin
var exampleArray = emptyArray<String>()
```

### Primitive type arrays

Kotlin also has classes that represent arrays of primitive types without boxing overhead:

| | | | |
|--|--|--|--|
|[`BooleanArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-boolean-array/)|[`ByteArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-byte-array/)|[`CharArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-char-array/)|[`DoubleArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-double-array/)|
|[`FloatArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-float-array/)|[`IntArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-int-array/)|[`LongArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-long-array/)|[`ShortArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-short-array/)|

These classes have no inheritance relation to the `Array` class, but they
have the same set of methods and properties. 

```kotlin
// Array of Int of size 5 with values [0, 0, 0, 0, 0]
val exampleArray = IntArray(5)
```

> To convert primitive arrays to typed arrays, use [`toTypedArray()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-typed-array.html).
>
{type="note"}

### Multidimensional arrays

Arrays can be nested within each other to create multidimensional arrays:

```kotlin
fun main() {
//sampleStart
    // Creates a two-dimensional array
    val twoDArray = Array(2) { IntArray(2) }
    println(twoDArray.contentDeepToString())

    // Creates a three-dimensional array
    val threeDArray = Array(3) { Array(3) { IntArray(3)} }
    println(threeDArray.contentDeepToString())
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="multidimensional-array-kotlin"}

> Nested arrays don't have to be the same type or the same size.
>
{type="note"}

## Access and modify elements

Square brackets `[]` are transformed into `get()` or `set()` function calls, like in Java. For example, `x[a,b]` instead of `x.get(a,b)`:

```kotlin
fun main() {
//sampleStart
    val simpleArray = arrayOf(1, 2, 3)
    simpleArray[0] = 10
    println(simpleArray[0].toString())
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="access-array-kotlin"}

Arrays in Kotlin are _invariant_. This means that Kotlin does not let us assign an `Array<String>`
to an `Array<Any>`, which prevents a possible runtime failure (but you can use `Array<out Any>`,
see [Type Projections](generics.md#type-projections)).

## Compare arrays

To compare whether two arrays have the same elements in the same order, use [`contentEquals()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/content-equals.html).

```kotlin
fun main() {
//sampleStart
    val simpleArray = arrayOf(1, 2, 3)
    val anotherArray = arrayOf(1, 2, 3)
    println(simpleArray.contentEquals(anotherArray))

    simpleArray[0] = 10
    println(simpleArray.contentEquals(anotherArray))
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="compare-array-kotlin"}

> Don't use equality (`==`) and inequality (`!=`) [operators](equality.md#structural-equality) to compare the contents of arrays. These operators
> check whether the assigned variables point to the same object.
> 
{type="warning"}

## Transform arrays

There are many useful functions that you can use to transform arrays. We've highlighted a few below but this isn't an exhaustive list. For the full list of functions, see our [API reference](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-array/).

### Sum()

To return the sum of all elements in an array, use the [sum()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sum.html) function.

```Kotlin
fun main() { 
// sampleStart
    val sumArray = arrayOf(1, 2, 3)

    println(sumArray.sum()) 
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="sum-array-kotlin"}

> The `sum()` function can only be used with arrays of numeric data types.
>
{type="note"}

### Shuffle()

To randomly shuffle the elements in an array, use the [shuffle()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/shuffle.html) function.

```Kotlin
fun main() {
// sampleStart
    val simpleArray = arrayOf(1, 2, 3)

    simpleArray.shuffle()
    println(simpleArray.joinToString())

    simpleArray.shuffle()
    println(simpleArray.joinToString())
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="shuffle-array-kotlin"}

### Zip()

To combine two arrays into an array of pairs, use the [zip()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/zip.html).

```Kotlin
fun main() { 
//sampleStart
    val firstArray = arrayOf(1, 2, 3)
    val secondArray = arrayOf(1, 2, 3)

    val zipArray = firstArray.zip(secondArray)

    println(zipArray.joinToString(" "))
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="zip-array-kotlin"}

To return a pair of lists from an array of pairs, use the [unzip()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/unzip.html) function.

```Kotlin
fun main() { 
//sampleStart
    val pairArray = arrayOf(Pair(1,1), Pair(2,2), Pair(3,3))

    val unzipArray = pairArray.unzip()
    println(unzipArray)
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="unzip-array-kotlin"}

## Concatenate arrays

Arrays of type `String` can be concatenated using the `+` operator.

```kotlin
fun main() {
//sampleStart
    val simpleArray = arrayOf("a", "b", "c")
    val anotherArray = arrayOf("d", "e", "f")

    println((simpleArray + anotherArray).joinToString())
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="concatenate-array-kotlin"}

## Pass array as vararg

To pass an array to the `vararg` parameter of a function, use the _spread_ operator (`*`).

```kotlin
val a = arrayOf(1, 2, 3)
val list = asList(-1, 0, *a, 4)
```

For more information, see [Variable number of args (varargs)](functions.md#variable-number-of-arguments-varargs).

## Convert to collection

To convert an array to a [collection](collections-overview.md) (`Set`, `List`, or `Map`), use the [`toSet()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-set.html), [`toList()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-list.html), [`toMap()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-map.html) functions respectively.

```kotlin
fun main() {
//sampleStart
    val simpleArray = arrayOf("a", "b", "c", "c")
    
    // Convert to Set
    println(simpleArray.toSet())
    
    // Convert to List
    println(simpleArray.toList())

    val pairArray = arrayOf(Pair(1,1), Pair(2,2), Pair(3,3))
    
    // Convert to Map
    println(pairArray.toMap())
    
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="convert-array-kotlin"}

## What's next?

* Learn about other [basic types](basic-types.md).
* Learn about [collections](collections-overview.md).