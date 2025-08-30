### Phase 1: Foundations & Core Syntax (Weeks 1-8)

---

#### **Week 1: The Grand Setup & The First Token**
- **Requirement:** Set up the project environment and implement a lexer for basic tokens (keywords, identifiers, numbers, operators).
- **Uses this concept:** Lexical Analysis.
- **Learn this:** Regular Expressions, Finite Automata (NFAs/DFAs), and how they map to tokenizing source code.
- **Problem:** Write a lexer that can take `let x: int = 10 + y;` and produce a stream of tokens: `LET`, `IDENT("x")`, `COLON`, `TYPE("int")`, `EQ`, `INT(10)`, `PLUS`, `IDENT("y")`, `SEMICOLON`.
- **Sources:**
  - **Book:** "Crafting Interpreters" by Robert Nystrom (Part II, Chapter 4: Scanning). [Online Version](https://craftinginterpreters.com/scanning.html)
  - **Tool:** Use a lexer generator like `Flex` for learning, but write your own by hand for the project to understand the mechanics.
  - **Blog:** "Let's Build a Simple Interpreter" by Ruslan Spivak.
- **Teacher’s note:** Welcome to the journey! Don't get bogged down in making the perfect lexer. A simple, hand-rolled one that handles basic syntax is the perfect start. You will iterate on it constantly.

---

#### **Week 2: Parsing Expressions - The Heart of Logic**
- **Requirement:** Implement a recursive descent parser for arithmetic expressions, handling operator precedence and associativity.
- **Uses this concept:** Parsing, Abstract Syntax Trees (AST), Pratt Parsing.
- **Learn this:** The difference between token streams and structured data (ASTs), context-free grammars (CFGs), and how to handle operator precedence (e.g., `*` before `+`).
- **Problem:** Parse `1 + 2 * 3` into an AST that correctly represents `1 + (2 * 3)`.
- **Sources:**
  - **Book:** "Crafting Interpreters" (Chapter 6: Parsing Expressions). [Online Version](https://craftinginterpreters.com/parsing-expressions.html)
  - **Video:** "Pratt Parsers: Expression Parsing Made Easy" by Thorsten Ball.
- **Teacher’s note:** Pratt parsing can feel like magic at first. Trace its execution on paper with a simple expression. Once it clicks, you'll see how elegant it is for handling expressions.

---

#### **Week 3: Parsing Statements & Control Flow**
- **Requirement:** Extend the parser to handle statements: variable declarations (`let`), assignments, and `if`/`else` control flow.
- **Uses this concept:** Top-Down Recursive Descent Parsing.
- **Learn this:** The distinction between expressions (which evaluate to a value) and statements (which perform an action).
- **Problem:** Parse a simple function body containing `let`, assignment, and an `if-else` block into a coherent AST.
- **Sources:**
  - **Book:** "Crafting Interpreters" (Chapter 8: Statements and State). [Online Version](https://craftinginterpreters.com/statements-and-state.html)
  - **Reference:** The grammar files for a simple C-like language (e.g., Tiny C Compiler).
- **Teacher’s note:** Your AST is now starting to look like a real program. This is a huge milestone. Start thinking about how you'll represent types in your AST nodes.

---

#### **Week 4: The Backend Connection - Introduction to LLVM**
- **Requirement:** Set up the LLVM toolchain and generate LLVM Intermediate Representation (IR) for a simple "add" function.
- **Uses this concept:** Intermediate Representation (IR), Code Generation.
- **Learn this:** The basics of LLVM IR syntax, the concept of a compiler backend, and how to use the LLVM C++ (or C) API to build IR.
- **Problem:** Write a C++ program using the LLVM API that programmatically generates the IR for a function `int add(int a, int b) { return a + b; }`, then use `llc` to compile it to machine code.
- **Sources:**
  - **Tutorial:** "My First Language Frontend with LLVM Tutorial". [Official LLVM Tutorial](https://llvm.org/docs/tutorial/MyFirstLanguageFrontend/index.html)
  - **Docs:** LLVM Language Reference Manual.
- **Teacher’s note:** LLVM is a beast, but its core concepts are straightforward. Focus on the "Kaleidoscope" tutorial. Don't try to understand everything at once; just get your first bit of IR generated.

---

#### **Week 5: "Hello, World!" - End-to-End Compilation**
- **Requirement:** Connect your frontend (lexer/parser) to your backend (LLVM IR generator). Compile a simple function that returns an integer constant.
- **Uses this concept:** Compiler Pipelines, AST Traversal.
- **Learn this:** How to walk your AST (Visitor pattern) and translate each node into LLVM IR.
- **Problem:** Make your compiler take a source file `main.aura` containing `fn main() -> int { return 42; }` and produce a runnable executable that returns the exit code 42.
- **Sources:**
  - **Tutorial:** Continue with the LLVM Kaleidoscope tutorial, applying it to your own AST structure.
- **Teacher’s note:** This is the most rewarding week so far. Seeing your language produce a real, running executable is a major breakthrough. It proves your core pipeline works.

---

#### **Week 6: Variables & Memory**
- **Requirement:** Implement local variables. Generate LLVM IR for variable declaration, assignment, and usage.
- **Uses this concept:** Symbol Tables, Stack Allocation (`alloca`).
- **Learn this:** How local variables are stored on the stack and how a symbol table maps variable names to their memory locations or values.
- **Problem:** Compile `fn main() -> int { let x: int = 10; let y: int = 20; return x + y; }` to a working executable.
- **Sources:**
  - **Book:** "Engineering a Compiler" by Cooper and Torczon (Chapter 2: Symbol Tables).
  - **LLVM Docs:** Search for the `alloca` instruction.
- **Teacher’s note:** The symbol table is the brain of your compiler's frontend. A simple `std::map` or hash table is a great way to start. Think about variable scope now, as it will be crucial later.

---

#### **Week 7: Control Flow Compilation**
- **Requirement:** Generate LLVM IR for `if`/`else` statements and `while` loops.
- **Uses this concept:** Control Flow Graphs (CFGs), Basic Blocks, Branching Instructions.
- **Learn this:** How high-level control flow maps to low-level conditional branches (`br`) and labels in LLVM IR.
- **Problem:** Compile a function with a `while` loop that counts down and returns a value.
- **Sources:**
  - **LLVM Tutorial:** Chapter 5 covers `if/then/else`.
  - **Blog:** "How to compile a while loop" (many resources cover this basic pattern).
- **Teacher’s note:** Drawing the control flow graphs on paper first will make writing the code generation logic much easier. Think in terms of "entry block," "then block," "else block," and "merge block."

---

#### **Week 8: Functions and Calls**
- **Requirement:** Implement function definitions and function calls.
- **Uses this concept:** Calling Conventions, Application Binary Interface (ABI).
- **Learn this:** How function arguments are passed and return values are handled at the machine level.
- **Problem:** Compile a program with two functions, where `main` calls another function `add(a, b)` and returns its result.
- **Sources:**
  - **LLVM Tutorial:** Chapter 4 covers functions.
- **Teacher’s note:** For now, stick to simple types like integers. Handling complex types (structs, arrays) across function calls comes later. You now have a compiler for a basic but Turing-complete language!

---

### Phase 2: The Type System & Safety Model (Weeks 9-18)

---

#### **Week 9: Type Checking & Inference (Part 1)**
- **Requirement:** Build a semantic analysis pass that performs type checking for expressions and variable declarations.
- **Uses this concept:** Type Systems, Semantic Analysis.
- **Learn this:** The role of a type checker in preventing errors before code generation. How to annotate your AST with type information.
- **Problem:** Your compiler should now reject `let x: int = 10 + true;` with a clear error message: "Type mismatch: cannot add 'int' and 'bool'".
- **Sources:**
  - **Book:** "Types and Programming Languages" by Benjamin C. Pierce (Chapters 1-3 are a great intro).
  - **Book:** "Crafting Interpreters" (Chapter 11: Resolving and Binding).
- **Teacher’s note:** This is where your language starts to form its personality. A strong type system is the foundation of safety. Your symbol table will now need to store type information alongside variable names.

---

#### **Week 10: The Custom Memory Model - Scoped Regions**
- **Requirement:** Design and implement the core of the memory model: scoped memory regions. All local variables and pointers must be associated with the function scope they are defined in.
- **Uses this concept:** Static Analysis, Lifetime Analysis (Simplified).
- **Learn this:** How to track memory ownership/access rights without a full borrow checker. Associate every pointer type with a 'region' token (e.g., `*'scope T`).
- **Problem:** Implement a semantic pass that rejects code where a pointer to a local variable is returned from a function. For `fn dangling() -> *int { let x = 10; return &x; }`, the compiler must error: "Cannot return pointer to local variable 'x' as it would escape its scope."
- **Sources:**
  - **Paper:** "Region-based memory management" (academic papers can be dense, but the concept is key).
  - **Blog:** Search for "memory regions compilers" for simpler explanations.
- **Teacher’s note:** This is the first major departure from C and the core of your unique safety proposition. You are essentially implementing a simplified, compile-time-only version of regions. This prevents the most common use-after-free errors.

---

#### **Week 11: Static Array Types & Safe Indexing**
- **Requirement:** Implement fixed-size arrays (`let arr: [int; 5]`). Implement compile-time bounds checking for constant indices.
- **Uses this concept:** Dependent Types (in spirit), Constant Folding.
- **Learn this:** How to represent array types in your type system and how to perform checks during the semantic analysis phase.
- **Problem:** The compiler must accept `arr[4]` but reject `arr[5]` for an array of size 5 with a compile-time error: "Index 5 is out of bounds for array of size 5."
- **Sources:**
  - **Your own logic:** This is a direct implementation of your design. The type of `[T; N]` contains `N`. When indexing with a constant `C`, the compiler just checks if `C < N`.
- **Teacher’s note:** This first step of bounds checking costs nothing at runtime. It's pure compile-time safety and a huge win.

---

#### **Week 12: Dynamic Bounds Checking & Zero-Cost Exceptions**
- **Requirement:** Implement the fallback for runtime indices. When an index is a variable, inject a check that panics on failure.
- **Uses this concept:** Zero-Cost Abstractions, Exception Handling.
- **Learn this:** How to structure code generation to emit a conditional branch for the bounds check, and how to implement a simple panic/abort mechanism.
- **Problem:** Compile `arr[i]` where `i` is a variable. The generated code should be equivalent to: `if (i >= 5) { panic("OutOfBounds"); } ... arr[i] ...`.
- **Sources:**
  - **LLVM Docs:** Learn about structured exception handling in LLVM if you want true unwinding, or simply call an external `panic` function for now.
  - **Article:** "Making exceptions zero-cost" (C++ papers on this are informative).
- **Teacher’s note:** The "zero-cost" part means there is no overhead if the check passes and the exception is *not* thrown. The branch predictor on modern CPUs will make this extremely fast.

---

#### **Week 13: The `--force-safe` Flag: Static Analysis in Practice**
- **Requirement:** Implement the `--force-safe` compilation mode. The compiler must statically verify that a user-provided check dominates any array access with a dynamic index.
- **Uses this concept:** Data Flow Analysis, Control Flow Graphs (CFGs).
- **Learn this:** How to build a CFG from your AST and perform a simple backward data flow analysis to find dominating checks.
- **Problem:** With `--force-safe` on, the compiler must accept `if i < arr.len { print(arr[i]); }` but reject `print(arr[i]);` if `i` is a runtime variable and no such check is found on all paths leading to the access.
- **Sources:**
  - **Book:** "Modern Compiler Implementation in C" (Appel book) has great sections on data flow analysis.
  - **Tutorial:** Search for "Dominator Tree in Compilers."
- **Teacher’s note:** This is a genuinely advanced and powerful feature. It perfectly balances your C-like performance goal with Rust-like safety by shifting the "cost" of the check from runtime execution to the developer's explicit code, which the compiler then verifies for free.

---

#### **Week 14: Slices and Strings**
- **Requirement:** Implement slices (`&[T]`) as a fat pointer (pointer + length). Implement strings as slices of UTF-8 bytes (`&str`). Enforce UTF-8 validity.
- **Uses this concept:** Fat Pointers, Data Structures.
- **Learn this:** How slices provide safe views into arrays or other contiguous memory. The basics of UTF-8 encoding.
- **Problem:** Implement slice creation `let s = &arr[1..4];`. Accessing `s[0]` should access `arr[1]`. `s.len` should be 3. Bounds checks should now use the slice's length, not the original array's.
- **Sources:**
  - **Blog:** "How Rust implements slices" provides a great model.
  - **Standard:** The official UTF-8 standard documentation.
- **Teacher’s note:** Your lexer will need to be updated to handle string literals and potentially validate UTF-8 at parse time. Slices are the backbone of safe, efficient data manipulation.

---

#### **Week 15: User-Defined Types (Structs)**
- **Requirement:** Implement struct definitions and member access.
- **Uses this concept:** Aggregate Data Types, Memory Layout.
- **Learn this:** How to calculate struct memory layouts, including size and member offsets.
- **Problem:** Compile code that defines a `Point { x: int, y: int }`, creates an instance of it, and accesses its members (`p.x`).
- **Sources:**
  - **LLVM Docs:** Learn about `struct` types in LLVM IR.
- **Teacher’s note:** Pay attention to data alignment. For now, you can let LLVM handle it, but it's good to be aware of how padding can affect struct sizes.

---

#### **Week 16: Error Handling - `Result<T, E>`**
- **Requirement:** Implement a `Result` enum in the core language/library and the `?` operator for error propagation.
- **Uses this concept:** Sum Types (Enums), Syntactic Sugar.
- **Learn this:** How enums with associated data are represented in memory. How to transform code during parsing or semantic analysis to de-sugar the `?` operator.
- **Problem:** Implement `Result<T, E>` and make the `?` operator in `let value = might_fail()?` expand to a match or if-let block that returns early on the error case.
- **Sources:**
  - **Blog:** "Implementing Rust's `?` operator in a compiler."
- **Teacher’s note:** This pattern is far superior to C's error codes. By building it into the language, you encourage robust error handling from day one.

---

#### **Week 17: Range-Restricted Integers**
- **Requirement:** Implement integer types with compile-time range restrictions, like `let x: int[0..10] = 5;`.
- **Uses this concept:** Dependent Types (again, in spirit).
- **Learn this:** How to embed value constraints into the type system.
- **Problem:** The compiler must reject `let x: int[0..10] = 11;` at compile time. For runtime assignments like `x = y;`, it must inject a runtime check (similar to bounds checking) that panics if `y` is outside the range `[0..10]`.
- **Sources:**
  - **Academic Papers:** Look for research on "range analysis" in compilers.
- **Teacher’s note:** This is another powerful safety feature. It can eliminate entire classes of bugs related to logical errors, not just memory errors.

---

#### **Week 18: The `uinl` Escape Hatch**
- **Requirement:** Implement the `uinl {}` (unsafe inline) block. Code inside this block can bypass safety checks like bounds checking and memory region rules.
- **Uses this concept:** Code Safety Boundaries.
- **Learn this:** How to toggle safety checks on and off within the compiler based on whether the current AST node is inside an `uinl` block.
- **Problem:** The compiler should normally reject a raw pointer dereference, but allow it inside `uinl { ... }`.
- **Sources:**
  - **Reference:** How Rust's `unsafe` block works.
- **Teacher’s note:** Every safe systems language needs an escape hatch. The key is making it explicit and contained. The `uinl` keyword serves as a clear signal to developers and auditors that "here be dragons."

---

### Phase 3: Abstractions & Advanced Features (Weeks 19-28)

---

#### **Week 19: Generics (Part 1) - Parsing & Type Checking**
- **Requirement:** Add syntax for generic functions and structs. Update the type checker to handle placeholder types.
- **Uses this concept:** Parametric Polymorphism.
- **Learn this:** The challenges of type checking code that operates on unknown types.
- **Problem:** Parse and type-check a generic function like `fn identity<T>(x: T) -> T { return x; }`. The type checker should ensure that if you call it with an `int`, it returns an `int`.
- **Sources:**
  - **Book:** "Types and Programming Languages" (Part IV: Polymorphism).
- **Teacher’s note:** Do not implement code generation yet. The goal this week is purely to get the frontend to understand and validate generic code. This is a complex step.

---

#### **Week 20: Generics (Part 2) - Monomorphization**
- **Requirement:** Implement monomorphization for generics.
- **Uses this concept:** Code Generation for Generics.
- **Learn this:** The process of creating a specialized version of a generic function for each concrete type it's used with.
- **Problem:** When the compiler sees `identity(10)` (int) and `identity(true)` (bool), it should generate two separate, concrete versions of the `identity` function in the LLVM IR: one for integers and one for booleans.
- **Sources:**
  - **Blog:** "How C++ implements templates" or "How Rust implements generics."
- **Teacher’s note:** Monomorphization gives you C++-level performance for generics at the cost of some binary size. This fits your zero-overhead principle perfectly. You'll need a mechanism to track which generic instances you've already generated.

---

#### **Week 21: Traits/Interfaces (Part 1) - Static Dispatch**
- **Requirement:** Implement traits for compile-time polymorphism.
- **Uses this concept:** Ad-hoc Polymorphism, Trait-based Generics.
- **Learn this:** How to use traits to constrain generic types (e.g., `fn print<T: Display>(item: T)`).
- **Problem:** Define a `Display` trait with a `to_string` method. Implement it for `int`. Write a generic function that only accepts types that implement `Display`.
- **Sources:**
  - **Blog:** Rust's documentation on traits is the best source for understanding the concepts.
- **Teacher’s note:** Traits are one of the most powerful features for building large, maintainable applications. Focus on static dispatch for now, where the concrete function to call is known at compile time via monomorphization.

---

#### **Week 22: Traits/Interfaces (Part 2) - Dynamic Dispatch**
- **Requirement:** Implement trait objects for runtime polymorphism (e.g., `let items: [&dyn Display]`).
- **Uses this concept:** Virtual Tables (vtables).
- **Learn this:** How to implement dynamic dispatch using fat pointers that contain a data pointer and a vtable pointer.
- **Problem:** Create a list of different types that all implement `Display` and iterate through them, calling the `to_string` method on each one. This requires generating a vtable for each type.
- **Sources:**
  - **Article:** "What is a vtable?" A deep dive into C++'s implementation is very instructive.
- **Teacher’s note:** This is an *explicitly requested* runtime cost. By making developers use the `dyn` keyword, you are upholding the "zero-cost unless you ask for it" principle.

---

#### **Week 23: C Foreign Function Interface (FFI)**
- **Requirement:** Implement the ability to call C functions from your language and to expose your functions to C.
- **Uses this concept:** ABI Compatibility.
- **Learn this:** How to map your language's types to C types and how to use LLVM to generate calls that respect the C calling convention.
- **Problem:** Call the standard C `printf` function from your language.
- **Sources:**
  - **LLVM Docs:** Look for attributes related to calling conventions (`ccc`, `fastcc`, etc.).
  - **Blog:** "Calling C from Rust" (and vice versa) explains the concepts well.
- **Teacher’s note:** A strong FFI is non-negotiable for a systems language. It allows your users to leverage the vast ecosystem of existing C libraries.

---

#### **Week 24: Advanced Error Reporting**
- **Requirement:** Improve compiler error messages to be significantly more user-friendly. Implement "caret diagnostics" that point to the exact location of an error in the source code.
- **Uses this concept:** Compiler Diagnostics, User Experience.
- **Learn this:** How to track detailed source location information (line, column, length) for every token and AST node.
- **Problem:** For the code `let x: int = true;`, instead of just `type mismatch`, the compiler should output a formatted error like `error: mismatched types\n --> main.aura:1:15\n  |\n1 | let x: int = true;\n  |               ^^^^ expected int, found bool`.
- **Sources:**
  - **Blog:** "How to write a good error message" by the Elm language creator.
  - **Reference:** Look at the output of `rustc` and `clang` for inspiration. `termcolor` libraries can help with colored output.
- **Teacher's note:** This week is all about empathy for your user. Great diagnostics can be the single most important feature that makes a language pleasant to use.

---

#### **Week 25: Building a Control Flow Graph (CFG)**
- **Requirement:** Implement a pass that converts the AST for each function into an explicit Control Flow Graph representation.
- **Uses this concept:** Intermediate Representations, Graph Theory.
- **Learn this:** The structure of a CFG (basic blocks and edges), and algorithms for building it from an AST by identifying leaders of basic blocks.
- **Problem:** For a function containing an `if-else` statement inside a `while` loop, draw the expected CFG on paper, then write the code to generate that graph structure from the AST.
- **Sources:**
  - **Book:** "Engineering a Compiler" (Chapter 5 has an excellent explanation of CFGs).
  - **Online Course:** Stanford's CS143 Compiler course materials often cover this well.
- **Teacher's note:** The CFG is a foundational structure for almost all serious optimizations and static analysis. Building it now will pay dividends for the `--force-safe` feature and future optimization passes.

---

#### **Week 26: Refactoring the Type System**
- **Requirement:** After adding many new features, the type system is likely complex. Dedicate this week to refactoring it for clarity, correctness, and extensibility.
- **Uses this concept:** Software Architecture, Code Refactoring.
- **Learn this:** How to represent types internally in a more structured way (e.g., using a single `Type` struct/class with a kind enum, instead of many different classes).
- **Problem:** Audit your type checker. Identify duplicated code, unclear logic, and potential bugs. Refactor the representation of generic types and trait bounds to be more robust.
- **Sources:**
  - **Book:** "Refactoring: Improving the Design of Existing Code" by Martin Fowler. The principles apply directly here.
- **Teacher's note:** Technical debt in a compiler's type system is particularly dangerous. A clean, well-architected type checker will make implementing future features much easier and less error-prone.

---

#### **Week 27: Closures (Part 1 - Frontend)**
- **Requirement:** Implement the frontend components for closures: parsing and type checking.
- **Uses this concept:** Lexical Scoping, Lambda Calculus (in spirit).
- **Learn this:** How to parse closure syntax (e.g., `|a, b| { a + b }`), and how to perform "capture analysis" to determine which variables from the enclosing scope the closure needs to access.
- **Problem:** Write a semantic pass that, for a given closure, generates a list of "captured" variables. For `let y = 10; let my_closure = || { print(y); };`, the analysis must identify that `y` is captured.
- **Sources:**
  - **Blog:** "How to implement closures in a compiler."
  - **Book:** "Crafting Interpreters" (Chapter 12 covers closures for an interpreter, but the capture analysis is similar).
- **Teacher's note:** Closures are a cornerstone of modern, expressive programming. Getting the frontend logic right, especially capture analysis, is the hardest part.

---

#### **Week 28: Closures (Part 2 - Backend)**
- **Requirement:** Implement the code generation for closures.
- **Uses this concept:** Code Generation, Data Structures.
- **Learn this:** How a closure is typically represented at runtime: a pair of pointers (a function pointer to the generated code, and an environment pointer to a heap-allocated struct holding the captured variables).
- **Problem:** Generate LLVM IR for a function that creates and calls a closure. This involves creating the environment struct, allocating it, populating it with captured variables, and passing it as a hidden first argument to the closure's function.
- **Sources:**
  - **LLVM Docs:** You'll need to create `struct` types for the environment and pass pointers to them.
  - **Article:** "How Clang implements closures" can provide deep insight.
- **Teacher's note:** Seeing a closure that captures a variable and runs correctly is a true sign of a maturing compiler. This is one of the most complex features you'll implement.

---

### Phase 4: The Ecosystem & Tooling (Weeks 29-40)

---

#### **Week 29: The Build System - `aura build`**
- **Requirement:** Create a command-line tool that orchestrates the compilation process.
- **Uses this concept:** Build Automation.
- **Learn this:** How to shell out to the compiler, linker, and other tools. How to manage source files and build outputs.
- **Problem:** Create a `aura` command that can take `aura build main.aura` and produce an executable without the user needing to manually invoke your compiler executable and then a C linker like `gcc` or `clang`.
- **Sources:**
  - **Your favorite language's standard library:** Look for process/command execution APIs.
- **Teacher’s note:** You are now building the developer experience. A good build tool is as important as a good compiler.

---

#### **Week 30: The Package Manager - `aura.toml`**
- **Requirement:** Design the `aura.toml` manifest format and implement a basic package manager that can handle dependencies (conceptually for now).
- **Uses this concept:** Dependency Management.
- **Learn this:** The basics of TOML parsing and semantic versioning.
- **Problem:** Write a tool that can parse an `aura.toml` file, understand the project name, version, and a list of (hypothetical) dependencies.
- **Sources:**
  - **Specification:** The TOML specification. Cargo's manifest format is a great inspiration.
- **Teacher’s note:** Don't build a full package repository yet. Just get the local package definition and build logic working. The ability to specify dependencies is the first step.

---

#### **Week 31: The Test Runner - `aura test`**
- **Requirement:** Implement a built-in test runner.
- **Uses this concept:** Test-Driven Development (TDD) Support.
- **Learn this:** How to create a test harness that can compile and run functions annotated with a `#[test]` attribute, and report pass/fail results.
- **Problem:** Create `aura test` that finds all `#[test]` functions in your project, runs them, and prints a summary like `2 tests run, 2 passed, 0 failed`.
- **Sources:**
  - **Inspiration:** Look at how `go test` and `cargo test` are designed.
- **Teacher’s note:** Integrating testing into the core tooling encourages good practices and makes your language much more attractive for serious projects.

---

#### **Week 32: The Standard Library (Part 1)**
- **Requirement:** Begin implementing a core standard library with essential data structures like a dynamic array (`Vec<T>`) and an optional value type (`Option<T>`).
- **Uses this concept:** Library Design, API Ergonomics.
- **Learn this:** The trade-offs in designing a standard library that is both ergonomic and performant. How to use your own `uinl` block to build safe abstractions.
- **Problem:** Implement a `Vec<T>` type in your language, using `uinl` internally for the low-level memory management, but exposing a completely safe public API.
- **Sources:**
  - **Reference:** Rust's `std::vec` and C++'s `std::vector` documentation.
- **Teacher’s note:** Your standard library is the first major "customer" of your own language. Writing it will reveal awkward parts of your language design that you can now refine.

---

#### **Week 33: Concurrency - Safe Threads & Channels**
- **Requirement:** Implement Go-style message-passing channels and a thread-spawning API.
- **Uses this concept:** Communicating Sequential Processes (CSP), Concurrency Primitives.
- **Learn this:** The design of buffered/unbuffered channels and how they prevent data races by transferring ownership of data rather than sharing memory.
- **Problem:** Implement a `Channel<T>` with `send` and `recv` methods. Create two threads that communicate a series of numbers over a channel. The `send` operation should transfer ownership of the value, preventing the sender from using it again.
- **Sources:**
  - **Book:** "Concurrency in Go" by Katherine Cox-Buday.
- **Teacher’s note:** "Share memory by communicating, don't communicate by sharing memory." This model is a fantastic fit for your language, as it provides a structured and safe way to do concurrency, avoiding the need for a full-blown race-detecting borrow checker.

---

#### **Week 34: Standard Library (Part 2) - I/O and Collections**
- **Requirement:** Expand the standard library to include basic file I/O and a `HashMap<K, V>`.
- **Uses this concept:** System Calls, Data Structures.
- **Learn this:** How to wrap OS-level file operations (using FFI) in a safe, high-level API. The implementation details of a high-performance hash map.
- **Problem:** Write an Aura program that reads a text file, counts the frequency of each word, and prints the results to the console. This will use both file I/O and your `HashMap`.
- **Sources:**
  - **Blog:** "Let's build a HashMap" (many tutorials exist for different languages).
  - **Reference:** Rust's `std::fs` and `std::collections::HashMap` APIs.
- **Teacher’s note:** A rich standard library is what makes a language productive. These components are essential building blocks for almost any real-world application.

---

#### **Week 35: Basic Compiler Optimizations**
- **Requirement:** Implement simple, classic optimization passes that run on your AST or IR.
- **Uses this concept:** Peephole Optimization, Constant Folding, Dead Code Elimination.
- **Learn this:** The difference between frontend and backend optimizations. How to write passes that transform the code to be more efficient without changing its meaning.
- **Problem:** Implement a constant folding pass. When the compiler sees `let x = 10 + 20 * 2;`, it should replace it with `let x = 50;` before code generation.
- **Sources:**
  - **Book:** "Engineering a Compiler" (Chapters on optimization).
  - **LLVM Docs:** Read about the built-in LLVM optimization passes. While you'll write your own simple ones, it's good to know what the backend can do.
- **Teacher’s note:** Even simple optimizations can have a big impact. This work directly serves your non-negotiable goal of C-level performance by eliminating unnecessary work at runtime.

---

#### **Week 36: Documentation Generation - `aura doc`**
- **Requirement:** Create a tool that parses documentation comments (like `///`) and generates HTML documentation.
- **Uses this concept:** Tooling, Static Site Generation.
- **Learn this:** How to integrate documentation parsing into your compiler's frontend and how to generate structured output (like JSON or HTML).
- **Problem:** Build `aura doc` that can process a source file with documented functions and structs, and produce a simple, hyperlinked HTML page showing the public API and the comments.
- **Sources:**
  - **Inspiration:** `rustdoc`, `doxygen`, `godoc`.
- **Teacher’s note:** Good documentation is a force multiplier. Building a documentation tool shows a commitment to the language's ecosystem and makes it far more approachable for new users.

---

#### **Week 37: Self-Hosting & Dogfooding**
- **Requirement:** Choose a component of your compiler or build tools and rewrite it in Aura.
- **Uses this concept:** Self-Hosting, Dogfooding.
- **Learn this:** The real-world pain points and strengths of your own language design.
- **Problem:** Rewrite your `aura build` tool (which might be a script or a C++ program) in Aura itself. Your Aura compiler must be able to compile its own build tool.
- **Sources:**
  - **History:** Read about the history of the first self-hosting C and Go compilers.
- **Teacher’s note:** This is a rite of passage for a systems language. When your language is good enough to build itself, you know you're on the right track. This will be challenging but will provide invaluable feedback.

---

#### **Week 38: Performance & Bug Hunt**
- **Requirement:** Dedicate a full week to profiling, benchmarking, and fixing bugs found during the dogfooding process.
- **Uses this concept:** Profiling, Debugging, Quality Assurance.
- **Learn this:** How to use profiling tools (like `perf` on Linux or Instruments on macOS) to find bottlenecks in your compiler and in the code it generates.
- **Problem:** Create a suite of performance benchmarks. Compare the executables generated by Aura against equivalent C code for tasks like matrix multiplication, string processing, and file I/O. Identify and fix sources of performance loss.
- **Sources:**
  - **Tools:** `valgrind`, `perf`, GDB, LLDB.
- **Teacher’s note:** A systems language lives and dies by its performance. This week is about rigorously upholding your #1 non-negotiable: be as fast as C. Measure, don't guess.

---

#### **Week 39: Language Polish & The V1.0 Spec**
- **Requirement:** Finalize the language syntax, keywords, and standard library API. Write a semi-formal language specification document.
- **Uses this concept:** Language Specification, API Stability.
- **Learn this:** The importance of committing to a stable design for a v1.0 release. How to clearly document language semantics.
- **Problem:** Review every keyword, every public function in the standard library, and every compiler error message. Make final breaking changes now. Write a markdown document that serves as the Aura v1.0 Language Reference.
- **Sources:**
  - **Reference:** The Go Programming Language Specification, The Rust Reference.
- **Teacher’s note:** A v1.0 release is a promise of stability to your users. This is your last chance to make breaking changes without a major version bump. Be decisive and document everything.

---

#### **Week 40: The Launch - Docs, Examples, and Release**
- **Requirement:** Create a "Getting Started" guide, build several example projects, and tag the official 1.0.0 release.
- **Uses this concept:** Project Release Management, Community Building.
- **Learn this:** How to package a release, write engaging tutorials, and present your project to the world.
- **Problem:** Create a public repository (e.g., on GitHub) for Aura. Write a `README.md` that explains what Aura is and how to install and use it. Create an `examples/` directory with at least three small but compelling projects. Tag the release.
- **Sources:**
  - **Your own work:** Everything you've built culminates here.
- **Teacher’s note:** Congratulations! After 40 weeks of intense work, you have designed and implemented a new systems programming language from scratch, adhering to an ambitious set of goals. You have built not just a compiler, but the beginnings of an entire developer ecosystem. This is a monumental achievement.

---

### Appendix: Aura Language Design Principles

This section summarizes the key design decisions for the **Aura** programming language as defined by your goals.

#### **1. Non-Negotiable Constraints**
- **Performance:** Must compile to machine code with performance characteristics identical to C. All abstractions are designed to be zero-cost at runtime unless explicitly requested by the developer (e.g., using `dyn` for dynamic dispatch).
- **Safety:** Must provide memory safety guarantees as strong as Rust's, preventing dangling pointers, buffer overflows, use-after-free, and data races through a combination of compile-time analysis and minimal, opt-out runtime checks.
- **Unique Memory Model:** Actively avoids a direct copy of Rust's pervasive ownership and borrow checking system in favor of a simpler, more direct model.

#### **2. Custom Memory & Safety Model**
Aura's safety model is a hybrid system designed for simplicity and C-like ergonomics without sacrificing safety.
- **Scoped Regions (Default):** The primary memory model. All pointers are lexically scoped by default. The compiler tracks which function scope (region) a pointer belongs to and makes it a compile-time error for a pointer to escape its scope (e.g., returning a pointer to a local variable). This eliminates the most common sources of use-after-free errors.
- **Optional Borrowing:** Simple, local borrow checking can be opted into for specific variables (e.g., `let y = &x;`). This is a tool for preventing aliasing bugs within a function, not a mandatory model for all variables.
- **Explicit Heap Management:** Heap-allocated objects (`new T`) are managed by a unique pointer type that automatically deallocates when it goes out of scope, similar to `std::unique_ptr` or `Box<T>`.
- **`uinl {}` Block:** A clearly demarcated `unsafe inline` block is provided as a necessary escape hatch for low-level programming, where all safety checks are disabled.

#### **3. Array, Slice, and Bounds Checking Rules**
- **Compile-Time First:** Array access with a constant index (`arr[5]`) is checked at compile time. Failure is a hard compiler error.
- **Runtime Fallback:** Access with a variable index (`arr[i]`) causes the compiler to inject a runtime check that panics on out-of-bounds access. This check is highly optimized by the CPU's branch predictor and has near-zero cost in the non-panicking case.
- **`--force-safe` Mode:** This special compiler flag changes the behavior of the runtime fallback. Instead of injecting a check, the compiler performs static analysis. It verifies that the programmer has *already written* a manual bounds check (e.g., `if i < arr.len`) that is guaranteed to execute before the access. If no such check can be proven to exist on all code paths, the compilation fails. This provides **full memory safety with absolutely zero runtime overhead**, shifting the responsibility of writing the check to the developer while the compiler verifies it for free.

#### **4. Build, Package, and Test Tooling**
The developer experience is first-class, inspired by modern toolchains like Cargo and Go.
- **`aura build`:** A single command to build a project, handling compilation and linking automatically.
- **`aura.toml`:** A manifest file defining the package's name, version, and dependencies, making projects declarative and reproducible.
- **`aura test`:** A built-in test runner that discovers and executes functions marked with a `#[test]` attribute, providing a seamless testing workflow without third-party tools.
