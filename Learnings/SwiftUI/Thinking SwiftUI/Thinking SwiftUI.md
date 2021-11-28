## Thinking SwiftUI - Objec.io

Debugging

```swift
extension View {
	func debug() -> Self {
		print(Mirror(reflecting: self).subjectType)
		return self
	}
}
```



View builder statments

```swift
The following example contains almost all possible statements in a view builder:

VStack {
	Text("Hello")
	if true {
		Image(systemName: "circle")
	}
	if false {
		Image(systemName: "square")
	} else {
		Divider()
	}
	Button(action: {}, label: {
		Text("Hi")
	})
}

```

