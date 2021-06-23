# as as? as! is



https://abhimuralidharan.medium.com/typecastinginswift-1bafacd39c99



checking type

```swift
for item in livingBeingArray {
	if item is Animal {
		print("item is of type Animal")// will get executed for first item
	} else if item is Human {
		print("item is of type Human")// will get executed for second item
	}
}

```





#### Difference between as? and as!

Downcasting can be done in two ways:
**Conditional downcasting (as?).**
**Forced downcasting (as!).**



#### Upcasting

```swift
let animalObj = livingBeingArray[0] as! Animal
let animalObjectAsLivingBeingObj = animalObj as LivingBeing
```

upcasts animal object back to the super class LivingBeing



<img src="/Users/congle/Documents/Dev/ios-all-learnings/Images/image-20210608030933558.png" alt="image-20210608030933558" style="zoom:33%;" />

#### Example of using as

```swift
var groups = [Any]()
groups.append(1.0)
groups.append(1)
groups.append("string")

for item in groups {
    switch item {
    case let anInt as Int:
        print("\(item) is an int")
    case let aDouble as Double:
        print("\(item) is a double")
    case let aString as String:
        print("\(item) is a string")

    default:
        print("dunno")
    }
}

/*
1.0 is a double
1 is an int
string is a string
C11lldb_expr_13Pop (has 1 child) is a Genre
*/

```

