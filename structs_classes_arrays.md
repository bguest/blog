Title: On the mutability of Swift structs and classes in arrays  
Comments: True  
Date: January 6, 2016  

As you may or may not expect, in swift, adding a struct to an array vs adding a classe to arrays ends
up being very different.

If you add classes to arrays they behave like you might expect if you are coming from Objective-C

```swift
class Klass{
    var varr: Int
    init(varr: Int){
        self.varr = varr
    }
}

let klass0 = Klass(varr: 0)       // The variables of classes are mutable even if the class is not
let klass1 = Klass(varr: 1)

let klasses = [klass0, klass1]    // => [{varr 0}, {varr 1}]

klass0.varr = 3                   // Again no problems here
klasses                           // => [{varr 3}, {varr 1}]
klasses[0].varr                   // => 3, object in array has been changed
```

Another way to look at this is that if you stick a class into an array your not creating any new objects,
the array just has a pointer to the existing object.

Structs are value based and so they behave differently.

```swift
struct Strukt{
    var varr: Int
    init(varr: Int){
        self.varr = varr
    }
}

var strukt0 = Strukt(varr: 0)     // If you are going to change the values in a struct,
let strukt1 = Strukt(varr: 1)     //   you have to make the struct mutable

let strukts = [strukt0, strukt1]  // => [{varr 0}, {varr 1}]

strukt0.varr = 3                  // => {varr 3}
sturct1.varr = 4                  // => COMPILER ERROR (you can't change variables of an imutable struct)

strukt0.varr                      // => 3 (Can change the object)

strukts                           // => [{varr 0}, {varr 1}]
strukts[0].varr                   // => 0, object in array remains unchanged
```

If you add a struct to an array, your creating a separate copy of the original struct,
changes are not consistent between the two structs
