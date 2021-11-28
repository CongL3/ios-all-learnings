# Pratical Combine notes

Functional Reactive Programming framework

A function that takes another function or closure as its parameter is called a higher-order function in Functional Programming. A function that only operates on the arguments it receives is called a pure function.

FRP can increase code readability and it reduces code complexity dramatically
FRP’s most significant drawback is that it tends to have quite a steep learning curve.



Without Combine

```swift
“let myUrl = URL(string: "https://www.donnywals.com")!

func requestData(_ completion: @escaping (Result<Data, Error>) -> Void) {
  URLSession.shared.dataTask(with: myUrl) { data, response, error in
    if let error = error {
      completion(.failure(error))
      return
    }

    guard let data = data else {
      preconditionFailure("If there is no error, data should be present...")
    }

    completion(.success(data))
  }.resume()
}

```



With Combine

```swift
“let myUrl = URL(string: "https://www.donnywals.com")!

func requestData() -> AnyPublisher<Data, URLError> {
  URLSession.shared.dataTaskPublisher(for: myUrl)
    .map(\.data)
    .eraseToAnyPublisher()
}
```



Overall, I think Combine is probably a better choice for the sole reason that it’s Apple’s FRP framework. It’s tightly integrated with some of Apple’s existing APIs and it’s the driving force behind SwiftUI

The completion event that is sent to the receiveCompletion closure has Subscribers.Completion<Self.Failure> as its type. This event is very similar to Swift’s Result type, except its success case is simply .finished without an associated value.



```swift
“[1, 2, 3].publisher.sink(receiveCompletion: { completion in
  switch completion {
  case .finished:
    print("finished succesfully")
  case .failure(let error):
    print(error)
  }
}, receiveValue: { value in
  print("received a value: \(value)")
})”
	
```



In fact, sink comes with a special flavor for publishers that have Never as their Failure type. It allows us to omit the receiveCompletion closure:

```swift
[1, 2, 3].publisher.sink(receiveValue: { value in
  print("received a value: \(value)")
})”

```



In Combine, a publisher that doesn’t have a subscriber will not emit any values. This means that publishers won’t perform any work unless they have a subscriber to send the result of their work to. This is a powerful feature of Combine, and it’s one of Combine’s core principles.

Every subscription can be wrapped in an AnyCancellable object that tears down its subscriber when it’s deallocated ????



## Operators

A function like map that wraps a publisher into another publisher is called an operator, and I will try to refer to them as operators where possible.

```swift
“let dataTaskPublisher = URLSession.shared.dataTaskPublisher(for: someURL)
  .retry(1)
  .map({ $0.data })
  .decode(type: User.self, decoder: JSONDecoder())
  .map({ $0.name })
  .replaceError(with: "unkown")
  .assign(to: \.text, on: userNameLabel)
```

- The preceding code goes through the following steps.
- Make a request to someURL.
- Retry the request once if it fails.
- Grab data from the network response.
- Decode the data into a model of type User.
- Grab the user’s name.
- Replace any errors that we’ve encountered along the way with the string “unknown”.
- Set the obtained string to a label’s text.”



compactMap omits all nils - All nil values are dropped as expected.



### FlatMap

Since we shouldn’t hide any URLErrors that are emitted by the data tasks, we need to change the sequence publisher’s error type to match URLError. We can do this with the setFailureType operator. This operator creates a new publisher with an unchanged Output, but it changes the Failure to the error type you supply:

```swift
let baseURL = URL(string: "https://www.donnywals.com")!
var cancellables = Set<AnyCancellable>()

["/", "/the-blog", "/speaking", "/newsletter"].publisher
	.setFailureType(to: URLError.self)
	.flatMap({ path -> URLSession.DataTaskPublisher in
		let url = baseURL.appendingPathComponent(path)
		return URLSession.shared.dataTaskPublisher(for: url)
	})
	.sink(receiveCompletion: { completion in
		print("Completed with: \(completion)")
	}, receiveValue: { result in
		print(result)
	}).store(in: &cancellables)

```



### Map and FlatMap

```swift
let numbers = [1, 2, 3, 4]

let mapped = numbers.map { Array(repeating: $0, count: $0) }
// [[1], [2, 2], [3, 3, 3], [4, 4, 4, 4]]

let flatMapped = numbers.flatMap { Array(repeating: $0, count: $0) }
// [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]
```



### Errors



```swift
enum MyError: Error {
  case outOfBounds
}

[1, 2, 3].publisher
  .tryMap({ int in
    guard int < 3 else {
      throw MyError.outOfBounds
    }

    return int * 2
  })
  .sink(receiveCompletion: { completion in
    print(completion)
  }, receiveValue: { val in
    print(val)
  })
```





### Custom Operator

```swift
 extension Publisher where Output == String, Failure == Never {
  func toURLSessionDataTask(baseURL: URL) -> AnyPublisher<URLSession.DataTaskPublisher.Output, URLError> {
    return self
	.setFailureType(to: URLError.self)
      .flatMap({ path -> URLSession.DataTaskPublisher in
        let url = baseURL.appendingPathComponent(path)
        return URLSession.shared.dataTaskPublisher(for: url)
      })
      .eraseToAnyPublisher()
  }
}
```



in use

```swift
var baseURL = URL(string: "https://www.donnywals.com")!

["/", "/the-blog", "/speaking", "/newsletter"].publisher
  .toURLSessionDataTask(baseURL: baseURL)
  .sink(receiveCompletion: { completion in
    print("Completed with: \(completion)")
  }, receiveValue: { result in
    print(result)
  }).store(in: &cancellables)
```





Along the way, I introduced you to several other operators like setFailureType, replaceNil and replaceError. You saw that all operators in Combine work in a similar manner. They take a publisher’s output and/or error and return a new publisher with a modified output and/or error.



When a publisher emits an error, the stream is considered completed and it can’t emit any new values.



### Connecting to your UI

##### Using a PassthroughSubject to send a stream of values.


Example:

```swift
var cancellables = Set<AnyCancellable>()

let notificationCenter = NotificationCenter.default
let notificationName = UIResponder.keyboardWillShowNotification
let publisher = notificationCenter.publisher(for: notificationName)

publisher
  .sink(receiveValue: { notification in
    print(notification)
  }).store(in: &cancellables)

notificationCenter.post(Notification(name: notificationName))
```

​	Rewritten with PassthroughSubject

```swift
var cancellables = Set<AnyCancellable>()

let notificationSubject = PassthroughSubject<Notification, Never>()
let notificationName = UIResponder.keyboardWillShowNotification
let notificationCenter = NotificationCenter.default

notificationCenter.addObserver(forName: notificationName, object: nil, queue: nil) { notification in
  notificationSubject.send(notification)
}

notificationSubject
  .sink(receiveValue: { notification in
    print(notification)
  }).store(in: &cancellables)

notificationCenter.post(Notification(name: notificationName))
```

Send something to it and it will send it to all subscribers





Using a CurrentValueSubject to represent a stateful stream of values.

```swift
class Car {
  var onBatteryChargeChanged: ((Double) -> Void)?
  var kwhInBattery = 50.0 {
    didSet {
      onBatteryChargeChanged?(kwhInBattery)
    }
  }

  let kwhPerKilometer = 0.14

  func drive(kilometers: Double) {
    let kwhNeeded = kilometers * kwhPerKilometer
    assert(kwhNeeded <= kwhInBattery, "Can't make trip, not enough char	ge in battery")

    kwhInBattery -= kwhNeeded
  }
}

////

let car = Car()

someLabel.text = "The car now has \(car.kwhInBattery)kwh in its battery"

car.onBatteryChargeChanged = { newCharge in
  someLabel.text = "The car now has \(newCharge)kwh in its battery"
}
```

#### Now in combine 

```swift
class Car {
  var kwhInBattery = CurrentValueSubject<Double, Never>(50.0)
  let kwhPerKilometer = 0.14

  func drive(kilometers: Double) {
    let kwhNeeded = kilometers * kwhPerKilometer

    assert(kwhNeeded <= kwhInBattery.value, "Can't make trip, not enough charge in battery")
    
    “kwhInBattery.value -= kwhNeeded
  }
}

let car = Car()
// don't forget to store the AnyCancellable if you're using this in a real app
car.kwhInBattery
  .sink(receiveValue: { newCharge in
    someLabel.text = "The car now has \(newCharge)kwh in its battery"
  })


car.drive(kilometers: 100) // label will now show that there's 36kwh remaining”

```

No need to use send(_:) as it's a subject 

Also, note that we don’t read the initial value of kwhInBattery to configure the label. That’s not a mistake. A CurrentValueSubject immediately sends its current value to any new subscribers

Wrapping properties with the @Published property wrapper to turn them into publishers.

As you’ll see throughout this chapter, CurrentValueSubject is a fantastic fit to expose a model’s properties through a reactive interface. There is one more very special way to publish property values in Combine by using the @Published property wrapper.



### @Published

```swift
class Car {
  @Published var kwhInBattery = 50.0
  let kwhPerKilometer = 0.14

  func drive(kilometers: Double) {
    let kwhNeeded = kilometers * kwhPerKilometer

    assert(kwhNeeded <= kwhInBattery, "Can't make trip, not enough charge in battery")
    
        kwhInBattery -= kwhNeeded
  }
}

let car = Car()
// don't forget to store the AnyCancellable if you're using this in a real app
car.$kwhInBattery
  .sink(receiveValue: { newCharge in
    someLabel.text = "The car now has \(newCharge)kwh in its battery"
  })
```

However, because kwhInBattery now refers to the underlying Double value, we can’t subscribe to it directly.
To subscribe to an @Published property’s changes, you need to use a $ prefix. This is a special convention for property wrappers that allows you to access the wrapper itself, also known as a projected value rather than the value that is wrapped by the property. In this case, the wrapper’s projected value is a publisher so we can subscribe to the $kwhInBattery property

#### Choosing the appropriate mechanism to publish information

Which publishing mechanism is best for you depends on several factors. If you’re publishing a stream of events without a concept of state, you’re probably looking for a PassthroughSubject. These are not commonly found on models, and they are better suited for publishing a stream of user interactions, or as I’ve shown, an event stream like NotificationCenter emits. If you have a model that’s defined as a struct, you can expose new data through a CurrentValueSubject. This allows you to continuously send new values to subscribers while keeping a concept of the current state, or value. And lastly, if you have a class and you want to have similar behavior to a CurrentValueSubject except for not being to end the stream of values if needed, the @Published property wrapper is likely to fit your needs perfectly.



The assign(to: on:) operator in Combine is similar to sink. It creates a new Subscriber, and it returns an AnyCancellable that we must retain to keep the subscription alive



#### “Directly assigning the output of a publisher with assign(to:on:)

In the previous section, I’ve shown you the following code:

```swift
class Car {
  @Published var kwhInBattery = 50.0
  let kwhPerKilometer = 0.14

  func drive(kilometers: Double) {
    let kwhNeeded = kilometers * kwhPerKilometer

    assert(kwhNeeded <= kwhInBattery, "Can't make trip, not enough charge in battery")
    
    kwhInBattery -= kwhNeeded

  }
}

let car = Car()
// don't forget to store the AnyCancellable if you're using this in a real app
car.$kwhInBattery
  .sink(receiveValue: { newCharge in
    someLabel.text = "The car now has \(newCharge)kwh in its battery"
  })
```


The text property of someLabel in this example is always set to a new string that reflects the car’s current battery status. Let’s expand this example a little bit and introduce a view controller and a view model:

```swift
class Car {
  @Published var kwhInBattery = 50.0

 let kwhPerKilometer = 0.14
}

struct CarViewModel {
  var car: Car

  mutating func drive(kilometers: Double) {
    let kwhNeeded = kilometers * car.kwhPerKilometer

    assert(kwhNeeded <= car.kwhInBattery, "Can't make trip, not enough charge in battery")

    car.kwhInBattery -= kwhNeeded
  }
}

class CarStatusViewController {
  let label = UILabel()
  let button = UIButton()
  var viewModel: CarViewModel
  var cancellables = Set<AnyCancellable>()

  init(viewModel: CarViewModel) {
    self.viewModel = viewModel
  }

  // setup code goes here

  func setupLabel() {
    // label setup will go here
  }

  func buttonTapped() {
    viewModel.drive(kilometers: 10)
  }
}
```

The VM has to listen to the @publish and and expose and format to the label

```swift
lazy var batterySubject: AnyPublisher<String?, Never> = {
  return car.$kwhInBattery.map({ newCharge in
    return "The car now has \(newCharge)kwh in its battery"
  }).eraseToAnyPublisher()
}()
```

