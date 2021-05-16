https://blog.appnation.co/how-to-make-http-api-requests-with-only-using-urlsession-in-swift-2ab6bef44ec5



```swift
let url = URL(string: "api.myplatform.com/endpoint")!
var urlRequest = URLRequest(url: url)
urlRequest.httpMethod = "POST"
urlRequest.addValue("no-cache", forHTTPHeaderField: "cache-control")
URLSession.shared.dataTask(with: urlRequest) { (data, response, error) in
  // response handling
}.resume()

```



```swift
func buildURLRequest(hostname: String, path: String, parameters: [String:String] = [:], body: [String:String] = [:], method: String) -> URLRequest? {
  let urlComponents = NSURLComponents()
  urlComponents.scheme = "https"
  urlComponents.host = hostname
  urlComponents.path = path
  var queryItems: [URLQueryItem] =  []
  let queryParameters = parameters.map({ URLQueryItem(name: $0.key, value: $0.value) })
  queryItems.append(contentsOf: queryParameters)
  urlComponents.queryItems = queryItems
  guard let completeUrl = urlComponents.url else { return nil }
  var urlRequest = URLRequest(url: completeUrl, cachePolicy: .reloadIgnoringLocalAndRemoteCacheData, timeoutInterval: 30)
  urlRequest.httpMethod = method
  urlRequest.httpBody = body
  return urlRequest
}

```

