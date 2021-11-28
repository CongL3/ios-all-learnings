# App States



- Not running
- Active
- Inactive
  - Inactive state is a brief state appearing while the app is leaving or entering the active state. While inactive, the app is updating the UI but it is not receiving user touches.
- Background
  - This state is triggered by the `application:didEnterBackground` callback. This state usually lasts about 10 seconds unless more time was requested from the system, in which case the app gets 180 seconds of background. VoIP apps can have longer background state. This state can be used for:
- Suspended



<img src="/Users/congle/Documents/Dev/ios-all-learnings/Images/ios-states-AppState-Apple.png" alt="ios-states-AppState-Apple" style="zoom: 50%;" />

- applicationWillTerminate
- applicationWillResignActive
- applicationWillBecomeActive
- applicationDidEnterBackground



<img src="/Users/congle/Documents/Dev/ios-all-learnings/Images/ios-states-AppStates.png" alt="ios-states-AppStates"  />

https://blog.codecentric.de/en/2016/07/handling-ios-app-states-state-machine/



