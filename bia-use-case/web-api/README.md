# BIA Use Case – Web API (Sandboxed Access Control)

> 🧾 **Message from the author:**
>
> I don’t know much about web development or how APIs work. But I believe one thing: if every action in a system must pass validation before execution, that system becomes safer, easier to maintain, and more scalable.
>
> I built this model not because I understand the technicalities, but because I believe this structure is worth trying. If you're an experienced backend or API developer, I humbly ask you to read this concept at least once. If it makes sense, please help me build even a small module.
>
> If you don’t find this model useful, feel free to ignore it. This is a non-profit effort meant to benefit the community.

---

## 🎯 Objective
Design an API system that uses BIA-style behavior sandboxing:
- API requests are wrapped into `Token`s
- `Wrapper` validates the token based on user roles and state
- If valid → pointer to behavior module is granted
- If invalid → no pointer → no logic can run

---

## 🧠 BIA Philosophy in API Context
- API doesn’t call logic directly
- API only sends a `Token` representing a request
- Logic (module) exists only if permission is granted
- No valid token → no executable behavior

> **No token → no pointer → no logic → no execution**

---

## 🔗 Behavior Processing Flow
1. User calls `POST /user/export`
2. Server generates a token: `{ "label": "export:user", "user_id": 42 }`
3. Wrapper checks:
   - Is this user an admin?
   - Is the account not locked?
4. If passed → return `ExportUserData` module
5. If rejected → return `nullptr` → nothing gets executed

---

## 📦 Example Token
```cpp
Token token = { "export:user", 42 };
```

---

## 🔒 Wrapper
```cpp
bool validate(const Token& token, const UserState& user) {
    return token.label == "export:user" && user.role == "admin" && !user.locked;
}
```

---

## 🧩 dispatch()
```cpp
Module* dispatch(const Token& token, const UserState& user) {
    if (!validate(token, user)) return nullptr;
    return new ExportUserData();
}
```

---

## ✅ Why BIA for API logic?
- Separates request, validation, and execution clearly
- Eliminates scattered permission checks inside controllers
- Makes each behavior testable in isolation
- Easy to extend without breaking existing routes

---

## 📌 Want to help? → See [TODO_web_api.md](./TODO_web_api.md)
