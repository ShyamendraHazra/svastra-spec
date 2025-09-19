# SVR Language Specification (v2.0 — Production Ready Edition)

**Svastra (SVR)** is a lean, safe, and performant systems programming language. It bridges the **low-level control of C** with the **safety guarantees of Rust**, but without Rust's complexity. SVR is designed for simplicity, predictability, and uncompromising performance.

---

## 1. Core Philosophy 🧠

SVR is built on the **Principle of Ascending Control**:

* **Start simple:** safe, C-like defaults.
* **Go deeper when needed:** explicit opt-in for advanced features (ownership, concurrency, unsafe blocks).
* **Never pay for what you don't use:** zero hidden runtime cost.

Philosophy pillars:

* **C-familiar syntax** with strong guarantees.
* **Rust-level safety** in normal and edge cases.
* **Performance as sacred**: SVR code runs as fast as idiomatic C.

---

## 2. Lexical Structure

### 2.1 Keywords

Reserved identifiers cannot be reused. Each keyword belongs to a semantic group.

| Keyword                               | Category     | Description                                                 |
| ------------------------------------- | ------------ | ----------------------------------------------------------- |
| let, mut, const                       | Declarations | let: immutable, mut: mutable, const: compile-time constant. |
| struct, enum, impl, trait             | Data         | Define structures, enums, implementations, and traits.      |
| if, else, match, try                  | Control Flow | Conditionals, pattern matching, and error grouping.        |
| while, for, return, break, continue   | Control Flow | Looping, function return, and loop control.                |
| raw                                   | Safety       | Enters unsafe mode (UB possible).                           |
| use, mod, pub                         | Modules      | Module declaration, import, and visibility.                 |
| async, await, spawn, timeout          | Concurrency  | Async functions, suspension, task spawning, and timeouts.   |
| extern, extern safe                   | FFI          | extern: unsafe interop. extern safe: whitelisted safe FFI.  |
| own, share, arena, heap, region, free | Memory       | Ownership and allocation model.                             |
| auto, where                           | Types        | Generic inference and constraints.                          |
| i8–i64, u8–u64, f32, f64              | Primitives   | Integer and float types.                                    |
| bool, char, str                       | Primitives   | Boolean, character, string slice.                           |
| true, false, null                     | Literals     | Boolean values and null pointer.                            |
| this                                  | Methods      | Refers to the current instance.                             |

### 2.2 Identifiers

* C-style: `[a-zA-Z_][a-zA-Z0-9_]*`
* Case-sensitive.
* Cannot start with digits.

### 2.3 Comments

* Line: `// like this`
* Block: `/* like this */`
* Documentation: `/// doc comment`

### 2.4 String Literals

* UTF-8: `"hello world"`
* Raw strings: `r"no escapes \n here"`
* Multi-line: `"""long text here"""`

---

## 3. Declarations

### 3.1 Variables

```svr
let x: i32 = 5;         // immutable
let mut y: i32 = 10;    // mutable
const PI: f64 = 3.1415; // compile-time constant
```

* `let` → runtime value, stack allocated.
* `mut` → mutable variable.
* `const` → inlined, no address.

**Type inference:**
```svr
let x = 42;        // inferred as i32
let name = "SVR";  // inferred as str
```

### 3.2 Functions

C-style syntax is used consistently.

```svr
i32 add(a: i32, b: i32) {
    return a + b;
}

// Generic functions
auto max(a: auto, b: auto) where auto: Comparable {
    return if a > b { a } else { b };
}
```

* Always `return_type name(params)`.
* No `fn` keyword.
* Parameters: `identifier: Type`.

---

## 4. Type System & Data Structures 📦

### 4.1 Complete Type Reference

| Category      | Type     | Size        | Range/Notes                         |
| ------------- | -------- | ----------- | ----------------------------------- |
| **Integers**  | i8       | 8 bits      | -128 to 127                         |
|               | i16      | 16 bits     | -32,768 to 32,767                   |
|               | i32      | 32 bits     | -2³¹ to 2³¹-1                       |
|               | i64      | 64 bits     | -2⁶³ to 2⁶³-1                       |
|               | isize    | pointer     | Platform-dependent signed integer   |
|               | u8       | 8 bits      | 0 to 255                            |
|               | u16      | 16 bits     | 0 to 65,535                         |
|               | u32      | 32 bits     | 0 to 2³²-1                          |
|               | u64      | 64 bits     | 0 to 2⁶⁴-1                          |
|               | usize    | pointer     | Platform-dependent unsigned integer |
| **Floats**    | f32      | 32 bits     | IEEE 754 single precision           |
|               | f64      | 64 bits     | IEEE 754 double precision           |
| **Text**      | bool     | 1 byte      | true or false                       |
|               | char     | 32 bits     | Unicode scalar value                |
|               | str      | fat pointer | UTF-8 string slice (ptr, len)       |
| **Pointers**  | *T       | pointer     | Raw pointer (unsafe)                |
|               | *mut T   | pointer     | Mutable raw pointer (unsafe)        |
|               | &T       | pointer     | Immutable reference                 |
|               | &mut T   | pointer     | Mutable reference                   |
| **Ownership** | own T    | pointer     | Exclusive ownership pointer         |
|               | share T  | fat pointer | Reference-counted pointer           |
| **Arrays**    | [T; N]   | N × size(T) | Fixed-size array                    |
|               | [T]      | fat pointer | Dynamic array slice (ptr, len)      |
| **Special**   | ()       | 0 bytes     | Unit type (empty tuple)             |
|               | !        | 0 bytes     | Never type (diverging functions)    |

### 4.2 Structs & Methods

```svr
struct User { 
    id: i32, 
    active: bool,
    name: str 
}

impl User {
    // Constructors
    User new(id: i32, name: str) {
        return User { id: id, active: false, name: name };
    }

    // Methods
    void activate(&mut this) {
        this.active = true;
    }

    bool has_id(&this, id: i32) {
        return this.id == id;
    }

    void consume(this) {
        // consumes self
    }
}
```

**Method types:**
* `&this` → immutable borrow.
* `&mut this` → mutable borrow.
* `this` → consuming method.

### 4.3 Enums & Pattern Matching

```svr
enum Option[T] { 
    Some(T), 
    None 
}

enum Result[T, E] { 
    Ok(T), 
    Err(E) 
}

// Pattern matching
str describe(opt: Option[i32]) {
    match opt {
        Some(val) if val > 0 => "positive number",
        Some(val) => "non-positive number",
        None => "no value",
        _ => "unreachable" // wildcard
    }
}
```

**Match requirements:**
* Exhaustive by default.
* Guards supported with `if`.
* Wildcard `_` allowed.

### 4.4 Traits

```svr
trait Display {
    str show(&this);
}

trait Comparable {
    bool less_than(&this, other: &this);
}

impl Display for i32 {
    str show(&this) {
        return int_to_string(*this);
    }
}
```

### 4.5 Arrays and Slices

```svr
let arr: [i32; 5] = [1, 2, 3, 4, 5];  // fixed-size array
let slice: [i32] = &arr[1..4];         // slice reference
```

### 4.6 Attributes

```svr
#[inline]        // optimization hint
#[repr(C)]       // C-compatible layout
#[test]          // test function
#[derive(Eq)]    // auto-derive traits
```

---

## 5. Memory & Lifetime Model 🪜

SVR enforces **Rust-level safety** while exposing **C-like clarity**.

### 5.1 Tier 1: Automatic Memory

* Stack allocated.
* Compiler infers lifetimes.
* Borrow checker ensures no dangling or invalid aliasing.

```svr
void borrow_example() {
    let x = 42;
    let r = &x;  // lifetime inferred automatically
    print(*r);
}  // x and r both drop here
```

### 5.2 Tier 2: Managed Memory

| Tool    | Syntax                     | Ownership              | Failure Mode    |
| ------- | -------------------------- | ---------------------- | --------------- |
| own T   | own p = heap.new(T);       | Exclusive owner.       | Returns Result  |
| share T | share p = heap.share(T);   | Reference-counted.     | Returns Result  |
| arena   | let a = arena(size);       | Bulk-allocated region. | Panics on OOM   |

**Usage examples:**
```svr
// Heap allocation with error handling
Result[own User, OutOfMemory] create_user() {
    let user = heap.new(User::new(1, "Alice"))?;
    return Ok(user);
}

// Arena allocation
void process_batch() {
    let workspace = arena(1024 * 1024); // 1MB arena
    let items = workspace.new_array(Item, 1000);
    // ... process items
}  // entire arena freed here
```

**Ownership rules:**
* `free(alias)` consumes an exclusive alias, invalidating it.
* Arenas auto-clean on drop.
* `share` uses atomic reference counting, panics on overflow.

### 5.3 Tier 3: Raw (Unsafe)

```svr
raw {
    let p: *mut i32 = heap.raw_alloc(4);
    *p = 42;
    heap.raw_free(p);
}
```

* UB only possible inside `raw`.
* Compiler still issues warnings, never silent.
* Required for: pointer arithmetic, type punning, calling unsafe extern functions.

### 5.4 Lifetimes in Practice

* Inference works in 90% of cases.
* Compiler suggests `region` annotations when inference fails.

```svr
// Simple case - no annotations needed
str first_word(text: str) {
    // lifetime automatically inferred
    return text.split(' ').first();
}

// Complex case - explicit regions required
str longer[region 'a](a: &'a str, b: &'a str) -> &'a str {
    return if a.len() > b.len() { a } else { b };
}
```

**Safety guarantees:**
* UB impossible outside `raw`.
* Double-free impossible: `free` consumes the alias.
* Use-after-free impossible: borrow checker prevents dangling references.
* Data races impossible: `share` enforces immutability across threads.

---

## 6. Error Handling ⚠️

### 6.1 Result Type

```svr
enum Result[T, E] { Ok(T), Err(E) }
```

### 6.2 Error Propagation

```svr
Result[str, IoError] read_config() {
    let file = open("config.txt")?;  // propagates error
    let content = file.read_all()?;
    return Ok(content);
}

// Group multiple operations
Result[void, Error] setup() {
    try {
        create_directory("logs");
        write_file("logs/app.log", "Started");
        init_database();
    }  // any error is automatically wrapped and returned
}
```

**Error handling rules:**
* `?` propagates errors upward in the call stack.
* `try` blocks group operations and return first error encountered.
* All fallible operations return `Result` types.

### 6.3 Panic vs Result

* `Result` for recoverable errors (I/O, parsing, allocation).
* `panic` for programming errors (array bounds, assertion failures).

---

## 7. Concurrency 🌐

### 7.1 Async/Await

```svr
async Result[str, NetworkError] fetch(url: str) {
    let response = http.get(url).await;
    return response.text();
}

async void main() {
    let task1 = spawn fetch("https://api1.com");
    let task2 = spawn fetch("https://api2.com");
    
    let results = await [task1, task2];  // join multiple tasks
}
```

**Async rules:**
* Tasks run on work-stealing thread pool.
* Data shared across `await` must be `share` types.
* Scope-exit automatically cancels child tasks.

### 7.2 Task Management

```svr
// Spawn detached task
spawn background_work();

// Timeout operations
async Result[str, Timeout] fetch_with_timeout(url: str) {
    return timeout(5000) {  // 5 second timeout
        http.get(url).await
    };
}
```

### 7.3 Concurrency Safety

* No data races: enforced at compile time.
* Send/Sync automatically derived for safe types.
* `share` types are immutable and thread-safe.

---

## 8. Foreign Function Interface 🔗

### 8.1 Unsafe Extern

```svr
extern "C" {
    i32 printf(*char, ...);
    *void malloc(usize);
}

void use_c_functions() {
    raw {
        printf(c"Hello from C\n");
        let ptr = malloc(100);
    }
}
```

* Must be called inside `raw` blocks.
* Variadic functions supported.
* C string literals: `c"null terminated"`

### 8.2 Safe Extern

```svr
extern safe "C" {
    i32 puts(*char);
    void exit(i32);
}

void safe_c_call() {
    puts(c"No raw block needed");  // safe to call anywhere
}
```

**Safe extern criteria:**
* Function must be known to be memory-safe.
* No raw pointers to mutable memory.
* No undefined behavior in typical usage.

### 8.3 C Interop

```svr
#[repr(C)]
struct CPoint { x: f64, y: f64 }

// Automatic padding and alignment matching C
#[repr(C, packed)]
struct CHeader { magic: u32, size: u16 }
```

---

## 9. Standard Library & Tooling 📚

### 9.1 Core Types

**Collections:**
* `Array[T]` - growable array
* `Map[K, V]` - hash map
* `Set[T]` - hash set

**Utilities:**
* `Option[T]` - nullable values
* `Result[T, E]` - error handling
* `Iterator[T]` - lazy iteration

**Strings:**
* `str` - string slice
* `String` - owned string
* UTF-8 guaranteed

### 9.2 The `svr` Tool

```bash
svr new project_name     # create new project
svr build               # compile project  
svr run                 # build and run
svr test                # run tests
svr check               # verify safety without compiling
svr fmt                 # format source code
svr doc                 # generate documentation
```

### 9.3 Package Management

```toml
# svr.toml
[package]
name = "my_project"
version = "1.0.0"

[dependencies]
json = "1.2.0"
http = { version = "0.5.0", features = ["tls"] }
```

### 9.4 Debugging

* Debug symbols emitted by default.
* Stack traces map to source lines.
* Integration with gdb, lldb.
* Built-in profiling: `svr profile`

---

## 10. Modules & Imports 📦

File-based module system for simplicity.

```svr
// main.svr
mod network;  // imports network.svr
mod database; // imports database.svr

use network.http;
use database.{connect, User};

void main() {
    let client = http.Client::new();
    let db = connect("localhost");
}
```

**Module rules:**
* Each file is a module.
* Directory with `mod.svr` is a module.
* `pub` makes items visible outside the module.
* Circular imports detected and forbidden.

---

## 11. Generics & Constraints

### 11.1 Simple Generics

```svr
// Auto inference for simple cases
auto identity(x: auto) {
    return x;
}

// Explicit type parameters
struct Container[T] {
    value: T
}

impl[T] Container[T] {
    T get(&this) {
        return this.value;
    }
}
```

### 11.2 Trait Bounds

```svr
auto print_all(items: auto) where auto: Iterator, auto::Item: Display {
    for item in items {
        print(item.show());
    }
}

// Multiple constraints
auto sort_and_display(mut arr: auto) 
    where auto: Array,
          auto::Item: Comparable + Display {
    arr.sort();
    print_all(arr);
}
```

---

## 12. Grammar (EBNF) 📜

```ebnf
program        = { item } ;
item           = function | struct | enum | impl | trait | mod | const | use ;
function       = type identifier "(" [ params ] ")" block ;
params         = param { "," param } ;
param          = identifier ":" type ;
type           = primitive | identifier [ "[" type_args "]" ] | "&" [ "mut" ] type ;
type_args      = type { "," type } ;
block          = "{" { statement } "}" ;
statement      = declaration | expression ";" | block | if | while | for | match | return ;
declaration    = ("let" | "let mut" | "const") identifier ":" type "=" expression ";" ;
expression     = literal | identifier | call | binary_op | unary_op ;
call           = identifier "(" [ arguments ] ")" ;
arguments      = expression { "," expression } ;
binary_op      = expression ( "+" | "-" | "*" | "/" | "==" | "!=" | "<" | ">" ) expression ;
literal        = integer | float | string | char | boolean | array ;
```

---

## 13. Migration & Interop 🚀

### 13.1 C Header Translation

```bash
svr bindgen header.h > bindings.svr
```

Automatically generates SVR extern declarations from C headers.

### 13.2 ABI Compatibility

* `#[repr(C)]` types have C-compatible layout.
* Functions with C calling convention can be called from C.
* Gradual migration supported.

### 13.3 Build Integration

```make
# Makefile integration
SOURCES = main.svr utils.svr
BINARY = my_program

$(BINARY): $(SOURCES)
    svr build --output $(BINARY)
```

---

## 14. Performance Guarantees 🚀

### 14.1 Zero-Cost Abstractions

* Generics monomorphize (no runtime dispatch).
* Iterators compile to simple loops.
* Match statements become jump tables or conditionals.
* Ownership tracking is compile-time only.

### 14.2 Memory Layout

* Structs have predictable layout (C-compatible by default).
* Enums use minimal space (tagged unions).
* No hidden heap allocations.

### 14.3 Optimization

* LLVM backend provides world-class optimization.
* Link-time optimization enabled by default.
* Profile-guided optimization supported.

---

## 15. Safety & Simplicity Proofs ✅

### 15.1 Memory Safety

* **No use-after-free**: Borrow checker prevents dangling references.
* **No double-free**: `free` consumes exclusive aliases.
* **No buffer overflows**: Array bounds checking in safe code.
* **No wild pointers**: References are always valid.

### 15.2 Type Safety

* **No type confusion**: Strong static typing with no implicit conversions.
* **Exhaustive matching**: All enum variants must be handled.
* **Initialize before use**: Variables must be initialized.

### 15.3 Concurrency Safety

* **No data races**: Shared data must be immutable (`share` types).
* **No iterator invalidation**: Borrow checker prevents concurrent modification.
* **Controlled mutability**: `&mut` references are exclusive.

### 15.4 Error Safety

* **All failures explicit**: Functions return `Result` for fallible operations.
* **Resource cleanup**: RAII ensures resources are properly released.
* **No silent failures**: Errors must be explicitly handled or propagated.

---

## 16. Comparison with Other Languages

| Feature              | C   | Rust | Go  | SVR |
| -------------------- | --- | ---- | --- | --- |
| Memory Safety        | ❌   | ✅    | ✅   | ✅   |
| Performance          | ✅   | ✅    | ⚠️   | ✅   |
| Simplicity           | ✅   | ❌    | ✅   | ✅   |
| Concurrency Safety   | ❌   | ✅    | ✅   | ✅   |
| C Interop            | ✅   | ✅    | ⚠️   | ✅   |
| Learning Curve       | ⚠️   | ❌    | ✅   | ✅   |
| Zero-cost Abstraction| ❌   | ✅    | ❌   | ✅   |

**SVR's unique position**: The safety of Rust with the simplicity of Go and the performance of C.

---

## Copyright

© 2025 Svastra Language Project. All rights reserved.
