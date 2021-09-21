```swift
struct CardModel: Hashable, Decodable {
  let title: String
  let subTitle: String
  let imageName: String
}

class DataProvider {
  let dataSubject = CurrentValueSubject<[CardModel], Never>([])

  func fetch() {
    let cards = (0..<20).map { i in
      CardModel(title: "Title \(i)", subTitle: "Subtitle \(i)", imageName: "image_\(i)")
    }

    dataSubject.value = cards
  }
}
```

```swift
class DataProvider {
  func fetch() -> AnyPublisher<[CardModel], Never> {
    let cards = (0..<20).map { i in
      CardModel(title: "Title \(i)", subTitle: "Subtitle \(i)", imageName: "image_\(i)")
    }

    return Just(cards).eraseToAnyPublisher()
  }
}
```



Alternitive 

```swift
class DataProvider {
  func fetch() -> AnyPublisher<[CardModel], Never> {
    let cards = (0..<20).map { i in
      CardModel(title: "Title \(i)", subTitle: "Subtitle \(i)", imageName: "image_\(i)")
    }

    return Just(cards).eraseToAnyPublisher()
  }
}
```

#### Just 

A Just publisher in Combine is a publisher that emits a single value and then immediately completes with success. This publisher never fails, and it always emits a single value. It’s perfect for an operation like fetch() where we perform work once, and then immediately consider the operation completed.

#### Infinite scrolling - not a great way to implement 

```swift
class DataProvider {
  let dataSubject = CurrentValueSubject<[CardModel], Never>([])

  var currentPage = 0
  var cancellables = Set<AnyCancellable>()

  func fetchNextPage() {
    currentPage += 1
    let url = URL(string: "https://myserver.com/page/\(currentPage)")!

    URLSession.shared.dataTaskPublisher(for: url)
      .sink(receiveCompletion: { _ in
        // handle completion
      }, receiveValue: { value in
        let jsonDecoder = JSONDecoder()
        if let models = try? jsonDecoder.decode([CardModel].self, from: value.data) {
          dataSubject.value += models
        }
      }).store(in: &cancellables)
  }
}
```

