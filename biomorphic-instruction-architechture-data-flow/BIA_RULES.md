# BIA_RULES.md

## BioMorphic Instruction Architecture – Core System Rules

---

### **Data Flow Layer**

**1. Struct Gen**  
Defines field names and types only – no value assignment, no access control.

**2. GenSlot**  
Struct Gen may reside inside or outside GenSlot – it's just a modeling style, not a philosophical constraint.

**3. MemoryAccess**  
Grants isolated memory to each module.  
- Each module has its own RAM  
- Data can only be accessed through dedicated memory interface  
- No module can access another module’s memory

---

### **Control Flow Layer**

**4. Interface**  
- The only place allowed to generate tokens  
- Contains protected verification functions  
- Does **not** decide whether behavior is accepted or not  
→ Token acts as a compile-time law – its structure determines strictness

**5. Token**  
- Must never be passed outside of Interface  
- No copying, cloning, or parameter passing  
→ A token is a gene for behavior – invalid gene = no expression

**6. Wrapper**  
- Decides whether a behavior is accepted or rejected  
- Only has two states: pass or reject  
→ No ambiguity – behavior either lives or doesn't

**7. ReadModule & WriteModule**  
- Classifies behavior as read or write  
- Passes that behavior to Wrapper for evaluation

---

### **Behavior Flow Layer**

**8. ModuleBase**  
- Contains exactly two virtual functions: getValue(), setValue()  
- Can be overridden, but memory is still sandboxed  
→ Override is allowed – but outside access is not

**9. Module**  
- Final subclass that performs execution  
- May contain public functions and private logic  
→ All operations are internal – no cross-module memory access

---

### **Rule 10 – Immutable Across All Flows**

**Only pointers are allowed to be passed. Never pass values.**  
- Prevents side effects  
- Preserves identity and access control  
- Avoids unauthorized behavior construction

→ You do not own the data.  
→ You may only point to it – if you are permitted to.

---

### **Final Law – Absolute Invariant**

**No class must be allowed to perform behavior beyond its tier or authority.**  
If a module can simultaneously generate token, verify behavior, and override logic unchecked  
→ **The entire system collapses.**

---

### **Summary**

> “Every behavior is a conditional expression.  
Each module owns its own memory.  
No behavior crosses layers.  
No token – no behavior.  
No verification – no existence.”