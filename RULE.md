---
description: "Require constructors to fully acquire resources and establish invariants"
alwaysApply: true
---

# RAII Rule (Resource Acquisition Is Initialization)

RAII is a language‑agnostic construction rule, originating in C++: a class acquires all of its resources and establishes all invariants in its constructor (or equivalent initializer). After construction, the object is fully usable and safe.

No separate “init” phase, no hidden ordering requirements, and no methods that must be called before the object works. Accessors and normal methods should be simple, assuming the object is already valid.

## Guidelines

- Constructors/initializers do all resource acquisition and setup needed for correct use.
- If construction cannot fully succeed, fail during construction rather than leaving a half‑initialized object.
- After construction, the object’s invariants always hold; methods should not need to re‑check or “finish setup.”
- Cleanup/release happens automatically with object lifetime (destructor/finalizer/`with`/`defer`), not via a separate public `close()` that callers may forget.
- Avoid APIs that require calling `start()`, `open()`, or `init()` before use unless the type makes that state explicit.

## Examples

C++ style:

```cpp
class FileReader {
public:
  explicit FileReader(const std::string& path)
      : file_(path) {           // acquire resource here
    if (!file_) throw std::runtime_error("open failed");
  }

  std::string read_all();       // assumes file_ is valid

private:
  std::ifstream file_;
};
```

Python analogue (context manager owns lifetime):

```py
class FileReader:
    def __init__(self, path: str):
        self._file = open(path)  # acquire in constructor

    def read_all(self) -> str:
        return self._file.read()

    def __enter__(self):
        return self

    def __exit__(self, exc_type, exc, tb):
        self._file.close()
```
