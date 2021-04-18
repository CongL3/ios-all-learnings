# Parsing a json

Parsing JSON into a struct :

1:1 example. All varibles need to have a coding key.

```swift
struct Flim : Codable {
	var title : String
	var year : String
	var rated : String
	var released : String
	
	enum CodingKeys: String, CodingKey {
		case title = "Title"
		case year = "Year"
		case rated = "Rated"
		case released = "Released"
	}
}
```

JSON file

```
{
  "Title": "Guardians of the Galaxy Vol. 2",
  "Year": "2017",
  "Rated": "PG-13",
  "Released": "05 May 2017",
  "Runtime": "136 min",
}
```

Parse into a struct

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

