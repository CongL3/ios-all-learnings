## URLSession

```swift
extension Networking {
  func fetch<T: Decodable>(_ endpoint: Endpoint, completion: @escaping (Result<T, Error>) -> Void) {
    let urlRequest = endpoint.urlRequest

    URLSession.shared.dataTask(with: urlRequest) { data, response, error in
      do {
        if let error = error {
          completion(.failure(error))
          return
        }

        guard let data = data else {
          preconditionFailure("No error was received but we also don't have data...")
        }

        let decodedObject = try JSONDecoder().decode(T.self, from: data)

        completion(.success(decodedObject))
      } catch {
        completion(.failure(error))
      }
    }.resume()
  }
}

```

https://www.donnywals.com/architecting-a-robust-networking-layer-with-protocols/



