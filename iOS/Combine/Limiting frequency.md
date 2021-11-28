## Limiting frequency



```swift
class DebounceViewController: UIViewController {
  let textField = UITextField()
  let label = UILabel()

  @Published var searchQuery: String?

  var cancellables = Set<AnyCancellable>()

  override func viewDidLoad() {
    super.viewDidLoad()

    // a bunch of setup code
		textField.addTarget(self, action: #selector(textChanged), for: .editingChanged)
    
    $searchQuery
      .debounce(for: 0.3, scheduler: DispatchQueue.main)
      .filter({ ($0 ?? "").count > 2 })
      .removeDuplicates()
      .print()
      .assign(to: \.text, on: label)
      .store(in: &cancellables)
  }

  @objc func textChanged() {
    searchQuery = textField.text
  }
}

    
```



#### Throttle

While throttle is somewhat similar to debounce, they serve very different purposes and are used in very different ways. When you apply a throttle, all you’re guaranteeing is that a publisher will not emit values more frequently than specified in your throttle.

I can imagine that throttling is useful for features where you might want to update a value regularly based on user input. Like for instance, the word count of a document might not need updating all the time. You might consider once every couple of seconds to be good enough regardless of whether the user is still in the middle of typing or not.
