Title:    Commutative Operators in Swift  
Author:   Benjamin Guest  
Subtitle: Dumb maths for CGSizes  
Date:     July 22, 2016  
Comments: true  

There isn't really anything special here that you couldn't learn in like 2 minutes of searching google,
 but what the hell, I think its kind of cool.

Say you have view A and you want to make it 10% bigger than view B, you would think this would work

```swift

A.bounds.size = B.bounds.size * 1.1 // Raises Error

```

But it doesn't, swift does know how to multiply sizes and floats. Luckily we can teach it.


```swift

func *(lhs: CGFloat, rhs: CGSize) -> CGSize{
    let height = lhs*rhs.height
    let width = lhs*rhs.width
    return CGSize(width: width, height: height)
}

func *(lhs: CGSize, rhs: CGFloat) -> CGSize{
    return rhs*lhs
}

```

Now both of the following will work.


```swift

A.bounds.size = B.bounds.size * 1.1
A.bounds.size = 1.1 * B.bounds.size

```
