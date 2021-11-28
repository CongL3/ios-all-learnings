## SwiftUI data

Use @ObservedObject to observe from multiple places



Textfield length validation

```swift
  .onReceive(Just(viewModel.editText)) { inputValue in
    if inputValue.count > 20 {
      self.viewModel.editText.removeLast()
    }
  }
```



becomesFirstResponder on appear

```swift
struct NameForm: View {
    
	@FocusState private var isTextFieldFocused: Bool
    
    @State private var name = ""
    
    var body: some View {
      HStack {
				TextField("\(viewModel.editText)", text: $viewModel.editText)
					.focused($isTextFieldFocused)
        
      }
      		.onAppear {
			DispatchQueue.main.asyncAfter(deadline: .now() + 0.5) {  /// Anything over 0.5 seems to work
				isTextFieldFocused = true
			}

    }
}
  
  // TextFieldEntryView in anniversary
```



Sheet capsule

```swift
		VStack(alignment: .center) {
			Capsule()
				.fill(Color.secondary)
				.frame(width: 80, height: 3)
				.padding(10)

		}
```



Multisheet single view

```swift
	enum ActiveSheet: Identifiable {
		case nameOne, nameTwo, title
		
		var id: Int {
			hashValue
		}
	}
	@State var activeSheet: ActiveSheet?

	var body: some View {
		VStack {

							Button(action: {
								activeSheet = .title
							}, label: {
								Text("\(titleTextFieldViewModel.text)")
								
							})
								.foregroundColor(.pink)
    
    							Button(action: {
								activeSheet = .nameOne
							}, label: {
								Text("\(nameOneTextFieldViewModel.text)")
									.background(.green)
								
							})

				}

    				.sheet(item: $activeSheet) { item in
					switch item {
					case .title:
						TextFieldEntryView(viewModel: titleTextFieldViewModel)
					case .nameOne:
						TextFieldEntryView(viewModel: nameOneTextFieldViewModel)
					case .nameTwo:
						TextFieldEntryView(viewModel: nameTwoTextFieldViewModel)
					}
					
				}

  }

```



@Stateobject

