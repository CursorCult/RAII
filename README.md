# Part of the [CursorCult](https://github.com/CursorCult)

# RAII

RAII (Resource Acquisition Is Initialization) enforces construction‑first correctness: objects acquire resources and establish invariants in the constructor so they are always ready‑to‑use afterward.

**Install**

```sh
pipx install cursorcult
cursorcult link RAII
```

Rule file format reference: https://cursor.com/docs/context/rules#rulemd-file-format

**When to use**

- You’re dealing with files, sockets, locks, handles, or other resources with lifetimes.
- You want to avoid “call X before Y” ordering bugs.
- You want methods to stay simple because invariants are guaranteed up front.

**What it enforces**

- Constructors/initializers perform all required acquisition and setup.
- Half‑initialized objects are forbidden; failures happen during construction.
- Cleanup is tied to object lifetime, not manual follow‑up calls.

**Credits**

- Developed by Will Wieselquist. Anyone can use it.

**Origins**

- RAII originated in C++ and was popularized by Bjarne Stroustrup; overview and history: https://www.stroustrup.com/bs_faq2.html#raii
