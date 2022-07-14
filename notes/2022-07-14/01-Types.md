# Let-Lang // Types

~~~
type Person = record {}

type Array1 = trait {
  covariant type T
  invariant let C :Nat32
}

let ints1 :Array1
  which C == 16
  and T == Nat32
let strs1 :Array1
  which C == 16
  and T == String

type Array2 = trait {
  self(C, T)
  covariant type T
  invariant let C :Nat32
}

let ints2 :Array2(16, Int32)
let strs2 :Array2(16, String)
~~~

## Sketches

~~~
type Arr = trait { self(C, T) ; covariant type T      ; invariant let C :Nat32 }
let  Arr = trait { self(C, T) ; covariant let T :Type ; invariant let C :Nat32 }

fun (type A)      Id(let Param :A)   :A          = Param
fun (let A :Type) Id(let Param :A)   :A          = Param
fun               Id(let Param :Any) :Param.type = Param

let Id :(type A)      -> (let Param :A)   -> A          = -> Param
let Id :(let A :Type) -> (let Param :A)   -> A          = -> Param
let Id :                 (let Param :Any) -> Param.type = -> Param

// alternative

fun <let A :Type> Id(let Param :A) :A = Param

let Id :<let A :Type>(let Param :A) -> A =           -> Param
let Id :<let A :Type>(:A)           -> A = let Param -> Param

fun (let A :Type) Id(:A) :A   ;  let Id :(let A :Type)(:A) -> A
fun <let A :Type> Id(:A) :A   ;  let Id :<let A :Type>(:A) -> A
fun [let A :Type] Id(:A) :A   ;  let Id :[let A :Type](:A) -> A

// () no way to distinguish generic
//    and normal parameters.
// <> hard to tokenize/parse.
//    see `<let F :() -> ()>`.
//    see `<let I :Int32 which > 2>`.
//    solution: parentheses?
//    see `<let F :(() -> ())>`
//    see `<let I :Int32 which (> 2)>`
// [] ambiguous
//    see `fun               [MySymbol]() :Unit`
//    see `fun [let A :Type] MyFunction() :Unit`
//    see `fun [let A :Type] [MySymbol]() :Unit`
//    solution: symbols are constants
//    generic parameters are patterns.

fun (let T :Type) [MySymbol]()
fun <let T :Type> [MySymbol]()
fun [let T :Type] [MySymbol]()

// alternative symbols
let MySymbol = Symbol()

fun  [MySymbol](let I :Int32)  ;  let MyVar. [get, set] :mutable Int32
fun $(MySymbol)(let I :Int32)  ;  let MyVar.$(get, set) :mutable Int32

// ==> constant patterns in parameters?
// ==> static dispatch?

fun Foo(0) :Int32
fun Foo(1) :Float64

Foo(0)  // <: Int32
Foo(1)  // <: Float32

let i :(0.type or 1.type)
// Foo(i)       // Error: Static dispatch.
Foo.dynamic(i)  // <: (Int32 or Float32)
~~~
