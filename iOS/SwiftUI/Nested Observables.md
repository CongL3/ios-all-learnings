# Nested observables

Look into combine and create a 

```
I wrote about this recently on my blog: Nested Observable Objects. The gist of the solution, if you really want a hierarchy of ObservableObjects, is to create your own top-level Combine Subject to conform to the ObservableObject protocol, and then encapsulate any logic of what you want to trigger updates into imperative code that updates that subject.

```

https://stackoverflow.com/questions/58406287/how-to-tell-swiftui-views-to-bind-to-nested-observableobjects



```swift
class SubModel: ObservableObject {
    @Published var count = 0
}

class AppModel: ObservableObject {
    @Published var submodel: SubModel = SubModel()
    
    var anyCancellable: AnyCancellable? = nil
    
    init() {
        anyCancellable = submodel.objectWillChange.sink { [weak self] (_) in
            self?.objectWillChange.send()
        }
    } 
}
```



