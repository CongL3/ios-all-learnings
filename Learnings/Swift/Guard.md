```swift
guard let nextDate = theCalendar.date(byAdding: dayComponent, to: startDate) else {
	return startDate
}
```



Optional chaining



```swift
	func load(url: String?) {
		
		guard let link = url,
		   let url = URL(string: link) else { return }
		
		openURL(url)
	}

```

