# Let-Lang // Syntax

~~~
// `let` declaration

// every new symbols is introduced with
// a `let`-binding (thus the name).
// such a binding is a constant,
// it cannot change during runtime.
// some bindings allow it to be
// overridden.


// typed binding

  let Name :Type = Expression()


// untyped binding

  let Name = Expression()

  //  the type of the new symbol
  //  is inferred from the return
  //  type of the expression.

  //  what about constants?
  //  like what happens to:

    let Foo = 7

  //  * infer a "proper" runtime type
  //    ==> Foo :Int32
  //    basically a javaish solution.
  //    its something that really bothers
  //    me about kotlin!
  //  * infer a "universal" runtime type
  //    ==> Foo :VarInt
  //    the idea is basically to have a
  //    bigint behing a tagged pointer
  //    so if the int is small enough it
  //    is inlined into the pointer bits.
  //    signed or unsigned?
  //  * infer a universal compiler type
  //    ==> Foo :IntLiteral
  //    what is an int-literal!?
  //  * infer an "individual" type
  //    ==> Foo :7.type
  //    it is a constant?
  //    can it be changed?
  //    clients convert it into the
  //    format they need.
  //  * infer an "individual" type (2)
  //    ==> Foo :IntLiteral which == 7
  //    again, what is an int-literal?
  //    is it the ast-node?
  //    externalized into the binary?
  //  it is a constant!
  //  it is not converted to
  //  any runtime type!
  //  it is converted on demand!


// abstract typed bindings

  let Name :Type

  //  introduces a new symbol but does not
  //  provide its value (implementation).
  //  the implementation must be provided
  //  elsewhere. e.g. for abstract types
  //  in an (instantiatable) subtype.
  //  for top-level bindings in another
  //  source-file (-> prototype).
~~~
