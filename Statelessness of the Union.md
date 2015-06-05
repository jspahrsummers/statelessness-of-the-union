build-lists: true

# [fit] ReactiveCocoa
# [fit] **“Statelessness”**
# [fit] of the Union

---

# [fit] @jspahrsummers

---

# [fit] ReactiveCocoa
# [fit] Developers Conference
# (RACDC)

---

# What is
# [fit] ReactiveCocoa?

---

# [fit] Not just **KVO**

---

# [fit] Not just **bindings**

---

# [fit] Not just **futures**

---

# [fit] **Signals**
# [fit] unify all of these paradigms
# (and more)

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
