# Enums

## Without Parameters 
~~~
let Answer = enum {
  with Yes   = object {}
  with No    = object {}
  with Maybe = object {}
}
~~~

## With Parameters
~~~
let Answer = enum {
  with Yes   = object { with Value = True }
  with No    = object { with Value = False }
  with Maybe = object { with Value = None }

  let Value :Boolean?
}
~~~

## With Records
~~~
let Gender = enum {
  with PreferNotToSay = object { with Name = "Prefer Not To Say" }
  with Female         = object { with Name = "Female" }
  with Male           = object { with Name = "Male" }
  with Other          = record { ovr with Name :String }

  let Name :String
}
~~~

## Issues

### Scoping

Are enum members in the same scope as the enum
or nested inside the enum?

~~~
let Answer = enum {
  with Answer.Yes = object {}
  with Answer.No  = object {}
}
~~~

### Simplified Syntax

~~~
let Answer = enum {
  with Yes
  with No
  with Maybe
}
~~~
