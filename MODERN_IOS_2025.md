# Modern iOS Development 2025
## What's Changed Since 2020

**Last Roadmap Update:** 2020
**Current Year:** 2025
**Time Gap:** 5 years of major changes

This guide covers the critical iOS development topics that have emerged or evolved since 2020. These topics are **essential** for interviews in 2025.

---

## Table of Contents

1. [SwiftUI - The Modern UI Framework](#swiftui---the-modern-ui-framework)
2. [Modern Swift Concurrency](#modern-swift-concurrency)
3. [Modern Swift Language Features](#modern-swift-language-features)
4. [iOS 14-18 Frameworks & APIs](#ios-14-18-frameworks--apis)
5. [Privacy & Security Requirements](#privacy--security-requirements)
6. [Modern Architecture Patterns](#modern-architecture-patterns)
7. [Migration Guides](#migration-guides)
8. [Interview Preparation Tips](#interview-preparation-tips)

---

## SwiftUI - The Modern UI Framework

### Why It Matters
SwiftUI has been Apple's recommended UI framework since iOS 13 (2019) and is now the **primary focus** for new iOS development. Most companies hiring in 2025 expect SwiftUI knowledge.

### Core Concepts

#### 1. Declarative UI Paradigm

**UIKit (Imperative):**
```swift
let label = UILabel()
label.text = "Hello"
label.textColor = .blue
view.addSubview(label)
```

**SwiftUI (Declarative):**
```swift
Text("Hello")
    .foregroundColor(.blue)
```

#### 2. State Management (CRITICAL for Interviews)

**@State** - Local view state
```swift
struct CounterView: View {
    @State private var count = 0

    var body: some View {
        VStack {
            Text("Count: \(count)")
            Button("Increment") {
                count += 1
            }
        }
    }
}
```

**@Binding** - Two-way connection
```swift
struct ChildView: View {
    @Binding var isOn: Bool

    var body: some View {
        Toggle("Switch", isOn: $isOn)
    }
}

struct ParentView: View {
    @State private var isOn = false

    var body: some View {
        ChildView(isOn: $isOn)
    }
}
```

**@StateObject** - Own the ObservableObject
```swift
class ViewModel: ObservableObject {
    @Published var items: [String] = []
}

struct ContentView: View {
    @StateObject private var viewModel = ViewModel()

    var body: some View {
        List(viewModel.items, id: \.self) { item in
            Text(item)
        }
    }
}
```

**@ObservedObject** - Observe someone else's object
```swift
struct DetailView: View {
    @ObservedObject var viewModel: ViewModel

    var body: some View {
        Text("Items: \(viewModel.items.count)")
    }
}
```

**@EnvironmentObject** - Dependency injection
```swift
class AppState: ObservableObject {
    @Published var isLoggedIn = false
}

@main
struct MyApp: App {
    @StateObject private var appState = AppState()

    var body: some Scene {
        WindowGroup {
            ContentView()
                .environmentObject(appState)
        }
    }
}

struct ContentView: View {
    @EnvironmentObject var appState: AppState

    var body: some View {
        Text(appState.isLoggedIn ? "Logged In" : "Logged Out")
    }
}
```

**@Observable (iOS 17+)** - Modern observation
```swift
@Observable
class ViewModel {
    var count = 0
    var name = ""
}

struct ContentView: View {
    var viewModel = ViewModel()

    var body: some View {
        VStack {
            Text("Count: \(viewModel.count)")
            Text("Name: \(viewModel.name)")
        }
    }
}
// No @StateObject/@ObservedObject needed!
```

#### 3. Navigation

**NavigationStack (iOS 16+)** - Modern navigation
```swift
struct ContentView: View {
    @State private var path = NavigationPath()

    var body: some View {
        NavigationStack(path: $path) {
            List(1...10, id: \.self) { number in
                NavigationLink("Item \(number)", value: number)
            }
            .navigationDestination(for: Int.self) { number in
                DetailView(number: number)
            }
            .navigationTitle("Items")
        }
    }
}
```

**Programmatic Navigation:**
```swift
@State private var path = NavigationPath()

// Push
path.append(item)

// Pop to root
path.removeLast(path.count)

// Pop one
path.removeLast()
```

#### 4. Lists and Lazy Loading

```swift
struct ContentView: View {
    let items = Array(1...1000)

    var body: some View {
        List(items, id: \.self) { item in
            Text("Item \(item)")
        }

        // Or with sections
        List {
            Section("Section 1") {
                ForEach(items, id: \.self) { item in
                    Text("Item \(item)")
                }
            }
        }
    }
}
```

**LazyVGrid / LazyHGrid:**
```swift
let columns = [
    GridItem(.flexible()),
    GridItem(.flexible()),
    GridItem(.flexible())
]

ScrollView {
    LazyVGrid(columns: columns) {
        ForEach(0..<100) { index in
            Rectangle()
                .fill(.blue)
                .frame(height: 100)
        }
    }
}
```

#### 5. Animation

```swift
struct AnimationExample: View {
    @State private var isExpanded = false

    var body: some View {
        VStack {
            Rectangle()
                .fill(.blue)
                .frame(width: isExpanded ? 200 : 100,
                       height: isExpanded ? 200 : 100)
                .animation(.spring(response: 0.5), value: isExpanded)

            Button("Toggle") {
                withAnimation(.easeInOut(duration: 0.3)) {
                    isExpanded.toggle()
                }
            }
        }
    }
}
```

#### 6. UIKit Interoperability

**UIViewRepresentable** - Wrap UIKit views
```swift
struct ActivityIndicator: UIViewRepresentable {
    func makeUIView(context: Context) -> UIActivityIndicatorView {
        let view = UIActivityIndicatorView(style: .large)
        view.startAnimating()
        return view
    }

    func updateUIView(_ uiView: UIActivityIndicatorView, context: Context) {
        // Update when SwiftUI state changes
    }
}
```

**UIViewControllerRepresentable** - Wrap UIKit view controllers
```swift
struct ImagePicker: UIViewControllerRepresentable {
    @Binding var image: UIImage?
    @Environment(\.dismiss) var dismiss

    func makeUIViewController(context: Context) -> UIImagePickerController {
        let picker = UIImagePickerController()
        picker.delegate = context.coordinator
        return picker
    }

    func updateUIViewController(_ uiViewController: UIImagePickerController, context: Context) {}

    func makeCoordinator() -> Coordinator {
        Coordinator(self)
    }

    class Coordinator: NSObject, UIImagePickerControllerDelegate, UINavigationControllerDelegate {
        let parent: ImagePicker

        init(_ parent: ImagePicker) {
            self.parent = parent
        }

        func imagePickerController(_ picker: UIImagePickerController,
                                 didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
            if let image = info[.originalImage] as? UIImage {
                parent.image = image
            }
            parent.dismiss()
        }
    }
}
```

### Common SwiftUI Interview Questions

**Q: What's the difference between @State and @StateObject?**
- @State: For value types (String, Int, Bool, structs)
- @StateObject: For reference types (classes conforming to ObservableObject)

**Q: When do you use @ObservedObject vs @StateObject?**
- @StateObject: When the view **owns** the object (creates it)
- @ObservedObject: When the object is **passed in** from parent

**Q: How does SwiftUI determine when to update views?**
- When @State, @Binding, @StateObject, @ObservedObject properties change
- When @Published properties in ObservableObject change
- SwiftUI creates a dependency graph and only re-renders affected views

**Q: What's the difference between NavigationView and NavigationStack?**
- NavigationView: Deprecated, iOS 13-15 style
- NavigationStack: Modern, iOS 16+, better programmatic control, type-safe

---

## Modern Swift Concurrency

### Why It Matters
This is the **most significant change** to Swift since its introduction. async/await has replaced completion handlers as the standard for asynchronous programming. **Critical for 2025 interviews.**

### 1. async/await Basics

**Old Way (Completion Handlers):**
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

// Usage - callback hell
fetchData { result in
    switch result {
    case .success(let data):
        parseData(data) { parseResult in
            // More nesting...
        }
    case .failure(let error):
        print(error)
    }
}
```

**New Way (async/await):**
```swift
func fetchData() async throws -> Data {
    let (data, _) = try await URLSession.shared.data(from: url)
    return data
}

// Usage - linear code
do {
    let data = try await fetchData()
    let parsed = try await parseData(data)
    // Clean, readable
} catch {
    print(error)
}
```

### 2. Structured Concurrency

**Task** - Unit of asynchronous work
```swift
// In SwiftUI
Button("Load Data") {
    Task {
        do {
            let data = try await fetchData()
            self.items = data
        } catch {
            print(error)
        }
    }
}

// In UIKit
Task {
    let image = try await downloadImage()
    imageView.image = image
}
```

**Task Groups** - Multiple concurrent operations
```swift
func fetchMultipleImages() async throws -> [UIImage] {
    try await withThrowingTaskGroup(of: UIImage.self) { group in
        for url in imageURLs {
            group.addTask {
                try await downloadImage(from: url)
            }
        }

        var images: [UIImage] = []
        for try await image in group {
            images.append(image)
        }
        return images
    }
}
```

**async let** - Concurrent bindings
```swift
func loadMultiple() async throws {
    // These run concurrently
    async let user = fetchUser()
    async let posts = fetchPosts()
    async let comments = fetchComments()

    // Wait for all to complete
    let (u, p, c) = try await (user, posts, comments)
}
```

### 3. Actors - Safe Concurrency

**Problem: Data races**
```swift
class Counter {
    var value = 0

    func increment() {
        value += 1 // NOT thread-safe!
    }
}
```

**Solution: Actor**
```swift
actor Counter {
    var value = 0

    func increment() {
        value += 1 // Thread-safe!
    }

    func getValue() -> Int {
        return value
    }
}

// Usage
let counter = Counter()
Task {
    await counter.increment() // Must use await
    let value = await counter.getValue()
}
```

**Actor isolation** - Actors protect their state
```swift
actor BankAccount {
    private var balance: Double = 0

    func deposit(_ amount: Double) {
        balance += amount // Safe
    }

    func withdraw(_ amount: Double) -> Bool {
        guard balance >= amount else { return false }
        balance -= amount
        return true
    }

    func getBalance() -> Double {
        return balance
    }
}
```

### 4. @MainActor - UI Updates

**Ensuring main thread:**
```swift
@MainActor
class ViewModel: ObservableObject {
    @Published var items: [Item] = []

    func loadItems() async {
        // This is already on main actor
        items = try await fetchItems()
        // UI updates are safe
    }
}

// Or for specific functions
func updateUI() async {
    await MainActor.run {
        label.text = "Updated"
    }
}

// Or isolated parameter
@MainActor
func updateLabel(_ text: String) {
    label.text = text
}
```

### 5. Sendable - Safe Data Sharing

```swift
// Value types are automatically Sendable
struct User: Sendable {
    let id: Int
    let name: String
}

// Classes need explicit conformance
final class ThreadSafeCache: @unchecked Sendable {
    private let lock = NSLock()
    private var storage: [String: Any] = [:]

    func set(_ value: Any, for key: String) {
        lock.lock()
        defer { lock.unlock() }
        storage[key] = value
    }
}
```

### 6. AsyncSequence

```swift
// Streaming data
func fetchItems() async throws {
    let url = URL(string: "https://api.example.com/stream")!
    let (bytes, _) = try await URLSession.shared.bytes(from: url)

    for try await byte in bytes {
        process(byte)
    }
}

// Custom AsyncSequence
struct CountDown: AsyncSequence {
    typealias Element = Int
    let start: Int

    struct AsyncIterator: AsyncIteratorProtocol {
        var current: Int

        mutating func next() async -> Int? {
            guard current > 0 else { return nil }
            try? await Task.sleep(nanoseconds: 1_000_000_000)
            defer { current -= 1 }
            return current
        }
    }

    func makeAsyncIterator() -> AsyncIterator {
        AsyncIterator(current: start)
    }
}

// Usage
for await number in CountDown(start: 5) {
    print(number) // 5, 4, 3, 2, 1
}
```

### 7. Task Cancellation

```swift
let task = Task {
    for i in 1...100 {
        // Check for cancellation
        try Task.checkCancellation()

        // Or
        guard !Task.isCancelled else { return }

        await doWork(i)
    }
}

// Cancel from elsewhere
task.cancel()
```

### Common Interview Questions

**Q: What's the difference between async and await?**
- **async**: Marks function as asynchronous (can suspend)
- **await**: Marks suspension point where function might pause

**Q: Why use actors instead of DispatchQueue?**
- Actors provide compile-time safety against data races
- Automatic isolation, no need for manual locking
- Better performance, compiler optimizations

**Q: When do you use @MainActor?**
- For classes/functions that update UI
- For ObservableObject view models in SwiftUI
- Any code that must run on main thread

**Q: What's structured concurrency?**
- Parent tasks wait for child tasks to complete
- Automatic cancellation propagation
- Clear task hierarchy and lifetime

---

## Modern Swift Language Features

### 1. Property Wrappers (Swift 5.1)

**Built-in Examples:** @State, @Published, @Environment

**Custom Property Wrapper:**
```swift
@propertyWrapper
struct Clamped<Value: Comparable> {
    private var value: Value
    private let range: ClosedRange<Value>

    init(wrappedValue: Value, _ range: ClosedRange<Value>) {
        self.range = range
        self.value = min(max(wrappedValue, range.lowerBound), range.upperBound)
    }

    var wrappedValue: Value {
        get { value }
        set { value = min(max(newValue, range.lowerBound), range.upperBound) }
    }
}

// Usage
struct Game {
    @Clamped(0...100) var health = 100
}

var game = Game()
game.health = 150 // Automatically clamped to 100
```

### 2. Result Builders (Swift 5.4)

**Foundation of SwiftUI DSL:**
```swift
@resultBuilder
struct StringBuilder {
    static func buildBlock(_ components: String...) -> String {
        components.joined(separator: "\n")
    }
}

@StringBuilder
func makeGreeting() -> String {
    "Hello"
    "World"
    "!"
}

print(makeGreeting())
// Hello
// World
// !
```

### 3. Opaque Types (some/any)

**some** - Opaque return type
```swift
func makeView() -> some View {
    Text("Hello")
}
// Compiler knows exact type, but caller doesn't
```

**any** - Existential type
```swift
func makeAnimal() -> any Animal {
    if Bool.random() {
        return Dog()
    } else {
        return Cat()
    }
}
// Type-erased, more flexible but less performant
```

### 4. Macros (Swift 5.9)

**@Observable** - Most common
```swift
@Observable
class ViewModel {
    var name = ""
    var age = 0
}
// Compiler generates observation code
```

**Custom Macro Example:**
```swift
@attached(member)
public macro AddInit() = #externalMacro(...)

@AddInit
struct User {
    var name: String
    var age: Int
}
// Generates init automatically
```

### 5. if/switch Expressions (Swift 5.9)

```swift
let message = if isLoggedIn {
    "Welcome back"
} else {
    "Please log in"
}

let color = switch status {
case .success: .green
case .warning: .yellow
case .error: .red
}
```

---

## iOS 14-18 Frameworks & APIs

### WidgetKit (iOS 14+)

**Home screen widgets:**
```swift
struct SimpleWidget: Widget {
    let kind: String = "SimpleWidget"

    var body: some WidgetConfiguration {
        StaticConfiguration(kind: kind, provider: Provider()) { entry in
            SimpleWidgetView(entry: entry)
        }
        .configurationDisplayName("My Widget")
        .description("This is an example widget.")
        .supportedFamilies([.systemSmall, .systemMedium, .systemLarge])
    }
}

struct Provider: TimelineProvider {
    func placeholder(in context: Context) -> SimpleEntry {
        SimpleEntry(date: Date())
    }

    func getSnapshot(in context: Context, completion: @escaping (SimpleEntry) -> ()) {
        let entry = SimpleEntry(date: Date())
        completion(entry)
    }

    func getTimeline(in context: Context, completion: @escaping (Timeline<SimpleEntry>) -> ()) {
        var entries: [SimpleEntry] = []
        let currentDate = Date()
        for hourOffset in 0..<5 {
            let entryDate = Calendar.current.date(byAdding: .hour, value: hourOffset, to: currentDate)!
            let entry = SimpleEntry(date: entryDate)
            entries.append(entry)
        }

        let timeline = Timeline(entries: entries, policy: .atEnd)
        completion(timeline)
    }
}
```

### Live Activities (iOS 16+)

**Dynamic Island support:**
```swift
struct DeliveryActivityAttributes: ActivityAttributes {
    public struct ContentState: Codable, Hashable {
        var status: String
        var estimatedTime: Date
    }

    var orderNumber: String
}

// Start activity
let attributes = DeliveryActivityAttributes(orderNumber: "12345")
let initialState = DeliveryActivityAttributes.ContentState(
    status: "Preparing",
    estimatedTime: Date().addingTimeInterval(1800)
)

let activity = try Activity.request(
    attributes: attributes,
    contentState: initialState,
    pushType: nil
)

// Update activity
await activity.update(using: DeliveryActivityAttributes.ContentState(
    status: "On the way",
    estimatedTime: Date().addingTimeInterval(600)
))

// End activity
await activity.end(dismissalPolicy: .immediate)
```

### SwiftData (iOS 17+)

**Modern Core Data replacement:**
```swift
import SwiftData

@Model
class Item {
    @Attribute(.unique) var id: UUID
    var name: String
    var createdAt: Date

    @Relationship(deleteRule: .cascade)
    var tags: [Tag]

    init(name: String) {
        self.id = UUID()
        self.name = name
        self.createdAt = Date()
        self.tags = []
    }
}

@Model
class Tag {
    var name: String
    var item: Item?

    init(name: String) {
        self.name = name
    }
}

// App setup
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
        .modelContainer(for: [Item.self, Tag.self])
    }
}

// Using in views
struct ContentView: View {
    @Environment(\.modelContext) private var modelContext
    @Query private var items: [Item]

    var body: some View {
        List(items) { item in
            Text(item.name)
        }
        .toolbar {
            Button("Add") {
                let newItem = Item(name: "New")
                modelContext.insert(newItem)
            }
        }
    }
}
```

### Swift Charts (iOS 16+)

```swift
import Charts

struct SalesData {
    let month: String
    let amount: Double
}

struct SalesChart: View {
    let data: [SalesData]

    var body: some View {
        Chart(data) { item in
            BarMark(
                x: .value("Month", item.month),
                y: .value("Amount", item.amount)
            )
            .foregroundStyle(.blue)
        }
        .chartXAxis {
            AxisMarks(values: .automatic)
        }
        .chartYAxis {
            AxisMarks(position: .leading)
        }
    }
}
```

### App Intents (iOS 16+)

**Shortcuts integration:**
```swift
import AppIntents

struct OrderCoffeeIntent: AppIntent {
    static var title: LocalizedStringResource = "Order Coffee"
    static var description = IntentDescription("Orders your favorite coffee")

    @Parameter(title: "Coffee Type")
    var coffeeType: String

    func perform() async throws -> some IntentResult {
        // Place order
        return .result(dialog: "Your \(coffeeType) is on the way!")
    }
}
```

---

## Privacy & Security Requirements

### App Tracking Transparency (iOS 14.5+)

**REQUIRED before tracking:**
```swift
import AppTrackingTransparency

func requestTracking() {
    ATTrackingManager.requestTrackingAuthorization { status in
        switch status {
        case .authorized:
            // User allowed tracking
            print("Authorized")
        case .denied:
            // User denied
            print("Denied")
        case .notDetermined:
            // Not yet asked
            print("Not determined")
        case .restricted:
            // Parental controls
            print("Restricted")
        @unknown default:
            break
        }
    }
}
```

**Info.plist requirement:**
```xml
<key>NSUserTrackingUsageDescription</key>
<string>We use tracking to personalize ads and improve your experience.</string>
```

### Privacy Manifests (Required 2024+)

**PrivacyInfo.xcprivacy:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN">
<plist version="1.0">
<dict>
    <key>NSPrivacyTracking</key>
    <false/>
    <key>NSPrivacyTrackingDomains</key>
    <array/>
    <key>NSPrivacyCollectedDataTypes</key>
    <array>
        <dict>
            <key>NSPrivacyCollectedDataType</key>
            <string>NSPrivacyCollectedDataTypeEmailAddress</string>
            <key>NSPrivacyCollectedDataTypeLinked</key>
            <true/>
            <key>NSPrivacyCollectedDataTypePurposes</key>
            <array>
                <string>NSPrivacyCollectedDataTypePurposeAppFunctionality</string>
            </array>
        </dict>
    </array>
    <key>NSPrivacyAccessedAPITypes</key>
    <array>
        <dict>
            <key>NSPrivacyAccessedAPIType</key>
            <string>NSPrivacyAccessedAPICategoryUserDefaults</string>
            <key>NSPrivacyAccessedAPITypeReasons</key>
            <array>
                <string>CA92.1</string>
            </array>
        </dict>
    </array>
</dict>
</plist>
```

---

## Modern Architecture Patterns

### SwiftUI + MVVM + async/await

```swift
@Observable
class HomeViewModel {
    var items: [Item] = []
    var isLoading = false
    var errorMessage: String?

    private let service: ItemService

    init(service: ItemService = ItemService()) {
        self.service = service
    }

    @MainActor
    func loadItems() async {
        isLoading = true
        errorMessage = nil

        do {
            items = try await service.fetchItems()
        } catch {
            errorMessage = error.localizedDescription
        }

        isLoading = false
    }
}

struct HomeView: View {
    @State private var viewModel = HomeViewModel()

    var body: some View {
        List(viewModel.items) { item in
            Text(item.name)
        }
        .overlay {
            if viewModel.isLoading {
                ProgressView()
            }
        }
        .alert("Error", isPresented: .constant(viewModel.errorMessage != nil)) {
            Button("OK") {
                viewModel.errorMessage = nil
            }
        } message: {
            Text(viewModel.errorMessage ?? "")
        }
        .task {
            await viewModel.loadItems()
        }
    }
}
```

---

## Migration Guides

### Completion Handlers → async/await

**Before:**
```swift
func fetchUser(id: String, completion: @escaping (Result<User, Error>) -> Void) {
    URLSession.shared.dataTask(with: url) { data, response, error in
        if let error = error {
            completion(.failure(error))
            return
        }

        guard let data = data else {
            completion(.failure(NetworkError.noData))
            return
        }

        do {
            let user = try JSONDecoder().decode(User.self, from: data)
            completion(.success(user))
        } catch {
            completion(.failure(error))
        }
    }.resume()
}
```

**After:**
```swift
func fetchUser(id: String) async throws -> User {
    let (data, _) = try await URLSession.shared.data(from: url)
    return try JSONDecoder().decode(User.self, from: data)
}
```

### UIKit → SwiftUI

**UITableView → List:**
```swift
// UIKit
class TableViewController: UITableViewController {
    var items: [String] = []

    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return items.count
    }

    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
        cell.textLabel?.text = items[indexPath.row]
        return cell
    }
}

// SwiftUI
struct ListView: View {
    let items: [String]

    var body: some View {
        List(items, id: \.self) { item in
            Text(item)
        }
    }
}
```

---

## Interview Preparation Tips

### Topics Ranked by Interview Frequency (2025)

**Tier 1 - Asked in 80%+ of interviews:**
1. SwiftUI basics (state management, views, navigation)
2. async/await fundamentals
3. Memory management (still essential)
4. UIKit basics (still asked, especially lifecycle)
5. Basic algorithms & data structures

**Tier 2 - Asked in 40-60% of interviews:**
6. Actors and concurrency safety
7. MVVM architecture
8. Networking with URLSession
9. SwiftUI + UIKit interop
10. WidgetKit basics

**Tier 3 - Nice to have:**
11. SwiftData/Core Data
12. Live Activities
13. App Intents
14. Privacy requirements (ATT)
15. Swift Charts

### What to Prioritize by Experience Level

**Junior (0-2 years):**
- SwiftUI fundamentals
- async/await basics
- UIKit essentials
- Simple MVVM
- Easy algorithms

**Mid-level (2-5 years):**
- Advanced SwiftUI
- Actors and structured concurrency
- SwiftUI + UIKit integration
- Modern architecture
- System design basics

**Senior (5+ years):**
- All of above plus:
- Performance optimization
- Large-scale architecture
- Team practices
- CI/CD and tooling
- Migration strategies

---

## Key Takeaways

✅ **SwiftUI is now essential** - Not knowing it in 2025 is like not knowing UIKit in 2020

✅ **async/await replaced completion handlers** - This is the standard now

✅ **Actors solve data race problems** - Safer than manual threading

✅ **Privacy is mandatory** - ATT, Privacy Manifests are requirements

✅ **SwiftData is the future** - Though Core Data knowledge still valuable

✅ **But UIKit isn't dead** - Most companies have legacy code, need both

---

**Remember:** In 2025 interviews, you need to know **both old and new** - UIKit AND SwiftUI, GCD AND async/await. The best candidates can bridge both worlds.
