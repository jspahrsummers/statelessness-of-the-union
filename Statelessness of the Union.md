# [fit] **ReactiveCocoa**
# [fit] **Developers Conference**
# (RACDC)

^ Hello, everyone, and welcome to the ReactiveCocoa Developers "Conference!" This "conference" dates back to a couple years ago, when (thanks to Scott Perry) we just did a simple Q&A at Square HQ.

^ Last year, we held it at GitHub, added some talks, and got a real A/V setup. We're doing some new things this year too, with the "lab time." If you're currentlyusing RAC and would like in-person troubleshooting, code review, or pair programming, we'll be happy to help you out a little later!

^ (Talk a bit more about the specific night or day event)

---

# [fit] @jspahrsummers

^ I'm Justin Spahr-Summers, or @jspahrsummers on the internet, and I'm one of the main contributor on ReactiveCocoa. Josh Abernathy (awkward point and stare), or @joshaber, started the project and is another main contributor.

^ We've got more RAC contributors here tonight as well, but I'll let everyone introduce themselves when we get to the Q&A portion.

---

# [fit] ReactiveCocoa
# [fit] **“Statelessness”**
# [fit] of the Union

^ I want to talk about the current state of ReactiveCocoa, and especially ReactiveCocoa 3 (which is currently in beta).

^ Whether you've used ReactiveCocoa 2, 3, both, or neither, I hope that this talk sheds light on the design choices we've made, and gets you interested in the new APIs.

---

# What is
# [fit] ReactiveCocoa?

^ But first, I want to talk about a big question: what _is_ ReactiveCocoa? Even experienced users of the framework sometimes have a hard time answering this question when other folks ask.

^ I'm not sure that I have a perfect answer either, but I'd like to share my thoughts.

---

# [fit] Not just **KVO**

^ One of the most common things we've heard said about ReactiveCocoa 2 is that "it's KVO done right." And while we certainly appreciate that sentiment, it's not the full story.

^ This really reached its head when Swift came out, and we got an onslaught of questions asking stuff like "what's going to happen to RAC now that KVO isn't supported anymore?"

^ The truth is, KVO has always been a _means to an end_ for RAC. It's a built-in way to capture changes over time, so it made sense to take advantage of it, but the use of RAC has never required KVO.

---

# [fit] Not just **bindings**

^ Another common one we heard is that ReactiveCocoa is "bindings for iOS." Putting aside the fact that we use it on the Mac as well, this too is only part of the story.

^ RAC is often perceived this way because it makes data binding really easy, especially as part of the Model-View-ViewModel pattern.

^ But honestly, from [my perspective](https://twitter.com/jspahrsummers/status/582298982992367616), bindings are one of the least interesting applications of RAC, and I think it's great that there are more and more other libraries offering them now.

---

# [fit] Not just **futures**

^ Finally, people often ask why ReactiveCocoa is valuable, when there are plenty of futures/promises libraries that are smaller and ostensibly easier to use.

^ In my (admittedly biased) opinion, streams are a strictly more powerful superset of futures, because they permit any number of values and have more flexible operators.

^ Still, I agree: if _all you want_ is a futures library for network requests (or whatever), RAC probably does too much for you.

---

# [fit] **Signals**
# [fit] unify all of these patterns
# (and more)

^ The real power of ReactiveCocoa is that it unifies _all_ of these things into one cohesive, internally consistent paradigm.

^ KVO, bindings, futures, delegates, timers, input/output, etc. are all just _signals_. We have all of these different ways to represent changes or events over time, which can now be subsumed into this one concept.

^ And unlike most of those patterns, the "over time" nature of signals is very clear, which allows _time itself_ to be operated upon (for example, with operators that delay or throttle by a time interval). It's beautiful.

---

# [fit] **Problems**
# [fit] with ReactiveCocoa 2

^ Before we could even start work on ReactiveCocoa 3, we had to identify the biggest problems in version 2 that needed fixing.

---

![](Resources/whats-in-the-box.jpg)

^ First and foremost is the lack of type information. RAC 2, being written in Objective-C, just has this one `RACSignal` type, and you can never tell what kind of values it's going to send—just like `NSArray` or `NSDictionary`.

^ This has probably been responsible for more bugs than anything else in RAC.

---

![fill](Resources/hot-n-cold.jpg)

^ Chances are, if you've heard signals referred to as "hot" or "cold," it was probably extraordinarily confusing. The difference between these terms is how the signal behaves when you "subscribe" (attach) to it.

^ A "hot" signal always sends the exact same events to all of its subscribers at the same time. When you "subscribe," you're really just observing this existing event stream, and your subscription to it doesn't start or stop it. For example, button taps are a hot signal, because the button will generate events whether you're listening or not.

^ A "cold" signal _starts_ when subscribed to, and may send different events each time. This one is more like a future or a promise, because you can run it to do some work and receive a result.

^ The problem in ReactiveCocoa 2 is that _you can't tell these apart_. Unless a method or property's documentation calls out which kind of signal it creates, you really have no idea whether your subscription will have side effects or not.

---

# [fit] Subjects
# [fit] Multicasting
# [fit] Replaying

^ I ran out of good images here.

^ These are some examples of how Rx implementations "solve" the problem of hot and cold signals. I won't go into too much detail about each, but suffice it to say that they allow you to convert between hot/cold signals, or even split the difference and create a "warm" signal.

^ Although RAC has these tools as well, they've always felt like workarounds for a more fundamental problem.

---

# [fit] `RACCommand`

^ I'm not sure how many people use `RACCommand`, but if you have, you're probably well-aware of how much trouble it can be.

^ Commands are intended to trigger work (like a network request) when an action is taken in the UI, then send any results as a signal. They're really useful in bindings, because you can set a command on a UI control just like you would set its target and action, and then observe the command elsewhere to obtain the results.

^ Unfortunately, they're also overcomplicated, with a rarely-used concurrent execution feature, confusing error handling, and a dependency upon the main thread (which makes unit testing especially hard).

---

> ReactiveCocoa has **too much magic**!
-- the internet

^ Finally, the most abstract complaint we hear is that RAC is simply "too magical," what with its macros and KVO and… that's it, I guess?

^ Although we've always offered "non-magical" ways to do the same thing, this has been a turnoff for potential users, so it is a problem for adoption.

---

# [fit] **Swift**

![fit](Resources/appears.jpg)

^ Of course, while we were already trying to solve all these problems, Swift happened. The new language brought major changes of its own.

---

# [fit] **Parameterized** types
# (generics)

^ Swift's stronger type system allows us to answer the question "what's in the box?" very clearly and succinctly. Problem solved!

---

# [fit] No **macros**

^ Swift currently has no metaprogramming facilities. So even if we wanted to, we couldn't continue using things like macros to make the framework easier to use.

^ Problem… solved… I guess?

---

# [fit] Less **dynamic programming**

^ Similarly, Swift makes features like KVC and KVO more difficult. It doesn't obsolete them, per se, but it's definitely a big impedance mismatch.

^ It's easier for RAC to just avoid those features as much as possible now. That's another "problem" we don't have to worry about anymore.

---

# [fit] :tada: **ReactiveCocoa 3** :tada:

^ After Swift was released, we feverishly started work on a Swift API for ReactiveCocoa. With enough time, and a few major rewrites, this developed into the RAC 3 beta that's available today.

---

# [fit] **`Signal<T,E>`**
# (parameterized values _and_ errors)

^ This denotes a signal that can send values of type T, and that may error with an error of type E.

---

# [fit] `Signal<String, CarthageError>`

^ This is a signal which will send zero or more strings. It may send a `CarthageError` (and terminate), but it doesn't have to.

---

# [fit] `Signal<Int, NoError>`

^ This is a signal which will send zero or more integers. This signal _cannot_ error, and trying to do so is a compile-time error!

^ `NoError` is useful for guaranteeing that you don't need to handle error events in certain cases (like property binding).

---

# [fit] **Signals**
# and
# [fit] **Signal Producers**

^ RAC 3 splits the venerable `RACSignal` into two different types:

^ Signals are like a "hot" RACSignal. They send the same events to all observers at the same time, and observing a signal doesn't trigger any extra work.

^ Signal producers are like a "cold" RACSignal. _Starting_ signal producer will create a unique signal, which may send different events than other created signals. Starting a signal producer may also involve side effects (like kicking off a network request).

^ This split means that side effects are now predictable _in the type system_, which makes RAC 3 code simpler.

---

# **Signals** and **Signal Producers**

```swift
let producer = timer(1, onScheduler: QueueScheduler())

let disposable = producer.start(next: { date in
    println("First timer fired at \(date)")
})

producer.startWithSignal { signal, signalDisposable in
    signal.observe(next: { date in
        println("Second timer fired at \(date)")
    })
}
```

^ This example shows a `SignalProducer` representing a timer that fires once every second, and then two different ways we can start that timer.

^ By starting the producer _twice_ here, we've started two timers. If we never started the producer, there would be no timers running.

^ The disposable objects you see here can be used to _cancel_ that particular invocation of the producer. They have no effect on other invocations.

---

# **Signals** and **Signal Producers**

```swift
let producer = timer(1, onScheduler: QueueScheduler())

producer.startWithSignal { signal, signalDisposable in
    signal.observe(next: { date in
        println("Timer fired at \(date)")
    })

    signal.observe(anotherObserver)
}
```

^ Using the `startWithSignal` method, you can also observe the signal multiple times (for example, to handle its events and also forward them somewhere else).

^ As mentioned before, adding and removing observers doesn't affect the signal, so this doesn't change the behavior of the started timer at all.

---

# [fit] **`Action`**
# (instead of `RACCommand`)

^ Actions, new in RAC 3, are very similar to commands, but with much-needed simplifications.

^ Most notably, actions can only be executed serially, and errors get no special treatment.

---

# **Actions**

```swift
let searchAction = Action(enabledIf: hasText) { text in
    let resultsProducer = APIClient.searchText(text)
    return resultsProducer
}

searchAction.values.observe(next: { result in
    println("Search result: \(result)")
})

let searchCatsProducer = searchAction.apply("cats")
searchCatsProducer.start()
```

^ Here's a contrived example of creating, observing, and starting an action.

^ As you can see, it's ultimately just a wrapper around SignalProducers that enforces serial execution and allows you to observe the values "from the outside."

---

# [fit] **`PropertyType`**

^ To replace uses of KVC and KVO, RAC 3 introduces its own concept of "properties." Properties always have a value, and always allow observation of their changes. Some concrete property types allow the value to be changed explicitly, but this is not a requirement.

---

# **`PropertyType`**

```swift
let text = MutableProperty("0")

text.producer.start(next: { string in
    println("Property value is now: \(string)")
})

text <~ timer(1, onScheduler: QueueScheduler())
        |> scan(0) { counter, _ in counter + 1 }
        |> map { counter in toString(counter) }
```

^ This example creates a counter that increments itself every second. We observe the changes and log every value of the property to the console.

---

# The theme is
# [fit] **simplicity**

^ All of these changes have been made in the name of simplicity. But before we can evaluate whether we've achieved that goal, let's talk about what it means.

---

# [fit] **EASY**[^1]
# [fit] familiar
# [fit] approachable

![autoplay right](Resources/easy.gif)

[^1]: See Rich Hickey’s talk, “[Simple Made Easy](http://www.infoq.com/presentations/Simple-Made-Easy)”

^ To talk about simplicity, we first need to talk about ease. Something is _easy_ if it is familiar or approachable.

^ Objective-C is _easy_ for most C programmers, because it's a very approachable step upward. Bindings and (to some extent) futures are also _easy_.

---

# [fit] **SIMPLE**[^1]
# [fit] separate concerns
# [fit] less complex

![autoplay right](Resources/decomplect-it.gif)

^ But simplicity means something different. Simplicity is the _opposite_ of complexity. Since complexity means intertwined concepts or concerns, you attain simplicity when you appropriately separate different concepts or concerns.

^ The "Unix philosophy" is an example of simplicity: tons of small utilities that can be composed together in whichever way, sometimes less _easy_ because the pattern isn't as familiar.

---

# [fit] ReactiveCocoa 2 is _neither_
# [fit] **easy**
# nor
# [fit] **simple**

^ Unfortunately, RAC version 2 kinda failed on both of these counts. While we certianly tried to make it as easy as possible, there's still a really steep learning curve, and the patterns are very unfamiliar to many Cocoa programmers.

^ It's also not simple. All of the problems that I mentioned earlier are examples of complexity that RAC 2 is exposing.

---

# [fit] ReactiveCocoa 3 is
# [fit] **simple**
# (hopefully)

^ Our fixes for RAC 3 have focused primarily on simplicity.

^ There will still be a learning curve (and thus it may still not be "easy"), partly because the paradigm is inherently so different from what we're used to, but hopefully the complexities of version 2 have been eliminated.

---

> Dr. **Changelog**, MD
-- @joshaber

^ If you want to learn more, the [ReactiveCocoa repository](https://github.com/ReactiveCocoa/ReactiveCocoa) contains a [CHANGELOG.md](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/ad1fd34561d3f2f42e0feb420ace5de321906b78/CHANGELOG.md) file at its root, which details all of these changes (and some others I haven't covered), and presents a more in-depth rationale for each one.

---

# Remaining work for 3.0

- **Long-form documentation**

^ That's it! That's really the only major piece we have to take care of before RAC can officially release 3.0. It should be pretty soon now!

---

# Contributors

- **Dave Lee** (@kastiglione)
- **Andy Matuschak** (@andymatuschak)
- **Nacho Soto** (@NachoSoto)
- **Javi Soto** (@Javi)
- **Syo Ikeda** (@ikesyo)
- **Neil Pankey** (@neilpa)
- … many more!

^ We've had a ton of amazing contributions to RAC, from more people than I could ever hope to list on one slide.

^ I especially want to recognize these folks for their help in designing and shepherding ReactiveCocoa 3—without them, we wouldn't be nearly as far along as we are today.

^ My sincere thanks to everyone who has contributed! It's been really great to see open source working.

---

# [fit] **Interested in contributing?**
# [fit] Join Slack by emailing justin@jspahrsummers.com

^ If any of you are interested in contributing as well, in any capacity, we'd more than welcome the help! Anything is valuable, including bug fixes, documentation help, answering issues and questions from others, etc.

^ If you're unsure how best to contribute, or would just like to chat with other
collaborators, we're happy to add you to our Slack channel. Just email me (at
this address here), and I'll send you an invite.

^ Thanks everyone!
