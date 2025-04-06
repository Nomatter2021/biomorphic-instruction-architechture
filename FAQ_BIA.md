## FAQ.md – BioMorphic Instruction Architecture (BIA)

This document consolidates the most important questions to train GPT and support developers in understanding and working with the BIA architecture correctly—without misinterpreting its logic or needing to study the theoretical background.

---

### [1] Execution flow & separation of power

**Q1. Is Interface just a token generator?**  
No. Interface does not generate tokens from the outside. It is a subclass of `MemoryAccess`, contains logic but does not execute it on its own. Only `Wrapper` (as a friend) has permission to call it, and only if `checkToken()` returns `true`.

**Q2. Does Wrapper directly call the module?**  
No. Wrapper only calls the module if Interface approves via `checkToken()`. Wrapper doesn't generate tokens; it just passes action labels and data.

**Q3. What is the execution flow?**  
From user/system → Interface → Wrapper → if approved → call `module->execute()`.

---

### [2] Module behavior and execution control

**Q4. Why does `ModuleBase` only have one virtual `execute()` function?**  
Because BIA validates based on action labels, not on functions. Using a single `execute()` function allows Wrapper to verify read/write rights.

**Q5. Can a module read and write at the same time?**  
No. Each module must inherit from either `ReadModule` or `WriteModule`. No hybrid modules allowed. Wrapper rejects mismatched labels.

**Q6. Where is the access type (read/write) defined?**  
Inside each module (`ReadModule`, `WriteModule`), not in Wrapper. Wrapper only compares the incoming label with the module's predefined access type.

---

### [3] RAM access & memory control

**Q7. Why split read and write modules?**  
Because RAM fundamentally only has two access directions: read and write. If both are uncontrolled, the sandbox breaks.

**Q8. Who allocates actual RAM?**  
`MemoryAccess` is the only class allowed to allocate memory. `GenSlot` holds type info but no pointers.

**Q9. Can two modules overwrite each other’s memory?**  
No. Each Interface call initiates a new session with separate RAM space. Modules never share RAM unless explicitly permitted.

---

### [4] Compile-time sandboxing

**Q10. Can macros override `private`, `friend`, etc.?**  
No. `#pragma poison` in core headers blocks unsafe macro overrides. Compiler will throw an error.

**Q11. Can I use `-Ofast` or `-DNDEBUG` flags?**  
No. BIA headers detect dangerous flags like `__FAST_MATH__` and `NDEBUG`, and stop the build.

---

### [5] Gen model and memory structure

**Q12. Why must `struct gen` be inside `GenSlot`?**  
If placed outside, pointers may leak. `GenSlot` only describes structure, holds no raw pointers. All memory access must go through Interface.

**Q13. Who is allowed to allocate memory?**  
Only Interface (via `MemoryAccess`) can allocate. Wrapper provides data and action label, Interface validates before allowing execution.

---

### [6] Behavior labels & validation logic

**Q14. Does Wrapper create tokens?**  
No. Wrapper only passes a string action label and related data.

**Q15. Are labels required to follow a strict format?**  
Not required, but the format `"action:target"` like `"submit:exercise"` is recommended for clarity.

**Q16. Can labels be generated externally?**  
No. Interface is the only entity allowed to validate labels.

---

### [7] Module logic sandboxing

**Q17. Can a module call another module?**  
No. Every behavior must go through Wrapper. Modules cannot invoke other modules.

**Q18. Can a module cache memory or pointers between sessions?**  
No. Memory is session-specific. Caching across sessions is not allowed.

**Q19. Can modules expand their logic internally?**  
Yes. As long as access rules are respected, modules can have internal variables and processing inside `execute()`.

---

### [8] Natural polymorphism

**Q20. Why is BIA considered closest to biological polymorphism?**  
Each module accesses a fixed RAM field (like gene expression), but its internal logic can vary freely. This mirrors how one gene can express different proteins in different contexts.

**Q21. Can I override methods in BIA?**  
No. All modules inherit `ModuleBase`, which exposes only one `execute()` function. No override or multi-inheritance.

---

### [9] Behavior expansion

**Q22. How do I add a new behavior?**  
Create a new class inheriting from `ReadModule` or `WriteModule`, implement `execute()`, assign a clear label, and register via Wrapper.

**Q23. Does each behavior need its own module?**  
Yes. Each behavior must have a dedicated module for clear validation and separation of logic.

---

### [10] Testing & real-world dev

**Q24. Can I test modules in isolation?**  
Yes. Mock a session using `GenSlot`, pass through Wrapper, and call `execute()` after validation.

**Q25. Can I log called behavior labels?**  
Yes. Wrapper or Interface can log incoming labels for flow auditing.

**Q26. I'm new and don’t understand the system. How do I start?**  
Open GPT and ask: “I want to build module `XYZ` in BIA. What label, data, and validation do I need?” GPT will guide step-by-step.

---

*End of FAQ (Phase 1) – Sufficient to train GPT for accurate real-world developer interaction.*