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