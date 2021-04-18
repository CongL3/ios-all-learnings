# Hiding keys

Create a plist to store your new keys


```swift
 if let path = Bundle.main.path(forResource: "{FileName}", ofType: "plist") {
        keys = NSDictionary(contentsOfFile: path)
    }
```

<img src="" alt="drawing" />
