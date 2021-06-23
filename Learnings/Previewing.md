# SwiftUI Previewing tips



For example, this creates a preview for `ContentView` that shows three different designs side by side: extra large text, dark mode, and a navigation view:

```swift
struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
            .environment(\.sizeCategory, .accessibilityExtraExtraExtraLarge)
        ContentView()
            .environment(\.colorScheme, .dark)
        NavigationView {
            ContentView()
        }
    }
}
```



#### Create custom modifiers

If you find yourself regularly repeating the same set of view modifiers – for example, making a text view have padding, be of a specific size, have fixed background and foreground colors, etc – then you should consider moving those to a custom modifier rather than repeating your code.

For example, this creates a new `PrimaryLabel` modifier that adds padding, a black background, white text, a large font, and some corner rounding:

```swift
struct PrimaryLabel: ViewModifier {
    func body(content: Content) -> some View {
        content
            .padding()
            .background(Color.black)
            .foregroundColor(.white)
            .font(.largeTitle)
            .cornerRadius(10)
    }
}
```

You can now attach that to any view using `.modifier(PrimaryLabel())`, like this:

```swift
struct ContentView: View {
    var body: some View {
        Text("Hello World")
            .modifier(PrimaryLabel())
    }
}
```

 

## Combine text views

You can create new text views out of several small ones using `+`, which is an easy way of creating more advanced formatting. For example, this creates three text views in different colors and combines them together:

```swift
struct ContentView: View {
    var body: some View {
        Text("Colored ")
            .foregroundColor(.red)
        +
        Text("SwifUI ")
            .foregroundColor(.green)
        +
        Text("Text")
            .foregroundColor(.blue)
    }
}
```

 

