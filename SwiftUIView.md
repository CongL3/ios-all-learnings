# Initialise values in SwiftUI

```swift
https://stackoverflow.com/questions/58758370/how-could-i-initialize-the-state-variable-in-the-init-function-in-swiftui

import SwiftUI

struct MyView: View {
    @State var mapState: Int

    init(inputMapState: Int)
    {
        _mapState = /*State<Int>*/.init(initialValue: inputMapState)
    }

    var body: some View {
        Text("Hello World!")
    }
}
```