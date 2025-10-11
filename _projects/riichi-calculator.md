---

title: "Riichi Mahjong Score Calculator"
description: "A stateless Django + HTMX web app (with a tiny JSON API) to
compute Riichi Mahjong payments for Ron/Tsumo, dealer, honba and optional
kiriage mangan."
tags: \[python, django, drf, htmx, webapp, riichi, mahjong]
date: 2024-03-31
summary: "Web calculator + API for Riichi Mahjong scoring (no DB, instant results)."
links:
code: "<https://github.com/tanyatree/riichi-calculator>"
live: "<https://riichi-calculator.onrender.com/>"
stack: "Django, Django REST Framework, HTMX, Simple.css, Gunicorn"

---

# 🀄 Riichi Mahjong Score Calculator

A **database‑free** score calculator for Riichi Mahjong built to be fast,
simple, and easy to use — both in the browser and via a tiny JSON API.

## ✨ Highlights

* 🧮 **Accurate scoring** for Ron/Tsumo based on **han** & **fu**
* 👑 **Dealer support** (東家) vs. non‑dealer payouts
* 🎲 **Honba** (本場) bonus handling
* 🔼 **Kiriage Mangan** toggle
* ⚡ **Instant UI** with HTMX (no full page reloads)
* 🔌 **/api/calculate** endpoint for programmatic use
* 📱 **Responsive** and lightweight (Simple.css)
* 🚫 **No database, no auth** — just compute and return

## 🧠 Why I built it

I wanted a practical playground to practice **Django REST Framework**,
**server‑rendered HTML + HTMX**, and **stateless app design**. A calculator
is perfect for keeping the logic clean and testable. This could have perfectly
been a client-side app, but I wanted to practice Django REST Framework.

## 🏗️ How it works (very short)

1. You provide inputs (han, fu, dealer?, honba, kiriage?).
2. Python computes the base score and applies Riichi limits (Mangan → Yakuman).
3. The view returns either an HTML fragment (HTMX) or JSON (API).

## 📡 Mini API

**POST** `/api/calculate/`

```json
{
  "han": 3,
  "fu": 30,
  "is_dealer": false,
  "kiriage": false,
  "honba": 0
}
```

**Response**

```json
{
  "ron": "3900",
  "tsumo": "2000/1000",
  "special_name": null
}
```

## 🚀 Quick start (local)

```bash
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
python manage.py runserver
# open http://127.0.0.1:8000/
```

## 🧱 Stack

**{{ page.stack }}**

## 🔗 Links

* 💻 Code: {{ page.links.code }}
* 🌐 Live: {{ page.links.live }}

***

Made with ❤️ + 🀄  by Tanya.
