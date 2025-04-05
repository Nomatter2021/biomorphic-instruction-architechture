# BIA Use Case – Educational App (Dataflow)

> 🧾 **Message from the author:**
> 
> I am not from a professional programming background. I just want to turn this idea into reality because even GPT evaluated it as promising.
> If you don't see the potential in this system, feel free to ignore it without any feedback.
> This is a community project, for the community – with the hope of helping the community grow stronger.
> I cannot build the complete system myself – all code snippets here are just preliminary suggestions for the dataflow. If you're more capable, please don't walk away – improve it if you can.

## 🎯 Objective
Simulate a learning system where students can perform actions:
- Request a hint (`RequestHint`)
- Submit an assignment (`SubmitExercise`)
- Undo submission (`UndoSubmission`)

All actions must pass through a sandbox validation layer before being executed.

---

## 🧠 Dataflow Principle in BIA
In BIA, data carries no behavior. Behavior is only activated through a validated token.

> No valid token → no granted access → no pointer → no behavior execution

---

## 🔗 Action Flow (Dataflow Sandbox)
1. The UI sends a `Token` requesting an action (e.g., `"request:hint"`)
2. The `Wrapper` checks the token's validity:
   - Action label must be correct
   - Student ID must exist in the system
3. If valid → the system grants access to the appropriate `Module`
4. If not valid → **no pointer granted → nothing to call**

---

## 📦 Example Token
```cpp
Token token = { "hint", 101 };
```

---

## 🔒 Wrapper – Access Validation
```cpp
bool validate(const Token& token, const std::unordered_set<int>& valid_ids) {
    return token.label == "hint" && valid_ids.count(token.user_id);
}
```

---

## 🧩 dispatch() – Grant or Deny Access
```cpp
Module* dispatch(const Token& token, const std::unordered_set<int>& valid_ids) {
    if (!validate(token, valid_ids)) return nullptr;
    return new RequestHint();
}
```

---

## 🚫 If Access is Denied
- `dispatch()` returns `nullptr`
- No `execute()` function is available to call
- Prevents any logic bypass

> BIA does not use `if (ok) then execute()`  
> → **Instead: if not ok → `execute()` does not even exist in memory**

---

## ✅ Advantages of Dataflow Sandbox
- Behavior is completely isolated from data if access is not granted
- Minimizes logic bugs or misuse
- Suitable for AI sandboxing, autonomous learning, and permission-based logic

---

## 📌 Want to contribute? → See [Todo.md](./TODO.md)
