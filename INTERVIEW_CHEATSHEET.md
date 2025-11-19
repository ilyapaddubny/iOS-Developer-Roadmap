# iOS Developer Interview Cheatsheet

Quick reference guide for common interview questions and key concepts.

**Review this the night before and morning of your interview!**

---

## Table of Contents
1. [🆕 Modern iOS (2020-2025)](#-modern-ios-2020-2025)
2. [Swift Fundamentals](#swift-fundamentals)
3. [Memory Management](#memory-management)
4. [UIKit & iOS Frameworks](#uikit--ios-frameworks)
5. [Architecture & Design Patterns](#architecture--design-patterns)
6. [Concurrency & Multithreading](#concurrency--multithreading)
7. [Networking & Data](#networking--data)
8. [Testing & Debugging](#testing--debugging)
9. [Algorithms & Data Structures](#algorithms--data-structures)
10. [Common Interview Questions](#common-interview-questions)
11. [Code Snippets](#code-snippets)

---

## 🆕 Modern iOS (2020-2025)

**CRITICAL: This section covers the most important changes since 2020. Most interviews in 2025 will ask about these topics!**

### SwiftUI Basics

**Q: What is SwiftUI and how is it different from UIKit?**

**A:**
- **SwiftUI**: Declarative UI framework (describe what you want)
- **UIKit**: Imperative framework (describe how to build it)
- SwiftUI uses state-driven updates, UIKit uses manual updates

```swift
// UIKit
let label = UILabel()
label.text = "Hello"
label.textColor = .blue
view.addSubview(label)

// SwiftUI
Text("Hello")
    .foregroundColor(.blue)
```

**Q: Explain SwiftUI state management property wrappers.**

**A:**
- **@State**: For simple value types owned by view
- **@Binding**: Two-way connection to @State in parent
- **@StateObject**: For ObservableObject owned by view
- **@ObservedObject**: For ObservableObject passed from parent
- **@EnvironmentObject**: Dependency injection across view hierarchy
- **@Observable (iOS 17+)**: Simpler observation without @Published

```swift
struct ParentView: View {
    @State private var count = 0  // Owns the state

    var body: some View {
        ChildView(count: $count)  // Passes binding
    }
}

struct ChildView: View {
    @Binding var count: Int  // Two-way binding

    var body: some View {
        Button("Increment") {
            count += 1
        }
    }
}
```

### async/await (CRITICAL!)

**Q: What is async/await and why is it better than completion handlers?**

**A:**
- Modern concurrency introduced in Swift 5.5
- **Benefits:**
  - Linear code flow (no callback hell)
  - Automatic error propagation
  - Built-in cancellation support
  - Compiler-enforced thread safety

```swift
// Old way
func fetchData(completion: @escaping (Result<Data, Error>) -> Void) {
    URLSession.shared.dataTask(with: url) { data, response, error in
        if let error = error {
            completion(.failure(error))
        } else if let data = data {
            completion(.success(data))
        }
    }.resume()
}

// New way
func fetchData() async throws -> Data {
    let (data, _) = try await URLSession.shared.data(from: url)
    return data
}

// Usage
Task {
    do {
        let data = try await fetchData()
        // Use data
    } catch {
        print(error)
    }
}
```

**Q: What is Task in Swift concurrency?**

**A:** Task creates a new asynchronous context. Use it to call async functions from sync code.

```swift
// In SwiftUI
Button("Load") {
    Task {
        await loadData()
    }
}

// In UIKit
Task {
    let image = try await downloadImage()
    imageView.image = image
}
```

### Actors

**Q: What are Actors and why do we need them?**

**A:**
- Actors protect shared mutable state from data races
- Compile-time guarantee of thread safety
- All actor methods are async when called from outside

```swift
actor Counter {
    var value = 0

    func increment() {
        value += 1  // Thread-safe!
    }
}

let counter = Counter()
await counter.increment()  // Must use await
```

**Q: What is @MainActor?**

**A:** Global actor that ensures code runs on main thread (for UI updates).

```swift
@MainActor
class ViewModel: ObservableObject {
    @Published var items: [Item] = []

    func loadItems() async {
        // Guaranteed to be on main thread
        items = try await fetchItems()
    }
}

// Or for specific functions
@MainActor
func updateUI() {
    label.text = "Updated"
}
```

### SwiftUI vs UIKit Quick Reference

| Task | UIKit | SwiftUI |
|------|-------|---------|
| Create Text | `UILabel()` | `Text("Hello")` |
| Button | `UIButton()` + target-action | `Button("Tap") { }` |
| List | `UITableView` + delegates | `List(items) { }` |
| Navigation | `UINavigationController` | `NavigationStack` |
| State | Manual property updates | `@State` property wrapper |
| Layout | Auto Layout constraints | VStack, HStack, modifiers |

### Modern Swift Features

**Q: What are property wrappers?**

**A:** Reusable code for property get/set logic. SwiftUI heavily uses them (@State, @Binding, etc.).

```swift
@propertyWrapper
struct Clamped<Value: Comparable> {
    private var value: Value
    private let range: ClosedRange<Value>

    var wrappedValue: Value {
        get { value }
        set { value = min(max(newValue, range.lowerBound), range.upperBound) }
    }

    init(wrappedValue: Value, _ range: ClosedRange<Value>) {
        self.range = range
        self.value = min(max(wrappedValue, range.lowerBound), range.upperBound)
    }
}

@Clamped(0...100) var health = 100
health = 150  // Automatically becomes 100
```

### Common Modern Interview Questions

**Q: When would you use SwiftUI vs UIKit?**
- **SwiftUI**: New projects, simple UIs, rapid prototyping, iOS 14+ target
- **UIKit**: Legacy code, complex custom UI, need iOS 12 support, precise control
- **Reality**: Most companies use both - UIKit for existing code, SwiftUI for new features

**Q: How do you bridge SwiftUI and UIKit?**
- **UIViewRepresentable**: Wrap UIKit views for use in SwiftUI
- **UIHostingController**: Wrap SwiftUI views for use in UIKit

```swift
// UIKit in SwiftUI
struct ActivityIndicator: UIViewRepresentable {
    func makeUIView(context: Context) -> UIActivityIndicatorView {
        let view = UIActivityIndicatorView(style: .large)
        view.startAnimating()
        return view
    }

    func updateUIView(_ uiView: UIActivityIndicatorView, context: Context) {}
}

// SwiftUI in UIKit
let swiftUIView = MySwiftUIView()
let hostingController = UIHostingController(rootView: swiftUIView)
```

**Q: What's the difference between GCD and async/await?**
- **GCD**: Lower-level, manual queue management, callback-based
- **async/await**: Higher-level, automatic thread management, linear code
- **When to use**: async/await for new code, GCD for legacy maintenance

**Q: What is @Observable (iOS 17+)?**
- Simpler observation without @Published
- Macro that generates observation code
- Cleaner syntax than ObservableObject

```swift
// Old way (iOS 13-16)
class ViewModel: ObservableObject {
    @Published var count = 0
}

// New way (iOS 17+)
@Observable
class ViewModel {
    var count = 0  // No @Published needed!
}
```

### iOS Privacy Requirements (CRITICAL for 2025!)

**Q: What is App Tracking Transparency (ATT)?**

**A:** Required since iOS 14.5. Must ask permission before tracking users.

```swift
import AppTrackingTransparency

ATTrackingManager.requestTrackingAuthorization { status in
    switch status {
    case .authorized: // Can track
    case .denied: // Cannot track
    default: break
    }
}
```

**Q: What are Privacy Manifests?**

**A:** Required in 2024. Declare what data you collect and what APIs you use.

### Quick Tips for Modern iOS Interviews

✅ **Know both UIKit and SwiftUI** - You'll need both
✅ **Prefer async/await** in code examples - Shows you're current
✅ **Mention @MainActor** when doing UI updates
✅ **Use Actors** instead of locks for thread safety
✅ **Know privacy requirements** - ATT is mandatory

**For detailed coverage, see:** [MODERN_IOS_2025.md](MODERN_IOS_2025.md)

---

## Swift Fundamentals

### Closures
**Q: What is a closure? What's the difference between escaping and non-escaping closures?**

**A:** Closures are self-contained blocks of functionality that can be passed around.
- **Non-escaping** (default): Closure executes before function returns
- **Escaping** (`@escaping`): Closure may execute after function returns (stored for later use)

```swift
// Non-escaping
func performOperation(operation: () -> Void) {
    operation() // executes immediately
}

// Escaping
func performLater(operation: @escaping () -> Void) {
    DispatchQueue.main.asyncAfter(deadline: .now() + 1) {
        operation() // executes later
    }
}
```

### Value vs Reference Types
**Q: What's the difference between structs and classes?**

**A:**
- **Struct** (value type): Copied on assignment, stack allocated, no inheritance
- **Class** (reference type): Shared reference, heap allocated, supports inheritance

**Use structs** for simple data models, use **classes** for complex objects with identity.

### Optionals
**Q: What are optionals and how do you unwrap them safely?**

**A:** Optionals represent a value that might be nil.

```swift
var name: String? = "John"

// 1. Optional binding (safe)
if let unwrappedName = name {
    print(unwrappedName)
}

// 2. Guard statement (early exit)
guard let unwrappedName = name else { return }

// 3. Nil coalescing
let finalName = name ?? "Default"

// 4. Optional chaining
let count = name?.count

// DON'T USE: Force unwrapping (unless 100% sure)
let forcedName = name! // Crashes if nil
```

### Protocols
**Q: Explain protocol-oriented programming in Swift.**

**A:** Designing with protocols instead of inheritance. Benefits:
- Value types (structs) can conform
- Multiple protocol conformance (no multiple inheritance)
- Protocol extensions provide default implementations

```swift
protocol Drivable {
    var speed: Int { get set }
    func drive()
}

extension Drivable {
    func drive() { // default implementation
        print("Driving at \(speed) mph")
    }
}

struct Car: Drivable {
    var speed = 60
}
```

### Generics
**Q: What are generics and why use them?**

**A:** Write flexible, reusable code that works with any type while maintaining type safety.

```swift
func swapValues<T>(_ a: inout T, _ b: inout T) {
    let temp = a
    a = b
    b = temp
}
```

---

## Memory Management

### ARC (Automatic Reference Counting)
**Q: How does ARC work?**

**A:**
- Swift automatically tracks references to class instances
- When reference count reaches 0, instance is deallocated
- Works at **compile time** (not runtime like garbage collection)
- Only applies to **classes** (reference types), not structs

### Strong, Weak, Unowned
**Q: When do you use weak vs unowned references?**

**A:**
- **Strong** (default): Increases reference count, prevents deallocation
- **Weak**: Optional reference, doesn't increase count, becomes nil when deallocated
- **Unowned**: Non-optional reference, doesn't increase count, assumes always valid

```swift
class Person {
    var apartment: Apartment?
}

class Apartment {
    weak var tenant: Person? // weak to prevent retain cycle
}

// Use weak when reference can become nil
// Use unowned when reference will never be nil during its lifetime
```

### Retain Cycles
**Q: What is a retain cycle and how do you prevent it?**

**A:** Two objects hold strong references to each other, preventing deallocation.

**Common scenarios:**
1. **Closures capturing self**
2. **Delegate patterns**
3. **Parent-child relationships**

**Solutions:**
```swift
// 1. Weak self in closures
someMethod { [weak self] in
    self?.doSomething()
}

// 2. Weak delegates
protocol SomeDelegate: AnyObject { }
class SomeClass {
    weak var delegate: SomeDelegate?
}

// 3. Unowned for parent-child
class Parent {
    var child: Child?
}
class Child {
    unowned var parent: Parent
}
```

### Stack vs Heap
**Q: What's the difference between stack and heap memory?**

**A:**
- **Stack**: Fast, automatic, limited size. For value types and local variables
- **Heap**: Slower, manual management (ARC), larger. For reference types

---

## UIKit & iOS Frameworks

### UIViewController Lifecycle
**Q: Explain the UIViewController lifecycle methods and when each is called.**

**A:**
```swift
1. init() - ViewController created
2. loadView() - Creates view hierarchy
3. viewDidLoad() - View loaded into memory (called once)
   ↓ Setup that needs to happen once
4. viewWillAppear(_:) - Before view appears (called each time)
   ↓ Updates that should happen every time
5. viewWillLayoutSubviews() - Before layout
6. viewDidLayoutSubviews() - After layout
7. viewDidAppear(_:) - View is on screen
   ↓ Start animations, timers
8. viewWillDisappear(_:) - Before view leaves
   ↓ Save data, pause operations
9. viewDidDisappear(_:) - After view is gone
10. deinit - ViewController deallocated
```

**Common use cases:**
- `viewDidLoad()`: One-time setup, load data
- `viewWillAppear()`: Update UI, refresh data
- `viewDidLayoutSubviews()`: Frame-based layout calculations
- `viewWillDisappear()`: Save state, stop animations

### Auto Layout
**Q: What is Auto Layout and how does it work?**

**A:** Auto Layout uses constraints to define relationships between views, allowing adaptive layouts.

**Key concepts:**
- Constraints define rules (x, y, width, height)
- Priority system (required = 1000, high = 750, low = 250)
- Intrinsic content size (UILabel, UIButton)

```swift
// Programmatic Auto Layout
view.translatesAutoresizingMaskIntoConstraints = false
NSLayoutConstraint.activate([
    view.topAnchor.constraint(equalTo: parent.topAnchor, constant: 20),
    view.leadingAnchor.constraint(equalTo: parent.leadingAnchor, constant: 20),
    view.widthAnchor.constraint(equalToConstant: 100),
    view.heightAnchor.constraint(equalToConstant: 50)
])
```

### UITableView
**Q: How does UITableView cell reuse work?**

**A:**
- Cells are reused to save memory for large datasets
- System maintains a reuse pool
- `dequeueReusableCell(withIdentifier:)` retrieves or creates cells
- **Must** reset cell state in `prepareForReuse()` or when configuring

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
    // Configure cell with data
    cell.textLabel?.text = data[indexPath.row]
    return cell
}
```

### Delegate vs Notification
**Q: When would you use delegation vs notifications?**

**A:**
- **Delegation**: One-to-one communication, when you need a response
  - Example: UITableViewDelegate, custom protocol for passing data back
- **Notifications**: One-to-many, broadcasting events
  - Example: App state changes, keyboard events

```swift
// Delegation
protocol DataDelegate: AnyObject {
    func didReceiveData(_ data: String)
}

// Notification
NotificationCenter.default.post(name: .didUpdateData, object: nil)
NotificationCenter.default.addObserver(self, selector: #selector(handleUpdate),
                                       name: .didUpdateData, object: nil)
```

---

## Architecture & Design Patterns

### MVC (Model-View-Controller)
**Q: Explain MVC and its problems in iOS.**

**A:**
- **Model**: Data and business logic
- **View**: UI components (UIView, UILabel)
- **Controller**: Mediates between Model and View (UIViewController)

**Problem: Massive View Controller**
- ViewController becomes too large with networking, data transformation, etc.
- **Solutions**: MVVM, Coordinators, extract logic into separate classes

### MVVM (Model-View-ViewModel)
**Q: How does MVVM improve upon MVC?**

**A:**
- **ViewModel** sits between Model and View
- Contains presentation logic, transforms model data for view
- View binds to ViewModel (not directly to Model)
- ViewController becomes thinner, focused on view setup

```swift
// ViewModel
class UserViewModel {
    private let user: User

    var displayName: String {
        return "\(user.firstName) \(user.lastName)"
    }

    init(user: User) {
        self.user = user
    }
}

// ViewController
let viewModel = UserViewModel(user: currentUser)
nameLabel.text = viewModel.displayName
```

### Common Design Patterns

**Singleton**
```swift
class NetworkManager {
    static let shared = NetworkManager()
    private init() {} // Prevent external initialization
}
```
**Use sparingly** - can make testing difficult, creates global state.

**Delegate**
```swift
protocol TaskDelegate: AnyObject {
    func didCompleteTask()
}

class Task {
    weak var delegate: TaskDelegate?

    func execute() {
        // do work
        delegate?.didCompleteTask()
    }
}
```

**Observer (NotificationCenter)**
```swift
// Post
NotificationCenter.default.post(name: .taskCompleted, object: nil)

// Observe
NotificationCenter.default.addObserver(self, selector: #selector(taskCompleted),
                                       name: .taskCompleted, object: nil)
```

### SOLID Principles

**S - Single Responsibility**: Class should have one reason to change
**O - Open/Closed**: Open for extension, closed for modification
**L - Liskov Substitution**: Subtypes must be substitutable for base types
**I - Interface Segregation**: Many specific interfaces > one general interface
**D - Dependency Inversion**: Depend on abstractions, not concretions

---

## Concurrency & Multithreading

### GCD (Grand Central Dispatch)
**Q: Explain async vs sync, serial vs concurrent queues.**

**A:**

**Async vs Sync:**
- **async**: Doesn't wait, returns immediately, executes in background
- **sync**: Waits until task completes before returning

**Serial vs Concurrent:**
- **Serial**: One task at a time, in order
- **Concurrent**: Multiple tasks simultaneously

```swift
// Main queue (serial, UI updates)
DispatchQueue.main.async {
    self.label.text = "Updated"
}

// Global queue (concurrent, background work)
DispatchQueue.global(qos: .background).async {
    let data = self.fetchData()
    DispatchQueue.main.async {
        self.updateUI(with: data)
    }
}

// Custom serial queue
let serialQueue = DispatchQueue(label: "com.app.serial")

// Custom concurrent queue
let concurrentQueue = DispatchQueue(label: "com.app.concurrent",
                                    attributes: .concurrent)
```

### Common Threading Problems

**Race Condition**
Multiple threads accessing shared data simultaneously
```swift
// Problem
var counter = 0
DispatchQueue.global().async { counter += 1 }
DispatchQueue.global().async { counter += 1 }

// Solution: Serial queue or synchronization
let queue = DispatchQueue(label: "counter")
queue.async { counter += 1 }
queue.async { counter += 1 }
```

**Deadlock**
Two threads waiting for each other to release resources
```swift
// DON'T DO THIS on main queue:
DispatchQueue.main.sync { } // Deadlock! Main queue waiting for itself
```

### Main Thread Rule
**Q: Why must UI updates happen on the main thread?**

**A:** UIKit is not thread-safe. All UI updates must be on main thread to prevent:
- Race conditions
- Inconsistent UI state
- Crashes

```swift
// Always update UI on main thread
DispatchQueue.main.async {
    self.imageView.image = downloadedImage
}
```

---

## Networking & Data

### URLSession
**Q: How do you make a network request in iOS?**

**A:**
```swift
let url = URL(string: "https://api.example.com/data")!

let task = URLSession.shared.dataTask(with: url) { data, response, error in
    guard let data = data, error == nil else {
        print("Error: \(error?.localizedDescription ?? "Unknown")")
        return
    }

    // Parse data
    do {
        let result = try JSONDecoder().decode(MyModel.self, from: data)
        DispatchQueue.main.async {
            // Update UI
        }
    } catch {
        print("Decode error: \(error)")
    }
}

task.resume()
```

### Codable
**Q: What is Codable and how does it work?**

**A:** Protocol for encoding/decoding data (JSON, plist, etc.)

```swift
struct User: Codable {
    let id: Int
    let name: String
    let email: String

    // Custom keys if JSON has different names
    enum CodingKeys: String, CodingKey {
        case id
        case name = "full_name"
        case email
    }
}

// Decode
let user = try JSONDecoder().decode(User.self, from: jsonData)

// Encode
let jsonData = try JSONEncoder().encode(user)
```

### Core Data
**Q: What is Core Data?**

**A:** Apple's framework for persisting data (object graph management, not just database)

**Key components:**
- **NSManagedObjectContext**: Scratch pad for working with objects
- **NSPersistentContainer**: Sets up Core Data stack
- **NSManagedObject**: Actual data objects

```swift
// Save
let context = persistentContainer.viewContext
let newUser = User(context: context)
newUser.name = "John"
try? context.save()

// Fetch
let fetchRequest: NSFetchRequest<User> = User.fetchRequest()
let users = try? context.fetch(fetchRequest)
```

---

## Testing & Debugging

### Unit Testing
**Q: What is unit testing and why is it important?**

**A:** Testing individual units/components in isolation.

**Benefits:**
- Catch bugs early
- Document code behavior
- Safe refactoring
- Faster development

```swift
import XCTest

class CalculatorTests: XCTestCase {
    var calculator: Calculator!

    override func setUp() {
        calculator = Calculator()
    }

    func testAddition() {
        let result = calculator.add(2, 3)
        XCTAssertEqual(result, 5)
    }
}
```

### Common XCTest Assertions
- `XCTAssertEqual(a, b)` - Values equal
- `XCTAssertTrue(condition)` - Condition is true
- `XCTAssertNil(value)` - Value is nil
- `XCTAssertThrowsError` - Code throws error

---

## Algorithms & Data Structures

### Big-O Complexity

| Notation | Name | Example |
|----------|------|---------|
| O(1) | Constant | Array access by index |
| O(log n) | Logarithmic | Binary search |
| O(n) | Linear | Array iteration |
| O(n log n) | Linearithmic | Merge sort, quick sort |
| O(n²) | Quadratic | Nested loops |
| O(2ⁿ) | Exponential | Recursive fibonacci |

**Rule:** Drop constants and lower-order terms
- O(2n) → O(n)
- O(n² + n) → O(n²)

### Common Data Structures

**Array**
- Access: O(1)
- Search: O(n)
- Insert/Delete: O(n) (shift elements)

**Dictionary (Hash Table)**
- Access: O(1) average
- Search: O(1) average
- Insert/Delete: O(1) average

**Set**
- Contains: O(1) average
- Insert/Delete: O(1) average
- Good for uniqueness checks

**Stack (LIFO)**
```swift
struct Stack<T> {
    private var items: [T] = []

    mutating func push(_ item: T) {
        items.append(item)
    }

    mutating func pop() -> T? {
        return items.popLast()
    }
}
```

**Queue (FIFO)**
```swift
struct Queue<T> {
    private var items: [T] = []

    mutating func enqueue(_ item: T) {
        items.append(item)
    }

    mutating func dequeue() -> T? {
        return items.isEmpty ? nil : items.removeFirst()
    }
}
```

### Common Algorithms

**Binary Search** - O(log n)
```swift
func binarySearch(_ array: [Int], target: Int) -> Int? {
    var left = 0
    var right = array.count - 1

    while left <= right {
        let mid = (left + right) / 2
        if array[mid] == target {
            return mid
        } else if array[mid] < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    return nil
}
```

**Two Pointers Technique**
```swift
func twoSum(_ numbers: [Int], _ target: Int) -> [Int] {
    var left = 0
    var right = numbers.count - 1

    while left < right {
        let sum = numbers[left] + numbers[right]
        if sum == target {
            return [left, right]
        } else if sum < target {
            left += 1
        } else {
            right -= 1
        }
    }
    return []
}
```

**Sliding Window**
```swift
func maxSubarraySum(_ array: [Int], _ k: Int) -> Int {
    var maxSum = 0
    var windowSum = 0

    // Initial window
    for i in 0..<k {
        windowSum += array[i]
    }
    maxSum = windowSum

    // Slide window
    for i in k..<array.count {
        windowSum = windowSum - array[i - k] + array[i]
        maxSum = max(maxSum, windowSum)
    }
    return maxSum
}
```

---

## Common Interview Questions

### Technical Questions

**Q: What happens when you tap an app icon?**
1. SpringBoard launches app process
2. UIApplication created, calls `application(_:didFinishLaunchingWithOptions:)`
3. Window and root view controller created
4. Views loaded and displayed

**Q: What is the responder chain?**
Series of objects that can respond to events. Order:
First Responder → View → Superview → View Controller → Window → UIApplication → App Delegate

**Q: Explain copy-on-write in Swift.**
Swift optimizes value types. Copies only made when mutated, not on assignment.

**Q: What are property observers?**
```swift
var temperature: Int = 72 {
    willSet { print("About to set to \(newValue)") }
    didSet { print("Changed from \(oldValue) to \(temperature)") }
}
```

**Q: Difference between frame and bounds?**
- **frame**: Position and size in superview's coordinate system
- **bounds**: Position and size in own coordinate system (origin usually 0,0)

**Q: What is method swizzling?**
Changing implementation of existing selector at runtime (Objective-C runtime feature). Use sparingly!

**Q: What is KVO?**
Key-Value Observing: Notification mechanism for property changes.

### Behavioral Questions (Use STAR Method)

**Situation** → **Task** → **Action** → **Result**

**Q: Tell me about a challenging bug you fixed.**
**Q: Describe a time you had to learn a new technology quickly.**
**Q: How do you handle conflicting feedback from team members?**
**Q: Tell me about a project you're proud of.**

**Prepare 3-5 STAR examples** covering:
- Technical challenge
- Learning experience
- Teamwork/communication
- Project success
- Handling failure/setback

---

## Code Snippets for Common Tasks

### Singleton with thread safety
```swift
class Manager {
    static let shared = Manager()
    private init() {}
}
```

### Completion handler
```swift
func fetchData(completion: @escaping (Result<Data, Error>) -> Void) {
    URLSession.shared.dataTask(with: url) { data, response, error in
        if let error = error {
            completion(.failure(error))
        } else if let data = data {
            completion(.success(data))
        }
    }.resume()
}
```

### Custom delegate pattern
```swift
protocol CustomDelegate: AnyObject {
    func didFinish(with result: String)
}

class CustomClass {
    weak var delegate: CustomDelegate?

    func doWork() {
        // work
        delegate?.didFinish(with: "Done")
    }
}
```

### Extension for reusable cells
```swift
extension UITableView {
    func dequeueReusableCell<T: UITableViewCell>(for indexPath: IndexPath) -> T {
        guard let cell = dequeueReusableCell(withIdentifier: T.reuseIdentifier,
                                              for: indexPath) as? T else {
            fatalError("Unable to dequeue \(T.self)")
        }
        return cell
    }
}
```

---

## Interview Day Tips

### Before Coding
1. **Ask clarifying questions** - Don't assume requirements
2. **State assumptions** - "I'm assuming the array is sorted..."
3. **Discuss approach** - Talk through solution before coding
4. **Consider edge cases** - Empty input, nil, negative numbers

### While Coding
1. **Think out loud** - Explain your reasoning
2. **Write clean code** - Proper naming, formatting
3. **Test as you go** - Walk through with example input
4. **Don't panic** - It's okay to take a moment to think

### After Coding
1. **Walk through solution** - Trace with example input
2. **Test edge cases** - What if array is empty?
3. **Analyze complexity** - Time and space
4. **Discuss optimizations** - "We could improve this by..."

### Questions to Ask Interviewer
- What does a typical day look like?
- How does the team handle code reviews?
- What's the deployment process?
- How do you handle technical debt?
- What technologies is the team excited about?

---

## Final Reminders

✅ **You don't need to know everything** - It's okay to say "I don't know, but here's how I'd find out"

✅ **Communication matters** - Clear explanation often more important than perfect code

✅ **Show growth mindset** - Discuss what you're learning and how you improve

✅ **Be honest** - Don't fake knowledge. Interviewers appreciate honesty

✅ **Stay calm** - Take a breath, think through problems methodically

### 🆕 Modern iOS Interview Tips (2025)

✅ **Know BOTH UIKit and SwiftUI** - Most companies use both

✅ **Use async/await in examples** - Shows you're up-to-date with modern Swift

✅ **Mention @MainActor for UI updates** - Demonstrates concurrency knowledge

✅ **Prefer Actors over locks** - Modern, safe concurrency

✅ **Understand the migration path** - Be ready to discuss moving from UIKit→SwiftUI, GCD→async/await

✅ **Privacy is mandatory** - Know ATT and privacy requirements

---

**Remember:** In 2025, the best iOS developers bridge both worlds - legacy (UIKit, GCD, Objective-C) and modern (SwiftUI, async/await, Actors). Show you can work with existing code AND build new features with the latest tech.

**Good luck! You've got this! 🚀**
