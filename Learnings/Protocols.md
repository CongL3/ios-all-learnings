Swift has matured significantly in the years since this answer was written. [The design guidelines now state](https://swift.org/documentation/api-design-guidelines/#protocols-describing-what-is-should-read-as-nouns):

> - Protocols that describe *what something* is should read as nouns (e.g. `Collection`).
> - Protocols that describe a *capability* should be named using the suffixes `able`, `ible`, or `ing` (e.g. `Equatable`, `ProgressReporting`).

Thank you to [David James](https://softwareengineering.stackexchange.com/users/188944/david-james) for spotting this!



### Protocol Extensions

“Don’t start with a class, start with a protocol.”  
Why? Protocols serve as better abstractions than classes.



#### Superclass example vs Protocol example

```swift
class Image {
    fileprivate var imageName: String
    fileprivate var imageData: Data

    var name: String {
        return imageName
    }

    init(name: String, data: Data) {
        imageName = name
        imageData = data
    }

    // persistence
    func save(to url: URL) throws {
        try self.imageData.write(to: url)
    }

    convenience init(name: String, contentsOf url: URL) throws {
        let data = try Data(contentsOf: url)
        self.init(name: name, data: data)
    }

    // compression
    convenience init?(named name: String, data: Data, compressionQuality: Double) {
        guard let image = UIImage.init(data: data) else { return nil }
        guard let jpegData = UIImageJPEGRepresentation(image, CGFloat(compressionQuality)) else { return nil }
        self.init(name: name, data: jpegData)
    }

    // BASE64 encoding
    var base64Encoded: String {
        return imageData.base64EncodedString()
    }
}

// Test
var image = Image(name: "Pic", data: Data(repeating: 0, count: 100))
print(image.base64Encoded)

do {
    // persist image
    let documentDirectory = try FileManager.default.url(for: .documentDirectory, in: .userDomainMask, appropriateFor:nil, create:false)
    let imageURL = documentDirectory.appendingPathComponent("MyImage")
    try image.save(to: imageURL)
    print("Image saved successfully to path \(imageURL)")

    // load image from persistence
    let storedImage = try Image.init(name: "MyRestoredImage", contentsOf: imageURL)
    print("Image loaded successfully from path \(imageURL)")
} catch {
    print(error)
}
```



```swift
protocol NamedImageData {
    var name: String { get }
    var data: Data { get }
    init(name: String, data: Data)
}

protocol ImageDataPersisting: NamedImageData {
    init(name: String, contentsOf url: URL) throws
    func save(to url: URL) throws
}

extension ImageDataPersisting {
    init(name: String, contentsOf url: URL) throws {
        let data = try Data(contentsOf: url)
        self.init(name: name, data: data)
    }

    func save(to url: URL) throws {
        try self.data.write(to: url)
    }
}

protocol ImageDataCompressing: NamedImageData {
    func compress(withQuality compressionQuality: Double) -> Self?
}

extension ImageDataCompressing {
    func compress(withQuality compressionQuality: Double) -> Self? {
        guard let uiImage = UIImage.init(data: self.data) else {
            return nil
        }
        guard let jpegData = UIImageJPEGRepresentation(uiImage, CGFloat(compressionQuality)) else {
            return nil
        }
        return Self(name: self.name, data: jpegData)
    }
}

protocol ImageDataEncoding: NamedImageData {
    var base64Encoded: String { get }
}

extension ImageDataEncoding {
    var base64Encoded: String {
        return self.data.base64EncodedString()
    }
}
```



If you want to adopt parts of implementatio then protocol is the way to go.



