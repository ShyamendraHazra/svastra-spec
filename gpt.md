
# Roadmap: Designing & Implementing a C-like Systems Language (Rust-Safe, C-Fast)

---

## Phase 1: Foundations (Weeks 1–8)

### Week 1: Language Design Principles & Specification
- **Requirement:** Write the full language specification (syntax, semantics, safety rules, memory model).
- **Uses this concept:** Programming language pragmatics, safety/performance trade-offs.
- **Learn this:** Writing formal specs, design principles, balancing ergonomics with soundness.
- **Problem:** Draft a 20–30 page spec including arrays, slices, UTF-8 strings, range types, `uinl {}`, and `--force-safe`.
- **Sources:**  
  - [Programming Language Pragmatics (Michael Scott)](https://dickgrune.com/Books/PLP_3rd_Edition/)  
  - [Rust RFCs](https://rust-lang.github.io/rfcs/)  
  - [Go Language Specification](https://golang.org/ref/spec)  
- **Teacher’s note:** This is your language’s constitution. Every later decision must trace back here.

### Week 2: Compiler Architecture & Tooling Blueprint
- **Requirement:** Define the compiler pipeline (lexer → parser → AST → MIR → borrow checker → LLVM IR → binary).
- **Uses this concept:** Compiler phases, intermediate representations, build tools.
- **Learn this:** Multi-pass vs single-pass design, Cargo-like build system architecture.
- **Problem:** Draw full architecture diagrams (compiler passes, IR stages, build/test/package workflow).
- **Sources:**  
  - [Engineering a Compiler (Cooper & Torczon)](https://www.cs.rice.edu/~keith/Comp422/lectures/Compilers.pdf)  
  - [The Cargo Book](https://doc.rust-lang.org/cargo/)  
  - [LLVM Language Reference Manual](https://llvm.org/docs/LangRef.html)  
- **Teacher’s note:** Think of this as designing your compiler’s factory floor before turning on the machines.

### Week 3: Lexer Implementation
- **Requirement:** Implement a hand-written lexer that supports UTF-8, keywords, identifiers, literals, comments.
- **Uses this concept:** Finite automata, regexes, token classification.
- **Learn this:** Unicode handling, error recovery, hand-written vs generated lexers.
- **Problem:** Tokenize `let arr: int[0..10] = [1,2,3];`.
- **Sources:**  
  - [Crafting Interpreters (Scanning chapter)](https://craftinginterpreters.com/scanning.html)  
  - [Flex & Bison Tutorial](http://aquamentus.com/flex_bison.html)  
  - [UTF-8 Everywhere Manifesto](https://utf8everywhere.org/)  
- **Teacher’s note:** UTF-8 correctness is mandatory — string bugs = security holes.

### Week 4: Grammar & Parser
- **Requirement:** Implement recursive descent parser with Pratt parsing for expressions.
- **Uses this concept:** Context-free grammars, precedence rules.
- **Learn this:** Eliminating left recursion, Pratt parsing, parser combinators.
- **Problem:** Parse nested array indexing with precedence: `arr[i + 2 * j]`.
- **Sources:**  
  - [Crafting Interpreters (Parsing Expressions)](https://craftinginterpreters.com/parsing-expressions.html)  
  - [Pratt Parsing by matklad](https://matklad.github.io/2020/04/13/simple-but-powerful-pratt-parsing.html)  
  - [Compilers: Principles, Techniques, and Tools (Dragon Book)](https://suif.stanford.edu/dragonbook/)  
- **Teacher’s note:** Aim for human-readable error messages — developer experience is part of language design.

### Week 5: AST & Semantic Analysis
- **Requirement:** Define AST structures; implement symbol tables, scope, and name resolution.
- **Uses this concept:** Abstract syntax trees, scoping rules.
- **Learn this:** Tree walking, lexical vs dynamic scoping.
- **Problem:** Parse and resolve variables in nested scopes with shadowing.
- **Sources:**  
  - [Crafting Interpreters (AST and scope)](https://craftinginterpreters.com/statements-and-state.html)  
  - [Modern Compiler Implementation in C](https://www.cs.princeton.edu/~appel/modern/)  
- **Teacher’s note:** Your AST is your program’s skeleton — build it strong.

### Week 6: Type System Basics
- **Requirement:** Implement type checker: primitive types, arrays, slices, range-restricted ints.
- **Uses this concept:** Static typing, Hindley-Milner basics.
- **Learn this:** Type inference, constraints.
- **Problem:** Ensure `int[0..10]` can’t assign `11`.
- **Sources:**  
  - [Types and Programming Languages (Pierce)](https://www.cis.upenn.edu/~bcpierce/tapl/)  
  - [Simple Type Checker in OCaml](https://dev.realworldocaml.org/type-checking.html)  
- **Teacher’s note:** Your type checker is the first safety net. Keep it uncompromising.

### Week 7: Intermediate Representation (IR)
- **Requirement:** Build MIR-like IR for borrow checking and optimizations.
- **Uses this concept:** Control-flow graphs, SSA form.
- **Learn this:** How MIR differs from AST and LLVM IR.
- **Problem:** Lower loops and conditionals into MIR with explicit temporaries.
- **Sources:**  
  - [MIR blog (nikomatsakis)](https://blog.rust-lang.org/2016/04/19/MIR.html)  
  - [LLVM SSA Construction](https://llvm.org/docs/doxygen/html/SSAUpdater_8cpp_source.html)  
- **Teacher’s note:** MIR is where safety and performance start to dance.

### Week 8: Borrow Checker Foundations
- **Requirement:** Implement ownership + borrow rules at MIR level.
- **Uses this concept:** Lifetimes, linear types.
- **Learn this:** Non-lexical lifetimes, region inference.
- **Problem:** Forbid mutable aliasing: `let a=&mut x; let b=&x;`.
- **Sources:**  
  - [Rust Non-Lexical Lifetimes RFC](https://rust-lang.github.io/rfcs/2094-nll.html)  
  - [Linear Types paper](https://homepages.inf.ed.ac.uk/wadler/papers/linear/linear.pdf)  
- **Teacher’s note:** This is the beating heart of safety. Fail loudly and early.

---

## Phase 2: Safety Infrastructure (Weeks 9–16)

### Week 9: Bounds Checking & `--force-safe`
- **Requirement:** Implement compile-time bounds checks; runtime traps only when proof impossible.
- **Uses this concept:** Refinement types, control-flow dominance.
- **Learn this:** Range analysis, static assertions.
- **Problem:** Verify `if (i < arr.len) arr[i]` is safe without runtime check.
- **Sources:**  
  - [Refinement Types (Liquid Haskell intro)](https://ucsd-progsys.github.io/liquidhaskell-tutorial/)  
  - [CompCert Range Analysis](https://hal.science/hal-02324106/document)  
- **Teacher’s note:** Teach the compiler to prove what the human already wrote.

### Week 10: Error Handling
- **Requirement:** Implement `Result<T,E>`, `Option<T>`, and zero-cost exceptions for UB prevention.
- **Uses this concept:** Algebraic data types, sum types.
- **Learn this:** Tagged unions, zero-cost EH tables.
- **Problem:** Lower `try {}` block into `Result` + unwinding tables.
- **Sources:**  
  - [Rust Error Handling Book](https://doc.rust-lang.org/book/ch09-00-error-handling.html)  
  - [Zero-cost exceptions (Itanium ABI)](https://itanium-cxx-abi.github.io/cxx-abi/abi-eh.html)  
- **Teacher’s note:** Error handling must feel natural, not bolted on.

### Week 11: Traits & Interfaces
- **Requirement:** Implement compile-time traits (interfaces) with monomorphization.
- **Uses this concept:** Type classes, static dispatch.
- **Learn this:** Trait resolution, coherence.
- **Problem:** Implement trait `Show { fn show(&self) -> str }` for `int` and arrays.
- **Sources:**  
  - [Rust Traits Reference](https://doc.rust-lang.org/reference/items/traits.html)  
  - [Haskell Type Classes](https://en.wikibooks.org/wiki/Haskell/Classes_and_types)  
- **Teacher’s note:** Compile-time polymorphism is power without runtime tax.

### Week 12: Generics
- **Requirement:** Add generic functions and types with monomorphization.
- **Uses this concept:** Parametric polymorphism.
- **Learn this:** Monomorphization vs reified generics.
- **Problem:** Implement `fn max<T: Ord>(a: T, b: T) -> T`.
- **Sources:**  
  - [Rust Generics](https://doc.rust-lang.org/book/ch10-00-generics.html)  
  - [C++ Templates Basics](https://en.cppreference.com/w/cpp/language/templates)  
- **Teacher’s note:** Generics without runtime overhead are worth the complexity.

### Week 13: Ownership Extensions
- **Requirement:** Add region-based memory model + optional borrowing.
- **Uses this concept:** Region inference, affine types.
- **Learn this:** Region-based memory (Tofte & Talpin).
- **Problem:** Free region at scope end without leaks or double frees.
- **Sources:**  
  - [Region-Based Memory Management paper](https://dl.acm.org/doi/10.1145/165180.165214)  
  - [Rust Ownership chapter](https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html)  
- **Teacher’s note:** Optional ownership is seasoning — don’t let it dilute safety.

### Week 14: Concurrency Model
- **Requirement:** Implement Go-style channels + safe threads with auto traits (`Send`, `Sync`).
- **Uses this concept:** CSP (Communicating Sequential Processes).
- **Learn this:** Thread safety, message passing.
- **Problem:** Spawn two threads, send/receive messages without data races.
- **Sources:**  
  - [Go Concurrency Patterns](https://go.dev/blog/pipelines)  
  - [Rust Send & Sync](https://doc.rust-lang.org/nomicon/send-and-sync.html)  
- **Teacher’s note:** Concurrency must be boringly safe, not excitingly dangerous.

### Week 15: Unsafe & `uinl {}` Blocks
- **Requirement:** Add `uinl {}` syntax; enforce contracts and quarantine unsafe.
- **Uses this concept:** Unsafe encapsulation, contracts.
- **Learn this:** Documenting invariants, linting.
- **Problem:** Wrap `memcpy` inside `uinl {}` with documented safety contract.
- **Sources:**  
  - [Rust Unsafe Code Guidelines](https://rust-lang.github.io/unsafe-code-guidelines/)  
  - [Designing Interfaces (Krug)](https://abseil.io/resources/swe-book/html/toc.html)  
- **Teacher’s note:** Unsafe is like nuclear power: contain it, monitor it, justify it.

### Week 16: Code Generation (LLVM IR)
- **Requirement:** Lower MIR into LLVM IR; emit working object files.
- **Uses this concept:** Backend compilation, SSA form.
- **Learn this:** LLVM IR basics, calling conventions.
- **Problem:** Lower simple arithmetic and function calls into LLVM IR.
- **Sources:**  
  - [LLVM Language Reference](https://llvm.org/docs/LangRef.html)  
  - [Kaleidoscope LLVM Tutorial](https://llvm.org/docs/tutorial/)  
- **Teacher’s note:** This is where your dream becomes machine code.

## Phase 3: Core Compiler & Runtime (Weeks 17–24)

### Week 17: Linking & FFI
- **Requirement:** Support calling C functions and linking against C libraries.
- **Uses this concept:** Foreign Function Interface (FFI), ABI compatibility.
- **Learn this:** Name mangling, extern declarations, linkage.
- **Problem:** Call `printf` from your language.
- **Sources:**  
  - [Rust FFI Guide](https://doc.rust-lang.org/nomicon/ffi.html)  
  - [System V ABI](https://uclibc.org/docs/psABI-x86_64.pdf)  
- **Teacher’s note:** FFI is how your language speaks with the world — it must be seamless.

### Week 18: Runtime & Startup
- **Requirement:** Write minimal runtime for startup, panic handling, and threading.
- **Uses this concept:** CRT (C runtime), language runtime vs library.
- **Learn this:** Entry points, stack setup, unwinding.
- **Problem:** Implement `_start` → `main` transition.
- **Sources:**  
  - [OSDev Bare Bones](https://wiki.osdev.org/Bare_Bones)  
  - [Musl libc source](https://musl.libc.org/)  
- **Teacher’s note:** Less runtime = more respect. Keep it lean.

### Week 19: Exception & Panic System
- **Requirement:** Finalize zero-cost exception model, panic unwinding vs abort.
- **Uses this concept:** Zero-cost EH tables, panic semantics.
- **Learn this:** Itanium unwinding, Rust `panic=abort`.
- **Problem:** Implement `panic!("bad")` unwinding tables.
- **Sources:**  
  - [Rust Unwinding](https://doc.rust-lang.org/nomicon/unwinding.html)  
  - [Itanium C++ ABI: Exception Handling](https://itanium-cxx-abi.github.io/cxx-abi/abi-eh.html)  
- **Teacher’s note:** Exceptions exist only for safety — not as control flow candy.

### Week 20: Optimizations Pass I
- **Requirement:** Add constant folding, DCE (dead code elimination), inlining.
- **Uses this concept:** IR optimizations.
- **Learn this:** Dataflow analysis, function inlining heuristics.
- **Problem:** Optimize away unused branch: `if (false) { ... }`.
- **Sources:**  
  - [LLVM Passes Documentation](https://llvm.org/docs/Passes.html)  
  - [Compilers: Principles, Techniques, and Tools (Dragon Book)](https://suif.stanford.edu/dragonbook/)  
- **Teacher’s note:** Safety first, performance right behind it. You must prove C-speed.

### Week 21: Debug Info & Tooling
- **Requirement:** Generate DWARF debug info, integrate with gdb/lldb.
- **Uses this concept:** Debug symbols, DWARF format.
- **Learn this:** How compilers emit debug metadata.
- **Problem:** Step through `langc` compiled code in gdb.
- **Sources:**  
  - [DWARF Standard](http://dwarfstd.org/)  
  - [LLVM Debugging Facilities](https://llvm.org/docs/SourceLevelDebugging.html)  
- **Teacher’s note:** Debuggers are your language’s stethoscopes. Make the heartbeat visible.

### Week 22: Build System (`lang build`)
- **Requirement:** Implement CLI build tool with manifests (`lang.toml`).
- **Uses this concept:** Build automation, dependency management.
- **Learn this:** TOML parsing, dependency graphs.
- **Problem:** Build and link multi-file project with `lang build`.
- **Sources:**  
  - [Cargo Manifest Reference](https://doc.rust-lang.org/cargo/reference/manifest.html)  
  - [TOML Spec](https://toml.io/en/)  
- **Teacher’s note:** No one wants Makefiles. Own your build system.

### Week 23: Testing Infrastructure
- **Requirement:** Add built-in test runner (`lang test`) with annotations.
- **Uses this concept:** Unit testing integration.
- **Learn this:** Test discovery, isolation.
- **Problem:** Run `#[test] fn check_bounds() { ... }`.
- **Sources:**  
  - [Rust Test Framework](https://doc.rust-lang.org/book/ch11-00-testing.html)  
  - [JUnit 5 Overview](https://junit.org/junit5/docs/current/user-guide/)  
- **Teacher’s note:** Testing is language-native. Don’t relegate it to libraries.

### Week 24: Package Manager
- **Requirement:** Build `lang add`, `lang remove`, `lang publish` commands.
- **Uses this concept:** Dependency resolution, semantic versioning.
- **Learn this:** Registry APIs, lockfiles.
- **Problem:** Publish a library and depend on it in another project.
- **Sources:**  
  - [Cargo Reference](https://doc.rust-lang.org/cargo/reference/publishing.html)  
  - [Semantic Versioning](https://semver.org/)  
- **Teacher’s note:** Ecosystem is king. Without packages, no adoption.

---

## Phase 4: Advanced Features (Weeks 25–32)

### Week 25: Macros & Metaprogramming
- **Requirement:** Implement hygienic macros (declarative, not procedural).
- **Uses this concept:** Macro expansion, hygiene.
- **Learn this:** Token streams, AST rewriting.
- **Problem:** Write macro `vec![1,2,3]`.
- **Sources:**  
  - [Rust Macros by Example](https://doc.rust-lang.org/reference/macros-by-example.html)  
  - [Scheme Hygienic Macros](https://www.scheme.com/macros/)  
- **Teacher’s note:** Macros are sharp tools. Give users scalpels, not chainsaws.

### Week 26: Pattern Matching
- **Requirement:** Add `match` expressions with exhaustiveness checking.
- **Uses this concept:** Algebraic data types, decision trees.
- **Learn this:** Exhaustiveness analysis, or-patterns.
- **Problem:** Match on `Result<T,E>` with all arms covered.
- **Sources:**  
  - [Rust Pattern Matching](https://doc.rust-lang.org/book/ch18-00-patterns.html)  
  - [ML Pattern Matching](https://smlfamily.github.io/sml97-defn.pdf)  
- **Teacher’s note:** Exhaustive matching prevents bugs hiding in unhandled corners.

### Week 27: Iterators
- **Requirement:** Build iterator trait and desugaring for `for` loops.
- **Uses this concept:** Lazy evaluation, trait-based iteration.
- **Learn this:** Implementing traits for arrays and slices.
- **Problem:** Implement `.map()` on iterators.
- **Sources:**  
  - [Rust Iterators](https://doc.rust-lang.org/book/ch13-02-iterators.html)  
  - [Go Range Loops](https://go.dev/ref/spec#For_statements)  
- **Teacher’s note:** Iterators are composable glue — simple, powerful, everywhere.

### Week 28: Modules & Imports
- **Requirement:** Implement module system with visibility (`pub`).
- **Uses this concept:** Namespaces, encapsulation.
- **Learn this:** Lexical scoping for modules, cyclic dependency handling.
- **Problem:** Import submodule and call a function with `use`.
- **Sources:**  
  - [Rust Modules](https://doc.rust-lang.org/book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html)  
  - [Go Modules](https://go.dev/ref/mod)  
- **Teacher’s note:** Modules keep complexity sane. Without them, projects collapse.

### Week 29: Strings & UTF-8
- **Requirement:** Finalize string model: always-valid UTF-8, with safe slicing.
- **Uses this concept:** Encoding invariants, rope-like structures.
- **Learn this:** Grapheme clusters vs codepoints vs bytes.
- **Problem:** Forbid slicing in middle of multibyte UTF-8 char.
- **Sources:**  
  - [UTF-8 Specification (RFC 3629)](https://www.rfc-editor.org/rfc/rfc3629)  
  - [Unicode Text Segmentation (UAX #29)](https://unicode.org/reports/tr29/)  
- **Teacher’s note:** Strings are political. UTF-8 safety is non-negotiable.

### Week 30: Reflection & Introspection
- **Requirement:** Add compile-time reflection (type names, sizes).
- **Uses this concept:** Compile-time metaprogramming.
- **Learn this:** AST metadata, type descriptors.
- **Problem:** Generate struct field names at compile-time.
- **Sources:**  
  - [Rust TypeId & Any](https://doc.rust-lang.org/std/any/)  
  - [D Language Traits](https://dlang.org/spec/traits.html)  
- **Teacher’s note:** Reflection is for tools, not runtime bloat. Keep it compile-only.

### Week 31: Inline Assembly
- **Requirement:** Support inline assembly in `uinl {}` with constraints.
- **Uses this concept:** Low-level escape hatches, register constraints.
- **Learn this:** `asm!` syntax, clobbers, volatile.
- **Problem:** Write `uinl { asm("nop"); }`.
- **Sources:**  
  - [Rust Inline Assembly](https://doc.rust-lang.org/reference/inline-assembly.html)  
  - [LLVM Inline Assembly](https://llvm.org/docs/LangRef.html#inline-assembler-expressions)  
- **Teacher’s note:** Assembly is the dragon’s cave. Only experts enter.

### Week 32: Foreign Modules & Linking
- **Requirement:** Import external shared libs (`.so`, `.dll`) safely.
- **Uses this concept:** Dynamic linking, symbol resolution.
- **Learn this:** dlopen, dlsym, Windows `LoadLibrary`.
- **Problem:** Load math library dynamically and call `sin`.
- **Sources:**  
  - [POSIX dlopen](https://man7.org/linux/man-pages/man3/dlopen.3.html)  
  - [Windows LoadLibrary](https://learn.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-loadlibrarya)  
- **Teacher’s note:** Foreign libs are sharp edges. Wrap them in velvet gloves.

## Phase 5: Polishing the Ecosystem (Weeks 33–40)

### Week 33: Documentation System
- **Requirement:** Build `lang doc` to generate HTML docs from code comments.
- **Uses this concept:** Documentation generators, AST walking.
- **Learn this:** Extracting structured comments, rendering docs.
- **Problem:** Generate docs with code snippets from annotated source.
- **Sources:**  
  - [Rustdoc Guide](https://doc.rust-lang.org/rustdoc/)  
  - [Javadoc Basics](https://www.oracle.com/technical-resources/articles/java/javadoc-tool.html)  
- **Teacher’s note:** A language without docs is a tree falling in an empty forest.

### Week 34: LSP (Language Server Protocol)
- **Requirement:** Implement LSP server for IDE integration.
- **Uses this concept:** JSON-RPC, editor tooling.
- **Learn this:** AST queries, incremental parsing.
- **Problem:** Provide hover + autocomplete for variables.
- **Sources:**  
  - [LSP Specification](https://microsoft.github.io/language-server-protocol/specifications/specification-current/)  
  - [Rust Analyzer Architecture](https://github.com/rust-lang/rust-analyzer)  
- **Teacher’s note:** IDE support is oxygen. Developers suffocate without it.

### Week 35: Formatter & Linter
- **Requirement:** Create `lang fmt` and `lang lint` tools.
- **Uses this concept:** Pretty-printing, static analysis.
- **Learn this:** AST formatting, lint rule design.
- **Problem:** Enforce spacing around operators automatically.
- **Sources:**  
  - [Rustfmt Guide](https://github.com/rust-lang/rustfmt)  
  - [Clippy Lints](https://doc.rust-lang.org/clippy/)  
- **Teacher’s note:** Opinionated tools prevent holy wars. Decide, then enforce.

### Week 36: Property Testing & Fuzzing
- **Requirement:** Add property-based testing + fuzz harness.
- **Uses this concept:** Randomized testing, shrinking.
- **Learn this:** Hypothesis-style testing, AFL fuzzing.
- **Problem:** Write property: reversing a list twice = original.
- **Sources:**  
  - [QuickCheck Paper](https://begriffs.com/posts/2017-01-14-design-use-quickcheck.html)  
  - [AFL Fuzzing](https://lcamtuf.coredump.cx/afl/)  
- **Teacher’s note:** Randomness finds bugs your brain cannot imagine.

### Week 37: Standard Library I
- **Requirement:** Core collections (Vec, Map, String).
- **Uses this concept:** Data structures, zero-cost abstractions.
- **Learn this:** Safe memory growth strategies, hashing.
- **Problem:** Implement `Vec::push` safely with reallocation.
- **Sources:**  
  - [Rust Vec Source](https://doc.rust-lang.org/src/alloc/vec/mod.rs.html)  
  - [Okasaki’s Purely Functional DS](https://www.cs.cmu.edu/~rwh/theses/okasaki.pdf)  
- **Teacher’s note:** A stdlib is a language’s vocabulary. Without it, you grunt.

### Week 38: Standard Library II
- **Requirement:** File I/O, networking, concurrency primitives.
- **Uses this concept:** Syscalls, safe wrappers.
- **Learn this:** RAII for resources, error handling in I/O.
- **Problem:** Implement `File::read_to_string`.
- **Sources:**  
  - [POSIX I/O](https://pubs.opengroup.org/onlinepubs/9699919799/functions/open.html)  
  - [Rust std::fs](https://doc.rust-lang.org/std/fs/)  
- **Teacher’s note:** Stdlib must hug the OS without exposing splinters.

### Week 39: Concurrency Library
- **Requirement:** Channels, thread pools, async foundations.
- **Uses this concept:** CSP, structured concurrency.
- **Learn this:** Futures, cooperative scheduling.
- **Problem:** Spawn thread pool, distribute jobs safely.
- **Sources:**  
  - [Go Concurrency Patterns](https://go.dev/blog/pipelines)  
  - [Structured Concurrency](https://ericlippert.com/2019/04/12/structured-concurrency-in-csharp/)  
- **Teacher’s note:** Concurrency should be safe by default, powerful by choice.

### Week 40: Error Library
- **Requirement:** Extend `Result`/`Option` ergonomics, error chaining.
- **Uses this concept:** Composable error handling.
- **Learn this:** Error propagation, backtraces.
- **Problem:** Implement `.map_err()` and backtrace capture.
- **Sources:**  
  - [Rust Error Handling Patterns](https://nrc.github.io/error-docs/)  
  - [Backtrace in Rust](https://doc.rust-lang.org/std/backtrace/)  
- **Teacher’s note:** Error libraries prevent developers from building Franken-errors.

---

## Phase 6: Stabilization & Launch (Weeks 41–48)

### Week 41: Security Audit
- **Requirement:** Audit compiler for memory safety, UB, injection attacks.
- **Uses this concept:** Security reviews, fuzzing integration.
- **Learn this:** Threat modeling, sanitizer tools.
- **Problem:** Run address sanitizer on compiler itself.
- **Sources:**  
  - [AddressSanitizer](https://clang.llvm.org/docs/AddressSanitizer.html)  
  - [LLVM Sanitizers](https://github.com/google/sanitizers)  
- **Teacher’s note:** The compiler must be a fortress. No cracks allowed.

### Week 42: Cross-Compilation
- **Requirement:** Enable building for Linux, Windows, macOS.
- **Uses this concept:** Cross toolchains, target triples.
- **Learn this:** LLVM target configs.
- **Problem:** Cross-compile hello world from Linux to Windows.
- **Sources:**  
  - [Rust Cross Compilation](https://rust-lang.github.io/rustup/cross-compilation.html)  
  - [LLVM Targets](https://llvm.org/docs/CodeGenerator.html#target-specific-information)  
- **Teacher’s note:** A systems language must go everywhere the system goes.

### Week 43: Performance Benchmarking
- **Requirement:** Compare against C and Rust on micro/macro benchmarks.
- **Uses this concept:** Benchmarking methodology.
- **Learn this:** Cache effects, perf counters.
- **Problem:** Benchmark matrix multiply vs C equivalent.
- **Sources:**  
  - [Rust Criterion Benchmarking](https://bheisler.github.io/criterion.rs/book/index.html)  
  - [Linux perf](https://perf.wiki.kernel.org/index.php/Main_Page)  
- **Teacher’s note:** If you’re slower than C, you’ve failed. Prove it.

### Week 44: Ecosystem Seeding
- **Requirement:** Publish stdlib, fmt, test, common crates.
- **Uses this concept:** Package ecosystem bootstrapping.
- **Learn this:** How to maintain first-party crates.
- **Problem:** Write and publish `lang-log` crate.
- **Sources:**  
  - [Cargo Publishing](https://doc.rust-lang.org/cargo/reference/publishing.html)  
  - [npm Ecosystem Lessons](https://blog.npmjs.org/post/141577284765/kik-left-pad-and-npm.html)  
- **Teacher’s note:** Ecosystem is social gravity. Seed it or drift away.

### Week 45: Community & Governance
- **Requirement:** Define RFC process, governance model.
- **Uses this concept:** Open source community design.
- **Learn this:** BDFL vs committee, contribution workflows.
- **Problem:** Draft an RFC template for new language features.
- **Sources:**  
  - [Rust RFC Process](https://rust-lang.github.io/rfcs/)  
  - [Python PEP Process](https://peps.python.org/pep-0001/)  
- **Teacher’s note:** Language design is politics. Set up good laws early.

### Week 46: Final Optimization Pass
- **Requirement:** Add loop unrolling, vectorization.
- **Uses this concept:** Advanced compiler optimizations.
- **Learn this:** Autovectorization, SIMD.
- **Problem:** Vectorize array sum loop.
- **Sources:**  
  - [LLVM Loop Vectorizer](https://llvm.org/docs/Vectorizers.html)  
  - [SIMD in Rust](https://doc.rust-lang.org/std/simd/)  
- **Teacher’s note:** Squeeze every cycle. Performance is your religion.

### Week 47: Release Candidate
- **Requirement:** Tag v1.0 RC, freeze features, fix critical bugs.
- **Uses this concept:** Release engineering.
- **Learn this:** Semver, release notes.
- **Problem:** Cut release tarball, distribute binaries.
- **Sources:**  
  - [SemVer](https://semver.org/)  
  - [Release Engineering Handbook](https://devrel.net/developer-experience/release-engineering)  
- **Teacher’s note:** Freeze means freeze. No shiny new features.

### Week 48: Launch
- **Requirement:** Publish v1.0: compiler, stdlib, docs, tools, package registry.
- **Uses this concept:** Language release, ecosystem launch.
- **Learn this:** Developer outreach, onboarding.
- **Problem:** Build and run a multi-crate project end-to-end.
- **Sources:**  
  - [Go 1.0 Announcement](https://go.dev/blog/hello-go)  
  - [Rust 1.0 Announcement](https://blog.rust-lang.org/2015/05/15/Rust-1.0.html)  
- **Teacher’s note:** Congratulations. You’ve birthed a systems language. Respect.

---

# Appendix: Core Language Principles

### Non-Negotiables
- **Performance:** Equal to C, zero runtime overhead.  
- **Safety:** Equal to Rust, no compromises.  
- **No Rust clone:** Memory model is unique, region-based + opt-in ownership.  

### Custom Memory Model
- Region-based memory allocation with deterministic scope cleanup.  
- Optional ownership/borrowing system for fine-grained safety.  
- No implicit GC.  
- Unsafe code (`uinl {}`) isolated and documented with invariants.  

### Array & Bound Rules
- Compile-time bound checks by default.  
- Runtime fallback with zero-cost exceptions if unavoidable.  
- `--force-safe`: requires user-written validation (`if (i < arr.len)`), verified by compiler.  
- Indexing safe by proof, otherwise `Option` return or trap.  

### Strings
- Always valid UTF-8, no exceptions.  
- Byte arrays available for raw data.  

### Build & Packaging
- Built-in `lang build`, `lang test`, `lang doc`.  
- Manifest file (`lang.toml`).  
- Central registry for package sharing.  

### Concurrency
- Safe threads + channels (Go-style CSP).  
- Auto traits (`Send`, `Sync`) enforced by type system.  

### Error Handling
- Typed `Result<T,E>` as default.  
- Exceptions only for safety violations (OOB, UB).  
- Zero-cost when not triggered.  

---

# Final Teacher’s Note
You now hold a **complete, year-long master plan** to build a new systems language:  
- As **safe as Rust**, as **fast as C**, but **simpler**.  
- Safety enforced at compile time, with runtime checks only when provably unavoidable.  
- Unsafe is quarantined (`uinl {}`), documented, and auditable.  
- Developer experience is central: docs, package manager, LSP, property testing, and fuzzing are all first-class.  

By Week 48, you’ll have shipped **v1.0: compiler, stdlib, build tools, package manager, docs, ecosystem seed**.  
This is not just a roadmap — it’s a launchpad for a living language that could rival Rust, Go, and C.  

---
