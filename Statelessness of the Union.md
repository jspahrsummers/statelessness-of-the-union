build-lists: true

# ReactiveCocoa
# [fit] **Statelessness**
# of the Union

---

# [fit] _@jspahrsummers_

---

# About RACDC

---

# What is
# [fit] ReactiveCocoa?

---

# Not just _KVO_

---

# Not just _bindings_

---

# Not just _futures_

---

# What, then?

---

# Problems with
# ReactiveCocoa 2

---

![](Resources/whats-in-the-box.jpg)

---

# “Hot” and “cold” signals

---

# Subjects

---

# Multicasting

---

# Replaying

---

# How do other Rx implementations solve this problem?
# _(They don’t.)_

---

# `RACCommand`

---

# `-flattenMap:`

^ Which is overemphasized versus `-concat` and `-switchToLatest`.

---

> Too much magic!
-- the internet

---

# Unpredictable error events

^ e.g., in property binding

---

# [fit] A wild Swift appears!

---

# Parameterized types

---

# Value types

---

# No macros

---

# Less dynamic programming

^ KVC, KVO, `-rac_signalForSelector:`

---

# RAC 3

---

# Parameterized values

---

# Parameterized errors
# `NoError`

---

# `Signal` and `SignalProducer` split

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

# Simple vs. easy

![autoplay fit](Resources/decomplect-it.gif)

^ There's probably nothing I've referenced more in the past year

---

# RAC 2 is neither
# [fit] easy
# nor
# [fit] simple

---

# RAC 3 is simple
# (hopefully)

^ But still not always easy.

---

# [fit] Remaining work

- Long-form documentation

^ Here's what's left before RAC officially hits 3.0.

---

# [fit] Contributors

TODO: Significant contributors to RAC 3

---

# Interested in contributing?
