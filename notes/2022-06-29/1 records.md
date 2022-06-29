# Records

## Untyped Record Object
~~~
let jane = object {
  with Firstname = "Jane"
  with Lastname  = "Doe"
}
~~~

## Typed Record Object
~~~
let Person = record {
  with Firstname :String
  with Lastname  :String
  with Birthdate :Date? = None
}

let john = Person {
  with Firstname = "John"
  with Lastname  = "Doe"
}

let jane = object {
  with Firstname = "Jane"
  with Lastname  = "Doe"
} as Person
~~~

## Patching Records
~~~
let j1 = object { with Firstname = "Jane" }
let j2 = j1 { with Lastname = "Doe" }
let j3 = j2 { with Firstname = "John" }

WriteLine(j3.Firstname) //> John
WriteLine(j2.Firstname) //> Jane

//WriteLine(j1.Lastname) // ERROR
~~~

## Nested Record Selection
~~~
let j1 = object {
  with Firstname = "John"
  with Lastname  = "Doe"

  with Address = object {
    with Town   = "Foo"
    with Street = "Bar"
    with Number = 22
  }
}

let j2 = j1 {
  with Address.Town = "Baz"
}
WriteLine(j2.Firstname)
WriteLine(j2.Address.Town)

let j3 = j1.Address {
  with Town = "Baz"
}
WriteLine(j3.Town)
~~~

## Alt Syntax: Previous
~~~
let Person = record {
  let Firstname :String
  let Lastname  :String
}

let john = Person {
  Firstname = "John"
  Lastname  = "Doe"
}

let jane = john with {
  Firstname = "Jane"
}

// ISSUES WITH PREVIOUS SYNTAX:
// - symbols prefixed with `let` are
//   intoduced to the current context,
//   i.e. by a pattern
// - symbols without prefixe do already
//   exist in the current context

// ISSUES WITH THE CURRENT SYNTAX:
// - the `foo { with bar = ...` looks
//   like `foo` is a dsl-function
~~~

## Alt Syntax: Bracket Less
~~~
let Person = record
  with Firstname :String
  with Lastname  :String

let jane = Person
  with Firstname = "Jane"
  with Lastname  = "Doe"
~~~

## Alt Syntax: Python-ish
~~~
let Person = record:
  with Firstname :String
  with Lastname  :String

let jane = Person:
  with Firstname = "Jane"
  with Lastname  = "Doe"
~~~

## Alt Syntax: Simplified
~~~
let Person {
  with Firstname = "John"
  with Lastname  = "Doe"
  with Address {
    with Town   = "Foo"
    with Street = "Bar"
    with Number = 23
  }
}
~~~
