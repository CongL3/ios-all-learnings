```swift
	@State private var modelState: ModelState?


enum ModelState: Hashable, Identifiable {
	case title, date, nameOne, nameTwo
	var id: ModelState { self }
}


HStack{
	Text("Temp")
}
.sheet(item: $modelState) { item in
  switch item {
  case .date:
    DatePicker("", selection: $selectedDate, displayedComponents: .date)
      .datePickerStyle(WheelDatePickerStyle())
      .padding()
      .onDisappear {
        self.user.profile!.anniversaryDate = selectedDate
      }

  case .title:
    Text("Text")
  case .nameOne:
    //						TextField("Enter name", text: $personOneName)
    //							.padding()
    DatePicker("", selection: $selectedDate, displayedComponents: .date)
      .datePickerStyle(WheelDatePickerStyle())
      .padding()
      .onDisappear {
        self.user.profile!.anniversaryDate = selectedDate
      }

  case .nameTwo:
    TextField("Enter name", text: $personTwoName)
      .padding()

  }
}

```

