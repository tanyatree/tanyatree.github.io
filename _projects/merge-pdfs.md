---
title: "Merge PDFs"
description: "A tiny Python CLI tool that merges all PDFs in a folder alphabetically into a single file."
tags: [python, cli, automation]
date: 2025-10-10
summary: "A minimalist Python CLI that merges all PDFs in a folder alphabetically into a single file."
links:
  code: "https://github.com/tanyatree/merge-pdfs"
stack: "Python CLI"
---

# 🗂️ Merge PDFs — Tiny Command‑Line PDF Merger

A minimalist Python CLI that merges all PDFs in a folder **alphabetically** into a single file.
Built for speed and simplicity — no drag‑and‑drop, no bloat, just one clean command. Thanks to my husband for needing this tool and giving me the idea!

```bash
merge-pdfs ~/Documents/invoices -o combined.pdf
```

## ✨ Features

- 🧩 Works both as a **stand‑alone script** or **CLI tool** (`pip install -e .`)
- 📁 Automatically scans and merges `.pdf` files alphabetically
- 🔍 Supports **dry‑run**, **verbose output**, and **custom output paths**
- ⚡ Perfect for automating reports, invoices, or scanned documents

## 🧠 Why I Made It

I often needed to merge PDFs quickly without using a heavy app or dragging files into a UI.
This script does it in one command — fast, portable, and under my control.

> 🛠️ My first experiment building a CLI with `argparse` — small, fast, and practical!

## 🚀 Quick Start

```bash
# Merge all PDFs in the current folder
merge-pdfs

# Merge from a specific folder with custom output
merge-pdfs ~/Documents/reports -o summary.pdf -v
```

---
