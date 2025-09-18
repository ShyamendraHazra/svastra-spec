# SVR (Svastra) Language Specification
## Version 3.1 - September 2025

> **A Modern, Statically-Verified Systems Programming Language**
> 
> *Complete Specification with Formalized Safety*

---

## Table of Contents

1. [Design Philosophy and Architecture](#1-design-philosophy-and-architecture)
2. [Type System and Data Structures](#2-type-system-and-data-structures)
3. [Memory Model: Ownership and Borrows](#3-memory-model-ownership-and-borrows)
4. [Safety Proof: The Memory State Machine](#4-safety-proof-the-memory-state-machine)
5. [Standard Library Architecture](#5-standard-library-architecture)
6. [Appendix](#6-appendix)

---

## 1. Design Philosophy and Architecture

### 1.1 Core Design Principles

SVR (Svastra) is a fundamental rethinking of systems programming, emerging from a critical analysis of the persistent conflict between performance and safety.

> **💡 Key Insight**
> 
> The majority of critical software vulnerabilities—such as use-after-free, buffer overflows, and data races—stem from the semantic ambiguity of the raw pointer in languages like C. A raw pointer is merely an address, lacking the essential context of ownership, lifetime, and bounds.

SVR resolves this by replacing the raw pointer with a new set of language primitives. These primitives build the concepts of ownership and lifetime directly into the type system, enabling verification at compile time.

### 1.2 Architectural Pillars

| Pillar | Description | Benefit |
|--------|-------------|---------|
| **Unique Ownership** | Every memory resource is uniquely owned by a `Box[T]` type. The compiler statically tracks this ownership, making memory management explicit and verifiable. | Eliminates use-after-free, double-free, and resource leaks at compile time. |
| **Safe Borrows** | Temporary, non-owning access to memory is exclusively managed through bounds-checked `Ref[T]` types. | Eliminates buffer overflows and provides C-like data manipulation capabilities without the associated risks. |
| **Zero-Cost Safety** | All ownership and borrow-checking analyses are performed at compile time. | Safety guarantees incur no runtime performance overhead, ensuring performance characteristics comparable to C. |

---

## 2. Type System and Data Structures

### 2.1 Primitive Types

| Category | Types |
|----------|-------|
| **Signed Integers** | `i8`, `i16`, `i32`, `i64` |
| **Unsigned Integers** | `u8`, `u16`, `u32`, `u64` |
| **Floating-Point** | `f32`, `f64` (IEEE 754) |
| **Character** | `char` (32-bit Unicode Scalar Value) |
| **Boolean** | `bool` (`true`, `false`) |

### 2.2 Composite Data Structures

#### `struct` - Aggregate Data Types
Defines aggregate data types with a defined memory layout.

```svr
struct NetworkPacket {
    source_ip: u32,
    dest_ip: u32,
    payload_size: u16,
}
```

#### `enum` - Tagged Union Types
Defines tagged union types for representing one-of-many possibilities.

```svr
enum IpAddr {
    V4(u32),
    V6(u128),
}
```

### 2.3 Ownership and Reference Types

| Type | Syntax | Role | Key Property | Common Use Case |
|------|--------|------|--------------|-----------------|
| **Owned Box** | `Box[T]` | Owner | Unique, ownership is moved on assignment. | Storing data on the heap, managing resource lifetime. |
| **Immutable Ref** | `Ref[T]` | Borrower | Temporary, non-owning, bounds-checked. | Function parameters, safe data access. |
| **Mutable Ref** | `MutRef[T]` | Mutable Borrower | Exclusive, non-owning, bounds-checked. | In-place modification of data. |

---

## 3. Memory Model: Ownership and Borrows

### 3.1 `Box[T]`: The Unique Owner

A `Box[T]` confers unique ownership of a heap-allocated memory resource.

**Key Properties:**
- **Unique Ownership**: For any given resource, there is exactly one owning `Box`.
- **Move Semantics**: Assigning a `Box` to a new variable *moves* ownership. The original variable is statically invalidated and cannot be used again.
- **Resource Consumption**: Functions that deallocate or consume a resource, such as `mem::free`, take ownership of the `Box`, rendering it permanently unusable.

```svr
// `b1` is the initial owner.
let b1: Box[i32] = mem::alloc(42);

// Ownership is moved to `b2`. `b1` is now considered invalid.
let b2 = b1;

// The following would be a COMPILE-TIME ERROR due to use of a moved value:
// *b1 = 99;

// `mem::free` consumes `b2`, invalidating it.
mem::free(b2);

// The following would be a COMPILE-TIME ERROR due to use of a moved value:
// mem::free(b2);
```

### 3.2 `Ref[T]` and `MutRef[T]`: The Safe Borrowers

A `Ref[T]` provides temporary, non-owning access to memory. It is a "fat pointer," containing both an address and a length.

**Key Properties:**
- **Borrowing**: A `Ref` is created (borrowed) from an active `Box`.
- **Lifetime**: A `Ref` is lexically scoped. The compiler guarantees that no `Ref` can outlive the `Box` from which it was borrowed.
- **Aliasing Rules**: For any given resource, you can have either one or more immutable `Ref[T]`s, or exactly one mutable `MutRef[T]`. These rules are mutually exclusive.
- **Bounds-Checking**: All memory access through a `Ref` or `MutRef` is statically bounds-checked, eliminating buffer overflows.

---

## 4. Safety Proof: The Memory State Machine

### 4.1 Formal Definition

The safety guarantees of SVR can be formally described. The compiler's static analysis acts as a verifier for a formal model of memory. The state of any owned resource (`Box[T]`) can be modeled as a finite state machine. A program is guaranteed to be safe if, for every resource, it only ever follows valid transitions on this state machine.

**The memory state machine for a resource R is a tuple (S, Σ, δ, s₀, F) where:**

| Symbol | Definition |
|--------|------------|
| **S** | `{ Allocated, ImmutablyBorrowed, MutablyBorrowed, Moved, Freed }` is the finite set of states. |
| **Σ** | is the set of operations (the alphabet), e.g., `borrow`, `mut_borrow`, `move`, `free`, `dereference`. |
| **δ** | is the state-transition function. |
| **s₀** | `Allocated` is the initial state following a call to `mem::alloc`. |
| **F** | `{ Moved, Freed }` is the set of terminal states. |

### 4.2 State Transition Table (δ)

| Current State | Operation | Next State | Notes |
|---------------|-----------|------------|-------|
| `Allocated` | `borrow as Ref` | `ImmutablyBorrowed` | Can create multiple immutable borrows. |
| `Allocated` | `borrow as MutRef` | `MutablyBorrowed` | Can only create one exclusive mutable borrow. |
| `Allocated` | `move` (assignment) | `Moved` | The original variable is statically invalidated. |
| `Allocated` | `free` | `Freed` | The resource is permanently deallocated and invalid. |
| `ImmutablyBorrowed` | `borrow as Ref` | `ImmutablyBorrowed` | Can create additional immutable borrows. |
| `ImmutablyBorrowed` | `release Ref` | `Allocated` | If this was the last `Ref`, state returns to `Allocated`. |
| `MutablyBorrowed` | `release MutRef` | `Allocated` | The `MutRef`'s lexical scope ends. |

### 4.3 Safety Proof via Invalid Transitions

Safety emerges from the transitions the compiler forbids:

- **🚫 Preventing Use-After-Free**: From state `Freed`, there are no valid transitions for `dereference` or any other access operation.
- **🚫 Preventing Double-Free**: From state `Freed`, there is no valid transition for the `free` operation.
- **🚫 Preventing Data Races**: From state `MutablyBorrowed`, there are no valid transitions for `borrow as Ref` or `borrow as MutRef`. From state `ImmutablyBorrowed`, there is no valid transition for `borrow as MutRef`.

---

## 5. Standard Library Architecture

### 5.1 Core Modules

| Module | Purpose | Key Features |
|--------|---------|--------------|
| **`core`** | Core language primitives and traits | `Option[T]`, `Result[T, E]`, `Box[T]`, `Ref[T]`, iterator traits |
| **`std::mem`** | Manual memory management | `alloc`, `free`, `alloc_array`, alignment control |
| **`std::collections`** | Common owned data structures | `Vec[T]`, `String`, `HashMap[K, V]` (all built on `Box`) |
| **`std::io`** | Input/Output operations | `File`, `Reader`, `Writer`, network streams |
| **`std::sync`** | Concurrency and synchronization | `Mutex[T]`, `Atomic[T]`, `channel[T]`, `thread::spawn` |
| **`std::rc`** | Advanced ownership patterns | `SharedBox[T]` (reference-counted `Box` for shared ownership) |

---

## 6. Appendix

### A.1 Keyword Reference

| Category | Keywords |
|----------|----------|
| **Type Definition** | `struct`, `enum`, `type`, `impl`, `trait` |
| **Control Flow** | `if`, `else`, `while`, `for`, `match`, `case`, `when`, `try`, `catch` |
| **Program Structure** | `module`, `import`, `pub`, `fn` |
| **System** | `const`, `unsafe` |
| **Reserved** | `break`, `continue`, `return`, `let`, `true`, `false` |

### A.2 Glossary of Core Concepts

| Term | Definition |
|------|------------|
| **Ownership** | A memory management paradigm where every resource has one, and only one, designated 'owner' responsible for its lifetime. |
| **`Box[T]`** | An 'owned box.' A type that confers unique ownership over a heap-allocated resource. It is the sole owner. |
| **`Ref[T]`** | A 'borrowed reference.' A temporary, non-owning, bounds-checked pointer to a resource owned by a `Box`. |
| **Move** | The act of transferring ownership from one variable to another. The original variable is statically invalidated after the move. |
| **Borrow** | The act of creating a temporary reference (`Ref` or `MutRef`) to a resource without taking ownership. The borrow is lexically scoped. |

---

> **© 2025 SVR Language Foundation**  
> *This specification defines a complete, formally verified systems programming language designed for safety without compromising performance.*