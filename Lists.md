# List

Items in List have to be conform to the Protocol "Identifiable"

```swift
struct Item: Decodable, Identifiable{
	var id = UUID().uuidString
}
```

