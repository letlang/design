# Let-Lang // Syntax

~~~
// parallel binding 
(let a, let b) = (12, "Hello")
echo(a)
echo(b)

// namespaced binding
let a.b = 12
echo(a.b)

// parallel namespaced binding
let a.(b, c) = (12, "Hello")
echo(a.b)
echo(a.c)

// parallel named binding
(let b, let c = a) = object { with a = 12, with b = "Hello" }
echo(b)
echo(c)

// parallel namespaced named binding
let a.(b, c = a) = object { with a = 12, with b = "Hello" }
echo(a.b)
echo(a.c)

// namespaced symbol binding
let foo = Symbol()
let a.[foo] = 12
let a.foo = "Hello"
echo(a.[foo])   //> 12
echo(a.foo)     //> Hello
let bar = foo
echo(a.[bar])   //> 12

// parallel namespaced named symbol binding
let foo = Symbol()
let bar = Symbol()
let a.[foo, bar] = object { with [foo] = 12, with [bar] = "Hello" }
echo(a.[foo])
echo(a.[bar])
~~~

## Syntax Problems

~~~
a.(let b, let c) = (12, "Hello")

let foo = Symbol()
let bar = Symbol()
a.(let [foo], let [bar]) = (12, "Hello")


// ALTERNATIVE SYNTAX
// selective binding

let foo = object {
  with a = 12
  with b = "Hello"
  with c = -5
}
echo(foo.a)
echo(foo.b)
echo(foo.c)

let bar.{a, b} = foo
echo(bar.a)
echo(bar.b)
//echo(bar.c) //ERROR

let baz.{a, private b} = bar
echo(baz.a)
echo(baz.b)
~~~
