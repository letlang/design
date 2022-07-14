# Let-Lang // Events

~~~
// 1st -- deconstructing

at LetLang.Predef {
  let symbol = Symbol()
  at type    { let [symbol] = Symbol() }
  at receive { let [symbol] = Symbol() }
  at send    { let [symbol] = Symbol() }
  at get     { let [symbol] = Symbol() }
  at set     { let [symbol] = Symbol() }
}

at MyEvent {
  public  type [type]    = record {}
  public  type [receive] = Source(MyEvent.[type])
  private type [send]    = Sink(MyEvent.[type])
}

at MyVar {
  public  let [get] :Gettable(Int32)
  private let [set] :Settable(Int32)
}

// 1st a)
// weird experimental syntax for
// deconstruction based on symbols

let MyEvent.[type, receive, private send] :event {}
//           ^^^^
//     weird but mandatory
//  (is the [type] required?)

let MyVar.[get, private set] :mutable Int32

// sugar

let MyEvent.[receive, private send] :event {}

// becomes

at MyEvent {
  public  [receive] :(event {}).[receive]
  private [send]    :(event {}).[send]
}

// sugar 

type MyTrait = trait {
  let MyEvent.[receive] :event {}
}

let MyObject = object {
  extends MyTrait
  let MyEvent.[ovr receive, private send] = EventBus.Create()
}

// becomes

type MyTrait = trait {
  at MyEvent {
    let [receive] :(event {}).[receive]
  }
}

let MyObject = object {
  extends MyTrait
  at MyEvent {
    private let $ev       = EventBus(record {}).Create()
    ovr     let [reveive] = $ev.[receive]
    private let [send]    = $ev.[send]
  }
}

// detailed

type MyTrait = trait {
  let MyEvent.[receive] :event {}
}

at LetLang.Predef {
  at receive { let [symbol] = Symbol() }
  at send    { let [symbol] = Symbol() }

  type EventEmitterExtension = extension {
    for Any {
      protected macro event(let Body :TypeBody(RecordBuilder)) {
        type _rec = record(Body)
        return object {
          type [receive] = Source(_rec)
          type [send]    = Sink(_rec)
        }
      }
    }
  }
}
~~~
