# Week-by-Week Roadmap for Building a C-Like Systems Language with Rust-Level Safety and Zero Overhead

## Phase 1: Foundations (Weeks 1-8)
### Week 1: Language Specification and Grammar  
- **Requirement:** Define core language syntax (variables, functions, types) and grammar rules.  
- **Uses this concept:** Context-free grammars, lexer/parser design.  
- **Learn this:** Formal grammar notation (EBNF), compiler frontend architecture.  
- **Problem:** Write a grammar for basic expressions and variable declarations.  
- **Sources:** [LLVM documentation](https://llvm.org/docs/), "Compilers: Principles, Techniques, and Tools" (Dragon Book).  
- **Teacher’s note:** Start with a subset of C-like syntax to avoid complexity. Use tools like ANTLR or handwrite a recursive descent parser.  

### Week 2: Memory Safety Foundations  
- **Requirement:** Study Rust’s memory safety guarantees and alternatives.  
- **Uses this concept:** Ownership/lifetimes vs. bounds checking.  
- **Learn this:** How Rust enforces safety at compile time :cite[5].  
- **Problem:** Implement a simple bounds-checked array in C.  
- **Sources:** [Rust Memory Safety Explained](https://www.infoworld.com/article/2336661/rust-memory-safety-explained.html) :cite[5], "Safe Systems Programming in C".  
- **Teacher’s note:** Focus on Rust’s compile-time checks but note we’re avoiding its ownership model.  

### Week 3: Bounds Checking Strategies  
- **Requirement:** Design compile-time and runtime bounds checking rules.  
- **Uses this concept:** Static analysis, runtime exception handling.  
- **Learn this:** Clang’s `-fbounds-safety` extension :cite[7].  
- **Problem:** Write a C macro for conditional bounds checks.  
- **Sources:** [Clang Bounds Safety](https://clang.llvm.org/docs/BoundsSafety.html) :cite[7].  
- **Teacher’s note:** Use `__counted_by`-style annotations for external bounds :cite[7].  

### Week 4: Zero-Cost Exception Mechanism  
- **Requirement:** Design `OutOfBounds` exception with zero runtime cost.  
- **Uses this concept:** Exception handling without overhead.  
- **Learn this:** LLVM’s exception model (e.g., `setjmp`/`longjmp` alternatives).  
- **Problem:** Implement a fault-tolerant array accessor in C.  
- **Sources:** LLVM Exception Handling docs.  
- **Teacher’s note:** Exceptions should use dedicated registers or compiler intrinsics.  

### Week 5: Type System Design  
- **Requirement:** Define range-restricted integers (e.g., `int[0..10]`).  
- **Uses this concept:** Dependent types, refinement types.  
- **Learn this:** How Ada/SPARK implements range types.  
- **Problem:** Create a type checker for range-constrained integers.  
- **Sources:** "Type-Driven Development with Idris", Ada Language Reference.  
- **Teacher’s note:** Represent ranges as compile-time constraints via generics.  

### Week 6: Unsafe Escape Hatch (`uinl`)  
- **Requirement:** Design `uinl {}` block semantics and safety rules.  
- **Uses this concept:** Unsafe code isolation.  
- **Learn this:** Rust’s `unsafe` model :cite[9].  
- **Problem:** Implement a macro to disable bounds checks in C.  
- **Sources:** [Rustonomicon](https://doc.rust-lang.org/nomicon/meet-safe-and-unsafe.html) :cite[9].  
- **Teacher’s note:** `uinl` should require explicit annotations for bypassing checks.  

### Week 7: String Encoding Guarantees  
- **Requirement:** Design UTF-8 strings with invalid encoding prevention.  
- **Uses this concept:** Unicode handling, invariant enforcement.  
- **Learn this:** How Rust handles UTF-8 validation.  
- **Problem:** Build a UTF-8 validator in C.  
- **Sources:** "Programming with Unicode", Rust `std::str` docs.  
- **Teacher’s note:** Store strings as byte arrays with metadata for length.  

### Week 8: Generics and Monomorphization  
- **Requirement:** Design generics system (like Go, no bloat).  
- **Uses this concept:** Monomorphization, type erasure.  
- **Learn this:** C++ templates vs. Go generics.  
- **Problem:** Implement a generic list in C using macros.  
- **Sources:** "Go Generics Proposal", C++ Template Metaprogramming.  
- **Teacher’s note:** Use compile-time code generation to avoid runtime costs.  

## Phase 2: Compiler Frontend (Weeks 9-20)
### Week 9: Lexer and Parser Implementation  
- **Requirement:** Build lexer/parser for core grammar.  
- **Uses this concept:** Recursive descent parsing, abstract syntax trees (ASTs).  
- **Learn this:** How to handle operator precedence.  
- **Problem:** Parse expressions with nested brackets.  
- **Sources:** "Crafting Interpreters", LLVM Tutorial.  
- **Teacher’s note:** Use visitor pattern for AST traversal.  

### Week 10: AST Design and Semantic Analysis  
- **Requirement:** Define AST structure and symbol table.  
- **Uses this concept:** Scope management, type resolution.  
- **Learn this:** Symbol table implementation.  
- **Problem:** Resolve variable names across scopes.  
- **Sources:** Dragon Book Chapter 6.  
- **Teacher’s note:** Implement a hierarchical symbol table.  

### Week 11: Type Checking Infrastructure  
- **Requirement:** Build type checker for base types and functions.  
- **Uses this concept:** Type inference, unification.  
- **Learn this:** Hindley-Milner type system.  
- **Problem:** Check type compatibility in expressions.  
- **Sources:** "Types and Programming Languages".  
- **Teacher’s note:** Start with explicit types; add inference later.  

### Week 12: Bounds Checking Integration  
- **Requirement:** Integrate bounds checking into type checker.  
- **Uses this concept:** Static analysis for array accesses.  
- **Learn this:** How to prove bounds safety :cite[7].  
- **Problem:** Validate array indices against `len` expressions.  
- **Sources:** [Clang Bounds Safety](https://clang.llvm.org/docs/BoundsSafety.html) :cite[7].  
- **Teacher’s note:** Use SMT solvers for complex bounds proofs.  

### Week 13: Range-Type Enforcement  
- **Requirement:** Implement range-restricted integer validation.  
- **Uses this concept:** Constraint propagation.  
- **Learn this:** How to track value ranges at compile time.  
- **Problem:** Propagate range constraints across assignments.  
- **Sources:** Ada Static Analysis tools.  
- **Teacher’s note:** Represent ranges as intervals in the type system.  

### Week 14: `--force-safe` Mode Implementation  
- **Requirement:** Add static analysis for manual bounds checks.  
- **Uses this concept:** Proof of validation existence.  
- **Learn this:** How to verify conditional guards.  
- **Problem:** Detect missing `if (i < len)` checks.  
- **Sources:** [Clang Bounds Safety](https://clang.llvm.org/docs/BoundsSafety.html) :cite[7].  
- **Teacher’s note:** Use control flow analysis to track conditionals.  

### Week 15: Error Handling Integration  
- **Requirement:** Add `Result<T,E>` and exception mechanisms.  
- **Uses this concept:** Sum types, zero-cost exceptions.  
- **Learn this:** How Rust implements `Result`.  
- **Problem:** Map bounds violations to exceptions.  
- **Sources:** [Rust Error Handling](https://doc.rust-lang.org/book/ch09-00-error-handling.html).  
- **Teacher’s note:** Use tagged unions for `Result` types.  

### Week 16: Traits/Interfaces Design  
- **Requirement:** Design trait system for compile-time polymorphism.  
- **Uses this concept:** Type classes, vtable avoidance.  
- **Learn this:** How Go interfaces work.  
- **Problem:** Resolve trait implementations statically.  
- **Sources:** "Go Interfaces", Rust Traits.  
- **Teacher’s note:** Use monomorphization to avoid runtime costs.  

### Week 17: Concurrency Model Design  
- **Requirement:** Design threads and channels (Go-style).  
- **Uses this concept:** Message passing, shared-nothing concurrency.  
- **Learn this:** How to prevent data races.  
- **Problem:** Implement a channel in C using pthreads.  
- **Sources:** "Concurrency in Go", Rust `std::sync`.  
- **Teacher’s note:** Use linear types to enforce channel safety.  

### Week 18: FFI Mechanism  
- **Requirement:** Design C FFI using `extern` blocks.  
- **Uses this concept:** Foreign function interface, ABI compatibility.  
- **Learn this:** How Rust handles C FFI.  
- **Problem:** Call a C function from your language.  
- **Sources:** "Rust FFI Omnibus".  
- **Teacher’s note:** Use C-compatible layouts for structs/types.  

### Week 19: Module System  
- **Requirement:** Design modules and visibility rules.  
- **Uses this concept:** Namespace management.  
- **Learn this:** How to avoid symbol collisions.  
- **Problem:** Resolve cross-module type references.  
- **Sources": "Modular Programming Languages".  
- **Teacher’s note:** Use file-based modules with explicit exports.  

### Week 20: Frontend Integration  
- **Requirement:** Combine all frontend components.  
- **Uses this concept:** Compiler architecture.  
- **Learn this:** How to manage compiler phases.  
- **Problem:** Report multiple errors gracefully.  
- **Sources:** "Engineering a Compiler".  
- **Teacher’s note:** Use incremental compilation for faster development.  

## Phase 3: Backend and Code Generation (Weeks 21-32)
### Week 21: LLVM IR Introduction  
- **Requirement:** Learn LLVM IR and code generation basics.  
- **Uses this concept:** Intermediate representation.  
- **Learn this:** How to generate LLVM IR from AST.  
- **Problem:** Emit IR for arithmetic expressions.  
- **Sources:** [LLVM Language Reference](https://llvm.org/docs/LangRef.html).  
- **Teacher’s note:** Use LLVM’s C++ API for IR generation.  

### Week 22: CodeGen for Expressions and Variables  
- **Requirement:** Generate IR for variables and expressions.  
- **Uses this concept:** Symbol table mapping to IR.  
- **Learn this:** How to handle integer ranges in IR.  
- **Problem:** Enforce range constraints in IR.  
- **Sources:** LLVM Tutorial Chapter 2.  
- **Teacher’s note:** Use LLVM’s range metadata for optimizations.  

### Week 23: CodeGen for Functions and Control Flow  
- **Requirement:** Generate IR for functions and branches.  
- **Uses this concept:** CFG construction.  
- **Learn this:** How to translate conditionals to IR.  
- **Problem:** Implement short-circuit logic in IR.  
- **Sources:** LLVM Tutorial Chapter 3.  
- **Teacher’s note:** Use LLVM’s `phi` nodes for SSA form.  

### Week 24: CodeGen for Arrays and Bounds Checks  
- **Requirement:** Emit bounds-checked array accesses.  
- **Uses this concept:** Runtime checks via LLVM intrinsics.  
- **Learn this:** How to insert conditional traps.  
- **Problem:** Optimize away redundant bounds checks.  
- **Sources:** [Clang Bounds Safety](https://clang.llvm.org/docs/BoundsSafety.html) :cite[7].  
- **Teacher’s note:** Use LLVM’s `@llvm.trap` for bounds violations.  

### Week 25: CodeGen for Traits and Generics  
- **Requirement:** Monomorphize generics and traits.  
- **Uses this concept:** Template instantiation.  
- **Learn this:** How to avoid code bloat.  
- **Problem:** Generate specialized functions for each type.  
- **Sources:** "C++ Templates".  
- **Teacher’s note:** Use LLVM’s `linkonce_odr` linkage for generics.  

### Week 26: Exception Handling CodeGen  
- **Requirement:** Implement zero-cost exceptions for bounds errors.  
- **Uses this concept:** Table-based exception handling.  
- **Learn this:** LLVM’s `landingpad` instruction.  
- **Problem:** Propagate exceptions across functions.  
- **Sources:** LLVM Exception Handling docs.  
- **Teacher’s note:** Use setjmp/longjmp for simplicity initially.  

### Week 27: Concurrency CodeGen  
- **Requirement:** Generate code for threads and channels.  
- **Uses this concept:** Runtime integration with pthreads.  
- **Learn this:** How to implement message queues.  
- **Problem:** Ensure thread-safe channel operations.  
- **Sources:** "Programming with POSIX Threads".  
- **Teacher’s note:** Use LLVM’s atomic operations for channels.  

### Week 28: Optimization Passes  
- **Requirement:** Add optimization passes (e.g., bounds check elimination).  
- **Uses this concept:** Compiler optimizations.  
- **Learn this:** How to use LLVM’s pass manager.  
- **Problem:** Remove redundant range checks.  
- **Sources:** LLVM Passes Documentation.  
- **Teacher’s note:** Write custom passes for language-specific optimizations.  

### Week 29: Debug Information  
- **Requirement:** Add DWARF debug information.  
- **Uses this concept:** Debug symbol generation.  
- **Learn this:** How to map IR to source lines.  
- **Problem:** Generate debug info for variables.  
- **Sources:** "Introduction to DWARF".  
- **Teacher’s note:** Use LLVM’s debug intrinsic functions.  

### Week 30: Executable Linking  
- **Requirement:** Link against C libraries via FFI.  
- **Uses this concept:** Dynamic linking.  
- **Learn this:** How to handle external symbols.  
- **Problem:** Call `libc` functions from generated code.  
- **Sources:** "Linkers and Loaders".  
- **Teacher’s note:** Use `llvm-link` to merge IR modules.  

### Week 31: Testing Compiler Output  
- **Requirement:** Validate generated code correctness.  
- **Uses this concept:** Testing frameworks.  
- **Learn this:** How to write compiler tests.  
- **Problem:** Test bounds checks with edge cases.  
- **Sources:** "Compiler Testing via Path Analysis".  
- **Teacher’s note:** Use LLVM’s `lit` testing tool.  

### Week 32: Performance Benchmarking  
- **Requirement:** Benchmark against C programs.  
- **Uses this concept:** Performance analysis.  
- **Learn this:** How to measure overhead.  
- **Problem:** Compare array access performance with C.  
- **Sources:** "Systems Performance: Enterprise and the Cloud".  
- **Teacher’s note:** Use `perf` to analyze cycle counts.  

## Phase 4: Ecosystem and Tooling (Weeks 33-48)
### Week 33: Build System Design  
- **Requirement:** Design `lang build` for compilation orchestration.  
- **Uses this concept:** Dependency graphing.  
- **Learn this:** How build tools work :cite[3]:cite[8].  
- **Problem:** Resolve inter-file dependencies.  
- **Sources:** [Build Tools Overview](https://www.browserstack.com/guide/build-tools) :cite[3], [List of Build Automation Software](https://en.wikipedia.org/wiki/List_of_build_automation_software) :cite[8].  
- **Teacher’s note:** Use a TOML manifest for project metadata.  

### Week 34: Package Manager Design  
- **Requirement:** Design `lang.toml` and package resolution.  
- **Uses this concept:** Semantic versioning, dependency resolution.  
- **Learn this:** How Cargo works.  
- **Problem:** Resolve version conflicts.  
- **Sources:** "Cargo Book", [Maven vs. Gradle](https://www.browserstack.com/guide/build-tools) :cite[3].  
- **Teacher’s note:** Use a central package registry for libraries.  

### Week 35: Test Runner Implementation  
- **Requirement:** Build `lang test` for unit/integration tests.  
- **Uses this concept:** Test discovery, mocking.  
- **Learn this:** How to isolate tests.  
- **Problem:** Run tests in parallel.  
- **Sources:** "Go Testing", [Jenkins CI](https://www.browserstack.com/guide/build-tools) :cite[3].  
- **Teacher’s note:** Use built-in assertions for bounds safety.  

### Week 36: Standard Library Basics  
- **Requirement:** Implement core types (e.g., `Array`, `String`).  
- **Uses this concept:** API design, memory safety.  
- **Learn this:** How to avoid standard library bottlenecks.  
- **Problem:** Implement UTF-8 validation for `String`.  
- **Sources:** "Rust Standard Library Design".  
- **Teacher’s note:** Write the standard library in your language.  

### Week 37: Memory Allocator Design  
- **Requirement:** Build a safe allocator (e.g., pool-based).  
- **Uses this concept:** Custom allocators :cite[6].  
- **Learn this:** How to avoid fragmentation.  
- **Problem:** Implement a real-time allocator :cite[6].  
- **Sources:** [Custom Allocator Tutorial](https://docs.ros.org/en/crystal/Tutorials/Allocator-Template-Tutorial.html) :cite[6].  
- **Teacher’s note:** Use TLSF allocator for real-time safety :cite[6].  

### Week 38: Concurrency Library  
- **Requirement:** Implement threads and channels in stdlib.  
- **Uses this concept:** Sync primitives.  
- **Learn this:** How to prevent deadlocks.  
- **Problem:** Build a thread-safe channel queue.  
- **Sources:** "Java Concurrency in Practice".  
- **Teacher’s note:** Use type system to enforce linear channel ownership.  

### Week 39: FFI Wrappers  
- **Requirement:** Generate bindings for C libraries.  
- **Uses this concept:** Automated wrapper generation.  
- **Learn this:** How to map C types to language types.  
- **Problem:** Wrap `libcurl` for HTTP requests.  
- **Sources:** "Automated FFI Binding Generation".  
- **Teacher’s note:** Use Clang’s AST to parse C headers.  

### Week 40: Debugger Integration  
- **Requirement:** Add support for GDB/LLDB.  
- **Uses this concept:** Debugger extensions.  
- **Learn this:** How to extend debuggers.  
- **Problem:** Add pretty-printers for range types.  
- **Sources:** "GDB Python API".  
- **Teacher’s note:** Use DWARF extensions for custom types.  

### Week 41: IDE Support (LSP)  
- **Requirement:** Implement Language Server Protocol.  
- **Uses this concept:** Editor integration.  
- **Learn this:** How to provide autocomplete.  
- **Problem:** Resolve symbols across files.  
- **Sources:** "LSP Specification".  
- **Teacher’s note:** Reuse the compiler’s symbol table.  

### Week 42: Documentation Generator  
- **Requirement:** Build `lang doc` for API docs.  
- **Uses this concept:** Static site generation.  
- **Learn this:** How to extract doc comments.  
- **Problem:** Generate Markdown from source comments.  
- **Sources:** "How Rustdoc Works".  
- **Teacher’s note:** Use a subset of Markdown for comments.  

### Week 43: Security Auditing Tools  
- **Requirement:** Add static analysis for safety violations.  
- **Uses this concept:** Security-focused linting.  
- **Learn this:** How to detect unsafe patterns.  
- **Problem:** Find missing `uinl` in unsafe code.  
- **Sources:** "Secure Programming Patterns".  
- **Teacher’s note:** Use data flow analysis for audit trails.  

### Week 44: Cross-Compilation  
- **Requirement:** Support cross-compilation to ARM/RISC-V.  
- **Uses this concept:** Triple-target code generation.  
- **Learn this:** How to handle ABI differences.  
- **Problem:** Generate ARM assembly from IR.  
- **Sources:** "LLVM Cross-Compilation".  
- **Teacher’s note:** Use LLVM’s target customization hooks.  

### Week 45: Runtime Minimization  
- **Requirement:** Eliminate runtime dependencies (e.g., no GC).  
- **Uses this concept:** Bare-metal programming.  
- **Learn this:** How to write OS kernels.  
- **Problem:** Build a freestanding "hello world".  
- **Sources:** "Writing an OS in Rust".  
- **Teacher’s note:** Use `-ffreestanding` flag in LLVM.  

### Week 46: Formal Verification Setup  
- **Requirement:** Add support for proof assistants (e.g., Coq).  
- **Uses this concept:** Formal methods.  
- **Learn this:** How to export axioms to Coq.  
- **Problem:** Prove bounds safety for a small function.  
- **Sources:** "Software Foundations".  
- **Teacher’s note:** Focus on core axioms for array accesses.  

### Week 47: Community and Packaging  
- **Requirement:** Set up package registry and documentation site.  
- **Uses this concept:** Open-source project management.  
- **Learn this:** How to manage contributions.  
- **Problem:** Accept external packages into registry.  
- **Sources:** "Producing Open Source Software".  
- **Teacher’s note:** Use CI to auto-test contributed packages :cite[3].  

### Week 48: Release and Optimization  
- **Requirement:** Release v1.0 and optimize critical paths.  
- **Uses this concept:** Performance tuning.  
- **Learn this:** How to profile entire programs.  
- **Problem:** Reduce code size by 10%.  
- **Sources:** "Optimizing Software in C++".  
- **Teacher’s note:** Use LLVM’s PGO for hot paths.  

## Appendix: Language Design Principles  
### Custom Memory Model  
- **No ownership cloning:** Lighter than Rust’s model; uses scope-based lifetimes with optional borrowing.  
- **Bounds-focused safety:** Relies on compile-time bounds checking and user-provided validation (`--force-safe` mode).  
- **Zero-cost abstractions:** All features must compile to efficient machine code.  

### Array Bound Rules  
- **Compile-time checking:** Indices must be provably safe at compile time where possible.  
- **Runtime fallback:** For dynamic indices, inject zero-cost traps (optimized away if safe).  
- `--force-safe` **mode:** Requires manual checks (e.g., `if (i < len)`); rejects code without them.  
- **Unsafe escape:** `uinl {}` blocks allow bypassing checks explicitly.  

### Build and Package Tools  
- **Built-in tools:** `lang build`, `lang test`, and package manager with `lang.toml`.  
- **Dependency resolution:** Semantic versioning with central registry.  
- **CI/CD integration:** Automated testing and benchmarking :cite[3].  

### Non-Negotiable Constraints  
1. **Performance = C:** Zero runtime overhead; monomorphized generics, no GC.  
2. **Safety = Rust:** Memory safety via bounds checking, range types, and concurrency guarantees.  
3. **No Rust ownership clone:** Custom model based on scope lifetimes and static analysis.  
4. **Simplicity:** Avoid complex features (e.g., full dependent types); focus on practical safety.  