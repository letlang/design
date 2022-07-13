# Let-Lang // Types

~~~
alias Num = Float32

type Point = record (with X :Num, with Y :Num)

type Shape = trait {
  let Area :Num
}

type Rectangle = record {
  with Min :Point
  with Max :Poiny

  extends Shape

  let Size :Point = (Max.X - Min.X, Max.Y - Min.Y)

  override let Area :Num = Size.X * Size.Y
}

let a1 :Rectangle = object {
  with Min = (0, 0)
  with Max = (4, 3)
}

// dualism? `object {...}` vs. `record {...}`?

let a2 :Rectangle = a1 patch {
  with Min.Y = 10
  with Max.Y = 13
}

// patch operator? `a1 { with ... }`

mutable a3 :Rectangle = a2
set a3.Min.X = 20
set a3.Max.X = 24

fun Foo(let A :Point, let B :Point, let Name :String? = None) :Num {
  if (Name !== None) {
    WriteLine("Computing ${Name}'s magnitude.")
  }
  let X = A.X * B.X
  let Y = A.Y * B.Y
  return SquareRoot(X * X, Y * Y)
}

Foo(a3.Min, a3.Max, with Name = "A-3")
~~~

## Interesting Thoughts

~~~
type Point = record { with X :Num, with Y :Num }

// `record` instead of `object`?
let zero :Point = record { with X = 0, with Y = 0 }

// direct instantiation?
let x1 :Point = Point { with X = 1, with Y = 0 }

// direct patching?
let y1 :Point = zero { with Y = 1 }
~~~

## Typing Of Records

~~~
let r1 = record { with X :Num = 12 }
//  r1 of <anonymous record>
//  > with X of Num

let r2 = r1 { with Y :String = "Hello" }
//  r2 of <anonymous record>
//  > with X of Num
//  > with Y of String

type Point = record { with X :Num, with Y :Num }

// let r4 :Point = r1 // Error: `Y` not defined.
// let r4 :Point = r2 // Error: `Y` has wrong type.

let r4 :Point = r2 { with Y = -9 }  // Overloads `r2::Y`
//  r4 of Point
//  > with X of Num
//  > with Y of Num

let r5 = r2 { with Z :Num = 94 }
//  r5 of <anonymous record>
//  > with X of Num
//  > with Y of Num
//  > with Z of Num

let r6 :Point = r5  // implicitly allowed?
//  r6 of Point
//  > with X of Num
//  > with Y of Num
~~~


