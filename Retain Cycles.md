https://stackoverflow.com/questions/50261303/swift-closures-causing-strong-retain-cycle-with-self



```swift
let myClosure = { [weak self] in 
  guard let strongSelf = self else { return }
  //...
  strongSelf.doSomething()
}
```

Note that a common pattern is to add a capture list, and then map the weak variable to a strong variable inside the closure:



That way, if the closure is still active but the object that owns it was released, the guard statement at the beginning detects that self is nil and exits at the beginning of the closure. Otherwise you have to unwrap the optional every time you refer to it.



In certain cases it's also possible that the object in the capture list (self in these examples) could be deallocated in the middle of the closure being executed, which can cause unpredictable behavior. (Gory details: This is only when the closure is run on a different thread than the owner of the object in the capture list, but completion handlers are pretty commonly run on a background thread, so it does happen)



#### Weak

A `weak` reference allows the possibility of it to become `nil` (this happens automatically when the referenced object is deallocated), therefore the type of your property must be optional - so you, as a programmer, are obligated to check it before you use it (basically the compiler forces you, as much as it can, to write safe code).



#### unowned

An `unowned` reference presumes that it will never become `nil` during its lifetime. An unowned reference must be set during initialization - this means that the reference will be defined as a non-optional type that can be used safely without checks. If somehow the object being referred to is deallocated, then the app will crash when the unowned reference is used.



```swift
class Customer {
    let name: String
    var card: CreditCard?
    init(name: String) { self.name = name }
}
 
class CreditCard {
    let number: UInt64
    unowned let customer: Customer
    init(number: UInt64, customer: Customer) { self.number = number; self.customer = customer }
}
```

In this example, a `Customer` may or may not have a `CreditCard`, but a `CreditCard` **will always** be associated with a `Customer`. To represent this, the `Customer` class has an optional `card` property, but the `CreditCard` class has a non-optional (and unowned) `customer` property.

```swift
Customer ===(strong)==> CreditCard
Customer <==(unowned)== CreditCard
```

https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html