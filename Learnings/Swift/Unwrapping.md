# Unwrapping stuff



#### Optional Binding 

```
var str: String? = nil
if let string = str {
    print (string)
}
```



```swift
let dave = Person(name: "Dave", age: 21, income: 20000)

if let age = dave.age {
    if let income = dave.income {
        if let name = dave.name {
            // we could do various things to Dave
        }
    }
}

```



Avoid large pyrmid

```swift
if let age = dave.age, let income = dave.income, let name = dave.name {
    // we could do various things to Dave
}
```



#### Optional chaining

```swift
let album = albumReleased(year: 2006)?.uppercased()
```

Note that there's a question mark in there, which is the optional chaining: everything after the question mark will only be run if everything before the question mark has a value



#### The nil coalescing operator

```swift
let album = albumReleased(year: 2006) ?? "unknown"
```

it stops optionals being nil as it gives it a default value.



#### Guard

guard lets you leave the current funcution loop etc when someone is nil

set the let variable but if it doesnt work *return*

```swift
func printMeaningOfLife() {
    guard let name = getMeaningOfLife() else {
        return
    }

    print(name)
}
```

same as 

```swift
func printMeaningOfLife() {
    if let name = getMeaningOfLife() {
        print(name)
    }
}
```



