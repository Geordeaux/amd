---
id: code_conventions
title: Android Coding Conventions
hide_title: true
---

# Android Coding Conventions
Version: 0.9.1

[中文版][cn version]

## Contents
* [1 Android Studio Specification](#1-Android-Studio-Specification)
* [2 Kotlin Naming Conventions](#2-Kotlin-naming-conventions)
* [3 Kotlin code style conventions](#3-Kotlin-code-style-conventions)
* [4 Kotlin coding conventions](#4-Kotlin-coding-conventions)
* [5 Documentation conventions (KDoc)](#5-Documentation-conventions-kdoc)
* [6 Resource file conventions](#6-Resource-file-conventions)
* [7 Gradle conventions](#7-Gradle-conventions)
* [8 Unit Test conventions](#8-Unit-Test-conventions)
* [9 Additional conventions](#9-Additional-conventions)

## Preface
Why do I need a coding conventions?
- It helps, not hurts
  > No rules can make a circle. The same way a good code Conventions can reduce low-level errors, improve code quality, enhance code readability, improve project maintainability, and improve Code Review efficiency.

- Necessity
  > It is a necessity to do after the team grows, and it is also the industry practice;

 Based on the above, we need to define "conventions". Not all existing code follows these rules, but we expect all new code to follow them.

## 1 Android Studio Specification
> To do a good job, you must first use the right tools.

1. use the latest **stable version** of the IDE for development. Unify the encoding format to **UTF-8**.
2. Use IntelliJ IDEA default code style.
3. after editing .kt, .java, .xml and other code files must be
    - (shortcut option+cmd+L) format them (use AS default template).
    - (shortcut ctl+option+O) remove the redundant import.


## 2 Kotlin Naming Conventions

Correct English spelling and syntax makes it easy for the reader to understand and avoid ambiguity. As an international development team, naming in code is forbidden in Pinyin, and direct use of Chinese is not allowed. However, common proper names such as `shanghai` can be treated as English.

### 2.1 Package names

Package names are all lowercase letters, with consecutive words directly joined together (no underscores)
```kotlin
// Okay
package com.example.deepspace
// WRONG!
package com.example.deepSpace
// WRONG!
package com.example.deep_space
```

Use of inverse domain naming rules (e.g.: com.binance.spot)，the first-level package name is the top-level domain name, usually `com`, `edu`, `gov`, `net`, `org`, etc. The second-level package name is the company name, and the third-level package name is named according to `module`.

### 2.2 Class names

- Class names are written in `PascalCase` (`UpperCamelCase`), usually as a noun or noun phrase. e.g.:Character or ImmutableList。 Try to avoid abbreviations unless the abbreviation is well known, e.g., HTML, URL, and if the class name contains a word abbreviation, the first letter of the word abbreviation is capitalized.

- Interface names can also be nouns or noun phrases (e.g. List), but sometimes they can also be adjectives or adjective phrases (e.g. Readable).

- Test classes are named by starting with the name of the class being tested and ending with Test. For example: `SpotTradeCalculationTest`.


| Class                   | Description                           | Example                           |
| :---------------------- | :------------------------------------ | :-------------------------------- |
| `Activity`              | with `Activity` as a suffix           | `OrderHistoryActivity`            |
| `Fragment`              | with `Fragment` as a suffix           | `SpotTradeFragment`               |
| `Adapter`               | with `Adapter` as a suffix            | `OpenOrderListAdapter`            |
| `ViewHolder`            | with `ViewHolder` as a suffix         | `OpenOrderItemViewHolder`         |
| Parsing class           | with `Parser` as a suffix             | `RouterParser`                    |
| Tools Classes           | with `Utils` or `Manager` as a suffix | `ImageUtils`, `ThreadPoolManager` |
| Database Class          | with 以 `DBHelper` as a suffix        | `NewsDBHelper`                    |
| `Service`               | with `Service` as a suffix            | `TimeService`                     |
| `BroadcastReceiver`     | with `Receiver` as a suffix           | `JPushReceiver`                   |
| `ContentProvider`       | with `Provider` as a suffix           | `ShareProvider`                   |
| Customized base classes | with `Base` as prefix                 | `BaseActivity`, `BaseFragment`    |


### 2.3 Method names

Function names are written in `camelCase`, usually as a verb or verb phrase. For example, `sendMessage` or `stop`.

| Function                    | Description                                                                               |
| :-------------------------- | :---------------------------------------------------------------------------------------- |
| `initXX()`                  | Initialization-related methods, using `init` as a prefix identifier, such as `initView()` |
| `isXX()`, `checkXX()`       | If the method returns a boolean value, use the `is/check` prefix to identify it           |
| `getXX()`                   | A method that returns a certain value, using `get` as the prefix label                    |
| `setXX()`                   | Set a property value                                                                      |
| `handleXX()`, `processXX()` | Methods for processing data                                                               |
| `displayXX()`, `showXX()`   | Pop-up dialog and prompt message, using `display/show` as prefix identifier               |
| `updateXX()`                | Update data                                                                               |
| `saveXX()`, `insertXX()`    | Save or insert data                                                                       |
| `resetXX()`                 | reset data                                                                                |
| `clearXX()`                 | clear data                                                                                |
| `removeXX()`, `deleteXX()`  | Remove data or views, etc., e.g. `removeView()`                                           |
| `drawXX()`                  | Draw data or effect-related, using the `draw` prefix identifier                           |

**Exception1**: functions with the `@Composable` annotation that return `Unit` (composable functions) are in `Pascal` case and named as if they were some type. For details, see [Jetpack Compose Basics]
```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            Greeting("Android")
        }
    }
}

@Composable
fun Greeting(name: String) {
    Text (text = "Hello $name!")
}
```

**Exception2**: Allows underscores to appear in test function names to separate the logical components of the name.

### 2.4 Constant Names

The naming convention for constant names: `UPPER_SNAKE_CASE`, all letters capitalized, with underscores separating the words.
But what is a constant, exactly?

Constants are val properties with no custom get function, whose contents are deeply immutable, and whose functions have no detectable side-effects. This includes immutable types and immutable collections of immutable types as well as scalars and string if marked as const. If any of an instance’s observable state can change, it is not a constant. Merely intending to never mutate the object is not enough.

Constant values can only be defined inside of an object or as a top-level declaration. Values otherwise meeting the requirement of a constant but defined inside of a class must use a non-constant name.


```kotlin
const val NUMBER = 5
val NAMES = listOf("Alice", "Bob")
val AGES = mapOf("Alice" to 35, "Bob" to 32)
val COMMA_JOINER = Joiner.on(',') // Joiner is immutable
val EMPTY_ARRAY = arrayOf()
```

##### string constants

Commonly used prefixes for string constants.

| Uses               | Field name prefixes |
| ------------------ | ------------------- |
| SharedPreferences  | `SP_`               |
| Bundle             | `BUNDLE_`           |
| Fragment Arguments | `ARGUMENT_`         |
| Intent Extra       | `EXTRA_`            |
| Intent Action      | `ACTION_`           |
| route path         | `ROUTE_`            |

For example：

```kotlin
// Note: The value of the field is the same as the name to avoid duplication issues
const val PREF_EMAIL = "PREF_EMAIL"
const val BUNDLE_AGE = "BUNDLE_AGE"
const val ARGUMENT_USER_ID = "ARGUMENT_USER_ID"

// String constant associated with an intent use the full package name as a prefix for the value
const val EXTRA_SURNAME = "com.binance.extras.EXTRA_SURNAME"
const val ACTION_OPEN_USER = "com.binance.action.ACTION_OPEN_USER"

const val ROUTE_ACCOUNT = "/account"

```


### 2.5 Non-constant names
Non-constant names are written in `camelCase` form. These apply to instance variables, local variables and method parameters.
This is extended to the following style: the basic structure is `{Type0}VariableName{Type1}`、`variableName{Type1}`。

Note: the content in the `{}` is optional.
Example: `futureOrderType`

No special prefixes or suffixes (such as those in the `name_`, `mName`, `s_name`, and `sName` examples) are used, except for [Backing properties](#351-backing-properties).

##### Type0（the type of component）

To avoid confusion between UI components and common member variables and to better express the meaning, all member variables used to represent UI components are prefixed with the UI component abbreviation（see [List of UI component abbreviations](#list-of-ui-component-abbreviations)）。

Example: `ivAvatar`、`rvOrders`、`flContainer`。
> `ViewBinding` automatically generates variable names based on the view id, so see below for the specification here [id Naming Rules](#77-id-naming-rules)

##### VariableName

Quantifiers may appear in variable names, we need to create uniform quantifiers, they are easier to understand and more searchable.

Example：`firstOrder`、`preOrder`、`curOrder`。

| 量词列表 | desc           |
| -------- | -------------- |
| `first`  | first value    |
| `last`   | last value     |
| `next`   | next value     |
| `pre`    | previous value |
| `cur`    | current value  |


##### Type1（Data Type）

For non-constant field that represent collections or arrays, we can add suffixes to enhance the readability of the fields, e.g.

suffixes for Collection：List、Map、Set。

Suffixes for Array：Arr。

e.g.: `ivAvatarList`、`userArr`、`firstNameSet`。

> Note: If the data type is indeterminate, for example, if it represents many orders, then it is possible to use its plural form, for example `orders`.

#### 2.5.1 Backing properties
When a backing propertie is required, its name should exactly match the name of the actual propertie, except with an underscore prefix.

```kotlin
private var _table: Map? = null

val table: Map
    get() {
        if (_table == null) {
            _table = HashMap()
        }
        return _table ?: throw AssertionError()
    }
```

#### 2.5.2 Temporary Variables

Temporary variables are usually named `i`, `j`, `k`, `m`, and `n`, which are generally used for integers, and `c`, `d`, and `e`, which are generally used for character types. For example: `for (int i = 0; i < len; i++)`.


### 2.6 Type variable name

Type variables can be named in one of the following two styles.

1. a single uppercase letter, which can be followed by a number (e.g., `E`, `T`, `X`, `T2`).
2. a class naming style (refer to [2.2 Class names](#22-class-names)) followed by an uppercase T (e.g. `RequestT`, `FooBarT`).


### 2.7 Additional examples of CamelCase.
| Text                   | Correct           | incorrect         |
| :--------------------- | :---------------- | :---------------- |
| “XML Http Request”     | XmlHttpRequest    | XMLHTTPRequest    |
| “new customer ID”      | newCustomerId     | newCustomerID     |
| “inner stopwatch”      | innerStopwatch    | innerStopWatch    |
| “supports IPv6 on iOS” | supportsIpv6OnIos | supportsIPv6OnIOS |
| “YouTube importer”     | YouTubeImporter   | YoutubeImporter*  |

Note: In English, some words are not explicitly required to be hyphenated. For example, `nonempty` and `non-empty` are both correct, so the method names `checkNonempty` and `checkNonEmpty` are also both correct.

## 3 Kotlin code style conventions

> In Kotlin, semicolons are optional, so line breaks are important. The language is designed to use Java-style bracket formatting, and unexpected behavior may be encountered if you try to use a different formatting style.

### 3.1 The standard use of curly brackets
> The left curly brace does not occupy a separate line, but is on the same line as the code before it

#### 3.1.1 Non-empty blocks & Empty blocks
For non-empty blocks and empty blocks, curly braces follow the Kernighan and Ritchie (`K&R`) style ("Egyptian brackets").   (Corresponding to the `ms` style)

- Non-empty blocks
```kotlin
class MyClass {
    fun foo() {
        if (condition1) {
            // ...
        } else if (condition2) {
            // ...
        } else {
            // ...
        }
    }
}
```
- Empty blocks

```kotlin
try {
    doSomething()
} catch (e: Exception) {} // WRONG!

try {
    doSomething()
} catch (e: Exception) {
} // Okay
```

#### 3.1.2 Expressions
We need to add curly brackets around the conditional statement. Exception: if the whole conditional statement (condition and body) fits on the same line, then it is possible (but not necessary) to put it all on one line. Example.

```kotlin
val value = if (string.isEmpty()) 0 else 1  // Okay

val value = if (string.isEmpty()) { // Okay
    0
} else {
    1
}
```

```kotlin
val value = if (string.isEmpty())  // WRONG!
    0
else
    1
    
if (condition) { body() }  // WRONG!
```

#### 3.1.3 Indentation
The indentation is increased by **four spaces** at the start of each new block or block-like construction. When the block ends, the indentation reverts to the previous indentation level. Indentation levels apply to code and comments throughout the block.

### 3.2 Line break standard

Each statement is followed by a newline character. No semicolons are used.
The column limit for code is a maximum of 200 characters. If the line length exceeds 200 (the vertical line on the right hand side of the AS window is the end of the set line width), we usually have two ways to reduce the line length.
* Extracting a local variable or method (preferably).
* Use a line break to replace one line with multiple lines.

Exceptions.
* If a comment line contains a sample command or text URL that is longer than 200 characters, then the line can be longer than 200 characters for cut and paste purposes.
* package and import statements


##### Line break strategy

There is no exact solution to this to determine how to make a line break, and usually different solutions are valid, but there are some rules that can be applied to common situations.

- When a line breaks at an operator or infix function name, the newline character will follow the operator or infix function name.
- A line break at the following "operator-like" symbols will be preceded by a line break.
  - The dot separator (. , ?.).
  - Two colons (::) for member references.
- The method or constructor name is always appended to the left parenthesis (() after it.
- The comma (,) is always affixed to the marker before it.
- The lambda arrow (->) is always attached to the list of arguments before it.
```kotlin
appendCommaSeparated(properties) { prop ->
    val propertyValue = prop.get(obj)
}
```

Specifically.
#### 3.2.1 Line break for operators

In addition to the assignment operator, we place newlines before operators, e.g.

```java
val longName = anotherVeryLongVariable + anEvenLongerOne - thisRidiculousLongOne
        + theFinalOne
```

The line break for the assignment operator we put after it, e.g.

```java
val longName =
        anotherVeryLongVariable + anEvenLongerOne - thisRidiculousLongOne + theFinalOne
```


#### 3.2.2 Line break for Chain call

When making a line break to a chain call, the `.` character or the `?` operator on the next line, with single indentation.

For example:

```kotlin
Glide.with(context)
        .load("https://blankj.com/images/avatar.jpg")
        .into(ivAvatar)
```
The first call in the call chain usually precedes the newline, although this can be ignored if it makes more sense to the code.

#### 3.2.3 Line break for functions

When the function signature does not fit on a single line, each parameter declaration should be given a separate line. Arguments defined in this format should use a single indent (+4). Right round brackets () and the return type occupy a separate line with no additional indentation.
For example.

```kotlin
//function definition
fun computeMaxQty(
    isReduceOnly: Boolean,
    tradeSide: Int,
    orderType: FutureOrderType,
    markPrice: BigDecimal,
    futurePosition: FuturePosition?,
    ...
): BigDecimal {
...
}

//function call
computeMaxQty(
    isReduceOnly = isReduceOnly,
    tradeSide = currentTradeSide,
    orderType = stateHolder.currentOrderType,
    markPrice = stateHolder.markPrice,
    futurePosition = futurePosition, //trailing comma
    ...
)


val deepLinkMap = mapOf(
    DeepLinkConstants.ORDERS_FUTURES_HISTORY_PATH to ROUTE_FUTURE_HISTORY,
    DeepLinkConstants.MAIN_SIMPLE_HOME_PATH to ROUTE_MAIN,
    ROUTE_MARKET_DETAIL to ROUTE_MARKET_FUTURES_DETAIL, //trailing comma
)
```

- Line feeds for expression functions
> When a function contains only one expression, it can be represented as a function of expressions. For example

```kotlin
fun main() = runBlocking {
  // …
}
```
A line break should only be made when an expression function starts a block. Unless the function itself is very long, then a normal function should be used instead.

#### 3.2.4 Line beak for attributes
When an attribute initializer does not fit on a line, it should be followed by a line break and indentation after the equal sign (=).
```kotlin
private val defaultCharset: Charset? =
        EncodingRegistry.getInstance().getDefaultCharsetForPropertiesFiles(file)
```

Properties declaring get and/or set functions should have each function on a separate line and use the normal indentation (+4). When formatting them, the same rules are used as for functions.
```kotlin
var directory: File? = null
    set(value) {
        // …
    }
```

Read-only attributes can be used with a shorter syntax that fits on one line.
```kotlin
val defaultExtension: String get() = "kt"
```

#### 3.2.5 Line break for `enum`
Enumerations that have no functions and no documentation about their constants can optionally be formatted as a single line.
```kotlin
enum class Answer { YES, NO, MAYBE }
```

When placing constants in an enumeration on separate lines, no blank lines are required between them, except when they define the body.
```kotlin
enum class Answer {
    YES,
    NO,

    MAYBE {
        override fun toString() = ""
    }
}
```
As `enum` are `class`, all other rules used for class formatting apply.

#### 3.2.6 Line break for annotation
Annotations are usually placed on a separate line, before the declarations they depend on, and use the same indent.
```kotlin
@Retention(SOURCE)
@Target(FUNCTION, PROPERTY_SETTER, FIELD)
annotation class Global
```

Annotations without parameters can be placed on the same line.
```kotlin
@JvmField @Volatile
var disposable: Disposable? = null
```

A single annotation without parameters can be placed on the same line as the corresponding declaration.
```kotlin
@Volatile var disposable: Disposable? = null

@Test fun selectAll() {
    // …
}
```


### 3.3 Code organisation and sorting
#### 3.3.1 Sorting of modifiers
If a declaration has more than one modifier, always place them in the following order.
```
public / protected / private / internal
expect / actual
final / open / abstract / sealed / const
external
override
lateinit
tailrec
vararg
suspend
inner
enum / annotation / fun // is a modifier in the `fun interface`
companion
inline
infix
operator
data
```

#### 3.3.2 Sorting of class members

The source files are usually read in top-to-bottom order, which means that the order should normally reflect the fact that declarations that are more highly placed will help in understanding declarations that are less highly placed.
It is important that some sort of logical order is used for each class. For example, new functions should not be added directly to the end of a class by convention, as this would produce a "date added order" sort, which is not a logical order.
The following ordering is usually recommended.

- Property declaration and initialization blocks
- Sub-constructor
- method declarations
- Companion objects
- Nested classes

Don't sort method declarations alphabetically or by visibility, and don't separate regular methods from extension methods. Rather, put related things together so that people reading the classes from top to bottom will be able to follow the logic of what is happening. Choose an order (high level first, or vice versa) and stick to it.
Place nested classes immediately after the code that uses them. If the intention is to use nested classes externally and they are not referenced in the class, then put them at the end, after the companion object.
Example.

```kotlin
class MyFragment： Fragment {

    private var title: String
    private var tvTitle: TextView
    
    init {
    }
    
    constructor{}

    override fun onCreate() {
        setUpView() 
    }

    private void setUpView(){
        setTitle()
    }

    private fun setTitle(title： String) {
    	tvTitle.text = title;
    }
    
    companion object{
        private const val TAG = "MyFragment"
        fun newInstance(value1: String, value2: String): MyFragment {
            //...
        }
    }
    
    class InnerClass {

    }
}
```

If the class extends an Android component (e.g. `Activity` or `Fragment`), it is a very good convention to sort the overridden functions according to their lifecycle, e.g. `Activity` implements `onCreate()`, `onDestroy()`, `onPause()`, `onResume()`, which is correctly sorted as follows.

```kotlin
class MainActivity: Activity {
    override fun onCreate() {}

    override fun onResume() {}

    override fun onPause() {}

    override fun onDestroy() {}
}
```


#### 3.3.3 Sorting of function parameters

In Android development, `Context` is very common in function parameters and it is better to take `Context` as its first parameter.

Conversely, we should take the callback interface as its last parameter.

For example.

```kotlin
// Context always goes first
fun loadUser(context: Context, userId: Int): User

// Callbacks always go last
fun loadUserAsync(context: Context, userId: Int, callback: UserCallback)
```

## 4 Kotlin coding conventions
### 4.1 Package division
For the division of packages, `PBF` (Package By Feature) is recommended. Compared to `PBL` (Package By Layer) it has the following advantages.

* High cohesion within a package, low coupling between packages

    If you want to add a new feature, you can only change something under a certain package.

    `PBL` reduces code coupling, but creates package coupling. If you want to add a new feature, you need to change bean, viewmodel, activity, view, etc. You need to change the code under several packages, and the more you change, the more likely you are to create new problems;

    with `PBF`, everything related to featureA is in featureA package, which is highly cohesive and modular within feature, and low coupling between different features.

* package has a package-private scope

    The `PBF` package has a private scope, featureA should not access anything under featureB (if it has to, then there is a problem with the interface definition).

* Easy to delete features

    If the code associated with a feature needs to be removed

    PBL, you have to pull out all the code and classes that can be deleted from the feature entry to the entire business process that are implicated, which is error-prone.

    PBF, delete the corresponding package first, then delete the feature entry (the entry will definitely report an error after the package is deleted), and done.


* High level of abstraction

    The general approach to problem solving is to move from the abstract to the concrete. PBF package names are abstractions of functional modules, and the classes within the package are implementation details.

    PBF starts with determining the AppName, divides the package according to the functional module, and then considers the concrete implementation details of each piece, whereas PBL starts by considering whether to have a dao layer, a com layer, etc.

* Package size makes sense now

    It makes sense for the package size to grow infinitely in PBL, as more and more functionality is added, while a large package (too many classes in the package) in PBF means that the piece needs to be refactored (divided into sub-packages).

For more information on the benefits of `PBF`, see **[Package by features, not layers][Package by features, not layers]** and Google has the corresponding Sample:**[architecture-samples][architecture-samples]**


`PBF` can be done specifically like this.

```
com
└── binance
    └── dev
        ├── App.java
        ├── Config.java
        ├── base 
        ├── view
        ├── data 
        │   ├── DataManager.java
        │   ├── local (data from local, e.g.: SP，Database，File)
        │   ├── bean
        │   └── remote
        ├── feature
        │   ├── feature0
        │   │   ├── feature0Activity.java
        │   │   ├── feature0Fragment.java
        │   │   ├── xxAdapter.java
        │   │   ├── feature0ViewModel.java
        │   │   └── ... other class
        │   └── ...other feature
        ├── util
        └── widget
```

### 4.2 import statement
Two requirements
- Wildcard imports (of any type) are not allowed.
- The need to format the import after changes to the code file.


### 4.3 Constants
- for constants defined within a class, declare them in a companion object { } rather than in a member variable.
- For constants shared between multiple classes or modules, it is recommended that a new object declaration be created, with the object name serving as a grouping; rather than declaring them directly at the top of the file.
- Replace `magicNumber` with a constant.
    ```kotlin
    class FutureTradeFragment {
        companion object {
            private const val DEFAULT_LEVERAGE = 20
        }
        private var currentLeverage: Int = DEFAULT_LEVERAGE
    }
    ```


### 4.4 Immutability
- Prefer `val` over `var`;
- Always use immutable collection interfaces (`Collection, List, Set, Map`) to declare collections which are not mutated. When using factory functions to create collection instances, always use functions that return immutable collection types when possible:
```kotlin
// Bad: use of mutable collection type for value which will not be mutated
fun validateValue(actualValue: String, allowedValues: HashSet<String>) { ... }

// Good: immutable collection type used instead
fun validateValue(actualValue: String, allowedValues: Set<String>) { ... }

// Bad: arrayListOf() returns ArrayList<T>, which is a mutable collection type
val allowedValues = arrayListOf("a", "b", "c")

// Good: listOf() returns List<T>
val allowedValues = listOf("a", "b", "c")
```

### 4.5 Default parameter values
Prefer declaring functions with default parameter values to declaring overloaded functions.
```kotlin
// Bad
fun foo() = foo("a")
fun foo(a: String) { /*...*/ }

// Good
fun foo(a: String = "a") { /*...*/ }
```

### 4.6 Type aliases
If you have a functional type or a type with type parameters which is used multiple times in a codebase, prefer defining a type alias for it:

```kotlin
typealias viewClickListener = (View) -> Unit
```

### 4.7 Conditional statements﻿
Prefer using the expression form of `try`, `if`, and `when`.
```kotlin
return if (x) foo() else bar()

return when(x) {
    0 -> "zero"
    else -> "nonzero"
}
```

The above is preferable to:
```kotlin
if (x)
    return foo()
else
    return bar()
    
when(x) {
    0 -> return "zero"
    else -> return "nonzero"
}
```

### 4.8 if versus when
Prefer using `if` for binary conditions instead of `when`. For example, use this syntax with `if`:
```kotlin
if (x == null) ... else ...
```
instead of this one with `when`:
```kotlin
when (x) {
    null -> // ……
    else -> // ……
}
```
Prefer using when if there are three or more options.

### 4.9 Functions vs properties
In some cases functions with no arguments might be interchangeable with read-only properties. Although the semantics are similar, there are some stylistic conventions on when to prefer one to another.

Prefer a property over a function when the underlying algorithm:
- does not throw
- is cheap to calculate (or cached on the first run)
- returns the same result over invocations if the object state hasn't changed

### 4.10 Strings
Prefer string templates to string concatenation.

Prefer multiline strings to embedding `\n` escape sequences into regular string literals.

To maintain indentation in multiline strings, use `trimIndent` when the resulting string does not require any internal indentation, or `trimMargin` when internal indentation is required:
```
assertEquals(
    """
    Foo
    Bar
    """.trimIndent(), 
    value
)

val a = """if(a > 1) {
          |    return a
          |}""".trimMargin()
```


### 4.11 Extension functions

Use extension functions liberally. Every time you have a function that works primarily on an object, consider making it an extension function accepting that object as a receiver. To minimize API pollution, restrict the visibility of extension functions as much as it makes sense. As necessary, use local extension functions, member extension functions, or top-level extension functions with private visibility.


### 4.12 inline
When wrapping methods with lamda callback inputs, the `inline` keyword is preferred.

```kotlin
public inline fun <T> Iterable<T>.forEach(action: (T) -> Unit): Unit {
    for (element in this) action(element)
}
```

### 4.13 Data Class Specification
> `data class`, compared to normal `class` which automatically generates overridden code for `toString, equals, hascode, copy` methods, so if there is no need for this part you can use normal `class`

#### A clear distinction is made between `PO` and `Bean`
- The former, `PO`, is an interface-related data class, which is the data protocol for front-end and back-end communication and is relatively stable and immutable over time;
- The latter, `Bean`, is an interface-independent data class, mainly used for local logic, and can be a wrapper class based on `PO`, or defined independently;

#### Common rules of both
- adding a comma after each field, with line breaks based on the comma.
- implement `Parcelable` or `Serializable` according to actual needs.
- If the `Parcelable` interface is implemented, the fields that need to be serialized for transfer via Parcel must be declared in **constructor**, not in the class, (i.e., declare the fields in `()`, not in `{}`) otherwise the fields will be lost after serialization.

#### Special rules of `PO`
- default values need to be set for all fields of data types, with `""` suggested as the default for String and `null` for other referenced data types. (This issue is detailed in [here][Android避坑指南，Gson与Kotlin碰撞出一个不安全的操作].)
- All fields need to be annotated with `@Expose` and `@SerializedName("")`, the annotations are indented equally with the fields, one annotation takes up one line. If you don't want to do this, add the `@keep` annotation to the class.
- For the abbreviated field names defined in the api documentation (to reduce the size of the data), please return to the full field name when naming locally.

```kotlin
data class AccountWsPo(
        @Expose
        @SerializedName("e")
        var event: String = "",
        @Expose
        @SerializedName("E")
        var eventTime: String = "",
        @Expose
        @SerializedName("a")
        var account: AccountBean? = null,
)
```

> Normally, if you add the `@keep` annotation to a class or the `@SerializedName` annotation to all fields, it will serve the purpose of anti-obfuscation. However, to be on the safe side, it is best to add some more configuration to the `proguard-rules.pro` file. The proguard rules are as follows:
```
-keep class * implements java.io.Serializable { *; }
-keep class * implements android.os.Parcelable { *; }
-keep class com.binance.dev.bean.** { *; }
-keep class com.binance.data.beans.** { *; }
```

An example of `PO` & `bean` is as follows.

```kotlin
@Parcelize
data class OrderHistoryPo(
        
        @Expose
        @SerializedName("symbol")
        var symbol: String = "",
        @Expose
        @SerializedName("price")
        var price: String = "",
        ...

): Parcelable

// a wrapper of OrderHistoryPo
@Parcelize
data class OrderHistoryBean(

    val data: OrderHistoryPo,

    //Local field (fields not defined in the api)
    var exchangeInfo: Symbol? = null
): Parcelable {

    val quantityPrecision: Int
        get() = exchangeInfo?.quantityPrecision ?: 3

    val pricePrecision: Int
        get() = exchangeInfo?.pricePrecision ?: 2

    val isPostOnly: Boolean
        get() = "GTX".equals(timeInForce, true)
}

```

### 4.14 Use logs with care
While logging is very necessary, it has a significant negative impact on performance and loses its usefulness if it is not kept somewhat concise.
- Choose the right logging level;
- Frequency control: If some logging is likely to occur multiple times, it is best to implement a frequency limitation mechanism to prevent a large number of duplicate logs with the same (or very similar) information;
- Content Length Control: Keep log records to one line as long as possible if the content makes sense. A line length of 80 to 100 characters is perfectly acceptable, and lengths longer than 130 or 160 characters (including the length of tags) should be avoided whenever possible;
- Log security: Avoid disclosing security information in the logs. Personal information should be avoided, and information about protected content must be avoided;
- Do not use `System.out.println()`. `system.out` and `System.err` redirect to `/dev/null,` so your print statement will not have a visible effect. However, all strings compiled for these calls will still be executed.
    ```kotlin
    Blog.d(TAG, "Android coding conventions.")
    ```

### 4.15 Declare local variables first, member variables second
### 4.16 Prefer to use private for variables/constants/methods
### 4.17 try catch
- Don't use `try...catch...` in a loop, put it at the outermost level.


### 4.18 Complex class splitting and writing short function

Whenever feasible, try to write short and concise methods. We understand that there are situations where longer methods are appropriate, so there is no hard limit on method code length. If a method has more than 40 lines of code, consider whether it can be disassembled without breaking the program structure.

### 4.19 Class Casting
Use `a as? MyClass` instead of `a as MyClass?`

## 5 Documentation conventions (KDoc)

To reduce the pain value for others to read your code, please comment well in key places.
Caution.
- (a) Comments should not be enclosed in frames drawn by asterisks or other characters, and should not be fancy and idiosyncratic.
- Do not write code-independent comments.

### 5.1 Class comments
```java
/**
 * author : xxx
 * e-mail : xxx@binance.com
 * time   : 2021/04/01
 * desc   : A description of Class
 */
class MyClass {
}
```

You can formulate your own in AS，first enter `Settings -> Editor -> File and Code Templates -> Includes -> File Header`，then input

```java
/**
 * author : your name
 * e-mail : xxx@binance.com
 * time   : ${YEAR}/${MONTH}/${DAY}
 * desc   :
 */
```

### 5.2 Method comments*
#### 5.2.1 Method comments consist of the following three parts:
- **summary fragment**
Each KDoc block starts with a short summary fragment. This fragment is very important: it is the only part of text that will appear in certain contexts (e.g. class and method indexes).

    It is a fragment (noun phrase or verb phrase), not a complete sentence. It does not start with "A `Foo` is a..." or "This method returns..." nor does it have to form a complete imperative (e.g., "Save the record."). However, this fragment is capitalized and punctuated as if it were a complete sentence.
    
- **Blank lines**
Lines that contain only an aligned leading asterisk (*) appear between paragraphs and before groups of block tags (if present).

- **Block Markers**
Any standard "block tags" used appear in the order `@constructor`, `@receiver`, `@param`, `@property`, `@return`, `@throws`, and `@see`, and these block tags never appear with an empty description. When block tags do not fit on a line, consecutive lines are indented by 4 spaces from the @ position.


#### 5.2.2 Usage
Type `/** + Enter` in the line before the method or set `Fix doc comment` (Settings -> Keymap -> Fix doc comment) shortcut and AS will generate the template for you, we just need to complete the parameters as shown below.

```kotlin
/**
 * Convert a bitmap to a byte array
 *
 * @param bitmap
 * @param format
 * @return byte array
 */
fun bitmap2Bytes(bitmap: Bitmap, format: CompressFormat): ByteArray {
    ...
    return byteArray
}
```

At a minimum, a KDoc should exist for every public type and every public or protected member of such a type, with some exceptions listed below.
Exceptions: Self-explanatory functions
KDoc is optional for "self-explanatory" functions like getTitle and "self-explanatory" attributes like title.

Of course, not all getXXX-like methods can omit KDoc. For example, for functions named getCanonicalName or properties named canonicalName, do not omit their documentation (for the reason that it only says /* Returns the canonical name. */) ), because the general public may not know what the term "canonical name" means!


### 5.3 Block comments

Block comments are at the same indentation level as the code around them. It can be `/* ... */` style, or `// ... ` style (**`//` preferably followed by a space**). For multi-line `/* ... */` comments, subsequent lines must start with `*` and be aligned with the previous line's `*`. The following sample comments are possible.

```java
/*
 * This is
 * okay.
 */

// And so
// is this.

/* Or you can
* even do this. */
```

### 5.4 Some other comments

AS has integrated some comment templates for you, we just need to use them directly, type `todo`, `fixme`, etc. into the code, enter and the following comments will appear.

```java
// TODO: 21/4/01 Description of features that need to be implemented, but are not currently implemented
// FIXME: 21/4/01 Need to fix, even the code is wrong, does not work, need to fix the instructions
```


## 6 Resource file conventions

General naming rules for all resource files: **All lowercase, using underscore nomenclature (abc_def)**. Pinyin naming of resources is not allowed.

To prevent resource file naming conflicts between different modules, all internal resource files in the business `module` must start with the business name or abbreviation, except for the override.
For example, `spot_ic_more.png`, `finance_ic_more.png`.
Module name enumeration (part):

| module             | prefix  |
| :----------------- | :------ |
| lib-base           | base    |
| lib-finance-common | finance |
| spot-internal      | spot    |
| margin-internal    | margin  |
| trade-common       | trade   |
| futures-internal   | um      |
| delivery-internal  | cm      |
| options-internal   | options |
| futures-common     | futures |

### 6.1 Layout（layout/）

#### 6.1.1 ContentView Naming Rules

Rule：`module_activity_description.xml`

Rule：`module_fragment_description.xml`

The contentView of all Activity or Fragment must correspond to its class name. The corresponding rules are: turn all letters to lowercase and swap the type and function (i.e. suffix to prefix).

Example：`spot_activity_history.xml`

Example：`future_fragment_open_order.xml`

#### 6.1.2 Dialog Naming Rules

Rule：`module_dialog_description.xml`

Example：`spot_dialog_trade_confirm.xml`


#### 6.1.3 PopupWindow Naming Rules

Rule：`module_ppw_description.xml`

Example：`future_ppw_more_info.xml`


#### 6.1.4 List items Naming Rules

Rule：`module_item_description.xml`

Example：`spot_item_open_order.xml`


#### 6.1.5 Incomplete layout Naming Rules
> Layout files that are included by the `<include>` tag or that `ViewStub` inflates to another layout

Rule：`module_layout_description.xml`
Example： `finance_layout_trade_head_bar.xml`

#### 6.1.6 Custom ViewGroup with Xml layout

Rule：`module_widget_description.xml`
Example：`finance_widget_order_book.xml`

### 6.2 Image（drawable/ and mipmap/）

The `res/drawable/` directory contains bitmap files (`.webp, .png, .9.png, .jpg, .gif`) or `XML` files compiled as subtypes of `Drawable`; **drawable needs to be adapted considering theme (day and night), size, RTL**
The `res/mipmap/` directory contains different densities of launcher icons;

Naming Rule: all lowercase, using underscore nomenclature, using the following rules.

* `module name_usage_logic name or description`

Description：the usage also alleges the UI component.（see [List of UI component abbreviations](#List-of-UI-component-abbreviations) in the appendix）

For example.

| name                             |
| :------------------------------- |
| `module_btn_back.png`            |
| `module_divider_maket_white.png` |
| `module_ic_edit.png`             |
| `module_ic_head_small.png`       |
| `module_bg_input.png`            |
| `module_divider_white.png`       |
| `module_ic_more.png`             |
| `module_divider_list_line.png`   |

If there are multiple state, such as button selectors

| name                   |
| ---------------------- |
| module_btn_xx_selector |
| module_btn_xx_normal   |
| module_btn_xx_pressed  |
| module_btn_xx_focused  |
| module_btn_xx_disabled |


### 6.3 Animation（anim/ and animator/）

Mainly includes property animation and view animation, where view animation includes interval animation and frame animation. The property animation files need to be placed in the `res/animator/` directory, and the view animation files need to be placed in the `res/anim/` directory.

Naming Rules: `module name_logic name`。

Example: `uikit_pull_to_refresh_progress.xml`、`spot_price_auto_input.xml`。

If it is a normal interval animation or property animation, you can use the naming rule: `animation type_direction`.

For example.

| name                |
| ------------------- |
| `fade_in`           |
| `fade_out`          |
| `push_down_in`      |
| `push_down_out`     |
| `push_left`         |
| `slide_in_from_top` |
| `zoom_enter`        |
| `slide_in`          |
| `shrink_to_middle`  |


### 6.4 color（color/）

Naming Rules: `module name_type_logic name`。

Example: `spot_btn_trade_font_selector.xml`。

### 6.5 menu（menu/）

Naming Rules: `module name_logic name`

Example: `spot_select_symbol_drawer.xml`、`navigation.xml`。

### 6.6 values（values/）

The files in the `values/` directory are named with an `s` ending, such as `attrs.xml`, `colors.xml`, `dimens.xml`, and it's not the file name that works, but the various tags under the `<resources>` tag, such as `<style>` that determines the style, and `<color >` determines the color, so a large `xml` file can be split into multiple smaller files, for example, there can be multiple `style` files such as `styles.xml`, `styles_home.xml`, `styles_market_details.xml`.


##### 6.6.1 colors.xml

> Use the color value already defined in [UIkit](https://confluence.toolsfdg.net/display/Technology/Android-UIKit) consistently，If `UIkit` does not contain a color value, it is likely that the UI draft itself does not conform to the design specification and the UI draft needs to be adjusted. If the design specification is adjusted, you need to adjust `UIkit` first.

Use `lower_snake_case` for the `name` of `<color>`, and in your `colors.xml` file should just map the name of the color an ARGB value and nothing else. Do not use it to define ARGB values for different buttons.

For example, do not do something like the following.

```xml
  <resources>
      <color name="button_foreground">#FFFFFF</color>
      <color name="button_background">#2A91BD</color>
      <color name="comment_background_inactive">#5F5F5F</color>
      <color name="comment_background_active">#939393</color>
      <color name="comment_foreground">#FFFFFF</color>
      <color name="comment_foreground_important">#FF9D2F</color>
      ...
      <color name="comment_shadow">#323232</color>
```

Using this format tends to lead to duplicate ARGB values and can be very difficult if the application wants to change the base color. Also, these definitions are associated with some context, such as `button` or `comment`, and should be placed in a button style, not in a `colors.xml` file.

Instead, you should do like this.

```xml
  <resources>
      <!-- grayscale -->
      <color name="white"     >#FFFFFF</color>
      <color name="gray_light">#DBDBDB</color>
      <color name="gray"      >#939393</color>
      <color name="gray_dark" >#5F5F5F</color>
      <color name="black"     >#323232</color>

      <!-- basic colors -->
      <color name="green">#27D34D</color>
      <color name="blue">#2A91BD</color>
      <color name="orange">#FF9D2F</color>
      <color name="red">#FF432F</color>
  </resources>
```

The name does not need to be the same as `"green"`, `"blue"`, etc., names like `"brand_primary"`, `"brand_secondary"`, `"brand_negative"` are also perfectly acceptable. A standardized color like this is easy to modify or refactor and will make it very clear how many different colors the application uses in total. It is often important for an aesthetically valuable UI to reduce the variety of colors used.


##### 6.6.2 dimens.xml

Similar to `colors.xml`, you should also define a "palette" of gap spacing and font size. A good example is as follows.

```xml
<resources>
    <!-- font sizes -->
    <dimen name="font_22">22sp</dimen>
    <dimen name="font_18">18sp</dimen>
    <dimen name="font_15">15sp</dimen>
    <dimen name="font_12">12sp</dimen>

    <!-- typical spacing between two views -->
    <dimen name="spacing_40">40dp</dimen>
    <dimen name="spacing_24">24dp</dimen>
    <dimen name="spacing_14">14dp</dimen>
    <dimen name="spacing_10">10dp</dimen>
    <dimen name="spacing_4">4dp</dimen>

    <!-- typical sizes of views -->
    <dimen name="button_height_60">60dp</dimen>
    <dimen name="button_height_40">40dp</dimen>
    <dimen name="button_height_32">32dp</dimen>
</resources>
```

When writing `margins` and `paddings` for layout, you should use the `spacing_xx` size format for layout instead of writing the values directly as you do for `string` strings, so that the canonical sizes can be easily modified or refactored, which will make it easier to organize and change the style or layout at a later stage and reduce the cost of modification or refactoring.


##### 6.6.3 strings.xml

The `name` of `<string>` is named using underscore naming, using the following rule: `module_name_logic_name`, which makes it easy to put all `string`s in the same interface together for easy searching. For example.

```xml
<string name="futures_order_confirmation">Order Confirmation</string>
<string name="futures_order_confirmation_tips">Order confirmation will be required every time an order is submitted if this function is enabled.</string>
```

##### 6.6.4 styles.xml
> The style associated with the UI specification is also available in Uikit.

Naming rules: `ModuleName.LogicalName`, UpperCamelCase. 
It is common for a view to have duplicate UI details, and it is recommended to put duplicate UI detail properties (`colors`, `padding`, `font`) in the `styles.xml` file. For example.

```
<style name="Spot.ContentText">
    <item name="android:textSize">@dimen/font_normal</item>
    <item name="android:textColor">@color/basic_black</item>
</style>
```

##### 6.6.5 attrs.xml
Naming rules: keep the same name as the custom view

```xml
    <declare-styleable name="BaseOrderBookLayout">
        <attr name="orientation" format="enum">
            <enum name="portrait" value="0" />
            <enum name="landscape" value="1" />
        </attr>
        <attr name="layoutProvider" format="string" />
        <attr name="visibleCount" format="integer" />
    </declare-styleable>
```


### 6.7 id Naming Rules

Naming rules: lower_snake_case (lowercase letters as well as numbers, underscore connected)

Rule: `view Abbreviation {_module name}_logic name`;

Example: `btn_main_search`、`btn_back`。

> Please use `ViewBinding` instead of `kotlin Synthetic`, `ViewBinding` generates code, viewId will be automatically converted to `camelCase`.

see [List of UI component abbreviations](#List-of-UI-component-abbreviations)

## 7 Gradle conventions
### 7.1 Dependencies
- Each module should have dependencies on demand, and remove any unneeded dependencies in time.
- Select implementation, api, etc. dependencies according to actual requirements.

### 7.2 Tripartite libraries
For the selection of open source libraries, choose a relatively stable version of the project that is still being maintained; also consider the author's response to and resolution of issues, as well as the developer's popularity. After the selection, a certain amount of packaging is necessary.
For libraries that have been relied on in the project but are no longer maintained, alternatives need to be considered based on the actual situation.

### 7.3 Version Unification
`compileSdkVersion`, `minSdkVersion`, `targetSdkVersion` and versions of libraries that depend on two-party libraries and three-party libraries in the project need to be declared uniformly in `Versions.kt` for the versions.


## 8 Unit Test conventions
//todo

## 9 Additional conventions
### 9.1 Fragment
- Be careful about creating `Fragment` objects via constructors, especially when used with `viewPager`, and forbid pre-constructing lists of `fragment` instance objects outside of `adapter` to be passed to `adapter` for use. Counterexample:
```kotlin
class BaseFragmentPagerAdapter(fragmentManager: FragmentManager, private val fragments: List<Fragment>,mode : Int) : FragmentPagerAdapter(fragmentManager,mode) {

    override fun getItem(position: Int): Fragment {
        return fragments[position]
    }

    override fun getItemPosition(obj: Any): Int = PagerAdapter.POSITION_NONE

    override fun getCount(): Int = fragments.size
}
```
- `Fragment` uses `setArgument` to pass parameters, which is normally forbidden with constructor methods, unless customizing `FragmentFactory`.
- child fragment uses `parentFragment` to communicate with parent fragment, instead of setting `callback`. Decoupling through interface abstraction

```kotlin
//WRONG
class ParentAFragment {
    fun setUpView() {
        //...
        childFragment.callback = {
            showDialog()
        }
    }

    fun showDialog(){}
}

class ChildFragment {
    var callback: (()->Unit)? = null
    
    fun foo(){
        //...
        callback?.invoke()
    }
}
//**************************************************

//Okay
interface Callback {
    fun showDialog()
}

class ParentAFragment: Callback {
    fun setUpView() {
        //...
    }

    override fun showDialog(){}
}

class ChildFragment {
    
    fun foo(){
        (parentFragment as? Callback)?.showDialog()
    }
}

```

### 9.2 MVVM
- Unify the use of `mvvm` architecture, and replace the existing `mvp` architecture in the project in a planned manner; prohibit the direct writing of interface requests and other logic not related to the `View` layer in Activity/Fragment.
- The methods encapsulated in `ViewModel` are prohibited from declaring callbacks in the input parameters and use `liveData` to communicate with the `view` layer. Prohibit holding `view` layer objects; counterexample.
    ```kotlin
    fun refreshLeverage(callBack: LeverageDataListCallBack? = null) {
        accessRepositoryByTag {
            RepositoryCentral.futureCommonRepo
                    .userLeverage()
                    ?.subscribeOn(Schedulers.io())
                    ?.observeOn(AndroidSchedulers.mainThread())
                    ?.subscribeWith(object : HttpSubscriber<List<FutureUserLeverage>>() {

                        override fun success(result: List<FutureUserLeverage>?) {
                            ...
                            callBack?.onSuccess()
                        }

                        override fun error(e: Throwable) {
                            ...
                            callBack?.onError(e)
                        }
                    })
            }
    }
    ```
- The external exposure in `ViewModel` should be `LiveData`, not `MutableLiveData`. External to `LiveData` should only be readable and not writable; see [Backing properties](#351-backing-properties)
    ```kotlin
    class GameViewModel : ViewModel() {
        // The current score
        private val _score = MutableLiveData<Int>()
        val score: LiveData<Int>
            get() = _score

        fun getScore(){
            ...
            _score.value = 100
        }
    }
    ```
- `LiveData` prefers to use `setValue` to update the internal payload, rather than `postValue`;

- Rational layout, effective use of `<merge>`, `<ViewStub>`, `<include>` tags.
Custom `ViewGroup`, if you need to define layout by `xml`, `xml` outermost tag must use `<merge>`, avoid invalid nesting
    ```kotlin
    LayoutInflater.from(context).inflate(R.layout.layout_x_tab, this)
    ```
- `TextView` must have a fixed width if the `text` needs to be refreshed frequently.
- In layout xml, set `backgroud` of `view` carefully as needed to avoid overdrawing.
- in layout xml, use `Space` instead of `View` for occupancy to avoid overdrawing.

### 9.4 Business code decoupling
- For example, logic that can be implemented in `Fragment` by itself, not in `MainActivity`, let alone in other business modules.
- Use inheritance, combination, etc. to replace business judgments related to `if else`.
- when logic is found to be reusable, the first thought is to extract the wrapper, not the CV.
- existing code that is already CV and costly to maintain, needs to be refactored and integrated in a timely manner.

### 9.5 Calculation
> finance business with strict precision requirements.

- DTO classes uniformly use String type fields to receive price, amount, etc..
- Business-related (finance) calculations are encapsulated into a separate pure function, stripping the `View` layer logic.
- The parameters, intermediate values and return values for calculations must use `BigDecimal`.
- Do not format the intermediate values of the calculation, and keep the full precision as much as possible to avoid large deviations in the final calculation results.
- The writing of the calculation formula is preferred to use kotlin operator to improve the readability.
    ```kotlin
    //operator
    side * (markPrice - price)
    //function
    side.times(markPrice.minus(price))
    ```
- Use `BigDecimal(String)` construction instead of `BigDecimal(Double)`;
- other calculations may use `Double`.
    ```kotlin
    //For example, to calculate OrderBook progress, the ultimate goal is to calculate the width to be drawn, which does not require high precision.
    progressWith = (amount / totalAmount) * totalViewWidth
    ```


### 9.6 Using DataBlock in the right context
> DataBlock is equivalent to a singleton, and excessive and unreasonable use can add to the memory burden

- multiple pages or modules sharing the same data source.
- the need for data to be cached in memory.
- the need to subscribe to real-time updates.


### 9.7 Reasonable use of the `object` singleton
The `object` is the equivalent of a hungry singleton in java, and its improper or excessive use can cause memory problems;
For example, the following counterexample can cause memory leaks:
```kotlin
object FutureGridTradeHelper {

    private var guidePopup: BNCToolTipPopupWindow? = null

    fun showGridTradeGuide(context: Context, anchorView: View?) {
        ...
        if (null == guidePopup) {
            guidePopup = BNCToolTipPopupWindow(
                context,
                context.getString(R.string.future_grid_trading_tips),
                ...
            )
        }
        guidePopup?.showAsDropDownRTL(anchorView, xoff, 0)
    }
}
```

---

## Appendix

### List of UI component abbreviations

| View           | abbreviations |
| -------------- | ------------- |
| Button         | btn           |
| CheckBox       | cb            |
| EditText       | et            |
| FrameLayout    | fl            |
| GridView       | gv            |
| ImageButton    | ib            |
| ImageView      | iv            |
| LinearLayout   | ll            |
| ListView       | lv            |
| ProgressBar    | pb            |
| RadioButtion   | rb            |
| RecyclerView   | rv            |
| RelativeLayout | rl            |
| ScrollView     | sv            |
| SeekBar        | sb            |
| Spinner        | spn           |
| TextView       | tv            |
| ToggleButton   | tb            |
| VideoView      | vv            |
| WebView        | wv            |

### List of common English words abbreviations

| name                 | abbreviations                                                                                                                          |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| average              | avg                                                                                                                                    |
| quantity             | qty                                                                                                                                    |
| available balance    | avbl                                                                                                                                   |
| Profit and Loss      | pnl                                                                                                                                    |
| background           | bg (mainly for layout and sub-layout backgrounds)                                                                                      |
| buffer               | buf                                                                                                                                    |
| control              | ctrl                                                                                                                                   |
| current              | cur                                                                                                                                    |
| default              | def                                                                                                                                    |
| delete               | del                                                                                                                                    |
| document             | doc                                                                                                                                    |
| error                | err                                                                                                                                    |
| escape               | esc                                                                                                                                    |
| icon                 | ic(mainly used in the App's icon)                                                                                                      |
| increment            | inc                                                                                                                                    |
| information          | info                                                                                                                                   |
| initial              | init                                                                                                                                   |
| image                | img                                                                                                                                    |
| Internationalization | I18N                                                                                                                                   |
| length               | len                                                                                                                                    |
| library              | lib                                                                                                                                    |
| message              | msg                                                                                                                                    |
| password             | pwd                                                                                                                                    |
| position             | pos                                                                                                                                    |
| previous             | pre                                                                                                                                    |
| selector             | sel(mainly used for a view with multiple states, including not only the selector in the ListView, but also the selector of the button) |
| server               | srv                                                                                                                                    |
| string               | str                                                                                                                                    |
| temporary            | tmp                                                                                                                                    |
| window               | win                                                                                                                                    |

Principle of using word abbreviations in programs: Do not use an abbreviation unless that abbreviation is agreed upon.


## Reference
- [Kotlin Coding Conventions][kotlin Coding conventions]
- [Architecture-samples][architecture-samples]
- [面向贡献者的 AOSP 代码样式指南][面向贡献者的 AOSP 代码样式指南]
- [Google Java Style Guide][Google Java Style Guide]
- [Backing properties][Backing properties]
- [Android 包命名规范][Android 包命名规范]
- [Android 开发最佳实践][Android 开发最佳实践]
- [Android 编码规范][Android 编码规范]
- [阿里巴巴 Java 开发手册][阿里巴巴 Java 开发手册]
- [Project and code style guidelines][Project and code style guidelines]
- [Android避坑指南，Gson与Kotlin碰撞出一个不安全的操作][Android避坑指南，Gson与Kotlin碰撞出一个不安全的操作]


[cn version]: https://confluence.toolsfdg.net/display/Technology/Android+Coding+Conventions
[Package by features, not layers]: https://medium.com/@cesarmcferreira/package-by-features-not-layers-2d076df1964d#.mp782izhh
[architecture-samples]: https://github.com/android/architecture-samples
[Android 开发规范（完结版）]: https://github.com/Blankj/AndroidStandardDevelop
[Android 开发者工具]: http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2017/0526/7973.html
[可绘制对象资源类型]: https://developer.android.com/guide/topics/resources/drawable-resource.html
[Android 开发之版本统一规范]: https://blankj.com/2016/09/21/android-keep-version-unity/
[Tinker]: https://github.com/Tencent/tinker
[Android 包命名规范]: http://www.ayqy.net/blog/android%E5%8C%85%E5%91%BD%E5%90%8D%E8%A7%84%E8%8C%83/
[Android 开发最佳实践]: https://github.com/futurice/android-best-practices/blob/master/translations/Chinese/README.cn.md
[Android 编码规范]: http://www.jianshu.com/p/0a984f999592
[阿里巴巴 Java 开发手册]: https://github.com/alibaba/p3c/blob/master/Java%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C%EF%BC%88%E5%B5%A9%E5%B1%B1%E7%89%88%EF%BC%89.pdf
[Project and code style guidelines]: https://github.com/ribot/android-guidelines/blob/master/project_and_code_guidelines.md
[kotlin Coding conventions]: https://kotlinlang.org/docs/coding-conventions.html
[面向贡献者的 AOSP 代码样式指南]: https://source.android.com/source/code-style
[Google Java Style Guide]: https://google.github.io/styleguide/javaguide.html
[Kotlin样式指南]: https://developer.android.com/kotlin/style-guide
[Jetpack Compose Basics]: https://developer.android.com/jetpack/compose/tutorial
[Backing properties]: https://kotlinlang.org/docs/properties.html#backing-properties
[Android避坑指南，Gson与Kotlin碰撞出一个不安全的操作]: https://blog.csdn.net/lmj623565791/article/details/106631386
