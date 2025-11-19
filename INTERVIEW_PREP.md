# 6-Week iOS Developer Interview Preparation Plan

**Start Date:** Week of [INSERT DATE]
**Interview Date:** ~6 weeks from start
**Experience Level:** Junior (0-2 years)
**Daily Time Commitment:** 2-3 hours recommended

---

## Overview

This plan focuses on **essential topics** most commonly asked in iOS developer interviews for junior positions. Each week builds on the previous, starting with fundamentals and progressing to more advanced concepts.

**Topics Coverage (Updated for 2025):**
- 30% Modern Swift & Language Features (async/await, SwiftUI)
- 25% UIKit & iOS Frameworks (Legacy + Modern)
- 25% Architecture & Design Patterns (MVVM, Actors)
- 20% Algorithms & Data Structures

**🆕 Modern iOS Focus:**
This plan has been updated for 2025 to include SwiftUI, async/await, and modern Swift concurrency. See [MODERN_IOS_2025.md](MODERN_IOS_2025.md) for comprehensive coverage of 2020-2025 changes.

---

## Week 1: Swift & Memory Management Fundamentals

### Day 1: Swift Basics
- [ ] **Closures** - Capturing values, escaping vs non-escaping
- [ ] **Optionals** - Unwrapping, optional chaining, nil coalescing
- [ ] Study: [Resources/Swift/Closures](RoadmapProject/Script/Generated/Resources/Practical%20knowledge/Languages/Swift/Closures/RESOURCES.md)
- [ ] Practice: Write 3-5 closure examples (map, filter, reduce)

### Day 2: Swift Types
- [ ] **Value vs Reference Types** - Structs vs Classes
- [ ] **Protocols** - Protocol-oriented programming basics
- [ ] Study: [Resources/Swift/Structs](RoadmapProject/Script/Generated/Resources/Practical%20knowledge/Languages/Swift/Structs/RESOURCES.md)
- [ ] Practice: Create examples showing copy vs reference behavior

### Day 3: Memory Management - Part 1
- [ ] **Stack vs Heap** - How memory is allocated
- [ ] **ARC (Automatic Reference Counting)** - How it works
- [ ] Study: [Resources/Memory Management/ARC](RoadmapProject/Script/Generated/Resources/Practical%20knowledge/Memory%20management/ARC/RESOURCES.md)
- [ ] Practice: Identify which types go on stack vs heap

### Day 4: Memory Management - Part 2
- [ ] **Retain Cycles** - What they are and how to identify them
- [ ] **Weak and Unowned References** - When to use each
- [ ] Study: [Resources/Memory Management/Retain Cycles](RoadmapProject/Script/Generated/Resources/Practical%20knowledge/Memory%20management/Retain%20cycles/RESOURCES.md)
- [ ] Practice: Fix retain cycle examples with closures

### Day 5: SwiftUI Basics 🆕
- [ ] **Declarative UI** - SwiftUI vs UIKit paradigm
- [ ] **Basic Views** - Text, Image, VStack, HStack
- [ ] **@State** - Local view state management
- [ ] Study: [MODERN_IOS_2025.md - SwiftUI Section](MODERN_IOS_2025.md#swiftui---the-modern-ui-framework)
- [ ] Practice: Build simple counter app in SwiftUI

### Day 6-7: Review & Practice
- [ ] Review all Week 1 topics
- [ ] Complete 5 easy Swift problems on LeetCode
- [ ] Write summary notes of key concepts

---

## Week 2: UIKit Fundamentals

### Day 8: UIViewController Lifecycle
- [ ] **Lifecycle Methods** - viewDidLoad, viewWillAppear, etc.
- [ ] **When each method is called** - Order and use cases
- [ ] Study: [Resources/UIKit/UIViewController](RoadmapProject/Script/Generated/Resources/Practical%20knowledge/Cocoa%20Touch/UIKit/UIViewController/RESOURCES.md)
- [ ] Practice: Create app demonstrating lifecycle with print statements

### Day 9: UIView & Layout - Part 1
- [ ] **UIView Basics** - Frame, bounds, center
- [ ] **Frame-based Layout** - Understanding CGRect
- [ ] Study: [Resources/UIKit/UIView](RoadmapProject/Script/Generated/Resources/Practical%20knowledge/Cocoa%20Touch/UIKit/UIView/RESOURCES.md)
- [ ] Practice: Create custom view with frame-based layout

### Day 10: UIView & Layout - Part 2
- [ ] **Auto Layout** - Constraints, priorities
- [ ] **Programmatic vs Interface Builder** - Pros and cons
- [ ] Study: [Resources/UIKit/Layout/Autolayout](RoadmapProject/Script/Generated/Resources/Practical%20knowledge/Cocoa%20Touch/UIKit/Layout/Autolayout/RESOURCES.md)
- [ ] Practice: Build simple form with Auto Layout constraints

### Day 11: UITableView
- [ ] **DataSource & Delegate** - Required methods
- [ ] **Cell Reuse** - How and why it works
- [ ] Study: [Resources/UIKit/UITableView](RoadmapProject/Script/Generated/Resources/Practical%20knowledge/Cocoa%20Touch/UIKit/UITableView/RESOURCES.md)
- [ ] Practice: Build table view with custom cells

### Day 12: Foundation Framework
- [ ] **Notifications vs Delegation** - When to use each
- [ ] **Collections** - Array, Dictionary, Set operations
- [ ] Study: [Resources/Foundation/Notifications vs Delegation](RoadmapProject/Script/Generated/Resources/Practical%20knowledge/Cocoa%20Touch/Foundation/Notifications%2C%20Delegation%20and%20Observing/RESOURCES.md)
- [ ] Practice: Implement both notification and delegation patterns

### Day 13: SwiftUI State Management 🆕
- [ ] **@Binding** - Two-way data flow
- [ ] **@StateObject** - Owning ObservableObject
- [ ] **@ObservedObject** - Observing passed objects
- [ ] Study: [MODERN_IOS_2025.md - State Management](MODERN_IOS_2025.md#2-state-management-critical-for-interviews)
- [ ] Practice: Build SwiftUI app with multiple views sharing data

### Day 14: Review & Practice
- [ ] Review all Week 2 topics
- [ ] Build small app combining UIKit concepts
- [ ] Update progress tracker

---

## Week 3: Architecture Patterns & Design

### Day 15: MVC Pattern
- [ ] **Model-View-Controller** - Understanding each layer
- [ ] **Massive View Controller Problem** - Common pitfalls
- [ ] Study: [Resources/Architecture/MVC](RoadmapProject/Script/Generated/Resources/Practical%20knowledge/Software%20Architecture/Architectures/MVC/RESOURCES.md)
- [ ] Practice: Refactor simple app into proper MVC

### Day 16: MVVM with SwiftUI 🆕
- [ ] **Model-View-ViewModel** - Benefits over MVC
- [ ] **@Observable (iOS 17+)** - Modern observation pattern
- [ ] **MVVM in SwiftUI** - Natural fit with declarative UI
- [ ] Study: [MODERN_IOS_2025.md - Modern Architecture](MODERN_IOS_2025.md#swiftui--mvvm--asyncawait)
- [ ] Practice: Build SwiftUI app with ViewModel pattern

### Day 17: Design Patterns - Part 1
- [ ] **Singleton** - When to use, pros and cons
- [ ] **Delegate Pattern** - Implementation details
- [ ] **Observer Pattern** - NotificationCenter usage
- [ ] Study: [Resources/Design Patterns](RoadmapProject/Script/Generated/Resources/Practical%20knowledge/Software%20Architecture/Design%20Patterns/RESOURCES.md)
- [ ] Practice: Implement each pattern in small examples

### Day 18: Design Patterns - Part 2
- [ ] **Factory Pattern** - Object creation
- [ ] **Adapter Pattern** - Interface conversion
- [ ] **Decorator Pattern** - Dynamic behavior addition
- [ ] Study: [Resources/Design Patterns/Structural](RoadmapProject/Script/Generated/Resources/Practical%20knowledge/Software%20Architecture/Design%20Patterns/Structural/RESOURCES.md)
- [ ] Practice: Identify patterns in iOS frameworks

### Day 19: SOLID Principles - Part 1
- [ ] **Single Responsibility Principle**
- [ ] **Open/Closed Principle**
- [ ] Study: [Resources/Design Principles/SOLID](RoadmapProject/Script/Generated/Resources/Practical%20knowledge/Software%20Architecture/Design%20Principles/SOLID/RESOURCES.md)
- [ ] Practice: Refactor code to follow SRP

### Day 20: SOLID Principles - Part 2
- [ ] **Liskov Substitution Principle**
- [ ] **Interface Segregation Principle**
- [ ] **Dependency Inversion Principle**
- [ ] Practice: Apply all SOLID principles to a sample project

### Day 21: Review & Practice
- [ ] Review architecture patterns
- [ ] Compare MVC vs MVVM with examples
- [ ] Write notes on when to use each pattern

---

## Week 4: Concurrency & Advanced Topics

### Day 22: Multithreading Basics
- [ ] **Threads** - Main thread vs background threads
- [ ] **Synchronization** - Race conditions, thread safety
- [ ] Study: [Resources/Multithreading](RoadmapProject/Script/Generated/Resources/Practical%20knowledge/Multithreading%20and%20concurrency/RESOURCES.md)
- [ ] Practice: Identify threading issues in sample code

### Day 23: async/await - Modern Concurrency 🆕
- [ ] **async/await Basics** - Modern asynchronous programming
- [ ] **Task** - Structured concurrency
- [ ] **Async sequences** - Streaming data
- [ ] Study: [MODERN_IOS_2025.md - async/await](MODERN_IOS_2025.md#1-asyncawait-basics)
- [ ] Practice: Convert completion handler code to async/await

### Day 24: Actors & Concurrency Safety 🆕
- [ ] **Actors** - Safe data isolation
- [ ] **@MainActor** - UI thread safety
- [ ] **Sendable** - Safe data sharing across concurrency domains
- [ ] Study: [MODERN_IOS_2025.md - Actors](MODERN_IOS_2025.md#3-actors---safe-concurrency)
- [ ] Practice: Refactor class with race conditions to use Actor

### Day 25: Modern Swift Features 🆕
- [ ] **Property Wrappers** - @State, @Published, custom wrappers
- [ ] **Result Builders** - SwiftUI DSL foundation
- [ ] **Opaque Types** - some vs any
- [ ] Study: [MODERN_IOS_2025.md - Modern Swift Language](MODERN_IOS_2025.md#modern-swift-language-features)
- [ ] Practice: Create custom property wrapper

### Day 26: Testing Basics
- [ ] **Unit Testing** - XCTest framework
- [ ] **Test-Driven Development** - Basic concepts
- [ ] Study: [Resources/Testing/Unit Tests](RoadmapProject/Script/Generated/Resources/Practical%20knowledge/UnitTesting/Unit%20Tests/RESOURCES.md)
- [ ] Practice: Write unit tests for previous code examples

### Day 27: GCD & Legacy Concurrency
- [ ] **GCD Basics** - Dispatch queues (still used in legacy code)
- [ ] **Serial vs Concurrent queues** - Understanding the difference
- [ ] **Comparing GCD vs async/await** - When to use each
- [ ] Study: [Resources/GCD](RoadmapProject/Script/Generated/Resources/Practical%20knowledge/Multithreading%20and%20concurrency/GCD/RESOURCES.md)
- [ ] Practice: Understand how to maintain legacy GCD code

### Day 28: Review & Practice
- [ ] Review Week 4 topics (async/await, Actors, modern Swift)
- [ ] Build app using async/await for networking + @MainActor for UI
- [ ] Write tests for the app
- [ ] Compare: Build same feature with GCD vs async/await

---

## Week 5: Algorithms & Data Structures

### Day 29: Big-O Notation
- [ ] **Time Complexity** - O(1), O(n), O(log n), O(n²)
- [ ] **Space Complexity** - Memory analysis
- [ ] Study: [Resources/Algorithms/Big-O](RoadmapProject/Script/Generated/Resources/Computer%20Science%20knowledge/Algorithms/Big-O%20Notation/RESOURCES.md)
- [ ] Practice: Analyze complexity of 10 functions

### Day 30: Arrays & Strings
- [ ] **Array operations** - Search, insert, delete
- [ ] **String manipulation** - Common patterns
- [ ] Practice: 5 LeetCode Easy problems on arrays
- [ ] Practice: 5 LeetCode Easy problems on strings

### Day 31: Linked Lists & Stacks
- [ ] **Linked List** - Implementation and operations
- [ ] **Stack** - LIFO implementation
- [ ] Study: [Resources/ADT/List](RoadmapProject/Script/Generated/Resources/Computer%20Science%20knowledge/Abstract%20Data%20Types/List/RESOURCES.md)
- [ ] Practice: Implement Stack in Swift

### Day 32: Trees & Binary Search
- [ ] **Binary Trees** - Traversal (inorder, preorder, postorder)
- [ ] **Binary Search Trees** - Search, insert, delete
- [ ] Study: [Resources/Algorithms/Trees](RoadmapProject/Script/Generated/Resources/Computer%20Science%20knowledge/Algorithms/Trees/RESOURCES.md)
- [ ] Practice: 3 LeetCode Easy tree problems

### Day 33: Sorting & Searching
- [ ] **Common Sorts** - Bubble, Merge, Quick sort
- [ ] **Binary Search** - Implementation and variations
- [ ] Study: [Resources/Algorithms/Sorting](RoadmapProject/Script/Generated/Resources/Computer%20Science%20knowledge/Algorithms/Sorting/RESOURCES.md)
- [ ] Practice: Implement 2 sorting algorithms

### Day 34: Hash Tables & Dictionaries
- [ ] **Hash Table** - How it works
- [ ] **Dictionary operations** - Swift Dictionary
- [ ] Study: [Resources/ADT/Map](RoadmapProject/Script/Generated/Resources/Computer%20Science%20knowledge/Abstract%20Data%20Types/Map/RESOURCES.md)
- [ ] Practice: 5 LeetCode problems using hash maps

### Day 35: Algorithm Practice Day
- [ ] Complete 10 mixed LeetCode Easy/Medium problems
- [ ] Focus on problems similar to interview questions
- [ ] Review and understand all solutions

---

## Week 6: Review, System Design & Mock Interviews

### Day 36: Modern iOS Features 🆕
- [ ] **WidgetKit** - Home screen widgets basics
- [ ] **App Intents** - Shortcuts integration
- [ ] **Privacy Requirements** - App Tracking Transparency (ATT)
- [ ] Study: [MODERN_IOS_2025.md - iOS 14-18 Frameworks](MODERN_IOS_2025.md#ios-14-18-frameworks--apis)
- [ ] Practice: Build simple widget

### Day 37: Performance & Optimization
- [ ] **Memory Footprint** - Reducing memory usage
- [ ] **FPS** - Smooth scrolling techniques
- [ ] Study: [Resources/Performance](RoadmapProject/Script/Generated/Resources/Practical%20knowledge/Performance%20optimization/RESOURCES.md)
- [ ] Practice: Optimize a slow table view

### Day 38: Data Persistence - Legacy & Modern 🆕
- [ ] **SwiftData (iOS 17+)** - Modern persistence framework
- [ ] **Core Data** - Legacy approach (still widely used)
- [ ] **Comparing SwiftData vs Core Data** - Migration path
- [ ] Study: [MODERN_IOS_2025.md - SwiftData](MODERN_IOS_2025.md#swiftdata-ios-17)
- [ ] Practice: Build simple SwiftData app

### Day 39: Comprehensive Review - Day 1
- [ ] Review Week 1-2 topics (Swift, SwiftUI, UIKit)
- [ ] Compare SwiftUI vs UIKit approaches side by side
- [ ] Redo challenging exercises
- [ ] Update weak areas list in progress tracker
- [ ] Practice explaining modern concepts out loud (async/await, SwiftUI state)

### Day 40: Comprehensive Review - Day 2
- [ ] Review Week 3-4 topics (MVVM with SwiftUI, async/await, Actors)
- [ ] Review design pattern examples
- [ ] Go through interview cheatsheet (especially modern iOS section)
- [ ] Practice whiteboard coding with async/await patterns
- [ ] Review [MODERN_IOS_2025.md](MODERN_IOS_2025.md) for quick refresh

### Day 41: Mock Interview Day
- [ ] **Technical Round** - Solve 2-3 coding problems (60 min)
- [ ] **iOS Concepts** - Answer 10-15 theoretical questions (30 min)
- [ ] **Architecture Discussion** - Design a feature (30 min)
- [ ] Get feedback or self-review performance

### Day 42: Final Review
- [ ] Review notes and cheatsheet
- [ ] Go through weak areas one more time
- [ ] Practice common interview questions
- [ ] Prepare questions to ask interviewer
- [ ] Get good rest before interview

---

## Additional Resources

### Modern iOS Resources (2025)
- **[MODERN_IOS_2025.md](MODERN_IOS_2025.md)** - Complete guide to 2020-2025 changes
- **Apple Developer Documentation** - SwiftUI, async/await official docs
- **WWDC Videos** - WWDC 2021 (async/await), WWDC 2019-2024 (SwiftUI evolution)
- **100 Days of SwiftUI** by Paul Hudson - Comprehensive SwiftUI course
- **Swift Concurrency** by Apple - Official async/await guide

### LeetCode Problem Lists
- **Easy (Complete 20+):** Two Sum, Reverse String, Valid Anagram, Merge Sorted Array
- **Medium (Complete 10+):** 3Sum, Group Anagrams, Product of Array Except Self

### Mock Interview Resources
- **Pramp** - Free peer mock interviews
- **LeetCode Mock** - Simulated coding interviews
- **Friends/Mentors** - Ask experienced developers for mock interviews

### Study Tips
1. **Active Learning:** Code along with tutorials, don't just read
2. **Spaced Repetition:** Review topics multiple times over weeks
3. **Teaching:** Explain concepts to others or write blog posts
4. **Practical Application:** Build small projects using what you learn
5. **Consistency:** 2-3 hours daily is better than cramming
6. **Modern First:** Learn SwiftUI and async/await alongside UIKit and GCD - you need both in 2025
7. **Practice Migration:** Convert UIKit code to SwiftUI, completion handlers to async/await

### Before Interview Day
- [ ] Review your completed projects and be ready to discuss them
- [ ] Prepare STAR method examples for behavioral questions
- [ ] Test your setup if it's a remote interview
- [ ] Have pen/paper ready for problem solving
- [ ] Prepare thoughtful questions about the company/role

---

## Progress Tracking

Use [PROGRESS.md](PROGRESS.md) to track your daily completion and notes.

Use [INTERVIEW_CHEATSHEET.md](INTERVIEW_CHEATSHEET.md) for quick review before interview.

Use [MODERN_IOS_2025.md](MODERN_IOS_2025.md) for comprehensive modern iOS topics reference.

---

**Good luck with your interview preparation!**

**Remember:** In 2025, you need to know both old and new - UIKit AND SwiftUI, GCD AND async/await. The best candidates can bridge both worlds. Consistency and understanding concepts deeply is better than rushing through topics.
