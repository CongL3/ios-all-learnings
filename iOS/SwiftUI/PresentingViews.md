# Presenting new views



```swift
struct SheetView: View {
    @Environment(\.presentationMode) var presentationMode

    var body: some View {
        Button("Press to dismiss") {
            presentationMode.wrappedValue.dismiss()
        }
        .font(.title)
        .padding()
        .background(Color.black)
    }
}

struct ContentView: View {
    @State private var showingSheet = false

    var body: some View {
        Button("Show Sheet") {
            showingSheet.toggle()
        }
        .sheet(isPresented: $showingSheet) {
            SheetView()
        }
    }
}
```


"Dismiss view controller"

```swift
import SwiftUI

struct GameView: View {
    
    @Environment(\.presentationMode) var presentation
    
    var body: some View {
        Button("Done") {
            self.presentation.wrappedValue.dismiss()
        }
    }
}
```
