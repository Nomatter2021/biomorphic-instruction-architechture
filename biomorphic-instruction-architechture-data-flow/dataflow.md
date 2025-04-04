# BIA Data Flow – Pointer-Only Compile-Time Architecture

## Overview

In BioMorphic Instruction Architecture (BIA), data does not flow as values passed between modules.  
Instead, **data is accessed strictly through permissioned pointers**, determined at compile-time.

This document explains how data is introduced into memory (RAM), how a module gets access to it,  
and how values can be retrieved or updated through controlled pointer access.

---

## 1. Data Is Born – Not Passed

In BIA, **data originates from the combination of `Struct Gen` and `MemoryAccess`**:

- `Struct Gen` defines the schema (field names and types).
- `GenSlot` shapes the expression of these structures.
- `MemoryAccess` allocates physical memory for the defined schema.

**No data is passed.  
No module owns values.  
Data exists statically and modularly.**

---

## 2. Access Is Gated – Not Given

Each module has its own isolated RAM via `MemoryAccess`.

To access any part of the RAM:
- A behavior (read/write) must be defined.
- A token must be generated (from Interface).
- The behavior and token must be evaluated by `Wrapper`.
- If passed, a pointer to a RAM region is granted.

**→ No value ever moves – only access rights move.**

---

## 3. Why Use Pointers Instead of Values?

- **Compile-time safety**: The pointer guarantees access to an approved memory region.
- **No duplication**: There is only one source of truth – the original RAM.
- **Behavior control**: Without a valid token, you can't even point – let alone read or write.

---

## 4. How Is Data "Put In" (from Bus)?

In traditional systems, a value is written directly into memory or passed to a function.  
In BIA, a value enters via:

- A compile-time initialized `MemoryAccess` object
- Or a wrapper-approved write operation with a pointer to that memory

**Example:**  
```cpp
if (Wrapper::approveWrite(token)) {
    T* ptr = MemoryAccess::getSlot<T>("field_name");
    *ptr = value_from_bus;
}
```

→ The bus does not write.  
→ The approved module writes *into* its memory using the pointer it was granted.

---

## 5. How Is Data Retrieved?

Retrieval follows the same sandboxed flow:

- The behavior must be classified as a read.
- Wrapper must approve it based on the token.
- If approved, the module gets a pointer to its memory.
- Then and only then, it may read the value.

**Example:**
```cpp
if (Wrapper::approveRead(token)) {
    const T* ptr = MemoryAccess::getSlot<T>("field_name");
    return *ptr;
}
```

---

## 6. Why This Matters

- Eliminates side effects
- Enforces memory isolation
- Makes all access verifiable at compile-time
- Models biological gene expression: no signal (token), no behavior (pointer), no effect (value)

---

## Final Note

**In BIA:**
- Data exists only where it was declared.
- You don’t carry it – you point to it.
- You can’t point unless you’re authorized.
- You can’t read unless it’s approved.

> “No token, no pointer. No pointer, no value.  
No value, no behavior.”