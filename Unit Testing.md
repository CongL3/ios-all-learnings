# Test Doubles

### Dummies

Only act as a placeholder. Dummy objects are used to satisfy method parameters. Essentially we use dummies to make the complier stop crying.

```swift
// Logger interface definition
protocol Logger {
    func log(message: String)
}

// Logger real implementation
class PrintLogger: Logger {

    func log(message: String) {
        print(message)
    }
}

// Dummy logger will only sit there to fill the dependency
class DummyLogger: Logger {
    
    // Will do nothing.
    func log(message: String) {
        // Nothing ...
    }
}
```

As a general rule, a dummy should do nothing. and return as close to nothing as possible.



### Fakes

A fake is an object that has working implementations that replicate the behavior***,\*** but not the same as the production one. Simple implementation of the production object. A fake is a simplified implementation of a more complex dependency. We usually use it when the SUT depends on another component which holds states.

Let’s say you have a database dependency. obviously we will not be using a real database for our unit tests. then we can create a fake database that will store data in an array to simulated a database under unit tests



```swift
class FakeUserDefaults: UserDefaults {

    // A dictionary we will use to store during uni tests
    private var fakeStorage: [String: Any] = [:]
    
    override func object(forKey defaultName: String) -> Any? {
            return fakeStorage[defaultName]
    }
    
    // Override the real set method to fake user default behavior
    override func set(_ value: Any?, forKey defaultName: String) {
            fakeStorage[defaultName] = value
    }
}
```



### Stubs

Stub can provide “canned responses”.

```swift
struct Notification {
    let id: String
    let title: String
}

protocol NotificationGetter {
    func getNotification(completion: (([Notification])-> Void))
}

class NotificationStub: NotificationGetter {
    private let notificaitons: [Notification]
    
    init(notifications: [Notification]) {
        self.notificaitons = notifications
    }
    
    func getNotification(completion: (([Notification]) -> Void)) {
        completion(notificaitons)
    }
}

func testNotification() {
    let notificationStub = NotificationStub(notifications: [Notification(id: "1", title: "Title 1")])
    notificationStub.getNotification { (notification) in
        // expectations on the received notifications
    }
}	
```



### Mocks

Mocks are objects that register calls they receive. They also track which method being called and how many times it was called. Mock object types are good for verifying exact behavior.



```swift
struct Notification {
    let id: String
    let title: String
}

protocol NotificationGetter {
    func getNotification(completion: (([Notification])-> Void))
}

class NotificationMock: NotificationGetter {
    var getNotificationCalled = false
    var getNotificationCounter = 0
    func getNotification(completion: (([Notification]) -> Void)) {
        getNotificationCalled = true
        getNotificationCounter += 1
    }
}

```



### Spy

Spy records interactions and “verify” later. Spies are very similar to mocks and the opposite of stubs. When your code under test performs a side effect based on a dependency, you can use spies to record the effect.

If we want to test whether notification preference is enabled or not, we will write the code as below.

```swift
protocol NotificationSettings {
    var darkModeEnabled: Bool? {get set}
}

class Settings: NotificationSettings {
    var darkModeEnabled: Bool?
    
    func set(notificaitonEnabled enable: Bool) {
        darkModeEnabled = enable
    }
}

func testSettingDarkMode() {
    let settings = Settings()
    settings.set(notificaitonEnabled: true)
    XCTAssert(settings.darkModeEnabled == true)
}
```

Spy only gets out the values we need for our tests. A mock also verifies that those values matches what is expected.



### Mock or Spy ?

A **Mock should be the option to go for rather than Spies**. we only use a spy when it’s not possible or complicated to use a mock. Or for instance your code core needs some refactoring to be cleanly testable. then you can use spies. Or let’s say you have a function callback as input for your SUT. and it’s not possible to mock it with the framework you are using.





