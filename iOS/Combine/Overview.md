#  Combine


<img src="/Images/image-20210922135611059.png" width=70%>
<img src="/Images/image-20210922135316901.png" width=70%>
<img src="/Images/image-20210922135121203.png" width=70%>
<img src="/Images/image-20210922134856763.png" width=70%>
<img src="/Images/image-20210922132244651.png" width=70%>
<img src="/Images/image-20210922132203922.png" width=70%>



### compactMap - Removes nils

```swift
var items: [String?] = ["Femi", "Pete", "Bryan", nil]

func filterNils(in items: [String?]) -> [String] {
    var myItems = [String]()
    for item in items {
        if let item = item {
            myItems.append(item)
        }
    }
    return myItems
}

print(filterNils(in: items))
print(items.compactMap({$0}))

```

```
Both print out

["Femi", "Pete", "Bryan"]
["Femi", "Pete", "Bryan"]
```



### Map

Map is used to create an array of the same size and you can mallipute the values to what ever you want

```swift
let numbers = [1, 2, 3, 4]
let doubled = numbers.map { $0 * 2 }

print(doubled)
[2, 4, 6, 8]
```



### Reacative programming

it is a pattern that listens to events and once a change happens the UI react to it
publishers (emit an event) and subscriptions (listen to the event)

<img src="/Images/Publisher and Subscription.png" width=50%>

### AnyCannellable

Holds subscriptions - When we want to listen to changes we need to store it somewhere. If we dont store it anywhere. It wont work. 



### Sink

When you want the final value out of the subscription 



### Example

<img src="/Images/Publisher and Subscription 2.png" width=50%>



### Combining publishers

```swift
let meals: Publishers.Sequence<[String?], Never> = ["🍔", "🌭", "🍕", nil].publisher
let people: Publishers.Sequence<[String?], Never> = ["Tunde", "Bob", "Toyo", "Jack"].publisher
```

Collection of objects that we will publis and listen to. 
Never - means there's no errors.
.publisher at the end turns the array into a publisher

#### Example 1

```swift
let meals: Publishers.Sequence<[String?], Never> = ["🍔", "🌭", "🍕", nil].publisher
let people: Publishers.Sequence<[String?], Never> = ["Tunde", "Bob", "Toyo", "Jack"].publisher

let subscription = people
	.zip(meals)
	.sink { completion in
		print("Subscription: \(completion)")
	} receiveValue: { (person, meal) in
		print("\(person) enjoys \(meals)")
	}
```

```swift
Optional("Tunde") enjoys Sequence<Array<Optional<String>>, Never>(sequence: [Optional("🍔"), Optional("🌭"), Optional("🍕"), nil])
Optional("Bob") enjoys Sequence<Array<Optional<String>>, Never>(sequence: [Optional("🍔"), Optional("🌭"), Optional("🍕"), nil])
Optional("Toyo") enjoys Sequence<Array<Optional<String>>, Never>(sequence: [Optional("🍔"), Optional("🌭"), Optional("🍕"), nil])
Optional("Jack") enjoys Sequence<Array<Optional<String>>, Never>(sequence: [Optional("🍔"), Optional("🌭"), Optional("🍕"), nil])
Subscription: finished
```

#### Example 2

with filter

```swift
let meals: Publishers.Sequence<[String?], Never> = ["🍔", "🌭", "🍕", nil].publisher
let people: Publishers.Sequence<[String?], Never> = ["Tunde", "Bob", "Toyo", "Jack"].publisher

let subscription = people
	.zip(meals)
	.filter({$0 != nil && $1 != nil})
	.sink { completion in
		print("Subscription: \(completion)")
	} receiveValue: { (person, meal) in
		print("\(person) enjoys \(meals)")
	}
```

```
Optional("Tunde") enjoys Sequence<Array<Optional<String>>, Never>(sequence: [Optional("🍔"), Optional("🌭"), Optional("🍕"), nil])
Optional("Bob") enjoys Sequence<Array<Optional<String>>, Never>(sequence: [Optional("🍔"), Optional("🌭"), Optional("🍕"), nil])
Optional("Toyo") enjoys Sequence<Array<Optional<String>>, Never>(sequence: [Optional("🍔"), Optional("🌭"), Optional("🍕"), nil])
Subscription: finished
```

It removes the nils 



#### Example  3 - try map

```swift
import UIKit
import Combine

/*
 * Combine two publishers and validate any nil values remove it
 *
 */

enum PersonError : Error {
	case emptyData
}

extension PersonError {
	
	public var errorDescription: String {
		switch self {
		case .emptyData:
			return "person is empty"
		}
	}
}

func validate(person: String?, meal: String?) throws -> String {
	
	guard let person = person,
		  let meal = meal else {
		throw PersonError.emptyData
	}
	
	return "\(person) enjoys \(meal)"
}

let meals: Publishers.Sequence<[String?], Never> = ["🍔", "🌭", "🍕", nil].publisher
let people: Publishers.Sequence<[String?], Never> = ["Tunde", "Bob", "Toyo", "Jack"].publisher

let subscription = people
	.zip(meals)
	.tryMap({ try validate(person: $0, meal: $1) })
	.sink { completion in
		switch completion {
		case .finished:
			print("finished")
		case .failure(let error as PersonError):
			print("Failed \(error.errorDescription)")
		case .failure(let error):
			print("Failed \(error.localizedDescription)")
		}
	} receiveValue: { message in
		print(message)
	}

```

```
Tunde enjoys 🍔
Bob enjoys 🌭
Toyo enjoys 🍕
Failed person is empty
```

<img src="/Images/image-20210916191717575.png" width=70%>
<img src="/Images/image-20210919195726312.png" width=70%>
<img src="/Images/image-20210919213914425.png" width=70%>



@publisher is subject value publisher with the type erased to anypublisher

<img src="/Images/image-20210922131355315.png" width=70%>
