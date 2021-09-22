## Builder

#### function 

```swift
final class SportCarBuilder {

    private var wheel: Wheel = Wheel(type: .racing(tyre20))
    private var color: Color = Color(.red)

    func withColor(color: UIColor) {
      self.color = Color(color) 
    }

        func withWheel(type: WheelType) {
      self.wheel = Wheel(type: type)
    }

        func build() -> SportCar {
        return SportCar(wheel: wheel, color: color)
    }
}

final class SportCar: Car {
  init(wheel: Wheel, color: Color) {
    super.init(doors: Door(2), engine: Engine(V85000), wheel: wheel, color: color)
  }
}
```

#### Block 

```swift
class SportCar {
  typealias Builder = (builder: SportCarBuilder) -> Void

  private init(builder: SportCarBuilder) {
    super.init(doors: Door(2), engine: Engine(V85000), wheel: builder.wheel, color: builder.color)
  }

  static func make(with builderBlock: Builder) -> SportCar {
    let builder = SportCarBuilder()
    builderBlock(builder)
    return SportCar(builder: builder)
  }
}

let sportCar = SportCar.make { builder in
  builder.color = .red
  builder.wheel = .racing
}
```

#### constructor 

```swift
// This one is a SportCarBuilder for building a sport car
// We just need two properties of wheel and color that can be added and modified
// However for engine and door has been defined by SportCar class
final class SportCarBuilder {

    private var wheel: Wheel = Wheel(type: .racing(tyre20))
    private var color: Color = Color(.red)

    func withColor(color: UIColor) {
      self.color = Color(color) 
    }

        func withWheel(type: WheelType) {
      self.wheel = Wheel(type: type)
    }

        func build() -> SportCar {
        return SportCar(wheel: wheel, color: color)
    }
}

// This one is a SportCar class that subclass of the Car base class
// Car base class has 4 properties, this class will initialize through the base class with designated parameters that we define for SportCar
final class SportCar: Car {
  init(wheel: Wheel, color: Color) {
    super.init(doors: Door(2), engine: Engine(V85000), wheel: wheel, color: color)
  }
}

// How to call :
let sportCar = SportCarBuilder()
                .withColor(color: .red)
                .withWheel(type: .normal)
                .build()

                // sportCar.wheel === .normal --> Defined by user
// sportCar.engine === Engine(V85000) --> Defined by SportCar implementation


```

