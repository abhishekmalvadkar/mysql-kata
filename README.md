# 🧠 SQL Kata Practice

Welcome to my personal SQL Kata collection.
This repository is designed for daily SQL problem-solving practice, documenting each kata using a clean, searchable MkDocs site with automatic GitHub Pages deployment.

> Each kata includes a problem description, schema, test data, final SQL solution, and key learnings.

---

## 🚀 Live Site

📘 **View the MkDocs Site:**
[https://abhishekmalvadkar.github.io/mysqk-kata](https://abhishekmalvadkar.github.io/mysql-kata)

---

## 📌 Project Structure

```
mysql-kata/
├── .github/workflows/deploy-mkdocs.yml     # GitHub Action for Pages
├── mkdocs.yml                              # MkDocs configuration
├── docs/
│   ├── index.md                            # Home page with links to katas
│   └── katas/
│       └── customer-orders-overview/
│           ├── problem.md                 # Problem description with schema & expected output
│           ├── test-data.md              # DDL & DML to reproduce scenario
│           ├── solution.md               # Final SQL query
│           └── lesson-learned.md         # Concepts & techniques learned
└── README.md
```

---

## ✅ Features

* 🧠 Focused, real-world SQL katas
* 🧪 Sample data for every problem
* 📘 Clean MkDocs-based documentation
* 🔍 Full-text search across all katas
* 🔄 Auto-deploy to GitHub Pages on push to `develop`

---

## 🧱 Kata Format

Each kata is broken down into four Markdown files:

* `problem.md` – Problem statement, schema, expected output
* `test-data.md` – Table creation + inserts
* `solution.md` – Final SQL query
* `lesson-learned.md` – Key SQL concepts used in this problem

---

## 🧪 Local Development

Install MkDocs Material:

```bash
pip install mkdocs-material
```

Serve the docs locally:

```bash
mkdocs serve
```

Access at: [http://localhost:8000](http://localhost:8000)

---

## 🚀 Deployment (GitHub Pages)

MkDocs is deployed automatically using GitHub Actions.
On every push to the `develop` branch, your site is rebuilt and published to GitHub Pages.

To enable GitHub Pages:

* Go to **Settings → Pages**
* Set `Source: Deploy from a GitHub Actions workflow`

---

## ✍️ Contributing

This is a personal learning space, but feel free to:

* Fork the repo
* Submit PRs with improvements or additional katas
* Share alternate solutions or notes

---

## 👨‍💻 Author

Made with ❤️ by [@abhishekmalvadkar](https://github.com/abhishekmalvadkar)
