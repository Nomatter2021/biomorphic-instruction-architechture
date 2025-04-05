# BIA Use Case – Game Core (Combat Logic)

> 🧾 **Message from the author:**
> 
> I am not from the game development industry. I just want to turn this idea into reality, because even GPT evaluated it as promising.  
> If you don't see potential in this system, you're free to ignore it and provide no feedback.  
> This is a community-driven project, for the community – aiming only to help it grow further.  
> I am not capable of writing the entire system – every code snippet here is just a sketch of the dataflow logic. If you're more skilled, please don't walk away – improve it if you can.  
> I believe that in games, combat logic can be modularized into sandboxed behaviors, validated by tokens. If sandboxed AI can learn this structure, it could open up many new paths for strategy games, PvP, and autonomous combat systems.

---

## 🎯 Objective
Design an RPG-style combat system:
- Each character has fixed stats (atk, def, spd, hp...)
- Combat actions like attack, buff, counter are modules
- Each action must be validated via `Wrapper` using a `Token` before execution

---

## 🧠 Dataflow Philosophy in Combat
- Stats do not directly trigger behavior
- Behavior is injected via token, verified → granted permission
- If the token is invalid → behavior does not exist

> **No valid token → no permission → no pointer to module → no behavior to call**

---

## 🔗 Action Processing Flow
1. Token contains: action label + source ID + target ID
2. Wrapper checks:
   - Is the actor stunned?
   - Are conditions met (e.g., HP < 50 for reflect)?
3. If valid → dispatch module → call `execute()`
4. If not → dispatch returns nullptr → no behavior triggered

---

## 🧩 Example Token
```cpp
Token token = { "attack:basic", source_id, target_id };
```

## 🔒 Example Wrapper
```cpp
bool validate(const Token& token, const CharacterStat& state) {
    if (token.label == "attack:basic") return !state.stunned;
    if (token.label == "reflect") return state.hp < 50;
    return false;
}
```

## 🧩 dispatch()
```cpp
Module* dispatch(const Token& token, const CharacterStat& state) {
    if (!validate(token, state)) return nullptr;
    if (token.label == "attack:basic") return new AttackBasic();
    return nullptr;
}
```

---

## ✅ Benefits of BIA for combat logic
- Easy to add new skills – just implement a module
- State-based validation like stun/debuff supported
- Ideal for AI/PvP logic – testable and modular
- Clear separation between behavior and data

---

## 📌 Want to contribute? → See [TODO_game_core.md](./TODO_game_core.md)
