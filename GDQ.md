# GDQ



#### DispatchQoS.QoSClass

Use quality-of-service classes to communicate the intent behind the work that your app performs. The system uses those intentions to determine the best way to execute your tasks given the available resources



[`case userInteractive`](https://developer.apple.com/documentation/dispatch/dispatchqos/qosclass/userinteractive)

The quality-of-service class for user-interactive tasks, such as animations, event handling, or updating your app's user interface.

[`case userInitiated`](https://developer.apple.com/documentation/dispatch/dispatchqos/qosclass/userinitiated)

The quality-of-service class for tasks that prevent the user from actively using your app.

[`case `default``](https://developer.apple.com/documentation/dispatch/dispatchqos/qosclass/default)

The default quality-of-service class.

[`case utility`](https://developer.apple.com/documentation/dispatch/dispatchqos/qosclass/utility)

The quality-of-service class for tasks that the user does not track actively.

[`case background`](https://developer.apple.com/documentation/dispatch/dispatchqos/qosclass/background)

The quality-of-service class for maintenance or cleanup tasks that you create.

[`case unspecified`](https://developer.apple.com/documentation/dispatch/dispatchqos/qosclass/unspecified)

The absence of a quality-of-service class.

