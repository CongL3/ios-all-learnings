### Combine 2 way binding

##### With

```swift
class ViewController: UIViewController {
  let slider = UISlider()
  let label = UILabel()
  
  @Published var sliderValue: Float = 50

  var cancellables = Set<AnyCancellable>()

  override func viewDidLoad() {
    super.viewDidLoad()

    $sliderValue
      .map({ value in
        "Slider is at \(value)"
      })
      .assign(to: \.text, on: label)
      .store(in: &cancellables)

    $sliderValue
      .assign(to: \.value, on: slider)
      .store(in: &cancellables)

    slider.addTarget(self, action: #selector(updateLabel), for: .valueChanged)
  }

  @objc func updateLabel() {
    sliderValue = slider.value
  }
}
```



##### Old way

```swift
class ViewController: UIViewController {

  let slider = UISlider()
  let label = UILabel()

  var sliderValue: Float = 50 {
    didSet {
      slider.value = sliderValue
      label.text = "Slider is at \(sliderValue)"
    }
  }

  override func viewDidLoad() {
    super.viewDidLoad()

    // a bunch of setup code

    slider.addTarget(self, action: #selector(updateLabel), for: .valueChanged)
  }

  @objc func updateLabel() {
    sliderValue = slider.value
  }
}
```



##### swiftui version

````swift
struct ExampleView: View {
  @State private var sliderValue: Float = 50

  var body: some View {
    VStack {
      Text("Slider is at \(sliderValue)")
      Slider(value: $sliderValue, in: (1...100))
    }
  }
}
````



@State

In SwiftUI, properties that are marked with @State trigger UI updates when they change. They can also be used to create bindings between a UI element, and your app’s state like this example shows. I don’t know whether @State uses Combine internally, or whether it uses some other mechanism to update the view when the value of the wrapped property changes. What I do know, is that Apple seems to have decided that the kind of binding I just demonstrated is important for SwiftUI and that it’s not as important in UIKit.
