## Testing Errors



```swift
enum APIError: Error, Equatable {
	case invalidURL
	case error(Error)
	case badServerResponse
	case invalidData
	case decoding
}
```



```swift
public func == (lhs: Error, rhs: Error) -> Bool {
	guard type(of: lhs) == type(of: rhs) else { return false }
	let error1 = lhs as NSError
	let error2 = rhs as NSError
	return error1.domain == error2.domain && error1.code == error2.code && "\(lhs)" == "\(rhs)"
}

extension Equatable where Self : Error {
	public static func == (lhs: Self, rhs: Self) -> Bool {
		lhs as Error == rhs as Error
	}
}
```



```swift
XCTAssertEqual(capturedOnGetPhotosError **as**? APIError, .invalidURL)
```



