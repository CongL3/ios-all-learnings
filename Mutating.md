# Mutating

In swift, classes are **reference type** whereas structures and enumerations are **value types**. The properties of value types cannot be modified within its instance methods by default. In order to modify the properties of a value type, you have to use the **mutating keyword** in the instance method. With this keyword, your method can then have the ability to mutate the values of the properties and write it back to the original structure when the method implementation ends.

Below is a simple implementation of Stack in Swift that illustrates the use of mutating functions.



```swift
struct Stack {
    public private(set) var items = [Int]() // Empty items array
    
    mutating func push(_ item: Int) {
        items.append(item)
    }
    
    mutating func pop() -> Int? {
        if !items.isEmpty {
           return items.removeLast()
        }
        return nil
    }
}

var stack = Stack()
stack.push(4)
stack.push(78)
stack.items // [4, 78]
stack.pop()
stack.items // [4]

```

