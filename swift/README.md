![alt text](img/swift.png "Swift")

# Swift Walkthrough

The main purpose of this guide is to create code that is concise, readable and simple✌🏻.

- [1 Naming](#1-naming)
   - [1.1 Protocols](#11-protocols)
   - [1.2 Enumerations](#12-enumerations)
   - [1.3 Language](#13-language)
- [2 Spacing](#2-spacing)
- [3 Conventions](#3-conventions)
   - [3.1 General Conventions](#31-general-conventions)
   - [3.2 Parameters](#32-parameters)
- [4 Code Organization](#4-code-organization)
    - [4.1  Protocol Conformance](#41-protocol-conformance)
    - [4.2 Unused code](#42-unused-code)
    - [4.3 Minimal imports](#43-minimal-imports)
    - [4.4 Classes and Structures](#44-classes-and-structures)
    - [4.5 Self](#45-self)
    - [4.6 Computed Properties](#46-computed-properties)
- [5 Optionals](#5-optionals)
    - [5.1 Optional Chaining](#51-optional-chaining)
    - [5.2 Optional Binding](#52-optional-binding)
    - [5.3 Guard](#53-guard)
    - [5.4 Without Guard](#54-without-guard)
- [6 Control Flow](#6-control-flow)
- [7 Initializer and Deinitializer](#7-initializer-and-deinitializer)
- [8 Correctness](#8-correctness)
- [Thanks to](#thanks-to)

# 1 Naming
Use descriptive names with camel case for classes, methods, variables, etc. Type names (classes, structures, enumerations and protocols) should be UpperCamelCase, while method names and variables should be lowerCamelCase.

Good:

```Swift
private let maximumBookCount = 100

class BookContainer {
  var bookButton: UIButton
  let bookWidthPercentage = 0.85
}
```

Bad:

```Swift
let MAX_BOOK_COUNT = 100

class app_bookContainer {
  var bBut: UIButton
  let bWidthPct = 0.85
}
```
According to [Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/#follow-case-conventions), abbreviations and initialisms that appear in all uppercase should be uniformly uppercase or lowercase.

Good:
```Swift
let urlString: URLString
let userID: UserID
```
Bad:
```Swift
let uRLString: UrlString
let userId: UserId
```

When specifying the type of an identifier, always put the colon immediately after the identifier, followed by a space and then the type name.

```Swift
class Coffee: Drink { ... }

let timeToCoffee: NSTimeInterval = 2

func makeCoffee(type: CoffeeType) -> Coffee { ... }
```

Rationale: The type specifier is saying something about the identifier so it should be positioned with it.

Also, when specifying the type of a dictionary, always put the colon immediately after the key type, followed by a space and then the value type.

```Swift
let capitals: [Country: City] = [ Spain: Madrid ]
```

## 1.1 Protocols
According to Apple's API Design Guidelines, protocols names that describe what something is should be a noun. Examples: ```Collection```, ```BookFactory```. Protocols names that describe an ability should end in -ing, -able, or -ible. Examples: ```Relatable```, ```Resizing```.


## 1.2 Enumerations

You introduce enumerations with the enum keyword and place their entire definition within a pair of braces. Cases are to be written in LowerCamelCase.

```Swift
enum CompassPoint {
    case north
    case south
    case east
    case west
}
```
or 
```Swift
enum CompassPoint {
    case north, south, east, west
}
```

## 1.3 Language

Everything should be in US English!

Good:
```Swift
let red = CGColor.redColor()
```
Bad:
```Swift
let rojo = CGColor.redColor()
```

# 2 Spacing

Indent using 2 spaces rather than tabs to conserve space and help prevent line wrapping. Be sure to set this preference in Xcode and in the Project settings as shown below:
![alt text](img/indentation.png "Indent")

Method braces and other braces (if/else/switch/while etc.) always open on the same line as the statement but close on a new line.

Good:
```Swift
if user.isRunning {
  // Do something
} else {
  // Do something else
}
```
Bad:
```Swift
if user.isRunning
{
  // Do something
} else {
  // Do something else
}
```


# 3 Conventions

## 3.1 General Conventions

Prefer methods and properties to free functions. Free functions are used only in special cases

- When there’s no obvious ```self```:
```Swift
  min(a, b, c)
```
- When the function is an unconstrained generic:
```Swift
  print(x)
```
- When function syntax is part of the established domain notation:
```Swift
  sin(x)
```
Good:
```Swift
extension Shape {
  // Returns true if `other` is within the area of `self`.
  func contains(_ other: Point) -> Bool { ... }

  // Returns true if `other` is entirely within the area of `self`.
  func contains(_ other: Shape) -> Bool { ... }

  // Returns true if `other` is within the area of `self`.
  func contains(_ other: LineSegment) -> Bool { ... }
}
```
However, these index methods have different semantics, and should have been named differently:

Bad: 
```Swift
extension Database {
  // Rebuilds the database's search index
  func index() { ... }

  // Returns the `n`th row in the given table.
  func index(_ n: Int, inTable: TableID) -> TableRow { ... }
}
```

Methods can share a base name when they share the same basic meaning or when they operate in distinct domains.

## 3.2 Parameters

Choose parameter names to serve documentation. Even though parameter names do not appear at a function or method’s point of use, they play an important explanatory role.

```Swift
mutating func replaceRange(subRange: Range, with newElements: [E])
```
Take advantage of defaulted parameters when it simplifies common uses. Any parameter with a single commonly-used value is a candidate for a default.

Bad:
```Swift
let order = lastName.compare(
  royalFamilyName, options: [], range: nil, locale: nil)
```

Good:
```Swift
let order = lastName.compare(royalFamilyName)
```

Prefer to locate parameters with defaults toward the end of the parameter list. Parameters without defaults are usually more essential to the semantics of a method, and provide a stable initial pattern of use where methods are invoked.



# 4 Code Organization

Use extensions to organize your code into logical blocks of functionality. Each extension should be set off with a ```// MARK: - ``` comment to keep things well-organized.

## 4.1 Protocol Conformance

In particular, when adding protocol conformance to a model, prefer adding a separate extension for the protocol methods. This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods.

Good:
```Swift
class MyViewcontroller: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewcontroller: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewcontroller: UIScrollViewDelegate {
  // scroll view delegate methods
}
```

Bad:
```Swift
class MyViewcontroller: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}

```

## 4.2 Unused Code
Unused code, including Xcode template code and placeholder comments should be removed. 

Aspirational methods not directly associated with the tutorial whose implementation simply calls the super class should also be removed. This includes any empty/unused UIApplicationDelegate methods.

Bad: 
```Swift
override func didReceiveMemoryWarning() {
   super.didReceiveMemoryWarning()
  // Dispose of any resources that can be recreated.
}

override func numberOfSectionsInTableView(tableView: UITableView) -> Int {
   // #warning Incomplete implementation, return the number of sections
   return 1
}

override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  // #warning Incomplete implementation, return the number of rows
  return Database.contacts.count
}
```
Good:
```Swift
override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  return Database.contacts.count
}
```

## 4.3 Minimal Imports

Keep imports minimal. For example, don't import ```UIKit``` when importing ```Foundation``` will suffice.

## 4.4 Classes and Structures
Remember, structs have value semantics. Use structs for things that do not have an identity. An array that contains [1, 2, 3] is really the same as another array that contains [1, 2, 3] and they are completely interchangeable. It doesn't matter whether you use the first array or the second, because they represent the exact same thing. That's why arrays are structs.

Classes have reference semantics. Use classes for things that do have an identity or a specific life cycle. You would model a person as a class because two person objects are two different things. Just because two people have the same name and birthdate, doesn't mean they are the same person. But the person's birthdate would be a struct because a date of 3 March 1950 is the same as any other date object for 3 March 1950. The date itself doesn't have an identity.

Classes and structures in Swift have many things in common. Both can:

- Define properties to store values
- Define methods to provide functionality
- Define subscripts to provide access to their values using subscript syntax
- Define initializers to set up their initial state
- Be extended to expand their functionality beyond a default implementation
- Conform to protocols to provide standard functionality of a certain kind


Classes have additional capabilities that structures do not:

- Inheritance enables one class to inherit the characteristics of another.
- Type casting enables you to check and interpret the type of a class instance at runtime.
- Deinitializers enable an instance of a class to free up any resources it has assigned.
- Reference counting allows more than one reference to a class instance.

This is an example of a well-defined class
```Swift
class Circle: Shape {
  var x: Int, y: Int
  var radius: Double
  var diameter: Double {
    get {
      return radius * 2
    }
    set {
      radius = newValue / 2
    }
  }

  init(x: Int, y: Int, radius: Double) {
    self.x = x
    self.y = y
    self.radius = radius
  }

  convenience init(x: Int, y: Int, diameter: Double) {
    self.init(x: x, y: y, radius: diameter / 2)
  }

  func describe() -> String {
    return "I am a circle at \(centerString()) with an area of \(computeArea())"
  }

  override func computeArea() -> Double {
    return M_PI * radius * radius
  }

  private func centerString() -> String {
    return "(\(x),\(y))"
  }
}
```

## 4.5 Self
Avoid using self since Swift does not require it to access an object's properties or invoke its methods.

Use self when required to differentiate between property names and arguments in initializers, and when referencing properties in closure expressions (the compiler will complain about that, don't worry):

```Swift
class BoardLocation {
  let row: Int, column: Int

  init(row: Int, column: Int) {
    self.row = row
    self.column = column

    let closure = {
      print(self.row)
    }
  }
}
```
## 4.6 Computed Properties
For conciseness, if a computed property is read-only, omit the get clause. The get clause is required only when a set clause is provided.

Good:
```Swift
var diameter: Double {
  return radius * 2
}
```
Bad: 
```Swift
var diameter: Double {
  get {
    return radius * 2
  }
}
```
# 5 Optionals 

Swift accepts the declaration of variables and function return types as optionals with ```?```  where a nil value is acceptable.

Use implicitly unwrapped types declared with ! only for instance variables that you know will be initialized later before use, such as subviews that will be set up in viewDidLoad.

## 5.1 Optional Chaining
Optional chaining is a process for querying and calling properties, methods, and subscripts on an optional that might currently be nil. If the optional contains a value, the property, method, or subscript call succeeds; if the optional is nil, the property, method, or subscript call returns nil. Multiple queries can be chained together, and the entire chain fails gracefully if any link in the chain is nil.

```Swift
navigationController?.popToRootViewControllerAnimated(true)
```
If the current view controller is embeded into a navigation controller, it will remove all the view controllers from the stack, until the root view controller is on top. If the current view controller is not within a navigation controller, then nothing will happen ☝🏻.

## 5.2 Optional Binding
When you want to be sure that you will perform an action on something with value, you can use optional binding.

```Swift
class Dog {
  func bark() {
    print("guau")
  }
}

class Person {
  var pet: Dog?
}

let joe = Person()

if let chester = joe.pet {
  chester.bark()
}
```

## 5.3 Guard
Whenever nil just won't be any useful, ```guard``` lets you handle that situation.

```Swift
func submit() {
    guard let name = nameField.text else {
        show("No name to submit")
        return
    }

    guard let address = addressField.text else {
        show("No address to submit")
        return
    }

    guard let phone = phoneField.text else {
        show("No phone to submit")
        return
    }

    sendToServer(name, address: address, phone: phone)
}

func sendToServer(name: String, address: String, phone: String) {
  ...
}
```
```guard```guarantees that you will have a value to work with and if you don't, you can act accordingly.

## 5.4 Without Guard

```Swift
func submit() {
    if let name = nameField.text {
        if let address = addressField.text {
            if let phone = phoneField.text {
                sendToServer(name, address: address, phone: phone)
            } else {
                show("no phone to submit")
            }
        } else {
            show("no address to submit")
        }
    } else {
        show("no name to submit")
    }
}
```
The code becomes hard to read and deeply nested 😐.

# 6 Control Flow
Prefer the ```for-in``` style of for loop over the ```while-condition-increment``` style.

Good:
```Swift
for _ in 0..<5 {
  print("Hello five times")
}

for (index, person) in attendeeList.enumerate() {
  print("\(person) is at position #\(index)")
}

for index in 0.stride(to: books, by: 3) {
  print(index)
}
```

Bad:

```Swift
var i = 0
while i < 5 {
  print("Hello five times")
  i += 1
}


var i = 0
while i < attendeeList.count {
  let person = attendeeList[i]
  print("\(person) is at position #\(i)")
  i += 1
}
```
#7 Initializer and Deinitializer
Initializers are called to create a new instance of a particular type. In its simplest form, an initializer is like an instance method with no parameters, written using the init keyword:

```Swift
struct Fahrenheit {
  var temperature: Double
    
    init() {
        temperature = 32.0
    }
}

var f = Fahrenheit()
print("The default temperature is \(f.temperature)° Fahrenheit")
// Prints "The default temperature is 32.0° Fahrenheit"
```

You can provide initialization parameters as part of an initializer’s definition, to define the types and names of values that customize the initialization process. Initialization parameters have the same capabilities and syntax as function and method parameters.

```Swift
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
}
let boilingPointOfWater = Celsius(fromFahrenheit: 212.0)
// boilingPointOfWater.temperatureInCelsius is 100.0
let freezingPointOfWater = Celsius(fromKelvin: 273.15)
// freezingPointOfWater.temperatureInCelsius is 0.0
```

A deinitializer is called immediately before a class instance is deallocated. You write deinitializers with the deinit keyword, similar to how initializers are written with the init keyword. Deinitializers are only available on class types.

Deinitializers are called automatically, just before instance deallocation takes place. You are not allowed to call a deinitializer yourself. 

Because an instance is not deallocated until after its deinitializer is called, a deinitializer can access all properties of the instance it is called on and can modify its behavior based on those properties.



```Swift
class Bank {
    static var coinsInBank = 10000
    static func vendCoins(numberOfCoinsRequested: Int) -> Int {
        let numberOfCoinsToVend = min(numberOfCoinsRequested, coinsInBank)
        coinsInBank -= numberOfCoinsToVend
        return numberOfCoinsToVend
    }
    static func receiveCoins(coins: Int) {
        coinsInBank += coins
    }
}

class Player {
    var coinsInPurse: Int
    init(coins: Int) {
        coinsInPurse = Bank.vendCoins(coins)
    }
    deinit {
        Bank.receiveCoins(coinsInPurse)
    }
}
```
```Swift
var playerOne: Player? = Player(coins: 100)

```
In this example, playerOne started with 100 coins, which means the bank now has 9,900 coins left
```Swift
playerOne = nil
```
Whenever that instance is no longer used, deinit() gets called and the bank gets all the coins that where assigned to playerOne.

#8 Correctness
Consider warnings to be errors. This rule informs many stylistic decisions such as not to use the ++ or -- operators, C-style for loops, or strings as selectors.

# Thanks to

This guide takes information from differences sources,   without them this guide couldn't exist:

[Swift.org](https://swift.org/documentation/#api-design-guidelines)

[Ray Wenderlich Swift Style Guide](https://github.com/raywenderlich/swift-style-guide)

[Github Swift Style Guide](https://github.com/github/swift-style-guide)

[Apple](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID309)










