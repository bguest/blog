Title:    Swift-4 custom Assignment Operators  
Subtitle: Where we you remake your favorite ruby operators  
Comments: true  
Date:     January 24, 2018  

So.. One of the things I've missed for some time is ruby's `||=` operator. It does the great thing
of. Hey, if this thing is nil, assign it to this other thing instead. In swift you *can* do the same
thing doing 

```swift
myThing = myThing ?? myOtherThing
```

but after coming from ruby even this seemed a little more verbose than what was really needed.

Turns out its pretty easy to set up your own operators in swift.

```swift
infix operator ||= : AssignmentPrecedence

func ||=<T: Any> ( oldValue:inout T?, newValue:T?){
    oldValue = oldValue ?? newValue
}

var a: CGFloat? = nil
a ||= 10                      // a => 10.0
```
