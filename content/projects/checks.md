+++
title = "Asset Checks"
date = 2023-10-10
draft = false
description = "A library for validating assets or shots before they are published."
template = "project_page.html"

[taxonomies]
categories = ["Programming"]
tags = ["programming", "python", "rust", "c", "pipeline"]

[extra]
url = "https://github.com/scott-wilson/checks"
+++

Overview
--------

This framework is designed to provide a system to write checks for studio work.
This includes validating assets (rigs, geometry, surfacing, etc), shots
(animation, lighting, simulation, etc), and whatever a studio would need to
validate. It provides a simple API with a rich result type that provides all the
information to let a user know why a check failed and what they need to do to
fix it. It also supports fixing issues if the issue can be fixed by the
computer.

Examples
--------

### A simple check

```python
import pychecks


class MyCheck(pychecks.BaseCheck):
    def __init__(self, value: int) -> None:
        self.__value = value

    def title(self) -> str:
        return "My Check"

    def description(self) -> str:
        return "Check if value is 0"

    def check(self) -> pychecks.CheckResult[int]:
        if self.__value != 0:
            return pychecks.CheckResult.failed("Value is not 0", [pychecks.Item(self.__value)])

        return pychecks.CheckResult.passed("Value is 0", [pychecks.Item(self.__value)])
```

### Running the simple check

```python
import pychecks

fail_check = MyCheck(1)
pass_check = MyCheck(0)

fail_result = pychecks.run(fail_check)
pass_result = pychecks.run(pass_check)

assert fail_result.status().has_failed()
assert pass_result.status().has_passed()
```

### Auto fix

```python
import pychecks


class MyAutoFixCheck(pychecks.BaseCheck):
    def __init__(self, value: int) -> None:
        self.__value = value

    def title(self) -> str:
        return "My Check"

    def description(self) -> str:
        return "Check if value is 0"

    def auto_fix(self) -> None:
        self.__value = 0

    def check(self) -> pychecks.CheckResult[int]:
        if self.__value != 0:
            return pychecks.CheckResult.failed("Value is not 0", [pychecks.Item(self.__value)], can_fix=True)

        return pychecks.CheckResult.passed("Value is 0", [pychecks.Item(self.__value)])
```

### Running the check with auto fix

```python
import pychecks

check = MyCheck(1)

result = pychecks.run(fail_check)

assert result.status().has_failed()
assert result.can_fix()

# Auto fix will attempt to fix the issue, and then re-run the check to validate
# it passed.
result = pychecks.auto_fix(check)

assert result.status().has_passed()
```

Features
--------

- A Rust, C, and Python 3 API
- Automatically fixing issues
- Marking checks with whether they are skippable or not.
- Exposing the result of checks in a user interface.

Requirements
------------

- Make
- Rust: 1.66 or later (This is not the guaranteed minimum supported Rust
  version)
- Python: 3.7 or later
- Poetry
- Maturin

Install
-------

### For development

```bash
cd /path/to/checks/bindings/python

make build
```

Design
------

### Status

The status is a machine readable part of the result.

- `Pending`: The check has not been run yet. This is a useful status in user
  interfaces to let users know that the checks are ready to be run.
- `Skipped`: The check has been skipped due to previous checks that this one
  depends on failing.
- `Passed`: The test has passed.
- `Warning`: The test has found things that might be an issue. However, this
  can still be treated the same as `Passed`.
- `Failed`: The test has found an issue with the object. This can be treated as
  `Passed` if the result allows skipping this test.
- `SystemError`: An issue with the test has happened. Either functionality it
  depends on has an error, or there is an issue with the test or test runner.
  Assume that the result of the test is invalid, and never allow the test to
  pass.

### Item

The item is a wrapper around the cause of a result. For example, if an asset
must be named a certain way, and an object under the asset is named wrong, then
the result can return the offending object as an item. The item wrapper is only
important for user interfaces, because it forces all types to be sortable and
displayable. For example, a file object may not have any knowledge of the file
path that created the file object, but the item wrapper could be extended to
include the file path with the file object. The item also includes a hint that
can tell a user interface what the type represents. For example, if the type of
data in an item is a string, but the string represents a scene path, then the
user interface could select the scene objects when the items are selected in the
check UI.

### Result

The result type contains information about what is the status of the check, a
human readable description of the result, the items that caused the result,
whether the result is fixable or skippable, error information for `SystemError`s
and timing information for the check and auto-fix.

### Check

A check is a unit of validation. For example, a check could be validating that
the asset is named correctly, textures exist, all parameters are set to their
defaults, etc. It is recommended that a check will only check one thing at a
time. For example, if an asset needs to be named correctly, the textures need to
exist, and the parameters are all defaults, then these should be three separate
checks. However, there might be checks that will all have to do the same work in
order to do their work. For example, if there are checks to make sure that
textures are the correct resolution, and other checks to make sure the textures
are the right types (8 bit intergers, 32 bit floats, etc), then both set of
checks would need to open the files, and therefore validate that the files
exist. The solution to this issue is left up to the team implementing the
checks.

### Runner

#### Check Runner

The runner takes a check and produces the result. It is also responsible for
making sure the check is in a valid state (returning a system error if it is
not), and producing timing information about the check for diagnostics.

#### Auto-Fix Runner

The auto-fix runner is similar to the check runner, but it will run the
auto-fix method for the check. The auto-fix runner should be run after the check
runner, and only if the check runner's result says that the result supports
fixing. After it has attempted fixing the issue, it will run the check again and
return a result to validate that the fix actually fixed the issue or not.
