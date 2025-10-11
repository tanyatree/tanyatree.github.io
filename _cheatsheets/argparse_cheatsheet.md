---
title: "Argparse Cheat Sheet"
description: "A concise reference for building command-line interfaces in Python using the standard library argparse."
tags: [python]
---

# 🧰 Argparse Cheat Sheet

A concise reference for building command-line interfaces in Python using the standard library `argparse`. ⚡

---

## ⚙️ Basic Example

> 💡 Example usage

```python
import argparse

parser = argparse.ArgumentParser(
    description="Example CLI tool.",
    epilog="Example: python merge_pdfs.py pdfs/ -o merged.pdf"
)

parser.add_argument("input", help="Input file or folder")
parser.add_argument("-o", "--output", help="Output file name")
args = parser.parse_args()
```

---

## 🧩 add_argument() Parameters

Each call to `add_argument()` defines one command-line argument.
Below are the most commonly used options.

| Parameter | Type | Description | Example |
|------------|------|-------------|----------|
| **name or flags** | `str` | The argument’s name. Positional arguments use plain names (`"file"`), optional arguments use dashes (`"-v"`, `"--verbose"`). | `"folder"` or `"-v", "--verbose"` |
| **action** | `str` / class | Specifies how to handle the argument. | `"store"`, `"store_true"`, `"append"`, `"count"` |
| **nargs** | `int` / `str` | Number of command-line arguments to consume. | `1`, `"?"`, `"*"`, `"+"` |
| **const** | any | A constant value used with certain actions (e.g., `store_const`). | `const=42` |
| **default** | any | Value used if the argument is not provided. | `default="out.pdf"` |
| **type** | callable | Function used to convert the argument type. | `type=int`, `type=Path` |
| **choices** | list / tuple | Restrict accepted values. | `choices=["low", "medium", "high"]` |
| **required** | `bool` | Force the argument to be provided (for optional args). | `required=True` |
| **help** | `str` | Text shown in `--help` output. | `"Path to the PDF folder"` |
| **metavar** | `str` | Custom name displayed in help text. | `metavar="FILE"` |
| **dest** | `str` | Variable name used in the parsed result. | `dest="output_path"` |
| **version** | `str` | Used with `action="version"` to show a version number. | `version="1.0.0"` |

---

## 🎯 Common `action` Values

| Action | Description | Example |
|---------|--------------|----------|
| `"store"` *(default)* | Store the next value. | `--output out.pdf → args.output == "out.pdf"` |
| `"store_true"` | Store `True` if flag present, else `False`. | `--verbose → args.verbose == True` |
| `"store_false"` | Store `False` if flag present, else `True`. | `--no-cache → args.cache == False` |
| `"append"` | Append multiple values to a list. | `-t tag1 -t tag2 → args.t == ["tag1", "tag2"]` |
| `"count"` | Count how many times the flag appears. | `-v -v → args.v == 2` |
| `"version"` | Print version info and exit. | `--version → prints "1.0.0"` |

---

## 🔢 nargs (Number of Arguments)

| Value | Meaning | Example |
|--------|----------|----------|
| `1` *(or number)* | Exactly that many values. | `--coords 1 2 3` |
| `"?"` | Zero or one value. | `--level info` or just `--level` |
| `"*"` | Zero or more values (returns list). | `--files a.pdf b.pdf` |
| `"+"` | One or more values (returns list). | `--tags tag1 tag2` |
| `argparse.REMAINDER` | Collect all remaining args. | `-- passthrough args` |

---

## 🧱 Argument Types

| Type | Behavior |
|------|-----------|
| `str` *(default)* | Leave as string. |
| `int`, `float`, `bool` | Auto-convert to numeric types. |
| `pathlib.Path` | Convert to a `Path` object. |
| Custom function | Define your own type conversion. |

> 💬 Tip: Use custom functions for argument validation and conversion.

Example:

```python
def positive_int(value):
    x = int(value)
    if x <= 0:
        raise argparse.ArgumentTypeError("Must be positive")
    return x

parser.add_argument("--limit", type=positive_int)
```

---

## 🪄 Special Methods

| Method | Description |
|---------|-------------|
| `parser.print_help()` | Show the full help text. |
| `parser.print_usage()` | Show only usage summary. |
| `parser.parse_args()` | Parse arguments into `args` object. |
| `parser.parse_known_args()` | Parse known args, ignore unknown. |

---

## 🖋️ Formatter Classes

Control how the help text is displayed.

| Formatter | Description |
|------------|--------------|
| `argparse.HelpFormatter` | Default formatter. |
| `argparse.RawTextHelpFormatter` | Preserve newlines in text. |
| `argparse.RawDescriptionHelpFormatter` | Preserve formatting in description/epilog. |
| `argparse.ArgumentDefaultsHelpFormatter` | Show default values in help output. |

Example:

```python
parser = argparse.ArgumentParser(
    description="Merge PDFs.",
    formatter_class=argparse.ArgumentDefaultsHelpFormatter
)
```

---

## 💡 Full Example

```python
import argparse
from pathlib import Path

parser = argparse.ArgumentParser(description="Merge all PDFs in a folder.")

parser.add_argument(
    "folder",
    type=Path,
    help="Folder containing PDF files"
)

parser.add_argument(
    "-o", "--output",
    dest="output_path",
    default="merged.pdf",
    help="Output file name",
    metavar="FILE"
)

parser.add_argument(
    "-v", "--verbose",
    action="store_true",
    help="Enable detailed output"
)

parser.add_argument(
    "--version",
    action="version",
    version="merge-pdfs 1.0.0"
)

args = parser.parse_args()
```

---

## 🚀 Quick Reference

> 🧠 Handy for remembering common patterns!

| Task | Use |
|------|-----|
| Optional flag with value | `-o`, `--output` |
| Boolean flag | `action="store_true"` |
| Default value | `default=...` |
| Custom variable name | `dest=...` |
| Restrict values | `choices=[...]` |
| Multiple values | `nargs="+"` |
| Optional argument | `nargs="?"` |
| Custom label in help | `metavar="NAME"` |
| Required flag | `required=True` |
| Convert type | `type=int` |
