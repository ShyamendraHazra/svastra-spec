# Systems Programming Language Design & Implementation Roadmap
*A 40-week journey to build a C-performance, Rust-safety systems language*

## Week 1: Language Design Philosophy & Requirements Analysis
- **Requirement:** Define complete language specification and core design principles
- **Uses this concept:** Programming language design theory, performance/safety trade-offs
- **Learn this:** How to balance competing language goals, specification writing
- **Problem:** Write a detailed language specification document covering syntax, semantics, and safety guarantees
- **Sources:** 
  - [Programming Language Pragmatics by Michael Scott - Free PDF](https://www.cs.rochester.edu/~scott/pragmatics/)
  - [Language Implementation Patterns by Terence Parr](https://pragprog.com/titles/tpantlr2/language-implementation-patterns/)
  - [Rust RFC Process Documentation](https://rust-lang.github.io/rfcs/)
  - [Go Language Specification](https://golang.org/ref/spec)
- **Teacher's note:** This week establishes your language's "constitution" - every design decision should trace back to these core principles

## Week 2: Compiler Architecture & Toolchain Design
- **Requirement:** Design complete compiler pipeline and build system architecture
- **Uses this concept:** Compiler design phases, build system theory
- **Learn this:** Multi-pass vs single-pass compilation, intermediate representations, build dependency graphs
- **Problem:** Create architectural diagrams for your compiler pipeline and build tool integration
- **Sources:**
  - [Engineering a Compiler by Cooper & Torczon - PDF](https://www.cs.rice.edu/~keith/Comp506/2017/)
  - [Crafting Interpreters by Bob Nystrom - Free Online](https://craftinginterpreters.com/)
  - [LLVM Architecture Guide](https://llvm.org/docs/LangRef.html)
  - [The Cargo Book](https://doc.rust-lang.org/cargo/)
- **Teacher's note:** Think of this as designing the factory that will build your language - get the architecture right before writing code

## Week 3: Lexical Analysis & Token Design
- **Requirement:** Implement complete lexer for your language syntax
- **Uses this concept:** Finite automata, regular expressions, token classification
- **Learn this:** Lexer generators vs hand-written lexers, Unicode handling, error recovery
- **Problem:** Build a lexer that handles UTF-8, comments, string literals, and numeric types with proper error messages
- **Sources:**
  - [Compilers: Principles, Techniques, and Tools (Dragon Book) - PDF](https://suif.stanford.edu/dragonbook/)
  - [Modern Compiler Implementation in C by Andrew Appel](https://www.cs.princeton.edu/~appel/modern/c/)
  - [Flex and Bison Tutorial](https://aquamentus.com/flex_bison.html)
  - [Unicode Normalization Forms](https://unicode.org/reports/tr15/)
- **Teacher's note:** Your lexer must handle UTF-8 perfectly since string safety is non-negotiable

## Week 4: Grammar Design & Parser Implementation
- **Requirement:** Implement recursive descent parser for your language grammar
- **Uses this concept:** Context-free grammars, LL parsing, precedence rules
- **Learn this:** Left recursion elimination, operator precedence, parser combinators
- **Problem:** Parse expressions, statements, and declarations with proper precedence and associativity
- **Sources:**
  - [Dragon Book Chapter 4 - Syntax Analysis](https://suif.stanford.edu/dragonbook/)
  - [Language Implementation Patterns Chapters 3-5](https://pragprog.com/titles/tpantlr2/language-implementation-patterns/)
  - [Simple but Powerful Pratt Parsing](https://matklad.github.io/2020/04/13/simple-but-powerful-pratt-parsing.html)
  - [PEG Parsing with Pest.rs](https://pest.rs/book/)
- **Teacher's note:** Hand-written recursive descent gives you full control over error messages - crucial for developer experience

## Week 5: Abstract Syntax Tree & Symbol Tables
- **Requirement:** Build AST representation and implement symbol table with scoping
- **Uses this concept:** Tree data structures, hash tables, scope resolution
- **Learn this:** AST visitor patterns, symbol binding, namespace management
- **Problem:** Implement nested scopes, variable shadowing, and forward declarations
- **Sources:**
  - [Crafting Interpreters - Trees and Environments](https://craftinginterpreters.com/representing-code.html)
  - [Modern Compiler Implementation Chapters 3-4](https://www.cs.princeton.edu/~appel/modern/c/)
  - [Symbol Table Implementation Guide](https://www.geeksforgeeks.org/symbol-table-compiler/)
  - [AST Design Patterns](https://eli.thegreenplace.net/2009/02/16/abstract-vs-concrete-syntax-trees/)
- **Teacher's note:** Your AST design impacts every subsequent compiler phase - make it extensible

## Week 6: Type System Foundation
- **Requirement:** Implement basic type checking for primitives and user-defined types
- **Uses this concept:** Type theory, type inference, structural vs nominal typing
- **Learn this:** Hindley-Milner type inference, type unification, polymorphism
- **Problem:** Type check variable assignments, function calls, and basic expressions
- **Sources:**
  - [Types and Programming Languages by Benjamin Pierce](https://www.cis.upenn.edu/~bcpierce/tapl/)
  - [Programming Languages: Application and Interpretation](https://cs.brown.edu/courses/cs173/2012/book/)
  - [Rust Nomicon - Subtyping and Variance](https://doc.rust-lang.org/nomicon/subtyping.html)
  - [Type Systems Tutorial](https://steshaw.org/hm/)
- **Teacher's note:** Start simple - complex features like generics build on solid fundamentals

## Week 7: Memory Safety Analysis - Compile-time Bounds Checking
- **Requirement:** Implement static analysis to verify array bounds at compile time
- **Uses this concept:** Static analysis, abstract interpretation, constraint solving
- **Learn this:** Range analysis, symbolic execution, SMT solvers for verification
- **Problem:** Detect compile-time array bounds violations and prove safety where possible
- **Sources:**
  - [Principles of Program Analysis by Nielson](https://www.springer.com/gp/book/9783540654100)
  - [Static Program Analysis Lecture Notes](https://cs.au.dk/~amoeller/spa/)
  - [CBMC Bounded Model Checker](https://www.cprover.org/cbmc/)
  - [Abstract Interpretation Tutorial](https://www.di.ens.fr/~cousot/AI/)
- **Teacher's note:** This is where your language gets its safety superpowers - invest heavily here

## Week 8: Range-Restricted Integer Types
- **Requirement:** Implement integer types with compile-time range constraints
- **Uses this concept:** Dependent types, constraint systems, overflow detection
- **Learn this:** Value range propagation, constraint satisfaction, overflow semantics
- **Problem:** Implement `int[min..max]` types with automatic bounds checking and arithmetic overflow detection
- **Sources:**
  - [Dependent Types at Work Tutorial](https://www.cse.chalmers.se/~peterd/papers/DependentTypesAtWork.pdf)
  - [The Rust Programming Language - Integer Overflow](https://doc.rust-lang.org/book/ch03-02-data-types.html#integer-overflow)
  - [Z3 SMT Solver Guide](https://microsoft.github.io/z3guide/)
  - [Ada SPARK Reference Manual](https://docs.adacore.com/spark2014-docs/html/ug/)
- **Teacher's note:** Range types are your secret weapon for preventing entire classes of bugs

## Week 9: Runtime Bounds Checking & Exception System
- **Requirement:** Implement zero-cost exception handling for bounds violations
- **Uses this concept:** Exception handling, stack unwinding, zero-cost abstractions
- **Learn this:** RAII, exception safety guarantees, unwinding mechanisms
- **Problem:** Build exception system that compiles to zero overhead when no exceptions occur
- **Sources:**
  - [Exceptional C++ by Herb Sutter](https://www.gotw.ca/publications/xc++.htm)
  - [LLVM Exception Handling](https://llvm.org/docs/ExceptionHandling.html)
  - [Zero-cost Exceptions Aren't Actually Zero Cost](https://devblogs.microsoft.com/oldnewthing/20220228-00/?p=106296)
  - [Rust Panic Handling Internals](https://doc.rust-lang.org/nomicon/panic-safety.html)
- **Teacher's note:** The goal is C performance with safety nets - exceptions should be rare and fast

## Week 10: Force-Safe Mode Implementation
- **Requirement:** Implement `--force-safe` compilation mode with mandatory manual bounds checking
- **Uses this concept:** Static analysis, control flow analysis, proof obligations
- **Learn this:** Dataflow analysis, reaching definitions, path-sensitive analysis
- **Problem:** Verify that all dynamic array accesses have user-written bounds checks before them
- **Sources:**
  - [Data Flow Analysis: Theory and Practice](https://link.springer.com/book/10.1007/978-3-540-28967-5)
  - [SPARK Ada Verification Conditions](https://docs.adacore.com/spark2014-docs/html/ug/en/source/how_to_view_gnatprove_output.html)
  - [Dafny Specification Language](https://dafny-lang.github.io/dafny/)
  - [Why3 Verification Platform](https://www.why3.org/)
- **Teacher's note:** This mode gives C-like control while maintaining safety - the compiler becomes a proof checker

## Week 11: Memory Model Design - Custom Ownership System
- **Requirement:** Design and implement your custom memory ownership system (not Rust's)
- **Uses this concept:** Linear types, affine types, resource management
- **Learn this:** Ownership vs borrowing, move semantics, automatic resource cleanup
- **Problem:** Create a simpler alternative to Rust's ownership that still prevents use-after-free and double-free
- **Sources:**
  - [Linear Types can Change the World! Paper](https://www.cs.cmu.edu/~fp/papers/lasc.pdf)
  - [Cyclone Language Memory Management](https://cyclone.thelanguage.org/)
  - [Ownership You Can Count On Paper](https://www.microsoft.com/en-us/research/publication/ownership-you-can-count-on/)
  - [Val Language Memory Model](https://www.val-lang.dev/memory-safety)
- **Teacher's note:** This is your chance to improve on Rust's complexity while maintaining safety

## Week 12: Unsafe Blocks - `uinl {}` Implementation
- **Requirement:** Implement unsafe escape hatch with proper encapsulation
- **Uses this concept:** Type system escape hatches, invariant preservation, API design
- **Learn this:** Safe abstractions over unsafe code, capability-based security
- **Problem:** Allow unsafe operations while maintaining safety invariants at boundaries
- **Sources:**
  - [The Rust Programming Language - Unsafe Rust](https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html)
  - [Fearless Concurrency in Rust](https://blog.rust-lang.org/2015/04/10/Fearless-Concurrency.html)
  - [Capability-based Security Overview](https://cap-lore.com/CapTheory/)
  - [Safe C++ Subset Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)
- **Teacher's note:** Unsafe should be a sharp knife - powerful but clearly dangerous

## Week 13: String Safety & UTF-8 Handling
- **Requirement:** Implement guaranteed UTF-8 safe string type with zero-copy operations
- **Uses this concept:** String encoding, Unicode normalization, zero-copy design
- **Learn this:** UTF-8 validation, string slicing safety, internationalization
- **Problem:** Create string APIs that never allow invalid UTF-8 while maintaining performance
- **Sources:**
  - [Unicode Standard Annex #15 - Normalization](https://unicode.org/reports/tr15/)
  - [The Absolute Minimum Every Software Developer Must Know About Unicode](https://www.joelonsoftware.com/2003/10/08/the-absolute-minimum-every-software-developer-absolutely-positively-must-know-about-unicode-and-character-sets-no-excuses/)
  - [Rust String and str Documentation](https://doc.rust-lang.org/std/string/)
  - [UTF-8 Everywhere Manifesto](https://utf8everywhere.org/)
- **Teacher's note:** String bugs cause security vulnerabilities - get this bulletproof from day one

## Week 14: Array and Slice Types
- **Requirement:** Implement safe array and slice types with compile-time length tracking
- **Uses this concept:** Dependent types, phantom types, zero-cost abstractions
- **Learn this:** Static vs dynamic array bounds, slice safety, iterator invalidation prevention
- **Problem:** Create array/slice APIs that prevent buffer overflows without runtime overhead
- **Sources:**
  - [Rust Slice Implementation Analysis](https://doc.rust-lang.org/std/slice/)
  - [Making Pointers Safe Research](https://www.cs.cornell.edu/courses/cs6120/2019fa/blog/safe-pointers/)
  - [C++ std::span Design Rationale](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p0122r7.pdf)
  - [Ada Array Constraint System](https://www.adaic.org/resources/add_content/standards/12rm/html/RM-3-6.html)
- **Teacher's note:** Arrays are where most C vulnerabilities happen - make them impossible to misuse

## Week 15: Generic Type System - Monomorphization
- **Requirement:** Implement compile-time generics with monomorphization (no template bloat)
- **Uses this concept:** Parametric polymorphism, code specialization, template instantiation
- **Learn this:** Generic specialization, trait bounds, monomorphization strategies
- **Problem:** Support generic functions and types while maintaining zero runtime cost
- **Sources:**
  - [Generic Programming and the STL](https://www.boost.org/sgi/stl/)
  - [Rust Generics and Monomorphization Internals](https://blog.rust-lang.org/2015/08/06/Rust-1.2.html)
  - [C++ Template Instantiation Model](https://en.cppreference.com/w/cpp/language/templates)
  - [Go Generics Design Document](https://go.googlesource.com/proposal/+/refs/heads/master/design/43651-type-parameters.md)
- **Teacher's note:** Think of generics as compile-time code generation - keep it simple but powerful

## Week 16: Trait System for Compile-time Polymorphism
- **Requirement:** Implement trait system for zero-cost abstraction
- **Uses this concept:** Ad-hoc polymorphism, type classes, interface segregation
- **Learn this:** Trait coherence, orphan rules, default implementations
- **Problem:** Build trait system that enables generic programming without vtable overhead
- **Sources:**
  - [Haskell Type Class Tutorial](http://learnyouahaskell.com/types-and-typeclasses)
  - [Rust Trait System Guide](https://doc.rust-lang.org/book/ch10-02-traits.html)
  - [Type Classes: Exploring the Design Space](https://www.microsoft.com/en-us/research/publication/type-classes-exploring-design-space/)
  - [Swift Protocol Design](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html)
- **Teacher's note:** Traits should feel as natural as C++ templates but with better error messages

## Week 17: Error Handling - Result Types & Panic System
- **Requirement:** Implement Result<T,E> error handling with panic fallback
- **Uses this concept:** Algebraic data types, error propagation, control flow
- **Learn this:** Monadic error handling, error context preservation, panic vs exception distinction
- **Problem:** Create ergonomic error handling that encourages explicit error management
- **Sources:**
  - [Error Handling in a Correctness-Critical Rust Project](https://blog.burntsushi.net/rust-error-handling/)
  - [Haskell Either Monad Tutorial](https://hackage.haskell.org/package/base/docs/Data-Either.html)
  - [Railway Oriented Programming by Scott Wlaschin](https://fsharpforfunandprofit.com/rop/)
  - [Go Error Handling Design](https://go.dev/blog/error-handling-and-go)
- **Teacher's note:** Make the right thing (handling errors) easy and the wrong thing (ignoring them) hard

## Week 18: Concurrency Primitives - Threads & Channels
- **Requirement:** Implement safe thread spawning and channel-based message passing
- **Uses this concept:** Message passing concurrency, CSP model, data race prevention
- **Learn this:** Channel implementation, thread safety, deadlock prevention
- **Problem:** Create Go-style concurrency that prevents data races at compile time
- **Sources:**
  - [Communicating Sequential Processes by Tony Hoare](https://www.cs.cmu.edu/~crary/819-f09/Hoare78.pdf)
  - [Go Concurrency Patterns](https://go.dev/talks/2012/concurrency.slide)
  - [Rust Concurrency Chapter](https://doc.rust-lang.org/book/ch16-00-concurrency.html)
  - [The Implementation of Channels in Go](https://speakerdeck.com/kavya719/the-scheduler-saga)
- **Teacher's note:** Concurrency should feel safe and natural - no more mutex debugging nightmares

## Week 19: Foreign Function Interface (FFI)
- **Requirement:** Implement seamless C library integration
- **Uses this concept:** ABI compatibility, marshalling, foreign function interfaces
- **Learn this:** Calling conventions, memory layout compatibility, header translation
- **Problem:** Call C functions safely while maintaining memory safety invariants
- **Sources:**
  - [System V ABI Specification](https://www.uclibc.org/docs/psABI-x86_64.pdf)
  - [Rust FFI Guide](https://doc.rust-lang.org/nomicon/ffi.html)
  - [The Rustonomicon FFI Chapter](https://doc.rust-lang.org/nomicon/ffi.html)
  - [SWIG Language Bindings](http://www.swig.org/tutorial.html)
- **Teacher's note:** FFI is your bridge to the C ecosystem - make it safe but not cumbersome

## Week 20: Code Generation - LLVM Backend
- **Requirement:** Implement LLVM IR generation for your language constructs
- **Uses this concept:** Intermediate representations, code generation, optimization passes
- **Learn this:** LLVM IR, basic block construction, optimization pipeline
- **Problem:** Generate efficient LLVM IR that enables aggressive optimization
- **Sources:**
  - [LLVM Language Reference Manual](https://llvm.org/docs/LangRef.html)
  - [LLVM Cookbook by Mayur Pandey](https://www.packtpub.com/product/llvm-cookbook/9781785285981)
  - [Kaleidoscope Tutorial](https://llvm.org/docs/tutorial/)
  - [Getting Started with LLVM Core Libraries](https://www.packtpub.com/product/getting-started-with-llvm-core-libraries/9781782166924)
- **Teacher's note:** LLVM does the heavy lifting - focus on generating clean, optimizable IR

## Week 21: Optimization Strategy - Zero-cost Abstractions
- **Requirement:** Implement optimization passes that eliminate abstraction overhead
- **Uses this concept:** Dead code elimination, inlining, constant propagation
- **Learn this:** LLVM optimization passes, performance measurement, benchmarking
- **Problem:** Ensure high-level code compiles to the same assembly as hand-optimized C
- **Sources:**
  - [Advanced Compiler Design and Implementation](https://www.amazon.com/Advanced-Compiler-Design-Implementation-Muchnick/dp/1558603204)
  - [LLVM Optimization Pass Documentation](https://llvm.org/docs/Passes.html)
  - [MIT Performance Engineering Course](https://ocw.mit.edu/courses/6-172-performance-engineering-of-software-systems-fall-2018/)
  - [Computer Systems: A Programmer's Perspective](https://csapp.cs.cmu.edu/)
- **Teacher's note:** Zero-cost means exactly that - your abstractions should disappear entirely

## Week 22: Memory Layout & ABI Compatibility
- **Requirement:** Implement C-compatible memory layout and calling conventions
- **Uses this concept:** Memory alignment, padding, structure layout, ABI specs
- **Learn this:** Platform ABIs, struct packing, alignment requirements
- **Problem:** Ensure your structs can be used in C code without wrapper functions
- **Sources:**
  - [Computer Organization and Design by Patterson & Hennessy](https://www.amazon.com/Computer-Organization-Design-MIPS-Architecture/dp/0124077269)
  - [System V ABI x86-64](https://www.uclibc.org/docs/psABI-x86_64.pdf)
  - [Data Alignment: Straighten Up and Fly Right](https://developer.ibm.com/articles/pa-dalign/)
  - [C Standard Structure Layout](https://port70.net/~nsz/c/c11/n1570.html#6.7.2.1)
- **Teacher's note:** ABI compatibility is essential for C interop - get the bytes exactly right

## Week 23: Debugging Support - DWARF Generation
- **Requirement:** Generate debug information for debugger integration
- **Uses this concept:** Debug information formats, symbol tables, source mapping
- **Learn this:** DWARF format, debug info generation, debugger protocols
- **Problem:** Enable GDB/LLDB debugging of your language with full source information
- **Sources:**
  - [DWARF Debugging Standard](http://dwarfstd.org/)
  - [Introduction to the DWARF Debugging Format](https://dwarfstd.org/doc/Debugging%20using%20DWARF-2012.pdf)
  - [LLVM Debug Info Tutorial](https://llvm.org/docs/tutorial/MyFirstLanguageFrontend/LangImpl09.html)
  - [GDB Internals Documentation](https://sourceware.org/gdb/onlinedocs/gdbint/)
- **Teacher's note:** Good debugging support is crucial for developer productivity

## Week 24: Build System Implementation
- **Requirement:** Build integrated build system (`lang build` command)
- **Uses this concept:** Build systems, dependency graphs, incremental compilation
- **Learn this:** Build file parsing, dependency tracking, parallel builds
- **Problem:** Create fast, reliable builds with proper dependency management
- **Sources:**
  - [Software Build Systems: Principles and Experience](https://www.springer.com/gp/book/9781852337797)
  - [Ninja Build System](https://ninja-build.org/)
  - [Bazel Concepts and Terminology](https://bazel.build/concepts/build-ref)
  - [Recursive Make Considered Harmful](https://aegis.sourceforge.net/auug97.pdf)
- **Teacher's note:** A great build system makes the language pleasant to use daily

## Week 25: Package Manager Design
- **Requirement:** Implement package manager with manifest file (`lang.toml`)
- **Uses this concept:** Package management, version resolution, dependency graphs
- **Learn this:** Semantic versioning, dependency resolution algorithms, package registries
- **Problem:** Resolve complex dependency graphs while avoiding version conflicts
- **Sources:**
  - [So you want to write a package manager](https://medium.com/@sdboyer/so-you-want-to-write-a-package-manager-4ae9c17d9527)
  - [npm Dependency Resolution Algorithm](https://npm.github.io/how-npm-works-docs/npm3/how-npm3-works.html)
  - [Version SAT - Dependency Solving as SAT](https://research.swtch.com/version-sat)
  - [Semantic Versioning Specification](https://semver.org/)
- **Teacher's note:** Package management is social engineering as much as technical - design for humans

## Week 26: Test Framework Implementation
- **Requirement:** Build integrated test runner (`lang test` command)
- **Uses this concept:** Unit testing frameworks, test discovery, assertion macros
- **Learn this:** Test organization, parameterized tests, property-based testing
- **Problem:** Create testing tools that encourage comprehensive test coverage
- **Sources:**
  - [xUnit Test Patterns by Gerard Meszaros](http://xunitpatterns.com/)
  - [QuickCheck Property-based Testing](https://hackage.haskell.org/package/QuickCheck)
  - [Rust Test Framework Source](https://github.com/rust-lang/rust/tree/master/library/test)
  - [The Art of Unit Testing by Roy Osherove](https://www.manning.com/books/the-art-of-unit-testing-third-edition)
- **Teacher's note:** Testing should be so easy that not doing it feels wrong

## Week 27: Standard Library Design - Core Types
- **Requirement:** Implement essential standard library types and functions
- **Uses this concept:** API design, standard library organization, performance
- **Learn this:** API ergonomics, naming conventions, documentation standards
- **Problem:** Design APIs that are both safe and efficient for common operations
- **Sources:**
  - [API Design for C++ by Martin Reddy](https://www.apibook.com/)
  - [Rust Standard Library Source](https://github.com/rust-lang/rust/tree/master/library)
  - [Beautiful Code - Standard Library Chapters](https://www.oreilly.com/library/view/beautiful-code/9780596510046/)
  - [C Standard Library Specification](https://port70.net/~nsz/c/c11/n1570.html#7)
- **Teacher's note:** Your standard library is your language's first impression - make it count

## Week 28: Memory Allocator Integration
- **Requirement:** Integrate custom allocators with safety guarantees
- **Uses this concept:** Memory allocation strategies, allocator APIs, safety invariants
- **Learn this:** Allocation patterns, memory pools, RAII for resources
- **Problem:** Support custom allocators while maintaining memory safety guarantees
- **Sources:**
  - [The Memory Management Reference](https://www.memorymanagement.org/)
  - [Effective C++ Memory Management Items](https://www.aristeia.com/books.html)
  - [jemalloc Design Documentation](http://jemalloc.net/jemalloc.3.html)
  - [Reconsidering Custom Memory Allocation](https://www.cs.umass.edu/~emery/pubs/berger-pldi2002.pdf)
- **Teacher's note:** Good allocation strategy can make or break performance-critical code

## Week 29: Compiler Self-hosting Preparation
- **Requirement:** Begin rewriting compiler in your own language
- **Uses this concept:** Self-hosting compilers, bootstrap compilation, language maturity
- **Learn this:** Circular dependencies, cross-compilation, version management
- **Problem:** Use your language to build increasingly complex programs including parts of itself
- **Sources:**
  - [The Implementation of Functional Programming Languages](https://www.microsoft.com/en-us/research/publication/the-implementation-of-functional-programming-languages/)
  - [Compilers and Compiler Generators](http://www.cs.ru.ac.za/research/g02w2114/entry/)
  - [Historical Accounts of Self-hosting Compilers](https://www.cs.virginia.edu/~weimer/2008-415/reading/VanDerPoel-HistoricalSurvey.pdf)
  - [Reflections on Trusting Trust by Ken Thompson](https://www.cs.cmu.edu/~rdriley/487/papers/Thompson_1984_ReflectionsonTrustingTrust.pdf)
- **Teacher's note:** Self-hosting is the ultimate test - your language must be pleasant enough to build itself

## Week 30: Performance Profiling & Benchmarking
- **Requirement:** Implement profiling tools and comprehensive benchmarks vs C
- **Uses this concept:** Performance measurement, statistical analysis, benchmarking methodology
- **Learn this:** Microbenchmarks, profiling techniques, performance regression testing
- **Problem:** Prove your zero-overhead claims with rigorous benchmarks
- **Sources:**
  - [Systems Performance by Brendan Gregg](http://www.brendangregg.com/systems-performance-book.html)
  - [The Art of Computer Systems Performance Analysis](https://www.wiley.com/en-us/The+Art+of+Computer+Systems+Performance+Analysis-p-9780471503361)
  - [Google Benchmark Library](https://github.com/google/benchmark)
  - [Producing Wrong Data Without Doing Anything Obviously Wrong!](https://users.cs.northwestern.edu/~robby/courses/322-2013-spring/mytkowicz-wrong-data.pdf)
- **Teacher's note:** Numbers don't lie - your performance claims need solid evidence

## Week 31: IDE Integration & Language Server
- **Requirement:** Implement Language Server Protocol for editor integration
- **Uses this concept:** Language servers, IDE integration, developer tooling
- **Learn this:** LSP specification, incremental parsing, IDE UX design
- **Problem:** Provide rich editing experience with autocomplete, error checking, refactoring
- **Sources:**
  - [Language Server Protocol Specification](https://microsoft.github.io/language-server-protocol/)
  - [Creating a Language Server Tutorial](https://code.visualstudio.com/api/language-extensions/language-server-extension-guide)
  - [rust-analyzer Architecture](https://github.com/rust-lang/rust-analyzer/blob/master/docs/dev/architecture.md)
  - [Tree-sitter Parsing Library](https://tree-sitter.github.io/tree-sitter/)
- **Teacher's note:** Great tooling makes your language feel professional and polished

## Week 32: Documentation System
- **Requirement:** Build integrated documentation generator (`lang doc`)
- **Uses this concept:** Documentation generation, API documentation, literate programming
- **Learn this:** Doc comment parsing, cross-referencing, documentation UX
- **Problem:** Generate beautiful, searchable documentation from source code
- **Sources:**
  - [Docs or it Didn't Happen - Documentation Best Practices](https://www.writethedocs.org/guide/)
  - [rustdoc Implementation Study](https://github.com/rust-lang/rust/tree/master/src/librustdoc)
  - [The Art of Readable Code](https://www.oreilly.com/library/view/the-art-of/9781449318482/)
  - [Sphinx Documentation Generator](https://www.sphinx-doc.org/)
- **Teacher's note:** Great docs turn users into advocates - invest in making them shine

## Week 33: Error Message Engineering
- **Requirement:** Implement world-class compiler error messages
- **Uses this concept:** Human-computer interaction, error reporting UX, compiler diagnostics
- **Learn this:** Error message psychology, source location tracking, suggestion systems
- **Problem:** Make compiler errors helpful enough that beginners can fix them independently
- **Sources:**
  - [Compiler Error Messages Considered Harmful](https://dl.acm.org/doi/10.1145/3462.315057)
  - [Rust Compiler Error Message Design](https://blog.rust-lang.org/2016/08/10/Shape-of-errors-to-come.html)
  - [The Design and Implementation of Programming Languages](https://cs.brown.edu/courses/cs173/2012/book/)
  - [User Testing for Developer Tools](https://increment.com/testing/user-research-for-developer-tools/)
- **Teacher's note:** Error messages are often the most-read part of your documentation

## Week 34: WebAssembly Backend
- **Requirement:** Add WebAssembly compilation target
- **Uses this concept:** Multiple compilation targets, portable bytecode, web integration
- **Learn this:** WebAssembly instruction set, WASI system interface, web APIs
- **Problem:** Enable your systems language to run in browsers and edge environments
- **Sources:**
  - [WebAssembly Specification](https://webassembly.github.io/spec/)
  - [Programming WebAssembly with Rust](https://pragprog.com/titles/khrust/programming-webassembly-with-rust/)
  - [WASI Tutorial and Specification](https://wasi.dev/)
  - [Emscripten Toolchain Documentation](https://emscripten.org/docs/)
- **Teacher's note:** WebAssembly opens up entirely new deployment targets for systems code

## Week 35: Cross-compilation Support
- **Requirement:** Enable cross-compilation to multiple architectures
- **Uses this concept:** Cross-compilation, target specifications, toolchain management
- **Learn this:** Target triples, cross-linkers, architecture-specific code generation
- **Problem:** Compile on x86-64 for ARM, RISC-V, and other architectures
- **Sources:**
  - [LLVM Target Documentation](https://llvm.org/docs/WritingAnLLVMBackend.html)
  - [Cross-compilation with Clang](https://clang.llvm.org/docs/CrossCompilation.html)
  - [GNU Autotools Cross-compilation Manual](https://www.gnu.org/software/automake/manual/html_node/Cross_002dCompilation.html)
  - [Rust Cross-compilation Guide](https://rust-lang.github.io/rustup/cross-compilation.html)
- **Teacher's note:** Cross-compilation multiplies your language's reach - embedded systems await

## Week 36: Security Analysis Tools
- **Requirement:** Implement static analysis for security vulnerabilities
- **Uses this concept:** Security analysis, taint analysis, information flow
- **Learn this:** Security vulnerability patterns, static analysis for security, threat modeling
- **Problem:** Detect potential security issues beyond memory safety
- **Sources:**
  - [Secure Programming with Static Analysis](https://www.informit.com/store/secure-programming-with-static-analysis-9780321424778)
  - [The CERT C Secure Coding Standard](https://wiki.sei.cmu.edu/confluence/display/c/SEI+CERT+C+Coding+Standard)
  - [Coverity Static Analysis Patterns](https://scan.coverity.com/documents/10379/c-cpp-checkers)
  - [Building Secure Software by Viega & McGraw](https://www.amazon.com/Building-Secure-Software-Addison-Wesley-Professional/dp/020172152X)
- **Teacher's note:** Security goes beyond memory safety - help developers avoid entire vulnerability classes

## Week 37: Formal Verification Integration
- **Requirement:** Add formal verification support for critical code sections
- **Uses this concept:** Formal methods, proof assistants, specification languages
- **Learn this:** Temporal logic, model checking, interactive theorem proving
- **Problem:** Verify critical algorithms are correct with mathematical certainty
- **Sources:**
  - [Software Foundations by Pierce et al.](https://softwarefoundations.cis.upenn.edu/)
  - [Concrete Semantics with Isabelle/HOL](http://concrete-semantics.org/)
  - [CBMC Bounded Model Checker Tutorial](https://www.cprover.org/cbmc/doc/)
  - [Dafny Verification-aware Programming Language](https://dafny-lang.github.io/dafny/)
- **Teacher's note:** Formal verification is the ultimate safety net for mission-critical code

## Week 38: Community & Ecosystem Development
- **Requirement:** Launch public compiler release and build initial community
- **Uses this concept:** Open source community building, project governance, ecosystem development
- **Learn this:** Community management, contribution guidelines, project roadmapping
- **Problem:** Attract contributors and early adopters to your language ecosystem
- **Sources:**
  - [Producing Open Source Software by Karl Fogel](https://producingoss.com/)
  - [The Architecture of Open Source Applications](https://aosabook.org/)
  - [Mozilla/Rust Community Building Case Studies](https://blog.rust-lang.org/2017/02/06/roadmap.html)
  - [Working in Public by Nadia Eghbal](https://www.amazon.com/Working-Public-Making-Maintenance-Software/dp/0578675862)
- **Teacher's note:** Technical excellence needs community to thrive - start building relationships early

## Week 39: Performance Optimization Deep Dive
- **Requirement:** Implement advanced optimizations for hot paths
- **Uses this concept:** Advanced compiler optimizations, profile-guided optimization, vectorization
- **Learn this:** Loop optimization, vectorization, profile-guided optimization, LTO
- **Problem:** Achieve performance that consistently matches or exceeds hand-optimized C
- **Sources:**
  - [Advanced Compiler Design and Implementation](https://www.amazon.com/Advanced-Compiler-Design-Implementation-Muchnick/dp/1558603204)
  - [Intel Optimization Reference Manual](https://www.intel.com/content/www/us/en/architecture-and-technology/64-ia-32-architectures-optimization-manual.html)
  - [LLVM Vectorizer Documentation](https://llvm.org/docs/Vectorizers.html)
  - [Software Optimization Resources by Agner Fog](https://www.agner.org/optimize/)
- **Teacher's note:** The final 5% performance gains often require the deepest compiler knowledge

## Week 40: Language Specification & Future Roadmap
- **Requirement:** Complete formal language specification and development roadmap
- **Uses this concept:** Language standardization, specification writing, evolution planning
- **Learn this:** Specification formalism, backward compatibility, language evolution
- **Problem:** Document your language precisely enough for alternative implementations
- **Sources:**
  - [ISO C Standard Structure](https://www.iso.org/standard/74528.html)
  - [Programming Language Specification Writing Guide](https://cs.brown.edu/courses/cs173/2012/book/types.html)
  - [Language Evolution Case Studies](https://www.stroustrup.com/hopl2.pdf)
  - [The Design and Evolution of C++ by Stroustrup](https://www.stroustrup.com/dne.html)
- **Teacher's note:** A complete specification marks your transition from prototype to production language

---

## Appendix: Language Design Principles Summary

### Core Non-Negotiables
- **Performance:** Absolutely zero runtime overhead compared to C
- **Safety:** Memory safety guarantees as strong as Rust
- **Memory Model:** Custom ownership system (NOT Rust's borrow checker)
- **Compliance:** Never compromise on these requirements

### Custom Memory Model
- **Linear ownership:** Resources have exactly one owner at a time
- **Automatic cleanup:** RAII-style resource management
- **Move semantics:** Transfer ownership explicitly with move operations
- **Borrowing (optional):** Lightweight references with lexical lifetimes
- **Simpler than Rust:** No complex lifetime annotations or borrow checker

### Array Bounds Checking Rules
- **Compile-time verification:** Static bounds checking wherever possible
- **Runtime fallback:** Zero-cost exceptions for dynamic bounds violations
- **Force-safe mode (`--force-safe`):**
  - Permits dynamic array indexing
  - Requires manual bounds validation in user code  
  - Compiler verifies validation exists via static analysis
  - Maintains zero runtime overhead while ensuring safety
- **Unsafe escape:** `uinl {}` blocks bypass all bounds checking

### Build & Package System
- **Integrated toolchain:** `lang build`, `lang test`, `lang doc` commands
- **Package manifest:** `lang.toml` files like Cargo
- **Dependency management:** Semantic versioning with automatic resolution
- **Cross-compilation:** First-class support for multiple targets

### Safety Guarantees
- **Memory safety:** No use-after-free, double-free, or buffer overflows
- **Type safety:** No undefined behavior from type system violations  
- **Thread safety:** Data race prevention through ownership system
- **String safety:** UTF-8 validation guaranteed at all boundaries
- **Integer safety:** Range-restricted types prevent overflow vulnerabilities

### Performance Characteristics  
- **Zero-cost abstractions:** High-level code compiles to optimal machine code
- **Monomorphized generics:** No runtime overhead for generic programming
- **Static dispatch:** Trait system uses compile-time resolution
- **Exception model:** Zero overhead when no exceptions occur
- **Memory model:** Deterministic cleanup without garbage collection

### Development Philosophy
- **C interoperability:** Seamless FFI and ABI compatibility
- **Developer experience:** World-class error messages and tooling
- **Safety by default:** Unsafe operations require explicit opt-in
- **Performance transparency:** Predictable cost model for all operations
- **Ecosystem integration:** Built-in build system and package management