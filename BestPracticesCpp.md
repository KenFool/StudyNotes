# Applied Best Practices - _Picking a Project to Work On_ [(Jason Turner @ CppCon2018)][1]

<!-- move to the end of file when done --> [1]: https://www.youtube.com/watch?v=DHOlsEd0eDE "(Jason Turner)"

## Trailing Return Types

```cpp
noexecpt       //... trivial types do not throw exceptions (trivial types: trivially copy-, move-,move-construct- and copy-construct-able types) (it is a type-trait)
[[nodiscard]]  //... this throws an error if the return type will not be used
constexpr      //... Ähnlich zu const
auto           //... automatically deducts lvalue-type by evaluating rvalue-type
```

__Was ist ein Trailing Return Type?__
_Ein Trailing Return ist, wenn ich den Rückgabe-Typ der Funktion hinter die Funktion mit `->type` schreibe. Dies steigert die Lesbarkeit des Funktionsnamens_

```cpp
// Option A
[[nodiscard]] constexpr auto value() const noexecpt->Type;
[[nodiscard]] constexpr auto op()    const noexecpt->std::pair<bool, uint32_t>;
[[nodiscard]] constexpr auto type() const noexecpt->0p_Type;

// Option B
[[nodiscard]] constexpr Type value() const noexecpt;
[[nodiscard]] constexpr std::pair<bool, uint32_t> op() const noexecpt;  // it gets harder to find the actual name of the function:  op()
[[nodiscard]] constexpr 0p_Type type() const noexecpt;
```

Best Practice hier würde __Option A__ sein

## constexpr

- um __constexpr__ zu sein muss alles in header stehen
- constexpr selber verursacht keine langen compile-times
  - diese kommen von großen symbol-tafeln
    - lange symbol-namen und viele symbol-namen sind schuld daran
- constexpr kann undefined behaviour finden, weil es verboten undefined behaviour zu invoken
  - dadurch können Fehler gefunden werden, vor denen der Compiler nicht gewarnt hätte
