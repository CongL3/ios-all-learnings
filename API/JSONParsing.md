# Parsing JSON into a struct

1:1 example. All varibles need to have a coding key.

```swift
public typealias Codable = Decodable & Encodable
```

```swift
struct Flim : Codable {
	let title : String
	let year : String
	let rated : String
	let released : String
	let identifier: Int

	enum CodingKeys: String, CodingKey {
		case title = "Title"
		case year = "Year"
		case rated = "Rated"
		case released = "Released"
		case identifier = "id"
	}
}
```

## JSON file example

Check property types of the json values.

```
{
  "Title": "Guardians of the Galaxy Vol. 2",
  "Year": "2017",
  "Rated": "PG-13",
  "Released": "05 May 2017",
  "Runtime": "136 min",
}
```

## Parse into a struct

```swift
	func readLocalFile(forName name: String) -> Data? {
		if let path = Bundle.main.path(forResource: name, ofType: "json") {
			do {
				let data = try Data(contentsOf: URL(fileURLWithPath: path), options: .mappedIfSafe)
				let decoder = JSONDecoder()
				if let film = try? decoder.decode(Film.self, from: data) {
					print(film)
				}
			} catch {
			}
		}
		
		return nil
	}
```

## Custom format - Date

```json
{
    "title": "Optionals in Swift explained: 5 things you should know",
    "date": "2019-10-21T09:15:00Z"
}
```

```swift
struct BlogPost: Decodable {
    let title: String
    let date: Date
}

let decoder = JSONDecoder()
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "yyyy-MM-dd'T'HH:mm:ssZ"
dateFormatter.locale = Locale(identifier: "en_US")
dateFormatter.timeZone = TimeZone(secondsFromGMT: 0)
decoder.dateDecodingStrategy = .formatted(dateFormatter)

let blogPost: BlogPost = try! decoder.decode(BlogPost.self, from: jsonData)
print(blogPost.date) // Prints: 2019-10-21 09:15:00 +0000

```

Reference : https://www.avanderlee.com/swift/json-parsing-decoding/