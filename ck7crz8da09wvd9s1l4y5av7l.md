## Who should do simple calculations: server or client?

The **server** should do calculations that require proper business logic, or that require information from multiple sources in order to generate a result.


The **client** should do all simple calculations, like adding together `firstName` and `lastName`.


If there's a chance two people could do the calculation and end up with different results, then the server should do this. 

I am not including careless / simple mistakes in the category of different results. For example, if I forget to put in a space between first and last name, this is a careless / simple mistake. I should still do this calculation on the client. e.g. 
```swift
let fullName = firstName + lastName // this is a simple mistake on the client
let fullName = firstName + " " + lastName // this is what I meant to do
```

I mean different results because we could reasonably interpret the logic in different ways. For example, `legalName` could or could not include `middleName`. Or perhaps depending on the locale, `legalName` might have the last name first, or the first name first.

In this scenario, two developers could reasonably interpret `legalName` differently, and so the server should generate this.