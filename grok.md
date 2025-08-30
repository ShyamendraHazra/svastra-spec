# Roadmap for Designing and Implementing a C-like Systems Language

## Week 1
- **Requirement:** Study foundational concepts of programming languages and compilers; set up development environment for prototyping.
- **Uses this concept:** Programming language theory basics, compiler pipeline overview.
- **Learn this:** Differences between interpreters and compilers; lexical analysis, syntax, semantics.
- **Problem:** Write a simple tokenizer for a toy language subset (e.g., arithmetic expressions) in C or Rust.
- **Sources:** [Crafting a Compiler by Fischer and LeBlanc (chapters 1-2)](https://www.pearson.com/store/p/crafting-a-compiler/P100001737986/9780136067054); [LLVM Tutorial (introduction section)](https://llvm.org/docs/tutorial/); [Let's Build a Compiler by Jack Crenshaw (parts 1-3)](https://compilers.iecc.com/crenshaw/); [Rust book (compiler basics analogy)](https://doc.rust-lang.org/book/).
- **Teacher’s note:** Start broad to understand the end-to-end process; this builds the mental model for integrating safety and performance from day one.

## Week 2
- **Requirement:** Implement a basic lexer for the language's syntax subset (keywords, identifiers, literals).
- **Uses this concept:** Finite state machines for tokenization.
- **Learn this:** Regular expressions for lexing; error reporting in lexers.
- **Problem:** Handle edge cases like invalid characters or unterminated strings in your lexer.
- **Sources:** [Compilers: Principles, Techniques, and Tools (Dragon Book) by Aho et al. (chapter 3)](https://dragonbook.stanford.edu/); [Writing a Simple Lexer in Rust on dev.to](https://dev.to/flyx/writing-a-simple-lexer-in-rust-4gcl); [Lexer Tutorial by Ruslan's Blog](https://ruslanspivak.com/lsbasi-part1/).
- **Teacher’s note:** Focus on efficiency; lexing must be O(n) to maintain zero overhead in the compiler itself.

## Week 3
- **Requirement:** Design initial syntax for the language (C-like with safe arrays and strings).
- **Uses this concept:** Context-free grammars (CFGs).
- **Learn this:** BNF/EBNF notation; designing grammars for expressions and statements.
- **Problem:** Write a grammar that includes safe array declarations like `int[10] arr;`.
- **Sources:** [Programming Language Pragmatics by Scott (chapter 2)](https://www.elsevier.com/books/programming-language-pragmatics/scott/978-0-12-410409-9); [Introduction to Grammars on ANTLR website](https://www.antlr.org/); [Designing a Programming Language Syntax on Medium](https://medium.com/@mortendahl/designing-a-programming-language-syntax-1b0b7ff8a6e8).
- **Teacher’s note:** Ensure syntax supports compile-time bounds; this ties into safety without runtime cost.

## Week 4
- **Requirement:** Implement a recursive descent parser for expressions and simple statements.
- **Uses this concept:** Top-down parsing.
- **Learn this:** Handling precedence and associativity in parsers.
- **Problem:** Parse nested expressions with bounds-checked array access syntax.
- **Sources:** [Crafting a Compiler (chapter 4)](https://www.pearson.com/store/p/crafting-a-compiler/P100001737986/9780136067054); [Recursive Descent Parser in C on GeeksforGeeks](https://www.geeksforgeeks.org/recursive-descent-parser/); [Jack Crenshaw's series (parts 4-6)](https://compilers.iecc.com/crenshaw/).
- **Teacher’s note:** Keep it simple; this parser will evolve to enforce safety rules statically.

## Week 5
- **Requirement:** Build an Abstract Syntax Tree (AST) from parsed input.
- **Uses this concept:** Tree data structures for semantic representation.
- **Learn this:** AST nodes for variables, arrays, and basic types.
- **Problem:** Represent range-restricted integers like `int[0..10]` in the AST.
- **Sources:** [Dragon Book (chapter 4)](https://dragonbook.stanford.edu/); [Building an AST in Rust on YouTube](https://www.youtube.com/watch?v=ZTO4uNiwgVA); [ASTs Explained by Vaidehi Joshi](https://vaidehijoshi.github.io/basecs-series/).
- **Teacher’s note:** AST is key for type checking; include metadata for bounds to enable compile-time safety.

## Week 6
- **Requirement:** Add semantic analysis pass to check variable declarations.
- **Uses this concept:** Symbol tables.
- **Learn this:** Scope management and name resolution.
- **Problem:** Detect redeclarations and undeclared variables in a simple program.
- **Sources:** [Programming Language Pragmatics (chapter 3)](https://www.elsevier.com/books/programming-language-pragmatics/scott/978-0-12-410409-9); [Symbol Tables in Compilers on Compiler Design playlist (YouTube)](https://www.youtube.com/watch?v=6XXpSynIrEc); [Implementing a Symbol Table on Eli Bendersky's site](https://eli.thegreenplace.net/2007/11/24/the-context-sensitivity-of-cs-grammar).
- **Teacher’s note:** This lays groundwork for tracking memory safety attributes.

## Week 7
- **Requirement:** Integrate type checking for basic types and arrays.
- **Uses this concept:** Type inference basics.
- **Learn this:** Static type systems; compatibility rules.
- **Problem:** Enforce type safety for array assignments with bounds.
- **Sources:** [Dragon Book (chapter 6)](https://dragonbook.stanford.edu/); [Type Checking in Compilers on Medium](https://medium.com/@aditya.shahi/type-checking-in-compiler-design-92e7f6a1c3e5); [Types and Programming Languages by Pierce (chapter 1, skim)](https://www.cis.upenn.edu/~bcpierce/tapl/).
- **Teacher’s note:** Ensure types include bound information for zero-cost safety enforcement.

## Week 8
- **Requirement:** Design the custom memory model: manual alloc/free with safe pointers and optional light ownership.
- **Uses this concept:** Region-based memory management inspired by Cyclone, with borrow checks optional.
- **Learn this:** Alternatives to Rust's ownership: affine types, regions for safety without full borrowing.
- **Problem:** Sketch rules for safe pointers that prevent dangling references statically.
- **Sources:** [Cyclone: A Safe Dialect of C paper (USENIX 2002)](https://www.cs.tufts.edu/~nr/cyclone/papers/cyclone-usenix02.pdf); [Memory Management Without Garbage Collection on dev.to](https://dev.to/hamatti/memory-management-without-garbage-collection-2b7); [Region-Based Memory Management on Wikipedia and related articles](https://en.wikipedia.org/wiki/Region-based_memory_management).
- **Teacher’s note:** Custom model uses static regions + optional borrows; ensures Rust-level safety, zero overhead, differs from Rust by making ownership opt-in.

## Week 9
- **Requirement:** Implement compile-time bounds checking in type checker.
- **Uses this concept:** Dependent types light (for bounds).
- **Learn this:** Static analysis for index verification.
- **Problem:** Reject code with out-of-bounds literals at compile time.
- **Sources:** [Types and Programming Languages (chapter 9)](https://www.cis.upenn.edu/~bcpierce/tapl/); [Bounds Checking in Compilers on Stack Overflow discussions](https://stackoverflow.com/questions/114254/how-does-a-compiler-implement-array-bounds-checking); [Compile-Time Bounds Checking in Rust context (adapt)](https://blog.rust-lang.org/2016/03/30/The-saddest-bound-check-elimination-ever.html).
- **Teacher’s note:** Core to safety: prioritize static rejection over runtime.

## Week 10
- **Requirement:** Add runtime fallback for bounds with zero-cost exceptions.
- **Uses this concept:** Exception handling in code gen (later).
- **Learn this:** Zero-cost abstractions for checks (like Itanium ABI).
- **Problem:** Design AST annotations for runtime checks that optimize away when safe.
- **Sources:** [LLVM Exception Handling docs](https://llvm.org/docs/ExceptionHandling.html); [Zero-Cost Exceptions on C++ blogs](https://blog.mozilla.org/nnethercote/2011/01/18/the-dangers-of-zero-cost-exceptions/); [Modern C++ by Meyers (exception sections)](https://www.oreilly.com/library/view/modern-c/9781492049159/).
- **Teacher’s note:** Exceptions only for violations; ensure no overhead when not thrown.

## Week 11
- **Requirement:** Implement unsafe escape hatch (`uinl {}`) in parser and semantics.
- **Uses this concept:** Modal analysis (safe/unsafe modes).
- **Learn this:** Tainting unsafe code to bypass checks.
- **Problem:** Parse and flag unsafe blocks, disabling safety analysis inside.
- **Sources:** [Rust Unsafe docs (for inspiration, not copy)](https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html); [Unsafe Code in Safe Languages on Medium](https://medium.com/@mikekestner/unsafe-code-in-safe-languages-8c0e57a6b683); [Implementing Unsafe in a Compiler](https://blog.sigplan.org/2020/07/23/unsafe-in-safe-languages/).
- **Teacher’s note:** Unsafe is explicit; maintains safety elsewhere without overhead.

## Week 12
- **Requirement:** Design and implement --force-safe mode logic.
- **Uses this concept:** Static verification of user checks.
- **Learn this:** Flow-sensitive analysis to verify manual bounds validations.
- **Problem:** Reject code without `if (i < arr.len)` before dynamic access in force-safe.
- **Sources:** [Static Analysis by Nielson et al. (chapter 2)](https://www.springer.com/gp/book/9781447124139); [Dataflow Analysis in Compilers on YouTube](https://www.youtube.com/watch?v=0zA97ELHS0Y); [Verifying User Checks in Compilers](https://blog.reverberate.org/2013/08/llvm-internals-part-3-static-analysis.html).
- **Teacher’s note:** Shifts responsibility to user; compiler verifies statically, zero runtime cost.

## Week 13
- **Requirement:** Add UTF-8 string handling to type system.
- **Uses this concept:** Encoding validation at compile/runtime.
- **Learn this:** String internals; safe slicing.
- **Problem:** Enforce valid UTF-8 in string literals and operations.
- **Sources:** [UTF-8 Everywhere manifesto](https://utf8everywhere.org/); [Handling UTF-8 in C on blogs](https://nullprogram.com/blog/2017/10/06/); [Rust String docs (adapt)](https://doc.rust-lang.org/std/string/struct.String.html).
- **Teacher’s note:** Safety includes encoding; check statically where possible.

## Week 14
- **Requirement:** Implement monomorphized generics in frontend.
- **Uses this concept:** Template instantiation.
- **Learn this:** Generic type substitution without bloat.
- **Problem:** Parse and expand simple generic functions like Go.
- **Sources:** [Generics in Go docs](https://go.dev/blog/intro-generics); [Implementing Generics in a Compiler on dev.to](https://dev.to/mintak/concept-of-implementing-generics-in-my-toy-compiler-2b1); [Dragon Book (chapter 7)](https://dragonbook.stanford.edu/).
- **Teacher’s note:** Keep it simple; monomorphize only used instances for performance.

## Week 15
- **Requirement:** Add compile-time traits/interfaces.
- **Uses this concept:** Structural subtyping.
- **Learn this:** Polymorphism without vtables (unless requested).
- **Problem:** Type check trait implementations statically.
- **Sources:** [Traits in Rust (adapt lightly)](https://doc.rust-lang.org/book/ch10-02-traits.html); [Interfaces in Compilers on Medium](https://medium.com/@mikekestner/interfaces-and-type-systems-5a5f5e5f4a4f); [Programming Language Pragmatics (chapter 9)](https://www.elsevier.com/books/programming-language-pragmatics/scott/978-0-12-410409-9).
- **Teacher’s note:** Zero-cost by default; runtime only if explicit.

## Week 16
- **Requirement:** Design error handling: Result<T,E> and exceptions.
- **Uses this concept:** Algebraic data types.
- **Learn this:** Sum types for Results; exception integration.
- **Problem:** Parse and type check Result usage.
- **Sources:** [Haskell error handling tutorials](https://www.haskell.org/tutorial/error.html); [Error Handling in Languages on YouTube](https://www.youtube.com/watch?v=0zA97ELHS0Y); [Rust Result docs](https://doc.rust-lang.org/std/result/).
- **Teacher’s note:** Exceptions zero-cost; use for safety violations only.

## Week 17
- **Requirement:** Introduce concurrency primitives: threads, channels.
- **Uses this concept:** Message passing concurrency.
- **Learn this:** Safe shared state avoidance.
- **Problem:** Syntax for spawning threads and sending messages.
- **Sources:** [Go Concurrency docs](https://go.dev/doc/effective_go#concurrency); [Implementing Channels in a Language on blogs](https://blog.cloudflare.com/channel-your-go/); [Concurrent Programming by Ben-Ari (basics)](https://www.springer.com/gp/book/9781848821187).
- **Teacher’s note:** Default safe; integrate with memory model for no races.

## Week 18
- **Requirement:** Implement FFI for C linking.
- **Uses this concept:** ABI compatibility.
- **Learn this:** Calling conventions; extern declarations.
- **Problem:** Parse extern functions and generate bindings.
- **Sources:** [LLVM FFI tutorials](https://mapping-high-level-constructs-to-llvm-ir.readthedocs.io/en/latest/basic-constructs/ffi.html); [C FFI in Rust (adapt)](https://doc.rust-lang.org/nomicon/ffi.html); [Foreign Function Interfaces blog](https://www.chiark.greenend.org.uk/~sgtatham/ffiblog.html).
- **Teacher’s note:** Seamless with C for performance.

## Week 19
- **Requirement:** Set up Intermediate Representation (IR) layer.
- **Uses this concept:** SSA form.
- **Learn this:** Lowering AST to IR.
- **Problem:** Convert simple expressions to IR.
- **Sources:** [Engineering a Compiler by Cooper and Torczon (chapter 8)](https://www.elsevier.com/books/engineering-a-compiler/cooper/978-0-12-088478-0); [LLVM IR Tutorial](https://llvm.org/docs/LangRef.html); [Introduction to SSA](https://blog.yossarian.net/2018/05/20/Implementing-SSA-in-LLVM).
- **Teacher’s note:** IR enables optimizations; include safety metadata.

## Week 20
- **Requirement:** Add optimizations for bounds checks (elide when provable).
- **Uses this concept:** Dead code elimination, constant folding.
- **Learn this:** Pass manager for optimizations.
- **Problem:** Optimize away static-safe runtime checks.
- **Sources:** [LLVM Optimization passes docs](https://llvm.org/docs/Passes.html); [Compiler Optimizations on YouTube](https://www.youtube.com/watch?v=FNgetbXzVMY); [Dragon Book (chapter 9)](https://dragonbook.stanford.edu/).
- **Teacher’s note:** Critical for zero overhead; safety doesn't cost at runtime.

## Week 21
- **Requirement:** Integrate optional ownership system in type checker.
- **Uses this concept:** Lightweight borrowing (opt-in).
- **Learn this:** Borrow checker basics, simpler than Rust.
- **Problem:** Enforce no aliasing in owned regions statically.
- **Sources:** [Borrow Checking Without Lifetimes papers](https://www.microsoft.com/en-us/research/uploads/prod/2018/08/submitted.pdf); [Simple Ownership in Languages on dev.to](https://dev.to/mintak/simple-ownership-in-my-toy-compiler-11a6); [MLKit regions](https://elsman.com/mlkit/).
- **Teacher’s note:** Optional for flexibility; ensures safety when used.

## Week 22
- **Requirement:** Generate code to LLVM backend.
- **Uses this concept:** Code generation patterns.
- **Learn this:** Mapping IR to LLVM instructions.
- **Problem:** Emit LLVM for simple functions with arrays.
- **Sources:** [Official LLVM Tutorial (Kaleidoscope series)](https://llvm.org/docs/tutorial/); [Writing a Compiler Backend by Chris Lattner blog](https://llvm.org/pubs/2008-10-04-ACAT-LLVM-Intro.html).
- **Teacher’s note:** LLVM ensures C-level performance; target it for zero overhead.

## Week 23
- **Requirement:** Implement runtime checks as LLVM exceptions.
- **Uses this concept:** Landing pads for exceptions.
- **Learn this:** Zero-cost exception model in LLVM.
- **Problem:** Generate code that throws on OOB only if triggered.
- **Sources:** [LLVM Exception Handling reference](https://llvm.org/docs/ExceptionHandling.html); [Exceptions in LLVM on blogs](https://blog.llvm.org/posts/2021-02-23-exception-handling-in-llvm/).
- **Teacher’s note:** Maintains safety with no normal-path cost.

## Week 24
- **Requirement:** Add support for --force-safe in compiler flags.
- **Uses this concept:** Command-line parsing and mode switching.
- **Learn this:** Flag-dependent analysis passes.
- **Problem:** Verify user manual checks in dataflow.
- **Sources:** [Clang flag handling docs](https://clang.llvm.org/docs/UsersManual.html); [Compiler Flags Implementation](https://www.aosabook.org/en/llvm.html).
- **Teacher’s note:** Enforces developer responsibility for safety.

## Week 25
- **Requirement:** Bootstrap basic build system (`lang build`).
- **Uses this concept:** Dependency resolution.
- **Learn this:** Simple makefile-like logic in code.
- **Problem:** Compile a single-file program to executable.
- **Sources:** [Build Systems à la Carte paper](https://www.microsoft.com/en-us/research/uploads/prod/2018/03/build-systems.pdf); [Writing a Build System on Medium](https://medium.com/@bartoszgorka/simple-build-system-in-python-3b845d0d4d4e); [Cargo internals (adapt)](https://doc.rust-lang.org/cargo/reference/).
- **Teacher’s note:** Integrated for ease; like Cargo but simpler.

## Week 26
- **Requirement:** Design package manager with lang.toml.
- **Uses this concept:** Manifest parsing, version solving.
- **Learn this:** TOML format; dependency graphs.
- **Problem:** Parse a sample manifest and resolve deps.
- **Sources:** [TOML spec](https://toml.io/en/); [Implementing a Package Manager on dev.to](https://dev.to/tgross35/building-a-package-manager-what-i-learned-from-cargo-and-yarn-1e4e); [Cargo book](https://doc.rust-lang.org/cargo/).
- **Teacher’s note:** Built-in for ecosystem; no external tools.

## Week 27
- **Requirement:** Implement test runner (`lang test`).
- **Uses this concept:** Attribute-based testing.
- **Learn this:** Running subsets of code as tests.
- **Problem:** Execute and report on test functions.
- **Sources:** [Rust testing docs (adapt)](https://doc.rust-lang.org/book/ch11-00-testing.html); [Building a Test Framework](https://os.phil-opp.com/testing/).
- **Teacher’s note:** Essential for verifying safety features.

## Week 28
- **Requirement:** Extend concurrency to safe channels in code gen.
- **Uses this concept:** Lock-free queues in IR.
- **Learn this:** Generating thread-safe code.
- **Problem:** Emit LLVM for channel send/receive.
- **Sources:** [Go runtime channels (source code study)](https://github.com/golang/go/blob/master/src/runtime/chan.go); [Channels in Compilers tutorial](https://eli.thegreenplace.net/2018/go-channels-under-the-hood/).
- **Teacher’s note:** Message passing for safety; zero unnecessary locks.

## Week 29
- **Requirement:** Add generics to code gen (monomorphization).
- **Uses this concept:** Instantiation on demand.
- **Learn this:** Duplicating code for types.
- **Problem:** Generate specialized LLVM for generics.
- **Sources:** [C++ templates (how-to avoid bloat)](https://isocpp.org/blog/2012/11/universal-references-in-c11-scott-meyers); [LLVM generics tutorials](https://mapping-high-level-constructs-to-llvm-ir.readthedocs.io/en/latest/basic-constructs/templates.html).
- **Teacher’s note:** No bloat: only instantiate used ones.

## Week 30
- **Requirement:** Implement traits in backend.
- **Uses this concept:** Dictionary passing (if runtime).
- **Learn this:** Static dispatch by default.
- **Problem:** Generate monomorphic code for traits.
- **Sources:** [Haskell typeclasses (analogy)](https://www.haskell.org/tutorial/classes.html); [Compile-Time Polymorphism tutorial](https://www.geeksforgeeks.org/compile-time-polymorphism-in-java/).
- **Teacher’s note:** Zero-cost default; aligns with performance goal.

## Week 31
- **Requirement:** Full error handling in code gen.
- **Uses this concept:** Unwinding for exceptions.
- **Learn this:** Result propagation.
- **Problem:** Emit code for Result unwrapping.
- **Sources:** [Swift error handling (study)](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/errorhandling/); [LLVM unwinding](https://llvm.org/docs/ExceptionHandling.html#unwind).
- **Teacher’s note:** Typed errors for safety; exceptions rare.

## Week 32
- **Requirement:** Integrate FFI in backend.
- **Uses this concept:** Linker integration.
- **Learn this:** Calling C from generated code.
- **Problem:** Link against libc in output.
- **Sources:** [Clang FFI examples](https://clang.llvm.org/docs/Modules.html); [FFI in LLVM tutorial](https://llvmtutorial.readthedocs.io/en/latest/chapter4.html).
- **Teacher’s note:** First-class for systems programming.

## Week 33
- **Requirement:** Optimize memory model in IR passes.
- **Uses this concept:** Escape analysis for regions.
- **Learn this:** Eliding unnecessary checks.
- **Problem:** Prove safety statically to remove overhead.
- **Sources:** [Region Inference papers](https://www.microsoft.com/en-us/research/publication/region-inference-for-an-object-oriented-language/); [LLVM optimizations](https://llvm.org/docs/Passes.html).
- **Teacher’s note:** Custom model: regions + opt-in ownership for Rust safety without full borrow checker.

## Week 34
- **Requirement:** Test bounds checking end-to-end.
- **Uses this concept:** Integration testing.
- **Learn this:** Writing safety violation tests.
- **Problem:** Compile and run programs with OOB exceptions.
- **Sources:** [Compiler testing strategies blogs](https://blog.regehr.org/archives/161); [Rust safety tests](https://doc.rust-lang.org/rustc/tests/index.html).
- **Teacher’s note:** Verify zero overhead in safe paths.

## Week 35
- **Requirement:** Add strings to full pipeline.
- **Uses this concept:** Immutable strings with views.
- **Learn this:** UTF-8 validation in code gen.
- **Problem:** Generate safe string operations.
- **Sources:** [Strings in Systems Languages tutorials](https://lord.io/text-strings/).
- **Teacher’s note:** Always valid; ties into safety.

## Week 36
- **Requirement:** Bootstrap compiler self-hosting basics.
- **Uses this concept:** Self-compilation.
- **Learn this:** Porting frontend to the language.
- **Problem:** Compile a simple subset with itself.
- **Sources:** [Bootstrapping Compilers on Wikipedia](https://en.wikipedia.org/wiki/Bootstrapping_(compilers)); [Tutorials on self-hosting](https://github.com/rui314/chibicc).
- **Teacher’s note:** Milestone for maturity.

## Week 37
- **Requirement:** Expand build system for multi-file projects.
- **Uses this concept:** Module system.
- **Learn this:** Import resolution.
- **Problem:** Handle dependencies in build.
- **Sources:** [Rust modules docs](https://doc.rust-lang.org/book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html).
- **Teacher’s note:** Scalable for real use.

## Week 38
- **Requirement:** Implement package fetching in manager.
- **Uses this concept:** Git integration or simple repo.
- **Learn this:** Version pinning.
- **Problem:** Download and build deps.
- **Sources:** [NPM internals (simple adapt)](https://docs.npmjs.com/cli/v10/using-npm/registry).
- **Teacher’s note:** Ecosystem growth.

## Week 39
- **Requirement:** Add concurrency safety checks.
- **Uses this concept:** Send/Sync traits light.
- **Learn this:** Static thread safety.
- **Problem:** Prevent unsafe sharing.
- **Sources:** [Go race detector (conceptual)](https://go.dev/blog/race-detector).
- **Teacher’s note:** Default safe.

## Week 40
- **Requirement:** Optimize generics and traits further.
- **Uses this concept:** Inlining across instances.
- **Learn this:** Reducing code size.
- **Problem:** Measure and minimize bloat.
- **Sources:** [C++ optimization blogs](https://isocpp.org/blog/tag/optimization).
- **Teacher’s note:** Performance focus.

## Week 41
- **Requirement:** Full FFI testing with C libs.
- **Uses this concept:** Interop safety.
- **Learn this:** Wrapping unsafe C calls.
- **Problem:** Link and call printf safely.
- **Sources:** [Zig FFI examples](https://ziglang.org/documentation/master/#C).
- **Teacher’s note:** Systems integration.

## Week 42
- **Requirement:** Implement range-restricted ints fully.
- **Uses this concept:** Value-dependent types.
- **Learn this:** Propagation of ranges.
- **Problem:** Type check operations within bounds.
- **Sources:** [Ada subtypes (study)](https://learn.adacore.com/courses/intro-to-ada/chapters/03_types.html#subtypes).
- **Teacher’s note:** Enhances safety statically.

## Week 43
- **Requirement:** Add diagnostics for safety violations.
- **Uses this concept:** Error messages with suggestions.
- **Learn this:** Helpful compiler errors.
- **Problem:** Suggest fixes for bounds issues.
- **Sources:** [Clang diagnostics](https://clang.llvm.org/docs/DiagnosticsReference.html).
- **Teacher’s note:** User-friendly safety.

## Week 44
- **Requirement:** Benchmark against C for performance.
- **Uses this concept:** Microbenchmarks.
- **Learn this:** Ensuring zero overhead.
- **Problem:** Write and run speed tests.
- **Sources:** [Benchmarking tutorials](https://github.com/google/benchmark).
- **Teacher’s note:** Validate non-negotiable.

## Week 45
- **Requirement:** Test memory safety exhaustively.
- **Uses this concept:** Fuzzing, unit tests.
- **Learn this:** Proving no leaks or UB.
- **Problem:** Simulate violations.
- **Sources:** [Rust miri tool (concept)](https://github.com/rust-lang/miri).
- **Teacher’s note:** Match Rust safety.

## Week 46
- **Requirement:** Document language spec.
- **Uses this concept:** Formal specification.
- **Learn this:** Writing lang docs.
- **Problem:** Cover memory model.
- **Sources:** [Language spec examples (Go, Rust)](https://go.dev/ref/spec); [Rust](https://doc.rust-lang.org/reference/).
- **Teacher’s note:** For users.

## Week 47
- **Requirement:** Polish build/packaging tools.
- **Uses this concept:** CLI usability.
- **Learn this:** Error handling in tools.
- **Problem:** Handle build failures gracefully.
- **Sources:** [Cargo advanced](https://doc.rust-lang.org/cargo/reference/features.html).
- **Teacher’s note:** Ready for v1.0.

## Week 48
- **Requirement:** Release v1.0: full compiler, examples, tests.
- **Uses this concept:** Integration.
- **Learn this:** Packaging for distribution.
- **Problem:** Compile a non-trivial program safely.
- **Sources:** [Open-source release guides](https://opensource.guide/starting-a-project/).
- **Teacher’s note:** Culmination; ensures all goals met.

# Appendix: Key Language Design Principles

- **Custom Memory Model:** Manual memory management (malloc/free) wrapped in safe pointers/slices with static regions for allocation lifetimes. Optional lightweight ownership/borrowing (opt-in, simpler than Rust: no lifetimes, just move/borrow flags verified statically). Prevents use-after-free, null derefs, and data races via compile-time checks; unsafe blocks bypass. Differs from Rust by making ownership optional and using regions for default safety, ensuring zero overhead through static analysis.

- **Array Bound Rules (incl. `--force-safe`):** Compile-time checking by default for static indices. For runtime indices: user-specified bounds or compiler-injected zero-cost exception checks (optimized away when safe). In `--force-safe`: allows dynamic indices but requires user-written manual validation (e.g., `if (i < arr.len)`), verified by compiler static analysis; rejects absent checks. Unsafe `uinl {}` bypasses all.

- **Build/Package Tools:** Built-in commands: `lang build` for compilation, `lang test` for running tests, package manager using `lang.toml` manifests for dependencies, versioning, and building.

- **Non-negotiable Constraints:** Performance equals C (zero runtime overhead, not slower). Safety equals Rust (memory safety guaranteed, no weaker). No copying Rust’s ownership model; use custom model above. All features added must preserve these.
