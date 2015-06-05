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

---

![](Resources/whats-in-the-box.jpg)

---

![fill](Resources/hot-n-cold.jpg)

---

# [fit] Subjects

^ I ran out of good images here.

---

# [fit] Multicasting

---

# [fit] Replaying

---

# [fit] How do **Rx implementations** solve these problems?

- They don’t

^ After all, RAC was originally inspired by Rx, and all of these concepts
originate from it, so Rx must have a solution, right?

---

# [fit] `RACCommand`

---

# [fit] `-flattenMap:`

^ Which is overemphasized versus `-concat` and `-switchToLatest`.

---

> ReactiveCocoa has **too much magic**!
-- the internet

---

# [fit] **Unpredictable**
# [fit] error events

^ e.g., in property binding

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

# Remaining work

- Long-form documentation

^ Here's what's left before RAC officially hits 3.0.

---

# Contributors

TODO: Significant contributors to RAC 3

---

# Interested in contributing?
