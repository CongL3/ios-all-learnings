# Combine networking



```swift
struct APIError: Decodable, Error {
  let statusCode: Int
}
```

```swift
func refreshToken() -> AnyPublisher<Bool, Never> {
  // normally you'd have your refresh logic here
  return Just(false).eraseToAnyPublisher()
}
```

```swift
func fetchURL<T: Decodable>(_ url: URL) -> AnyPublisher<T, Error> {
  URLSession.shared.dataTaskPublisher(for: url)
    .tryMap({ result in
      let decoder = JSONDecoder()
      guard let urlResponse = result.response as? HTTPURLResponse, (200...299).contains(urlResponse.statusCode) else {
        let apiError = try decoder.decode(APIError.self, from: result.data)
        throw apiError
      }

      return try decoder.decode(T.self, from: result.data)
    })
    .tryCatch({ error -> AnyPublisher<T, Error> in
      guard let apiError = error as? APIError, apiError.statusCode == 401 else {
        throw error
      }

      return refreshToken()
        .tryMap({ success -> AnyPublisher<T, Error> in
          guard success else { throw error }

          return fetchURL(url)
        }).switchToLatest().eraseToAnyPublisher()
    })
    .eraseToAnyPublisher()
```

