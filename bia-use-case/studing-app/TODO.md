# 📌 TODO – Contribution Suggestions for Use Case: Educational App (BIA Dataflow)

## 🧾 Important Notes
> 🔸 **The original author only provides the philosophy and structure of the BIA system**  
> 🔸 **Does not have the skillset to write a complete system – only intermediate syntax level**  
> 🔸 **All code samples are generated with GPT's help to illustrate dataflow logic**  
> 🔸 **If you're a skilled developer, feel free to improve, refactor, or provide feedback**

---

## ✅ Where to start?

### 1. Write Action Modules (inherit from `Module`)
- [ ] `RequestHint` – decrease `hint_remaining` if permitted
- [ ] `SubmitExercise` – set `submission_status` to `"submitted"`
- [ ] `UndoSubmission` – reset `submission_status` to `"not_submitted"`

### 2. Wrapper – Validation Logic
- [ ] Write `validate()` function to check token:
  - Label equals `"hint"`
  - `user_id` is in the `valid_ids` list

### 3. Dispatch Logic
- [ ] Write `dispatch(token, valid_ids)` function:
  - Return a pointer to the module if `validate == true`
  - Otherwise → return `nullptr`

### 4. Simple Integration
- [ ] Create a `main()` function to simulate calling `"request:hint"`
- [ ] Print the result to verify sandbox (invalid token → no module runs)

---

## 🔐 Sandbox Principles
> - Invalid token → no permission → no pointer  
> - No pointer → no `execute()`  
> - No `if (validate)` logic → only `dispatch()` grants or denies access

---

## 🙌 Small contributions are still valuable
You don't need to understand the entire BIA model – contributing a single module or improving one validation step is already greatly appreciated!
