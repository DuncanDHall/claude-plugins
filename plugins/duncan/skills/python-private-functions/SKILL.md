---
name: python-private-functions
description: Prefix private Python functions and methods with a single underscore. Use when writing, editing, or reviewing Python code.
---

# Private Python functions

A module-level function is private when no code outside its module calls it.
A method is private when no code outside its class and subclasses calls it.
Give each private function or method a single leading underscore: `_parse_row`, not `parse_row`.

## Find the callers

Search the whole repository before you decide, because templates, config, and strings can refer to a function by name:

```sh
git grep -nw NAME
```

Calls from tests do not make a function public.
A test may call `_parse_row` directly.

## Keep the public name

Do not add an underscore to a name that other code finds by name:

- Dunder methods, such as `__init__` and `__eq__`.
- An override of a public method from a parent class.
- A framework hook, such as Django's `save` and `get_queryset`, unittest's `setUp`, or pytest's `test_*` functions and fixtures.
- A name in `__all__`, an entry point in `pyproject.toml` or `setup.cfg`, or a name that code looks up with `getattr` or a string, such as a Celery task or a URL route.
- Public API of a library that other packages import.

A function nested inside another function is already local.
It does not need an underscore.

## Use one underscore, not two

Two leading underscores make Python mangle the name to `_ClassName__name`.
Mangling breaks access from subclasses and `mock.patch` targets.
Use two underscores only to stop a subclass attribute from clashing with the parent's.

## Apply it

- **Write.** Give each new helper an underscore unless code outside its module or class calls it.
- **Edit.** Add the underscore to a private function or method that the change adds or changes. Leave functions that the change does not touch, so the diff stays small. When you rename, update every caller, including `mock.patch("pkg.module.name")` strings.
- **Review.** Report each private function or method that has no underscore. Give the name and where it is defined.
