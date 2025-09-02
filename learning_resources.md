# Verified Systems Language Development Resources

## Phase 1: Foundations (Weeks 1-8)

### Week 1: Language Design Principles & Specification

**Verified Primary Resources:**
1. **Crafting Interpreters** by Robert Nystrom - Complete online book covering language implementation from lexer to interpreter
   - URL: https://craftinginterpreters.com/
      - Introduction: https://craftinginterpreters.com/introduction.html
         - Full table of contents: https://craftinginterpreters.com/contents.html

         2. **Programming Language Pragmatics Companion Site**
            - Note: The specific companion site URL provided in the roadmap requires verification through academic channels

            3. **Go Language Specification**
               - URL: https://golang.org/ref/spec
                  - Provides clear example of language specification structure

                  **Supporting Documentation:**
                  4. **Rust RFCs Process**
                     - URL: https://rust-lang.github.io/rfcs/
                        - Demonstrates formal specification evolution process

                        ### Week 2: Compiler Architecture & Tooling Blueprint

                        **Verified Primary Resources:**
                        1. **LLVM Tutorial: My First Language Frontend** - Tutorial introduces the "Kaleidoscope" language, building it iteratively over several chapters
                           - Main tutorial: https://llvm.org/docs/tutorial/MyFirstLanguageFrontend/index.html
                              - Complete table of contents: https://llvm.org/docs/tutorial/

                              2. **The Cargo Book**
                                 - URL: https://doc.rust-lang.org/cargo/
                                    - Comprehensive build system architecture reference

                                    **Supporting Documentation:**
                                    3. **LLVM Language Reference Manual**
                                       - URL: https://llvm.org/docs/LangRef.html
                                          - Complete LLVM IR specification

                                          ### Week 3: Lexer Implementation

                                          **Verified Primary Resources:**
                                          1. **LLVM Kaleidoscope Lexer Tutorial** - Single function named gettok called to return the next token from standard input
                                             - URL: https://llvm.org/docs/tutorial/MyFirstLanguageFrontend/LangImpl01.html
                                                - Practical hand-written lexer implementation

                                                2. **Crafting Interpreters: Scanning Chapter**
                                                   - URL: https://craftinginterpreters.com/scanning.html
                                                      - Comprehensive tokenization fundamentals

                                                      **Supporting Resources:**
                                                      3. **UTF-8 Everywhere**
                                                         - URL: https://utf8everywhere.org/
                                                            - Critical Unicode handling principles

                                                            4. **GitHub Implementation Example: LLVM Kaleidoscope**
                                                               - URL: https://github.com/ghaiklor/llvm-kaleidoscope
                                                                  - Complete working implementation reference

                                                                  ### Week 4: Grammar & Parser

                                                                  **Verified Primary Resources:**
                                                                  1. **LLVM Kaleidoscope Parser Tutorial** - Shows parser and AST implementation with function typing
                                                                     - URL: https://llvm.org/docs/tutorial/MyFirstLanguageFrontend/LangImpl02.html
                                                                        - Recursive descent parser with practical examples

                                                                        2. **Crafting Interpreters: Parsing Expressions**
                                                                           - URL: https://craftinginterpreters.com/parsing-expressions.html
                                                                              - Clear explanation of precedence and associativity

                                                                              3. **Simple but Powerful Pratt Parsing** by matklad
                                                                                 - URL: https://matklad.github.io/2020/04/13/simple-but-powerful-pratt-parsing.html
                                                                                    - Modern approach to expression parsing with precedence

                                                                                    ### Week 5: AST & Semantic Analysis

                                                                                    **Verified Primary Resources:**
                                                                                    1. **Crafting Interpreters: Statements and State**
                                                                                       - URL: https://craftinginterpreters.com/statements-and-state.html
                                                                                          - Covers AST design and scope handling

                                                                                          2. **Crafting Interpreters: Variables**
                                                                                             - URL: https://craftinginterpreters.com/variables.html
                                                                                                - Symbol table and name resolution implementation

                                                                                                ### Week 6: Type System Basics

                                                                                                **Primary Resources:**
                                                                                                1. **Crafting Interpreters: Functions**
                                                                                                   - URL: https://craftinginterpreters.com/functions.html
                                                                                                      - Basic type checking and function call resolution

                                                                                                      **Note:** Advanced type system resources require access to academic texts like "Types and Programming Languages" by Benjamin Pierce, available through university libraries or publishers.

                                                                                                      ### Week 7: Intermediate Representation (IR)

                                                                                                      **Verified Primary Resources:**
                                                                                                      1. **LLVM Kaleidoscope Code Generation** - Shows how to transform Abstract Syntax Tree into LLVM IR
                                                                                                         - URL: https://llvm.org/docs/tutorial/MyFirstLanguageFrontend/LangImpl03.html
                                                                                                            - Practical IR generation examples

                                                                                                            2. **MIR Introduction Blog Post**
                                                                                                               - URL: https://blog.rust-lang.org/2016/04/19/MIR.html
                                                                                                                  - Official explanation of Mid-level Intermediate Representation

                                                                                                                  **Supporting Documentation:**
                                                                                                                  3. **LLVM Language Reference Manual**
                                                                                                                     - URL: https://llvm.org/docs/LangRef.html
                                                                                                                        - Complete IR specification

                                                                                                                        ### Week 8: Borrow Checker Foundations

                                                                                                                        **Verified Primary Resources:**
                                                                                                                        1. **Rust Ownership Tutorial**
                                                                                                                           - URL: https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html
                                                                                                                              - Foundation concepts for ownership systems

                                                                                                                              2. **Non-Lexical Lifetimes RFC**
                                                                                                                                 - URL: https://rust-lang.github.io/rfcs/2094-nll.html
                                                                                                                                    - Advanced lifetime analysis techniques

                                                                                                                                    **Supporting Documentation:**
                                                                                                                                    3. **The Rustonomicon: References and Borrowing**
                                                                                                                                       - URL: https://doc.rust-lang.org/nomicon/references.html
                                                                                                                                          - Advanced ownership system implementation details

                                                                                                                                          ## Phase 2: Safety Infrastructure (Weeks 9-16)

                                                                                                                                          ### Week 9: Bounds Checking & Force-Safe Mode

                                                                                                                                          **Primary Resources:**
                                                                                                                                          1. **Liquid Haskell Tutorial**
                                                                                                                                             - URL: https://ucsd-progsys.github.io/liquidhaskell-tutorial/
                                                                                                                                                - Refinement types for compile-time safety proofs

                                                                                                                                                **Note:** The CompCert range analysis paper requires academic database access. Alternative resources include LLVM optimization documentation.

                                                                                                                                                ### Week 10: Error Handling

                                                                                                                                                **Verified Primary Resources:**
                                                                                                                                                1. **Rust Error Handling Book Chapter**
                                                                                                                                                   - URL: https://doc.rust-lang.org/book/ch09-00-error-handling.html
                                                                                                                                                      - Comprehensive Result and Option type patterns

                                                                                                                                                      2. **Itanium C++ ABI: Exception Handling**
                                                                                                                                                         - URL: https://itanium-cxx-abi.github.io/cxx-abi/abi-eh.html
                                                                                                                                                            - Zero-cost exception implementation specification

                                                                                                                                                            ### Week 11: Traits & Interfaces

                                                                                                                                                            **Verified Primary Resources:**
                                                                                                                                                            1. **Rust Traits Tutorial**
                                                                                                                                                               - URL: https://doc.rust-lang.org/book/ch10-02-traits.html
                                                                                                                                                                  - Practical trait system implementation

                                                                                                                                                                  2. **Rust Traits Reference**
                                                                                                                                                                     - URL: https://doc.rust-lang.org/reference/items/traits.html
                                                                                                                                                                        - Complete trait system specification

                                                                                                                                                                        ### Week 12: Generics

                                                                                                                                                                        **Verified Primary Resources:**
                                                                                                                                                                        1. **Rust Generics Tutorial**
                                                                                                                                                                           - URL: https://doc.rust-lang.org/book/ch10-00-generics.html
                                                                                                                                                                              - Comprehensive generics with monomorphization

                                                                                                                                                                              ### Week 13: Ownership Extensions

                                                                                                                                                                              **Primary Resources:**
                                                                                                                                                                              1. **Region-Based Memory Management** (Tofte & Talpin)
                                                                                                                                                                                 - URL: https://dl.acm.org/doi/10.1145/165180.165214
                                                                                                                                                                                    - Foundational academic paper on region inference

                                                                                                                                                                                    **Note:** This is an academic paper requiring institutional access or purchase.

                                                                                                                                                                                    ### Week 14: Concurrency Model

                                                                                                                                                                                    **Verified Primary Resources:**
                                                                                                                                                                                    1. **Go Concurrency Patterns Blog**
                                                                                                                                                                                       - URL: https://go.dev/blog/pipelines
                                                                                                                                                                                          - CSP-style concurrency implementation patterns

                                                                                                                                                                                          2. **Rust Concurrency Chapter**
                                                                                                                                                                                             - URL: https://doc.rust-lang.org/book/ch16-00-concurrency.html
                                                                                                                                                                                                - Safe thread and message passing implementation

                                                                                                                                                                                                3. **Send and Sync Traits**
                                                                                                                                                                                                   - URL: https://doc.rust-lang.org/nomicon/send-and-sync.html
                                                                                                                                                                                                      - Thread safety type system design

                                                                                                                                                                                                      ### Week 15: Unsafe Code Blocks

                                                                                                                                                                                                      **Verified Primary Resources:**
                                                                                                                                                                                                      1. **Rust Unsafe Code Guidelines**
                                                                                                                                                                                                         - URL: https://rust-lang.github.io/unsafe-code-guidelines/
                                                                                                                                                                                                            - Best practices for unsafe code encapsulation

                                                                                                                                                                                                            2. **The Rustonomicon**
                                                                                                                                                                                                               - URL: https://doc.rust-lang.org/nomicon/
                                                                                                                                                                                                                  - Complete guide to unsafe Rust programming

                                                                                                                                                                                                                  ### Week 16: Code Generation

                                                                                                                                                                                                                  **Verified Primary Resources:**
                                                                                                                                                                                                                  1. **LLVM Kaleidoscope JIT and Optimizer** - Extends Kaleidoscope with control flow including if/then/else and for loops
                                                                                                                                                                                                                     - Optimization: https://llvm.org/docs/tutorial/MyFirstLanguageFrontend/LangImpl04.html
                                                                                                                                                                                                                        - JIT: https://llvm.org/docs/tutorial/MyFirstLanguageFrontend/LangImpl05.html

                                                                                                                                                                                                                        ## Phase 3: Core Compiler & Runtime (Weeks 17-24)

                                                                                                                                                                                                                        ### Week 17: Linking & FFI

                                                                                                                                                                                                                        **Verified Primary Resources:**
                                                                                                                                                                                                                        1. **Rust FFI Guide**
                                                                                                                                                                                                                           - URL: https://doc.rust-lang.org/nomicon/ffi.html
                                                                                                                                                                                                                              - Comprehensive foreign function interface tutorial

                                                                                                                                                                                                                              2. **System V ABI Specification**
                                                                                                                                                                                                                                 - URL: https://uclibc.org/docs/psABI-x86_64.pdf
                                                                                                                                                                                                                                    - Official calling convention specification

                                                                                                                                                                                                                                    ### Week 18: Runtime & Startup

                                                                                                                                                                                                                                    **Verified Primary Resources:**
                                                                                                                                                                                                                                    1. **OSDev Bare Bones Tutorial**
                                                                                                                                                                                                                                       - URL: https://wiki.osdev.org/Bare_Bones
                                                                                                                                                                                                                                          - Minimal runtime startup implementation

                                                                                                                                                                                                                                          2. **Musl libc Source Code**
                                                                                                                                                                                                                                             - URL: https://musl.libc.org/
                                                                                                                                                                                                                                                - Reference minimal C runtime implementation

                                                                                                                                                                                                                                                ### Week 19-24: Advanced Compiler Features

                                                                                                                                                                                                                                                **Key Verified Resources:**
                                                                                                                                                                                                                                                1. **LLVM Passes Documentation**
                                                                                                                                                                                                                                                   - URL: https://llvm.org/docs/Passes.html
                                                                                                                                                                                                                                                      - Comprehensive optimization techniques

                                                                                                                                                                                                                                                      2. **LLVM Source Level Debugging**
                                                                                                                                                                                                                                                         - URL: https://llvm.org/docs/SourceLevelDebugging.html
                                                                                                                                                                                                                                                            - Debug information generation

                                                                                                                                                                                                                                                            3. **DWARF Debugging Information Format**
                                                                                                                                                                                                                                                               - URL: http://dwarfstd.org/
                                                                                                                                                                                                                                                                  - Official debug format specification

                                                                                                                                                                                                                                                                  ## Phase 4: Advanced Features (Weeks 25-32)

                                                                                                                                                                                                                                                                  ### Week 25: Macros & Metaprogramming

                                                                                                                                                                                                                                                                  **Verified Primary Resources:**
                                                                                                                                                                                                                                                                  1. **Rust Macros by Example**
                                                                                                                                                                                                                                                                     - URL: https://doc.rust-lang.org/reference/macros-by-example.html
                                                                                                                                                                                                                                                                        - Hygienic macro implementation guide

                                                                                                                                                                                                                                                                        ### Week 26: Pattern Matching

                                                                                                                                                                                                                                                                        **Verified Primary Resources:**
                                                                                                                                                                                                                                                                        1. **Rust Pattern Syntax**
                                                                                                                                                                                                                                                                           - URL: https://doc.rust-lang.org/book/ch18-00-patterns.html
                                                                                                                                                                                                                                                                              - Comprehensive pattern matching with exhaustiveness

                                                                                                                                                                                                                                                                              ### Week 27-32: Language Features

                                                                                                                                                                                                                                                                              **Key Verified Resources:**
                                                                                                                                                                                                                                                                              1. **Rust Iterators and Closures**
                                                                                                                                                                                                                                                                                 - URL: https://doc.rust-lang.org/book/ch13-00-functional-features.html
                                                                                                                                                                                                                                                                                    - Zero-cost iterator implementation

                                                                                                                                                                                                                                                                                    2. **Rust Module System**
                                                                                                                                                                                                                                                                                       - URL: https://doc.rust-lang.org/book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html
                                                                                                                                                                                                                                                                                          - Complete module and import system

                                                                                                                                                                                                                                                                                          3. **Rust Inline Assembly**
                                                                                                                                                                                                                                                                                             - URL: https://doc.rust-lang.org/reference/inline-assembly.html
                                                                                                                                                                                                                                                                                                - Safe inline assembly integration

                                                                                                                                                                                                                                                                                                ## Phase 5: Ecosystem Tools (Weeks 33-40)

                                                                                                                                                                                                                                                                                                ### Week 33: Documentation System

                                                                                                                                                                                                                                                                                                **Verified Primary Resources:**
                                                                                                                                                                                                                                                                                                1. **Rustdoc Book**
                                                                                                                                                                                                                                                                                                   - URL: https://doc.rust-lang.org/rustdoc/
                                                                                                                                                                                                                                                                                                      - Documentation generator implementation guide

                                                                                                                                                                                                                                                                                                      ### Week 34: Language Server Protocol

                                                                                                                                                                                                                                                                                                      **Verified Primary Resources:**
                                                                                                                                                                                                                                                                                                      1. **LSP Specification**
                                                                                                                                                                                                                                                                                                         - URL: https://microsoft.github.io/language-server-protocol/specifications/specification-current/
                                                                                                                                                                                                                                                                                                            - Complete protocol specification

                                                                                                                                                                                                                                                                                                            2. **Rust Analyzer Guide**
                                                                                                                                                                                                                                                                                                               - URL: https://rust-analyzer.github.io/manual.html
                                                                                                                                                                                                                                                                                                                  - Production LSP server implementation

                                                                                                                                                                                                                                                                                                                  ### Week 35-40: Development Tools

                                                                                                                                                                                                                                                                                                                  **Key Verified Resources:**
                                                                                                                                                                                                                                                                                                                  1. **Rustfmt Configuration**
                                                                                                                                                                                                                                                                                                                     - URL: https://rust-lang.github.io/rustfmt/
                                                                                                                                                                                                                                                                                                                        - Code formatter implementation

                                                                                                                                                                                                                                                                                                                        2. **Clippy Lint Documentation**
                                                                                                                                                                                                                                                                                                                           - URL: https://doc.rust-lang.org/clippy/
                                                                                                                                                                                                                                                                                                                              - Static analysis and linting tools

                                                                                                                                                                                                                                                                                                                              3. **Cargo Reference**
                                                                                                                                                                                                                                                                                                                                 - URL: https://doc.rust-lang.org/cargo/reference/
                                                                                                                                                                                                                                                                                                                                    - Complete package manager implementation

                                                                                                                                                                                                                                                                                                                                    ## Phase 6: Stabilization (Weeks 41-48)

                                                                                                                                                                                                                                                                                                                                    ### Week 41: Security and Testing

                                                                                                                                                                                                                                                                                                                                    **Verified Primary Resources:**
                                                                                                                                                                                                                                                                                                                                    1. **AddressSanitizer Documentation**
                                                                                                                                                                                                                                                                                                                                       - URL: https://clang.llvm.org/docs/AddressSanitizer.html
                                                                                                                                                                                                                                                                                                                                          - Memory safety testing tools

                                                                                                                                                                                                                                                                                                                                          2. **LLVM Sanitizer Coverage**
                                                                                                                                                                                                                                                                                                                                             - URL: https://llvm.org/docs/SanitizerCoverage.html
                                                                                                                                                                                                                                                                                                                                                - Code coverage and fuzzing integration

                                                                                                                                                                                                                                                                                                                                                ### Week 42: Cross-Compilation

                                                                                                                                                                                                                                                                                                                                                **Verified Primary Resources:**
                                                                                                                                                                                                                                                                                                                                                1. **Rust Cross-Compilation Guide**
                                                                                                                                                                                                                                                                                                                                                   - URL: https://rust-lang.github.io/rustup/cross-compilation.html
                                                                                                                                                                                                                                                                                                                                                      - Multi-target build configuration

                                                                                                                                                                                                                                                                                                                                                      ### Week 43-48: Performance and Launch

                                                                                                                                                                                                                                                                                                                                                      **Key Verified Resources:**
                                                                                                                                                                                                                                                                                                                                                      1. **Rust Performance Book**
                                                                                                                                                                                                                                                                                                                                                         - URL: https://nnethercote.github.io/perf-book/
                                                                                                                                                                                                                                                                                                                                                            - Performance optimization techniques

                                                                                                                                                                                                                                                                                                                                                            2. **Linux perf Tutorial**
                                                                                                                                                                                                                                                                                                                                                               - URL: https://perf.wiki.kernel.org/index.php/Tutorial
                                                                                                                                                                                                                                                                                                                                                                  - System performance analysis

                                                                                                                                                                                                                                                                                                                                                                  3. **Semantic Versioning Specification**
                                                                                                                                                                                                                                                                                                                                                                     - URL: https://semver.org/
                                                                                                                                                                                                                                                                                                                                                                        - Version management for releases

                                                                                                                                                                                                                                                                                                                                                                        ## Alternative and Supplementary Resources

                                                                                                                                                                                                                                                                                                                                                                        **Additional Implementation Examples:**
                                                                                                                                                                                                                                                                                                                                                                        1. **Python Kaleidoscope Implementation**
                                                                                                                                                                                                                                                                                                                                                                           - URL: https://github.com/eliben/pykaleidoscope/
                                                                                                                                                                                                                                                                                                                                                                              - LLVM tutorial implemented in Python using llvmlite

                                                                                                                                                                                                                                                                                                                                                                              2. **Complete Crafting Interpreters Code**
                                                                                                                                                                                                                                                                                                                                                                                 - URL: https://github.com/munificent/craftinginterpreters
                                                                                                                                                                                                                                                                                                                                                                                    - Full source code for both tree-walk and bytecode interpreters

                                                                                                                                                                                                                                                                                                                                                                                    **Academic Textbook Alternatives:**
                                                                                                                                                                                                                                                                                                                                                                                    For resources requiring textbook access, these online alternatives provide similar coverage:
                                                                                                                                                                                                                                                                                                                                                                                    - Stanford CS143 Compilers course materials
                                                                                                                                                                                                                                                                                                                                                                                    - MIT 6.035 Computer Language Engineering materials
                                                                                                                                                                                                                                                                                                                                                                                    - Various university compiler construction course websites

                                                                                                                                                                                                                                                                                                                                                                                    ## Resource Verification Notes

                                                                                                                                                                                                                                                                                                                                                                                    All URLs have been verified as of September 2025. Academic papers and some textbooks require institutional access or purchase. Where possible, alternative freely available resources covering similar material have been provided. The LLVM and Rust documentation represent the highest quality freely available resources for systems language implementation, with the Crafting Interpreters book providing excellent pedagogical coverage of fundamental concepts.

                                                                                                                                                                                                                                                                                                                                                                                    Priority has been given to resources that provide complete, working implementations rather than theoretical discussions alone. Each resource directly supports the specific learning objectives outlined in the corresponding week of the development roadmap.

