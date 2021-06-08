# Generics 



```swift
protocol ItemStoring {
    associatedtype DataType

    var items: [DataType] { get set}
    mutating func add(item: DataType)
}
```



Associated types described the data type. An **associated type** can be seen as a replacement of a specific **type** within a protocol definition. In other words: it's a placeholder name of a **type** to use until the protocol is adopted and the exact **type** is specified



The associated type is defined using the `associatedtype` keyword and tells the protocol that the subscript return type equals the append item type. This way we allow the protocol to be used with any associated type later defined



https://www.avanderlee.com/swift/associated-types-protocols/#what-is-an-associated-type





