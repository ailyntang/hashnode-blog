---
title: "dotnet: property declarations"
datePublished: Mon Apr 29 2024 06:43:51 GMT+0000 (Coordinated Universal Time)
cuid: clvklft7c000c0amnfhs53c1u
slug: dotnet-property-declarations
tags: csharp

---

There are multiple ways to declare a property.

Here are different ways, which vary the access level of the `setter`, from the most protected to the most open:

* `{ get; protected init; }`
    
    * Initialising this value can happen only in the class where this is declared, and also in classes that are derived from the base class
        
    * Once the value is set, it is read only, both internally and externally
        
* `{ get; init; }`
    
    * Any class can initialise and set this value
        
    * Once the value is set, it is read only, both internally and externally
        
* `{ get; }`
    
    * Read only externally
        
    * Can be overwritten internally. This is like `private(set)` in Swift
        
* `{ get; set; }`
    
    * Anything can overwrite the value of this property
        

---

With all the above property declarations, you may also need to add `= null!` at the end of the declaration.

This depends on a few things:

* the project settings
    
* whether the compiler thinks your solution could actually initialise the property and end up with a Null Reference Exception (NRE)
    
* whether you are using a value type (e.g. `bool`) or reference type
    
    * Value types have a default value. You will never need to add `= null!` when declaring a value type
        

```plaintext
<PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
</PropertyGroup>
```

For example, in the `.csproj` file, if you include this code snippet, you will need to add `=null!` to the end of your property declarations.

e.g. `public List<string> CakeFlavours { get; set; } = null!;`

* The `null!` silences the compiler warning. It says to the compiler - trust me, I have coded this correctly and we will never end up in a state where this property is `null`
    
* Be careful - if you do this, and you have a code path which will lead to an NRE, your app will compile and then crash when the NRE happens