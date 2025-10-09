---
title: "Big O Cheat Sheet for Python + Django Developers"
description: "Learning about Big O notation helps you understand how your code scales as your data grows. This cheat sheet gives you real-world context for Python and Django performance."
tags: [python, django]
---

# 🧮 Big O Cheat Sheet for Python + Django Developers

Learning about **Big O notation** helps you understand *how your code scales* as your data grows.
This cheat sheet gives you real-world context for Python and Django performance. 🚀

---

## ⚙️ 1. Common Time Complexities

| **Notation** | **Meaning** | **Example** | **Typical Speed** |
|---------------|-------------|--------------|----------------|
| **O(1)** | Constant time | `dict[key]`, list indexing, model instance access | ⚡ Instant |
| **O(log n)** | Logarithmic | `bisect()` lookup, balanced tree search | 🚀 Very fast |
| **O(n)** | Linear | `for x in queryset`, `list.count()` | 🐎 Scales linearly |
| **O(n log n)** | Linearithmic | `sorted(list)`, Django `order_by()` | 📈 Efficient sort |
| **O(n²)** | Quadratic | Nested loops (loops inside loops) | 🐢 Slow at scale |
| **O(2ⁿ)** | Exponential | Brute-force combinations | ☠️ Impossible past small n |

---

## 📊 2. Python Data Structures

| **Operation** | **Structure** | **Big O** | **Notes** |
|----------------|----------------|-----------|------------|
| Lookup | `dict[key]` | **O(1)** | Hash table lookup (super fast) |
| Insert | `dict[key]=value` | **O(1)** | Average case; may rehash occasionally |
| Lookup | `list[i]` | **O(1)** | By index only |
| Search (value) | `x in list` | **O(n)** | Linear scan |
| Append | `list.append(x)` | **O(1)** | Amortized |
| Sort | `list.sort()` | **O(n log n)** | TimSort algorithm |
| Set membership | `x in set` | **O(1)** | Same as dict hash table |

---

## 🗃️ 3. Django ORM Operations

| **Operation** | **Big O (Conceptually)** | **Notes** |
|----------------|--------------------------|------------|
| `.get()` | **O(1)** (with index) | Single query by primary key |
| `.filter()` | **O(n)** (DB-side scan) | Can be O(log n) with proper index |
| `.all()` | **O(n)** | Loads every row |
| `.select_related()` | **O(1)** per related object | Avoids extra queries by JOIN |
| `.prefetch_related()` | **O(1)** per object | Two queries + cached lookups |
| Looping w/out prefetch | **O(n + queries)** | Each iteration triggers a query 😱 |
| `.bulk_create()` | **O(n)** single query | Huge speed win for many inserts |
| `.update()` | **O(n)** single query | Changes all rows at once |

---

## 💡 4. Practical Lessons

| ✅ Do This | 🚫 Don’t Do This | Why |
|-------------|----------------|-----|
| Load all needed objects once | Query inside a loop | Avoid N+1 queries |
| Build a dict for lookups | Use `filter()` for each row | Dict = O(1) lookup |
| Use `.select_related()` & `.prefetch_related()` | Access related fields without them | Save hundreds of queries |
| Use indexes on frequently filtered fields | Query unindexed columns often | Index turns O(n) → O(log n) |
| Use pagination (`.iterator()`) for huge datasets | Load 100 000 rows into memory | Prevent RAM explosions |

---

## ⚖️ 5. Memory Complexity

| **Structure** | **Memory Usage** |
|----------------|----------------|
| `dict` | ~ 80–100 bytes per entry + key/value |
| `list` | ~ 8–12 bytes per element ref |
| Django QuerySet | Lazy until evaluated; then each model ≈ a few KB |

---

## 🧭 6. Mental Model

```text
Slow → O(n²)
       ↑
       │
       │ nested loops, repeated queries
       │
Fast → O(n)
       ↑
       │
       │ single loop, prefetch, lookup maps
       │
Fastest → O(1)
```

---

## 🧠 Key Takeaways

- 🏎️ **Fewer DB round-trips = massive speed ups**
- 🧩 **Cache or index by keys** when joining data manually
- ⚙️ **Django ORM optimizations** often come down to controlling query count
- 🧮 **Think in growth**, not in milliseconds — that’s scalability thinking

---

### 🍎 Bonus: Using in Apple Notes

You can copy this Markdown into Apple Notes directly.
Use **Menlo** or **SF Mono** font for perfect table alignment and emoji rendering 💻✨
