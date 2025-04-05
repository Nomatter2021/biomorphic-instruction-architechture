# 📌 TODO – Contribution Suggestions for Use Case: Web API (BIA Sandbox)

## 🧾 Note from the author
> I don’t really understand how APIs are written, but I believe permission logic should be separated and sandboxed.  
> The list below is not technical specs – they are real-world examples I think can be modeled with BIA.  
> If you're a backend dev, maybe you can help turn these into real modules.

---

## ✅ Realistic behaviors that need sandboxing

### 1. `ExportUserData`
- [ ] Only if user is admin
- [ ] User account is not locked

### 2. `UpdateEmail`
- [ ] User is updating their own account
- [ ] Email format should be valid (can be checked in module)

### 3. `DeletePost`
- [ ] Only post author or admin can delete
- [ ] Post must not be locked

### 4. `CreatePost`
- [ ] User must not be banned
- [ ] User must have verified their email

---

## ✅ Wrapper design suggestion
```cpp
bool validate(const Token& token, const UserState& user, const ResourceState& resource);
```
- `user` includes: id, role, locked, verified, banned...
- `resource` includes: post.owner_id, locked...

---

## ✅ Dispatch behavior
```cpp
Module* dispatch(const Token& token, const UserState& user, const ResourceState& res) {
    if (!validate(token, user, res)) return nullptr;
    if (token.label == "delete:post") return new DeletePost();
    return nullptr;
}
```

---

## ✅ Simple test idea
- Create a token like `{ "label": "delete:post", "user_id": 5, "post_id": 8 }`
- Build dummy user + post state
- Call dispatch → if module is granted, run `execute()`

---

## 🙌 Just writing one small module or validation helps a lot – feel free to join!
