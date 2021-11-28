# Widget

1. Add an app group

![image-20210611234307722](/Users/congle/Documents/Dev/ios-all-learnings/Images/image-20210611234307722.png)

2. Add a new group with the same bundle id

<img src="/Users/congle/Documents/Dev/ios-all-learnings/Images/image-20210611234418163.png" alt="image-20210611234418163" style="zoom:50%;" />



3. access userdefaults like this

   ```swift
   	let defaults = UserDefaults(suiteName: "group.com.congle.TEAMCONG.AnniversaryTrackerNoAds") ?? UserDefaults.standard
   ```

4. Create new target

<img src="/Users/congle/Documents/Dev/ios-all-learnings/Images/image-20210611235934782.png" alt="image-20210611235934782" style="zoom:50%;" />

5.



```
typealias Entry = type

// Entry type holds when the widget is suppose to be updated and presented
and addtional data that we think is valuble that is used to populate the widget
```



6. Widget file

```swift
struct AnniversaryEntry: TimelineEntry {
	var date: Date
	let anniversary: Anniversary
}

struct Provider: TimelineProvider {
	func placeholder(in context: Context) -> AnniversaryEntry {
		<#code#>
	}
	
  // snapshot is a rendering of the widget that gets used when its being selected
  // put in dummy / realistic data
  // no heavy processing at all
	func getSnapshot(in context: Context, completion: @escaping (AnniversaryEntry) -> Void) {
		<#code#>
	}
	
	func getTimeline(in context: Context, completion: @escaping (Timeline<AnniversaryEntry>) -> Void) {
		<#code#>
	}
	
	typealias Entry = AnniversaryEntry
}

```



7. Add app group to widget

<img src="/Users/congle/Documents/Dev/ios-all-learnings/Images/image-20210612003215801.png" alt="image-20210612003215801" style="zoom:50%;" />



