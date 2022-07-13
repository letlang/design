# Let-Lang // Events

~~~
let Counter = trait {

  let Count.[get] :readonly Int32

  let Changed.[receive] :Sourceable(CountEvent)

  let Capped.[receive] :Sourceable(CountEvent)

  fun Increase(let Count :Int32) :Unit
  fun Decrease(let Count :Int32) :Unit

  companion let CountEvent = event {
    let OldValue :Int32
    let NewValue :Int32
    let Change :Int32
  }

}

let CounterImpl = class {
  is Counter

  let Init :Int32 default 0
  let Min  :Int32 default Int32.Min
  let Max  :Int32 default Int32.Max

  let Count.[ovr get, private set] :readwrite Int32 = Init

  let Changed.[ovr receive, private send] :Flowable(Int32) = EventBus()

  ovr fun Increase(let Count :Int32) :Unit {
    (let Old, let New) = (set Count = Math.Clamp(it + Count, Min, Max))
    if (New != Old) then {
      send Changed {
        with OldValue = Old
        with NewValue = New
        with Change = Count
      }
    }
    if (New == Min or New == Max) then {
      send Capped {
        with OldValue = Old
        with NewValue = New
        with Change = Count
      }
    }
  }

  ovr fun Decrease(let Count :Int32) :Unit = Increase(-Count)
}

fun Main(let Env :Environment) :Unit {
  let counter :Counter = CounterImpl.Create {
    with Min = 0
    with Max = 20
  }
  join {
    on (counter.Changed, Daemon = True) {
      Env.IO.WriteLine("Changed to ${it.NewValue}")
    }
    on (counter.Capped, Daemon = True) {
      Env.IO.WriteLine("Capped at ${it.NewValue}")
    }
    join {
      repeat {
        try {
          let read = Env.IO.ReadLine()
          if (read.IsEmpty or Env.IO.In.IsClosed) {
            return'repeat
          }
          let count = Int32.Parse(read)
          counter.Increase(count)
        }
        catch {
          let exception :ParseException ->
          Env.IO.WriteLine("Please enter a valid number.")
        }
      }
    }
  }
}
~~~

