# Hiding keys

Create a plist to store your new keys

<img src="https://raw.githubusercontent.com/CongL3/ios-all-learnings/main/Images/HidingKeysPlist.png" alt="drawing" />

Retriving the data


```swift
		if let path = Bundle.main.path(forResource: "keys", ofType: "plist") {
				let keys = NSDictionary(contentsOfFile: path)
				print(keys)
		}
```

Example print out of data

```
Optional({
  omdiAPIKey = ********;
})
```