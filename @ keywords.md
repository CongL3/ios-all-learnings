```swift
@Published
```

When the property changes, publishing occurs in the property’s `willSet` block, meaning subscribers receive the new value before it’s actually set on the property. 

