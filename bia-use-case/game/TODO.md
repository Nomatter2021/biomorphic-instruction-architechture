# 📌 TODO – Contribution Suggestions for Use Case: Game Core (BIA Combat Sandbox)

## 🧾 Note from the author
> This is not a game engine project – it's a sandbox for combat logic.  
> I cannot build the entire system, but I hope skilled developers can bring this vision to life.  
> All code is illustrative – not production-ready.

---

## ✅ Suggested Combat Modules
Help create modular behavior by writing the following classes:

- [ ] `AttackBasic` – deal damage based on `atk - def`
- [ ] `BuffDef` – increase DEF for 3 turns
- [ ] `ReflectDamage` – reflect 50% damage when attacked
- [ ] `DebuffAtk` – reduce enemy ATK for 2 turns

Each module inherits from `Module` and implements `execute()`.

---

## ✅ Wrapper Conditions
- [ ] `AttackBasic`: only if not stunned
- [ ] `BuffDef`: only if not silenced
- [ ] `ReflectDamage`: only if HP < 50%
- [ ] `DebuffAtk`: only if target is not already debuffed

---

## ✅ Dispatch Logic
- [ ] Implement `dispatch(token, state)` that:
  - Returns pointer to module if valid
  - Otherwise → return `nullptr`

```cpp
Module* dispatch(const Token& token, const CharacterStat& state);
```

---

## ✅ Minimal Integration to Test
- [ ] Write a `main()` that:
  - Creates a `Token`, e.g., `"attack:basic"`
  - Runs `dispatch()`
  - Calls `execute()` if allowed
  - Prints combat result

---

## 🔐 Sandbox Rules
> - Invalid token → no permission → no pointer → no module  
> - No `if (validate) then execute()` pattern  
> - If invalid → `execute()` does not exist in memory

---

## 🙌 One module or one validation is already valuable – feel free to contribute!
