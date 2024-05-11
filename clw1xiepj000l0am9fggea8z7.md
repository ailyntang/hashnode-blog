---
title: "dotnet: value objects vs entity objects"
datePublished: Sat May 11 2024 09:53:53 GMT+0000 (Coordinated Universal Time)
cuid: clw1xiepj000l0am9fggea8z7
slug: dotnet-value-objects-vs-entity-objects
tags: csharp

---

Learnings from this article: [https://enterprisecraftsmanship.com/2016/01/11/entity-vs-value-object-the-ultimate-list-of-differences/](https://enterprisecraftsmanship.com/2016/01/11/entity-vs-value-object-the-ultimate-list-of-differences/)

# Definitions

Value objects do not make sense on their own.

* They need to be attached to an entity.
    
* E.g. an address is a value object. On its own, it makes no sense. What is the address of? So it needs to be attached to an entity, like a Person, or a Place Of Worship, or a Business, etc.
    

If you have two value objects, and the value of their fields are the same, then you can conclude they are equal.

* It doesn't matter if the two objects do not point to the same address in memory, because that's not the point of value object equality.
    
* So `address 1 = 1 happy lane, utah` and `address 2 = 1 happy lane, utah` are equal. Even though they will have two different memory addresses.
    

Entities are only equal if they have an identifier field, and that id is the same across both entities

* e.g. For a `class Person`, there can be two Paul Smiths. But, they are not the same person unless their underlying identifier (e.g. their Social Security Number) is the same.
    
    * So this is the exact opposite of a value object, where if the `Person` was a value object, the two Paul Smiths would be equal
        
* I also assume all fields must be equal, as well as the identifier
    

# It's better to create value objects wherever possible

Entity objects have a lifespan and a history, as they are often mutable

* This means `state` comes into play, which makes things trickier
    

Value objects `should` have zero lifespan

* We create them easily and destroy them easily
    
* The article says it's a debate about whether value objects should be immutable
    
    * In the author's view, yes, make value objects immutable to ensure they have zero state management / lifespan
        
    * If you need a new version of the value object in a different state, just destroy the old one and create a new one
        
    * Otherwise what's the point of having a value object?
        

# Could I make this a value object in my code?

Have a think about the memory address question.

For this object, does it absolutely need to be same object with the same memory address the next time I use it?

If yes, then it stays an entity.

But, if you can think of it like an `integer` - ie. as long as the value is `5`, then you don't care if the `int 5` is in a different memory address to the last `int 5` you used - then this is a value object.

# Inline value objects in your database

Value objects should never be a table on their own.

Add the fields of the value object as columns, in the table of their entity.