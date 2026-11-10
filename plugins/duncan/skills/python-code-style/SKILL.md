---
name: python-code-style
description: Code style guidelines for Python. Use when writing, editing, or reviewing Python code.
---

# Python code style

Follow every guideline below.

- **Write.** Follow each guideline in new code.
- **Edit.** Apply each guideline to the code that the change adds or changes. Leave code that the change does not touch, so the diff stays small.
- **Review.** Report each place that breaks a guideline. Give the guideline, the name, and the file and line.

## Prefix private functions and methods with `_`

A module-level function is private when no code outside its module calls it.
A method is private when no code outside its class and subclasses calls it.
Give each private function or method a single leading underscore: `_parse_row`, not `parse_row`.

The underscore marks an internal detail.
Subclasses may call and override a `_method`.
Python does not enforce it, so other code can still call `_parse_row`.
Its only effect is that `from module import *` skips the name when the module has no `__all__`.

### Find the callers

Search the whole repository first, because templates, config, and strings can refer to a function by name:

```sh
git grep -nw NAME
```

Calls from tests do not make a function public.
A test may call `_parse_row` directly.

### Keep the public name

Do not add an underscore to a name that other code finds by name:

- Dunder methods, such as `__init__` and `__eq__`.
- An override of a public method from a parent class.
- A framework hook, such as Django's `save` and `get_queryset`, unittest's `setUp`, or pytest's `test_*` functions and fixtures.
- A name in `__all__`, an entry point in `pyproject.toml` or `setup.cfg`, or a name that code looks up with `getattr` or a string, such as a Celery task or a URL route.
- Public API of a library that other packages import.

A function nested inside another function is already local.
It does not need an underscore.

### Update every caller

To rename a function, update every caller, including `mock.patch("pkg.module.name")` strings.

## Use `__` only to stop subclass name clashes

Inside a class body, Python mangles a name that starts with two underscores and does not end with two.
In class `Parser`, `__check` becomes `_Parser__check`.
A subclass that defines its own `__check` gets `_Child__check`, so it cannot override the parent's method by accident.

Mangling is not access control.
Code outside the class can still call `parser._Parser__check()`.

- Use one underscore for a private method by default.
- Use two underscores only when a subclass may define the same name and the parent must keep calling its own version, such as in a base class or mixin that other code extends.
- Do not use two underscores for a module-level function. Python mangles names only inside a class body, and a call to `__helper()` from a method looks up `_ClassName__helper` and raises `NameError`.
- To patch or read a mangled name from outside the class, use the mangled form, such as `mock.patch.object(Parser, "_Parser__check")`.
