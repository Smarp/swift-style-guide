
# Swift Style Guide for Smarp mobile projects.

## Introduction

Code that looks familiar is easier to understand and therefore also easier to review, debug and maintain. To ensure that code looks familiar to all developers on the project it’s important to agree on a common set of style guidelines.

This document provides guidelines for low level coding practices such as how to indent code and how to name types and variables. Many of the stylistic choices are subjective and different developers may have different opinions about them. Keep in mind however, that having a consistent style is more important than to satisfy each individual developers preference.

## Guiding Principles

Strive to make your code compile without warnings. This rule informs many style decisions.

Use the Swift naming conventions described in the [Swift Style Guide Naming](https://swift.org/documentation/api-design-guidelines/#naming).

## Where to put code

#### Groups and file names

```code

|-- data
|   |-- repositories
|   |   |-- LikeRepository.swift
|   |   `-- LikeRepositoryProtocol.swift
|   |-- local
|   |   |-- PermanentStorage.swift
|   |   `-- MemoryStorage.swift
|   |-- remote
|   |    `-- Server.swift
|   `-- DataProviderProtocol.swift
|-- domain
|   |-- entities
|   |   |-- Bookmark.swift
|   |   `-- Like.swift
|    `-- usecases
|       |-- LikeSendCallback.swift
|       |-- LikeUseCases.swift
|       `-- LikeUseCasesProtocol.swift
`-- presentation
    |-- navigation
    |   `-- Navigator.swift
     `-- channelSubscription
        |-- ChannelSubscriptionViewController.swift
        |-- ChannelSubscriptionViewControllerProtocol.swift
        |-- ChannelSubscriptionViewControllerPresenter.swift
        |-- ChannelSubscriptionViewControllerPresenterProtocol.swift
        `-- SelectedItemChangedCallback.swift´


```

#### Protocols

When adding protocol conformance to a model, prefer adding a separate extension for the protocol methods. This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods.

```diff
+ Preferred
```
```swift
extension MyViewController: UIScrollViewDelegate {
  // scroll view delegate methods
}
```

```diff
- Not Preferred
```
```swift
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```


## How to name

Descriptive and consistent naming makes software easier to read and understand. 

**Clarity at the point of use** is your most important goal. Entities such as methods and properties are declared only once but used repeatedly. Design APIs to make those uses clear and concise.

**Clarity is more important than brevity.** Although Swift code can be compact, it is a non-goal to enable the smallest possible code with the fewest characters. Brevity in Swift code, where it occurs, is a side-effect of the strong type system and features that naturally reduce boilerplate.


Use the Swift naming conventions described in the [API Design Guidelines](https://swift.org/documentation/api-design-guidelines/).


<details>
<summary>SwiftLint</summary>

```code
 - type_name
 - identifier_name
 ```

</details>

#### Presenter methods called by the view
onInit()   
onButtonClicked()   
onTextChanged()   
onSearchTriggered()   
onDataChanged()   
onQueryChanged()   

#### View methods called by the presenter
init/setUp()   
setUIElementColor()   
setUIElementText()   
setUIElementClickListener()   
navigateToNextScreen()   
showUIElement()   
hideUIElement()   
enableUIElement()   
disableUIElement()  

#### Naming UI elements (IBOutlet)

Use the whole name of the element without the UI prefix. After that it can be followed by a description
```diff
+ Preferred
```
```swift
@IBOutlet weak var labelEmailHint: UILabel!
@IBOutlet weak var navigationBar: UINavigationBar!
@IBOutlet weak var postAuthorViewCreator: PostAuthorView!

```

```diff
- Not Preferred
```
```swift
@IBOutlet weak var lblEmailHint: UILabel!
@IBOutlet weak var emailHintTextField: UILabel!
@IBOutlet weak var emailHint: UILabel!

```

#### Naming views that extend UI elements

Use a descriptive name followed by the ui view without the UI prefix

```diff
+ Preferred
```
```swift
class PostAuthorView: UIView {

```

```diff
- Not Preferred
```
```swift
class ViewPostAuthor: UIView {
class PostAuthor: UIView {
class UIViewPostAuthor: UIView {
class PostAuthorUIView: UIView {

```

#### Test methods

Example template of test method signatures is 
*test*`WhatYouAreTesting`*Should*`WhatIsExpected`

```diff
+ Preferred: "test"+WhatYouAreTesting+OptionalCondition+"Should"+WhatIsExpected
```
```Swift
testClickApproveButtonShouldChangeUIToApprove();
testClickApproveButtonWithNoInternetShouldChangeUIToApprove();
```


```diff
- Not Preferred
```
```Swift
testFirstLeaderBoardMustBeCalled();
```

<details>
<summary>checkstyle</summary>
 
 - MethodName
 
</details>


## What is not needed
<details>
<summary>SwiftLint</summary>

```code
 - control_statement
 - cyclomatic_complexity
 - empty_enum_arguments
 - empty_parameters
 - file_length
 - for_where
 - function_body_length
 - function_parameter_count
 - legacy_cggeometry_functions
 - legacy_constant
 - legacy_constructor
 - legacy_nsgeometry_functions
 - line_length
 - nesting
 - redundant_discardable_let
 - redundant_nil_coalescing
 - redundant_optional_initialization
 - redundant_string_enum_value
 - redundant_void_return
 - syntactic_sugar
 - trailing_semicolon
 - type_body_length
 - unneeded_parentheses_in_closure_argument
 - unused_closure_parameter
 - unused_enumerated
 - implicit_return
 - large_tuple
 ```

</details>

#### Unused Code

Unused (dead) code, including Xcode template code and placeholder comments should be removed.

#### Unused Imports

Keep imports minimal. For example, don't import `UIKit` when importing `Foundation` will suffice.

#### Self

For conciseness, avoid using `self` since Swift does not require it to access an object's properties or invoke its methods.

Use self only when required by the compiler (in `@escaping` closures, or in initializers to disambiguate properties from arguments). In other words, if it compiles without `self` then omit it.

#### Types

Prefer compact code and let the compiler infer the type for constants or variables of single instances. Type inference is also appropriate for small (non-empty) arrays and dictionaries. When required, specify the specific type such as `CGFloat` or `Int16`.

```diff
+ Preferred
```
```swift
let message = "Click the button"
let currentBounds = computeViewBounds()
var names = ["Mic", "Sam", "Christine"]
let maximumWidth: CGFloat = 106.5
```

```diff
- Not Preferred
```
```swift
let message: String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
let names = [String]()
```

#### Full generics syntax

Prefer the shortcut versions of type declarations over the full generics syntax.

```diff
+ Preferred
```
```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

```diff
- Not Preferred
```
```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

#### Static Methods and Variable Type Properties

Static methods and type properties work similarly to global functions and global variables and should be used sparingly. They are useful when functionality is scoped to a particular type or when Objective-C interoperability is required.


#### Use Type Inferred Context

Use compiler inferred context to write shorter, clear code.
```diff
+ Preferred
```
```swift
view.backgroundColor = .red
let toView = context.view(forKey: .to)
```

```diff
- Not Preferred
```
```swift
view.backgroundColor = UIColor.red
let toView = context.view(forKey: UITransitionContextViewKey.to)
```


## What is actually needed

<details>
<summary>SwiftLint</summary>

```code
 - class_delegate_protocol
 - fatal_error_message
 - let_var_whitespace
 - let_var_whitespace
 - literal_expression_end_indentation
 - mark
 - opening_brace
 - operator_usage_whitespace
 - operator_whitespace
 - overridden_super_call
 - return_arrow_whitespace
 - sorted_imports
 - statement_position
 - switch_case_alignment
 - switch_case_on_newline
 - trailing_whitespace
 - vertical_parameter_alignment
 - vertical_parameter_alignment_on_call
 - vertical_whitespace
 - weak_delegate
 - closing_brace
 - closure_end_indentation
 - closure_spacing
 - colon
 - comma
 - conditional_returns_on_newline
 - valid_ibinspectable
 ```

</details>

#### Spacing

* Colons always have no space on the left and one space on the right. Exceptions are the ternary operator `? :`, empty dictionary `[:]` and `#selector` syntax for unnamed parameters `(_:)`.

```diff
+ Preferred
```
```swift
class TestDatabase: Database {
  var data: [String: CGFloat] = ["A": 1.2, "B": 3.2]
}
```

```diff
- Not Preferred
```
```swift
class TestDatabase : Database {
  var data :[String:CGFloat] = ["A" : 1.2, "B":3.2]
}
```

#### Comments

When they are needed, use comments to explain **why** a particular piece of code does something. Comments must be kept up-to-date or deleted.

#### weak and unowned

Code should not create reference cycles. Analyze your object graph and prevent strong cycles with `weak` and `unowned` references. Alternatively, use value types (`struct`, `enum`) to prevent cycles altogether.


#### Failing Guards

Guard statements are required to exit in some way. Generally, this should be simple one line statement such as `return`, `throw`, `break`, `continue`, and `fatalError()`. Large code blocks should be avoided. If cleanup code is required for multiple exit points, consider using a `defer` block to avoid cleanup code duplication.


## Which one to use?

<details>
<summary>SwiftLint</summary>

```code
 - multiple_closures_with_trailing_closure
 - trailing_closure
 - block_based_kvo
 ```

</details>


#### Classes vs Structures

Remember, structs have [value semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_144). Use structs for things that do not have an identity. An array that contains [a, b, c] is really the same as another array that contains [a, b, c] and they are completely interchangeable. It doesn't matter whether you use the first array or the second, because they represent the exact same thing. That's why arrays are structs.

Classes have [reference semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_145). Use classes for things that do have an identity or a specific life cycle. You would model a person as a class because two person objects are two different things. Just because two people have the same name and birthdate, doesn't mean they are the same person. But the person's birthdate would be a struct because a date of 3 March 1950 is the same as any other date object for 3 March 1950. The date itself doesn't have an identity.

#### Functions vs Methods

Free functions, which aren't attached to a class or type, should be used sparingly. When possible, prefer to use a method instead of a free function. This aids in readability and discoverability.

Free functions are most appropriate when they aren't associated with any particular type or instance.

```diff
+ Preferred
```

```swift
// easily discoverable
let sorted = items.mergeSorted()  
rocket.launch()
```

```diff
- Not Preferred
```
```swift
// hard to discover
let sorted = mergeSort(items)  
launch(&rocket)
```

Free Function Exceptions   

```swift
 // feels natural as a free function (symmetry)
let tuples = zip(a, b) 
 // another free function that feels natural
let value = max(x, y, z) 
```
#### Swift Types vs Objective-C Types

Always use Swift's native types when available. Swift offers bridging to Objective-C so you can still use the full set of methods as needed.

#### let vs var

Constants are defined using the `let` keyword, and variables with the `var` keyword. Always use `let` instead of `var` if the value of the variable will not change.

**Tip:** A good technique is to define everything using `let` and only change it to `var` if the compiler complains!


#### ? vs !

Declare variables and function return types as optional with `?` where a nil value is acceptable.

Use implicitly unwrapped types declared with `!` only for instance variables that you know will be initialized later before use, such as subviews that will be set up in `viewDidLoad`.

When accessing an optional value, use optional chaining if the value is only accessed once or if there are many optionals in the chain:

```swift
self.textContainer?.textLabel?.setNeedsDisplay()
```

Use optional binding when it's more convenient to unwrap once and perform multiple operations:

```swift
if let textContainer = self.textContainer {
  // do many things with textContainer
}
```

When naming optional variables and properties, avoid naming them like `optionalString` or `maybeView` since their optional-ness is already in the type declaration.


#### `for-in` vs  `while-condition-increment` 

Prefer the `for-in` style of `for` loop over the `while-condition-increment` style.

```diff
+ Preferred
```
```swift
for _ in 0..<3 {
  print("Hello three times")
}

for (index, person) in attendeeList.enumerated() {
  print("\(person) is at position #\(index)")
}

for index in stride(from: 0, to: items.count, by: 2) {
  print(index)
}

for index in (0...3).reversed() {
  print(index)
}
```

```diff
- Not Preferred
```
```swift
var i = 0
while i < 3 {
  print("Hello three times")
  i += 1
}


var i = 0
while i < attendeeList.count {
  let person = attendeeList[i]
  print("\(person) is at position #\(i)")
  i += 1
}
```

#### Closure Expressions

Use trailing closure syntax only if there's a single closure expression parameter at the end of the argument list. Give the closure parameters descriptive names.


```diff
+ Preferred
```

```swift
UIView.animate(withDuration: 1.0) {
  self.myView.alpha = 0
}

UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
}, completion: { finished in
  self.myView.removeFromSuperview()
})
```

```diff
- Not Preferred
```
```swift
UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
})

UIView.animate(withDuration: 1.0, animations: {
  self.myView.alpha = 0
}) { f in
  self.myView.removeFromSuperview()
}
```

#### Extending object lifetime

Extend object lifetime using the `[weak self]` and `guard let strongSelf = self else { return }` idiom. `[weak self]` is preferred to `[unowned self]` where it is not immediately obvious that `self` outlives the closure. Explicitly extending lifetime is preferred to optional unwrapping.


```diff
+ Preferred
```

```swift
resource.request().onComplete { [weak self] response in
  guard let strongSelf = self else {
    return
  }
  let model = strongSelf.updateModel(response)
  strongSelf.updateUI(model)
}
```

```diff
- Not Preferred
```
```swift
// might crash if self is released before response returns
resource.request().onComplete { [unowned self] response in
  let model = self.updateModel(response)
  self.updateUI(model)
}
```
```diff
- Not Preferred
```
```swift
// deallocate could happen between updating the model and updating UI
resource.request().onComplete { [weak self] response in
  let model = self?.updateModel(response)
  self?.updateUI(model)
}
```

#### Type Annotation for Empty Arrays and Dictionaries

For empty arrays and dictionaries, use type annotation.
```diff
+ Preferred
```
```swift
var names: [String] = []
var lookup: [String: Int] = [:]
```

```diff
- Not Preferred
```
```swift
var names = [String]()
var lookup = [String: Int]()
```

## How not to crash

<details>
<summary>SwiftLint</summary>

```code
 - discouraged_direct_init
 - implicitly_unwrapped_optional
 - force_unwrapping
 - force_cast
 ```

</details>

## How to be fast

<details>
<summary>SwiftLint</summary>

```code
 - empty_count
 - empty_string
 - first_where
 ```

</details>

## Copyright Statement

The following copyright statement should be included at the top of every source
file:

```swift
/// Copyright (c) 2018 Razeware LLC
/// 
/// Permission is hereby granted, free of charge, to any person obtaining a copy
/// of this software and associated documentation files (the "Software"), to deal
/// in the Software without restriction, including without limitation the rights
/// to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
/// copies of the Software, and to permit persons to whom the Software is
/// furnished to do so, subject to the following conditions:
/// 
/// The above copyright notice and this permission notice shall be included in
/// all copies or substantial portions of the Software.
/// 
/// Notwithstanding the foregoing, you may not use, copy, modify, merge, publish,
/// distribute, sublicense, create a derivative work, and/or sell copies of the
/// Software in any work that is designed, intended, or marketed for pedagogical or
/// instructional purposes related to programming, coding, application development,
/// or information technology.  Permission for such use, copying, modification,
/// merger, publication, distribution, sublicensing, creation of derivative works,
/// or sale is expressly withheld.
/// 
/// THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
/// IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
/// FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
/// AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
/// LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
/// OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
/// THE SOFTWARE.
```


## References

* [The Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
* [The Swift Programming Language](https://developer.apple.com/library/prerelease/ios/documentation/swift/conceptual/swift_programming_language/index.html)
* [Using Swift with Cocoa and Objective-C](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html)
* [Swift Standard Library Reference](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/SwiftStandardLibraryReference/index.html)
* [The Official raywenderlich.com Swift Style Guide](https://github.com/raywenderlich/swift-style-guide)
* [SwiftLint rules list](https://github.com/realm/SwiftLint/blob/master/Rules.md)
