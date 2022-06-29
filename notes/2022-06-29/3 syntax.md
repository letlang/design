# Syntax

## Bindings
~~~
// introducing a new symbol: binding

// abstract binding (?)
let Foo

// binding to a value
let Foo = "bar"

// typed abstract binding
let Foo :String

// typed binding to a value
let Foo :String = "Bar"

// deconstruction binding
let Foo = ("hello", "world")
(let Bar, let Baz) = Foo

// symbol usage
let A = 7
let B = -3
let C = A + B
~~~

## Functions

~~~
// no-operation function
fun Foo() = {}

// simply returning function
fun Foo() = "hello"

// typed simply returning function
fun Foo() :String = "hello"

// parameterized function
fun Foo(let Arg :String) = {}
~~~

## Function: Equals Sign?
~~~
fun Foo() {
  return "hello"
}

fun Foo() = {
  return "hello"
}

// having the equals sign means
// `{...}` is an expression which
// can also be used everywhere else
// -> scala's `return`-less syntax
// -> scoped return
~~~

## Scoped Return
~~~
fun Foo() {
  let x = someDSL {
    if Foo then
      yield'Foo "Hello"
    else
      yield'someDSL "Hi"
  }
  yield x
}
~~~

## Pattern Issues
~~~
Inbox.Receive() match {
  "ping" -> "pong"
  "pong" -> "ping"
  let m  -> "unknown message: ${m}"
}

let PingMsg = "ping"
let PongMsg = "pong"
let ElseMsg = "unknown message"
Inbox.Receive() match {
  PingMsg -> PongMsg
  PongMsg -> PingMsg
  _       -> ElseMsg
}

// binding to constant patterns!?
"Hello" = "Hello" // :Unit
"Hello" = "World" // COMPILE TIME ERROR

// higher patterns
ignorecase "hello" = "HeLlO" // :Unit
regex "H...(o|O)"  = "HeLlO" // :Unit

// type guard
let Foo = "Hello" :Any
Assert(StaticTypeOf(Foo) == TypeOf(Any))

Foo :Any = "World" // COMPILE TIME ERROR

// maybe patterns that do not bind to a new
// symbol should not be usable in assignments

let Foo = "Hello"
Inbox.Receive() match {
  Foo :String -> "hi"
  let Foo :String -> "${Foo}"
}
~~~

## Let VS With
~~~
// new syntax

let jane = object {
  with Firstname = "Jane"
  with Lastname  = "Doe"
}
let john = jane {
  with Firstname = "John"
}


// old syntax (refined)

let jane = object {
  let Firstname = "Jane"
  let Lastname  = "Doe"
  // let because it adds new symbols to
  // the context of the newly created object
}
let john = jane with {
  let Firstname = "John"
  // no let because the symbol already exists!?
  // -> named parameters
  // what about symbols that did not exist!?
}

let Person = record {
  let Firstname :String
  let Lastname  :String
  // how to distinguish between record
  // components and other (derived)
  // properties and functions

  let Fullname = "${Firstname} ${Lastname}"

  fun IsRelatedTo(let Other :Person) :Boolean =
    this.Lastname == Other.Lastname
}
~~~
