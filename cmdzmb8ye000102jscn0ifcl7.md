---
title: "@MainActor attribute - when to apply"
datePublished: Wed Aug 06 2025 06:59:33 GMT+0000 (Coordinated Universal Time)
cuid: cmdzmb8ye000102jscn0ifcl7
slug: mainactor-attribute-when-to-apply
tags: swift

---

If you are in the middle of having Swift five and six in your code base, then you will need to proactively deal with applying the main actor attribute to your Swift five code, so that it doesn't break when you upgrade to Swift six.

This can happen if for example, you have one package in swift five, and another package in Swift six.

What is important to remember is that the main actor attribute needs to be applied whenever you are accessing a variable or a method inside a view. So this means if you have `view.count`, `view.color`.

If you are simply passing the view around, you aren't manipulating it. Therefore, there is no danger of its state being changed. Therefore, you don't need to apply the main actor attribute just because it is a view.

An example of this would be a protocol. A protocol is something that you define yourself, but a view could inherit this protocol. The view should always be on the main thread, because that is the default build setting.

Therefore you need to make sure that whatever happens inside the protocol which is related to UI also happens on the main thread. And the main actor attribute is how you ensure that happens.

If you have a number of different variables and methods inside this protocol, and only a handful of them relate to UI, then it can be preferable to apply the main actor attribute only to those specific variables and methods. This reduces the ripping main actor conforms that you need further up the chain. Because it means that something could conform to the protocol, but if it doesn't utilize one of the main actor variables nor main actor methods, then that class will not need to conform to main actor either.