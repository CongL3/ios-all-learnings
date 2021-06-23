

## @Published

```swift
@Published
```

When the property changes, publishing occurs in the property’s `willSet` block, meaning subscribers receive the new value before it’s actually set on the property. 



## @State

SwiftUI uses the `@State` property wrapper to allow us to modify values inside a struct, which would normally not be allowed because structs are value types.

When we put `@State` before a property, we effectively move its storage out from our struct and into shared storage managed by SwiftUI. This means SwiftUI can destroy and recreate our struct whenever needed (and this can happen a lot!), without losing the state it was storing.



`@State` should be used with simple struct types such as `String`, `Int`, and arrays, and generally shouldn’t be shared with other views. If you want to share values across views, you should probably use `@ObservedObject` or `@EnvironmentObject` instead – both of those will ensure that all views will be refreshed when the data changes.

To re-enforce the local nature of `@State` properties, Apple recommends you mark them as `private`, like this:

```swift
@State private var username = ""
```

