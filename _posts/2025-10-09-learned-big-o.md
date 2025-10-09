---
layout: post
title: "What I Learned About Big O (and why I wish I knew this sooner)"
date: 2025-10-09 10:00:00 -0300
tags: [django, python, bigO, learning]
---

After more than a decade of coding, I finally had one of those rare “click” moments where everything just made sense. It hit me like a lightning bolt—I realized what `O(n)` and `O(1)` *really* mean for my code. It felt like a veil was lifted after years of struggling with concepts that seemed abstract and distant. This discovery didn’t just improve my coding; it helped me fight off imposter syndrome and rekindled my curiosity and excitement for learning.

Sometimes, despite years of experience, it’s easy to feel stuck or doubt your skills. But moments like this remind me that growth is always possible, no matter how long you've been coding. It’s never too late to gain new insights that transform how you approach problems.

Here’s what I learned. Let’s take a look at an example from a project I’ve been working on, utopia-crm. We have a model called `Contact`, and I needed to match data from a CSV file against this model.

I used to iterate through the CSV and query the database for every row — which is effectively **O(n × query_time)**. After learning about dictionary lookups and preloading contacts by `id_document`, I switched to *one* bulk query + **O(1)** lookups. A script that took minutes now runs in under a second. ⚡

### **Snippet:**

```python
contacts = Contact.objects.filter(id_document__in=ci_list)
contact_lookup = {c.id_document: c for c in contacts}
contact = contact_lookup.get(ci_from_csv)  # O(1)
```

### **Takeaways**

- Fetch once, reuse many times.
- Build lookup maps (`dict`) for joins in memory.
- Control ORM queries with `select_related` / `prefetch_related`.
