# Add ads



package location - up to next major version version :  8.9.0
https://github.com/quanghits/GoogleMobileAds 

Create a new google ad 
https://apps.admob.com/v2/apps/create?pli=1

https://developers.google.com/admob/ios/quick-start





![image-20211017224245661](/Users/congle/Documents/Dev/ios-all-learnings/Images/image-20211017224245661.png)



​	**@IBOutlet** **weak** **var** banner: GADBannerView!

```swift
	private func configureBanner() {
		// test ca-app-pub-3940256099942544/2934735716
		// real ca-app-pub-4476487168042378/5164545893
		banner.adUnitID = "ca-app-pub-3940256099942544/2934735716"
		banner.rootViewController = self
		banner.load(GADRequest())
		banner.backgroundColor = .secondarySystemBackground
	}
```

