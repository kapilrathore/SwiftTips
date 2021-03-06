# Swift tips and tricks

Here's list of Swift tips & tricks with all additional sources (playgrounds, images) that I would like to share. Also you can find them on [Twitter @szubyak](https://twitter.com/szubyak) and [Facebook @szubyakdev](https://www.facebook.com/szubyakdev), where you can ask questions and respond with feedback. I will realy glad to have you there! 😀

## Table of contents

[#1 Safe way to return element at specified index](https://github.com/Luur/SwiftTips#1-safe-way-to-return-element-at-specified-index)<br />
[#2 Easy way to hide Status Bar](https://github.com/Luur/SwiftTips#2-easy-way-to-hide-status-bar)<br />
[#3 Enumerated iteration](https://github.com/Luur/SwiftTips#3-enumerated-iteration)<br />
[#4 Combinations of pure functions](https://github.com/Luur/SwiftTips#4-combinations-of-pure-functions)<br />
[#5 Profit to compiler](https://github.com/Luur/SwiftTips#5-profit-to-compiler)<br />
[#6 Tips for writing error messages](https://github.com/Luur/SwiftTips#6-tips-for-writing-error-messages)<br />
[#7 Testing settings](https://github.com/Luur/SwiftTips#7-testing-settings)<br />
[#8 `forEach` and `map` execution order difference](https://github.com/Luur/SwiftTips#8-foreach-and-map-execution-order-difference)<br />
[#9 Change type of items in array](https://github.com/Luur/SwiftTips#9-change-type-of-items-in-array)<br />
[#10 Invoke `didSet` when property’s value is set inside `init` context](https://github.com/Luur/SwiftTips#10-invoke-didset-when-propertys-value-is-set-inside-init-context)<br />
[#11 Fake AppDelegate](https://github.com/Luur/SwiftTips#11-fake-appdelegate)<br />
[#12 Semicolons in Swift](https://github.com/Luur/SwiftTips#12-semicolons-in-swift)<br />
[#13 Group objects by property](https://github.com/Luur/SwiftTips#13-group-objects-by-property)<br />
[#14 Transparent/Opaque Navigation Bar](https://github.com/Luur/SwiftTips#14-transparentopaque-navigation-bar)<br />
[#15 Split array by chunks of given size](https://github.com/Luur/SwiftTips#15-split-array-by-chunks-of-given-size)<br />
[#16 Get next element of array](https://github.com/Luur/SwiftTips#16-get-next-element-of-array)<br />
[#17 Apply gradient to Navigation Bar](https://github.com/Luur/SwiftTips#17-apply-gradient-to-navigation-bar)<br />
[#18 Common elements in two arraysr](https://github.com/Luur/SwiftTips#18-common-elements-in-two-arrays)<br />
[#19 Left/rigth text offset inside `UITextField`](https://github.com/Luur/SwiftTips#19-leftrigth-text-offset-inside-uitextfield)<br />
[#20 How to detect that user stop typing](https://github.com/Luur/SwiftTips#20-how-to-detect-that-user-stop-typing)<br />
[#21 Comparing tuples](https://github.com/Luur/SwiftTips#21-comparing-tuples)<br />

## [#1 Safe way to return element at specified index](https://twitter.com/szubyak/status/950345927054778368)

You can extend collections to return the element at the specified index if it is within bounds, otherwise nil.

```swift
extension Collection {
    subscript (safe index: Index) -> Element? {
        return indices.contains(index) ? self[index] : nil
    }
}

let cars = ["Lexus", "Ford", "Volvo", "Toyota", "Opel"]
let selectedCar1 = cars[safe: 3] // Toyota
let selectedCar2 = cars[safe: 6] // not crash, but nil
```
Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#2 Easy way to hide Status Bar](https://twitter.com/szubyak/status/950687583222337537)

Ever faced the problem that u can't hide status bar because of `prefersStatusBarHidden` is `get-only`? The simplest solution is to `override` it 🧐👨‍💻

```swift
let vc = UIViewController()
vc.prefersStatusBarHidden = true // error
print("statusBarHidded \(vc.prefersStatusBarHidden)") // false

class TestViewController: UIViewController {
    override var prefersStatusBarHidden: Bool {
        return true
    }
}

let testVC = TestViewController()
print("statusBarHidded \(testVC.prefersStatusBarHidden)") // true
```
Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#3 Enumerated iteration](https://twitter.com/szubyak/status/951039299759362048)

Use `enumerated` when you iterate over the collection to return a sequence of pairs `(n, c)`, where `n` - index for each element and `c` - its value 👨‍💻💻
```swift
for (n, c) in "Swift".enumerated() {
    print("\(n): \(c)")
}
```
Result:

```
0: S
1: w
2: i
3: f
4: t
```
Also be careful with this tricky thing, `enumerated` on collection will not provide actual indices, but monotonically increasing integer, which happens to be the same as the index for Array but not for anything else, especially slices.

Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#4 Combinations of pure functions](https://twitter.com/szubyak/status/953993641391017984)

`flatMap` func is effectively the combination of using `map` and `joined` in a single call, in that order. It maps items in array A into array B using a func you provide, then joins the results using concatenation.

Functions `min` and `max` could be also combinations of  `sorted.first` and `sorted.last` in single call.

```swift
let colors = ["red", "blue", "black", "white"]

let min = colors.min() // black
let first = colors.sorted().first // black

let max = colors.max() // white
let last = colors.sorted().last // white
```
Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#5 Profit to compiler](https://twitter.com/szubyak/status/954329152160780288)

Do you know that using `map` gives profit to the compiler: it's now clear we want to apply some code to every item in an array, then like in `for` loop we could have `break` on halfway through.

Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#6 Tips for writing error messages](https://twitter.com/szubyak/status/955054366419013632)

1) Say what happened and why
2) Suggest a next step
3) Find the right tone (If it’s a stressful or serious issue, then a silly tone would be inappropriate)

[Common​ ​Types​ ​of​ ​Error​ ​Messages](../master/Sources/6/Common​​Error​​Messages.pdf)

Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#7 Testing settings](https://twitter.com/szubyak/status/955468985121886208)

1) Even if you don't write UI Tests, they still take considerable amount of time to run. Just skip it.
2) Enable code coverage stats in Xcode, it helps to find which method was tested, not tested, partly tested. But don’t pay too much attention to the percentage 😊.

![](../master/Sources/7/img.jpeg)

Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#8 `forEach` and `map` execution order difference](https://twitter.com/szubyak/status/956538549444186112)

Execution order is interesting difference between `forEach` and `map`: `forEach` is guaranteed to go through array elements in its sequence, while `map` is free to go in any order.

Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#9 Change type of items in array](https://twitter.com/szubyak/status/956544356370059265)

Two ways of changing type of items in array and obvious difference between them 🧐👨‍💻

```swift
let numbers = ["1", "2", "3", "4", "notInt"]
let mapNumbers = numbers.map { Int($0) }  // [Optional(1), Optional(2), Optional(3), Optional(4), nil]
let flatNumbers = numbers.flatMap { Int($0) } // [1, 2, 3, 4]
```
Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#10 Invoke `didSet` when property’s value is set inside `init` context](https://twitter.com/szubyak/status/961707655105531905)

Apple's docs specify that: "Property observers are only called when the property’s value is set outside of initialization context."

`defer` can change situation 😊

```swift
class AA {
    var propertyAA: String! {
        didSet {
            print("Function: \(#function)")
        }
    }

    init(propertyAA: String) {
        self.propertyAA = propertyAA
    }
}

class BB {
    var propertyBB: String! {
        didSet {
            print("Function: \(#function)")
        }
    }

    init(propertyBB: String) {
        defer {
            self.propertyBB = propertyBB
        }
    }
}

let aa = AA(propertyAA: "aa")
let bb = BB(propertyBB: "bb")
```
Result:

```
Function: propertyBB
```
Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#11 Fake AppDelegate](https://twitter.com/szubyak/status/963013740265385984)

Unit testing shouldn’t have any side effects. While running tests, Xcode firstly launches app and thus having the side effect of executing any code we may have in our App Delegate and initial View Controller. Fake AppDelegate in your `main.swift` to prevent it.

You can find `main.swift` file [here](../master/Sources/11/main.swift)

Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#12 Semicolons in Swift](https://twitter.com/szubyak/status/963144748004454400)

Do you need semicolons in Swift ? Short answer is NO, but you can use them and it will give you interesting opportunity. Semicolons enable you to join related components into a single line.

```swift
func sum(a: Int, b: Int) -> Int {
    let sum = a + b; return sum
}
```

Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#13 Group objects by property](https://twitter.com/szubyak/status/966248375988490240)

One more useful extension 🔨💻 Gives you opportunity to group objects by property 👨‍💻🧐

```swift
extension Sequence {
    func group<GroupingType: Hashable>(by key: (Iterator.Element) -> GroupingType) -> [[Iterator.Element]] {
        var groups: [GroupingType: [Iterator.Element]] = [:]
        var groupsOrder: [GroupingType] = []
        forEach { element in
            let key = key(element)
            if case nil = groups[key]?.append(element) {
                groups[key] = [element]
                groupsOrder.append(key)
            }
        }
        return groupsOrder.map { groups[$0]! }
    }
}
```

Usage:

```swift
struct Person {
    var name: String
    var age: Int
}

let mike = Person(name: "Mike", age: 18)
let john = Person(name: "John", age: 18)
let bob = Person(name: "Bob", age: 56)
let jake = Person(name: "Jake", age: 56)
let roman = Person(name: "Roman", age: 25)

let persons = [mike, john, bob, jake, roman]

let groupedPersons = persons.group { $0.age }

for persons in groupedPersons {
    print(persons.map { $0.name })
}
```

Result:

```
["Mike", "John"]
["Bob", "Jake"]
["Roman"]
```

Also in-box [alternative](https://developer.apple.com/documentation/swift/dictionary/2919592-init)

Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#14 Transparent/Opaque Navigation Bar](https://twitter.com/szubyak/status/970962096245628929)
Scene with `UIImageView` on top looks stylish if navigation bar is transparent.
Easy way how to make navigation bar transparent or opaque.

```swift
func transparentNavigationBar() {
    self.setBackgroundImage(UIImage(), for: .default)
    self.shadowImage = UIImage()
}

func opaqueNavigationBar() {
    self.shadowImage = nil
    self.setBackgroundImage(nil, for: .default)
}
```
Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#15 Split array by chunks of given size](https://twitter.com/szubyak/status/971351860820037632)

Great extension to split array by chunks of given size

```swift
extension Array {
    func chunk(_ chunkSize: Int) -> [[Element]] {
        return stride(from: 0, to: self.count, by: chunkSize).map({ (startIndex) -> [Element] in
            let endIndex = (startIndex.advanced(by: chunkSize) > self.count) ? self.count-startIndex : chunkSize
            return Array(self[startIndex..<startIndex.advanced(by: endIndex)])
        })
    }
}
```
Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#16 Get next element of array](https://twitter.com/szubyak/status/973146709030207489)

Easy way how to get next element of array

```swift
extension Array where Element: Hashable {
    func after(item: Element) -> Element? {
        if let index = self.index(of: item), index + 1 < self.count {
            return self[index + 1]
        }
        return nil
    }
}
```
Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#17 Apply gradient to Navigation Bar](https://twitter.com/szubyak/status/974580828750704641)

Gradient 🏳️‍🌈 on Navigation Bar is really good looking, but not very easy to implement 🧐🔨👨‍💻
Works with iOS 11 largeTitle navigation bar too 👌

```swift
struct GradientComponents {
    var colors: [CGColor]
    var locations: [NSNumber]
    var startPoint: CGPoint
    var endPoint: CGPoint
}

extension UINavigationBar {

    func applyNavigationBarGradient(with components: GradientComponents) {

        let size = CGSize(width: UIScreen.main.bounds.size.width, height: 1)
        let gradient = CAGradientLayer()
        gradient.frame = CGRect(x: 0, y: 0, width: size.width, height: size.height)

        gradient.colors = components.colors
        gradient.locations = components.locations
        gradient.startPoint = components.startPoint
        gradient.endPoint = components.endPoint

        UIGraphicsBeginImageContext(gradient.bounds.size)
        gradient.render(in: UIGraphicsGetCurrentContext()!)
        let image = UIGraphicsGetImageFromCurrentImageContext()
        UIGraphicsEndImageContext()
        self.barTintColor = UIColor(patternImage: image!)
    }
}
```
Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#18 Common elements in two arrays](https://twitter.com/szubyak/status/975736596661178369)

I'm not huge fan of custom operators 😐 because they are intuitively obvious only to their authors, but I've created one which gives you opportunity to get common elements in two arrays whos elements implement `Equatable` protocol 🔨🧐💻

```swift
infix operator &
func  &<T : Equatable>(lhs: [T], rhs: [T]) -> [T] {
    return lhs.filter { rhs.contains($0) }
}
```
Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#19 Left/rigth text offset inside `UITextField`](https://twitter.com/szubyak/status/976547738908323840)

Clear way of adding left\right text offset inside `UItextField` 🔨🧐💻  Also, because of `@IBInspectable` it could be easily editable in Interface Builder’s inspector panel.

```swift
@IBDesignable
extension UITextField {

    @IBInspectable var leftPaddingWidth: CGFloat {
        get {
            return leftView!.frame.size.width
        }
        set {
            let paddingView = UIView(frame: CGRect(x: 0, y: 0, width: newValue, height: frame.size.height))
            leftView = paddingView
            leftViewMode = .always
        }
    }

    @IBInspectable var rigthPaddingWidth: CGFloat {
        get {
            return rightView!.frame.size.width
        }
        set {
            let paddingView = UIView(frame: CGRect(x: 0, y: 0, width: newValue, height: frame.size.height))
            rightView = paddingView
            rightViewMode = .always
        }
    }
}
```
Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#20 How to detect that user stop typing](https://twitter.com/szubyak/status/978659051893555201)

Painless way ( NO to timers from now ⛔️ ) how to detect that user stop typing text in text field ⌨️ Could be usefull for lifetime search 🔍

```swift
class TestViewController: UIViewController {

    @objc func searchBarDidEndTyping(_ textField: UISearchBar) {
        print("User finsihed typing text in search bar")
    }

    @objc func textFieldDidEndTyping(_ textField: UITextField) {
        print("User finished typing text in text field")
    }
}

extension TestViewController: UISearchBarDelegate {
    func searchBar(_ searchBar: UISearchBar, shouldChangeTextIn range: NSRange, replacementText text: String) -> Bool {
        NSObject.cancelPreviousPerformRequests(withTarget: self, selector: #selector(searchBarDidEndTyping), object: searchBar)
        self.perform(#selector(searchBarDidEndTyping), with: searchBar, afterDelay: 0.5)
        return true
    }
}

extension TestViewController: UITextFieldDelegate {
    func textField(_ textField: UITextField, shouldChangeCharactersIn range: NSRange, replacementString string: String) -> Bool {
        NSObject.cancelPreviousPerformRequests(withTarget: self, selector: #selector(textFieldDidEndTyping), object: textField)
        self.perform(#selector(textFieldDidEndTyping), with: textField, afterDelay: 0.5)
        return true
    }
}
```
Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)

## [#21 Comparing tuples](https://twitter.com/szubyak/status/980119909387599873)

I discovered strange behavior of tuples during comparing 🤪. Comparison cares only about types and ignores labels 😦. So result can be unexpected. Be careful ⚠️. 

```swift
let car = (model: "Tesla", producer: "USA")
let company = (name: "Tesla", country: "USA")
if car == company {
    print("Equal")
} else {
    print("Not equal")
}
```
Printed result will be: `Equal`

Back to [Top](https://github.com/Luur/SwiftTips#table-of-contents)
