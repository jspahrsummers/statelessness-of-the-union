build-lists: true

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

---

# [fit] **Parameterized** types
# (generics)

---

# [fit] **Value** types
# (structs, enums)

---

# [fit] No **macros**

---

# [fit] Less **dynamic programming**

^ KVC, KVO, `-rac_signalForSelector:`

---

# [fit] :tada: **ReactiveCocoa 3** :tada:

---

# Parameterized values

---

# Parameterized errors
# `NoError`

---

# [fit] **Signals**
# and
# [fit] **Signal Producers**

---

# `Signal.pipe` instead of subjects

---

# `SignalProducer.buffer` instead of replaying

---

# `startWithSignal` instead of multicasting

---

# `Action` instead of `RACCommand`

---

# `flatMap` and `flatten` “strategies”

---

# `PropertyType`

---

# [fit] **EASY**
# [fit] familiar
# [fit] approachable

![autoplay right](Resources/easy.gif)

---

# [fit] **SIMPLE**
# [fit] separate concerns
# [fit] less complex

![autoplay right](Resources/decomplect-it.gif)

^ There's probably nothing I've referenced more in the past year.

---

# [fit] ReactiveCocoa 2 is _neither_
# [fit] **easy**
# nor
# [fit] **simple**

---

# [fit] ReactiveCocoa 3 is
# [fit] **simple**
# (hopefully)

^ But still not always easy.

---

> Dr. **Changelog**, MD
-- @joshaber

---

# Remaining work

- Long-form documentation

^ Here's what's left before RAC officially hits 3.0.

---

# Contributors

TODO: Significant contributors to RAC 3

---

# Interested in contributing?
