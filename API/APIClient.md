# Quickstart for HTTP Requests



## HTTPClient example

```swift
//
//  HTTPClient.swift
//  everyLife
//
//  Created by Cong Le on 13/05/2021.
//

import Foundation

enum HTTPErrors: Error {
	case parsingError
}

class HTTPClient {
	
	static var parameters: [String: Any] = [:
	]
	
	func get<T: Decodable>(url: String,
						   parameters: [String: Any]? = [String: Any](),
						   completionHandler: @escaping(Swift.Result<T, HTTPErrors>) -> Void) {
			
		let url = URL(string: url)!
		let task = URLSession.shared.dataTask(with: url, completionHandler: { (data, response, error) in
			if let error = error {
				print("Error with fetching: \(error)")
				return
			}
			
			guard let httpResponse = response as? HTTPURLResponse,
				  (200...299).contains(httpResponse.statusCode) else {
				print("Error with status code: \(String(describing: response))")
				return
			}
			
			if let data = data,
			   let decodedResponseData = try? JSONDecoder().decode(T.self, from: data) {
				print("decodedResponseData : \(decodedResponseData)")
				completionHandler(.success(decodedResponseData))
			}
		})
		task.resume()

	}
}


// Allows Data to be printed - For debugging purposes
extension Data {
	var prettyPrintedJSONString: NSString? {
		guard let object = try? JSONSerialization.jsonObject(with: self, options: []),
			  let data = try? JSONSerialization.data(withJSONObject: object, options: [.prettyPrinted]),
			  let prettyPrintedString = NSString(data: data, encoding: String.Encoding.utf8.rawValue) else { return nil }
		
		return prettyPrintedString
	}
}


```



## APIClass example

```swift
//
//  TaskAPI.swift
//  everyLife
//
//  Created by Cong Le on 13/05/2021.
//

import Foundation

class TaskAPI {
	static let shared = TaskAPI()
	
	private let httpClient = HTTPClient()
	private init() {}


	func fetchTasks(completionHandler: @escaping(Result<[Item], HTTPErrors>) -> Void) {

		httpClient.get(url: "https://adam-deleteme.s3.amazonaws.com/tasks.json",
					   completionHandler: completionHandler)

	}
}

```



## Response Example

```swift
struct Item: Decodable {
	let identifier: String
	let name: String
	let info: String
	let type: String

	private enum CodingKeys: String, CodingKey {
		case identifier = "id"
		case name = "name"
		case info = "description"
		case type = "type"
	}
}
```



## ViewModel Example

```swift
class ViewModel {
	
	var tasks: [Item] = [Item]()
	
	func fetchData() {
		let apiService = TaskAPI.shared
		
		apiService.fetchTasks() { [weak self] result in
			
			switch result {
			case .success(let response):
				self?.tasks = response
				print("result : \(response)")
			case .failure(let error):
				print("error.localizedDescription : \(error.localizedDescription)")
			}
		}
	}
}

```



## Makes Data pretty

```swift
extension Data {
	var prettyPrintedJSONString: NSString? {
		guard let object = try? JSONSerialization.jsonObject(with: self, options: []),
			  let data = try? JSONSerialization.data(withJSONObject: object, options: [.prettyPrinted]),
			  let prettyPrintedString = NSString(data: data, encoding: String.Encoding.utf8.rawValue) else { return nil }
		
		return prettyPrintedString
	}
}
```

