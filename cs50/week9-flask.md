# CS50 Week 9 — Flask

## Summary
Flask is a lightweight Python web framework that connects the backend (Python logic + SQL database) to the frontend (HTML/CSS/JS). Routes defined with `@app.route()` map URLs to Python functions that return HTML responses. Jinja2 templating allows variables and control flow to be embedded in HTML, and template inheritance via `layout.html` eliminates repeated boilerplate. The GET/POST distinction governs how forms transmit data — GET appends parameters to the URL while POST hides them in the request body. Data persistence is achieved by combining Flask routes with the cs50 SQL library using parameterized queries. Sessions store user-specific state as a server-side dictionary keyed to a browser cookie, enabling login/logout flows. AJAX lets JavaScript call Flask routes in the background with `fetch()`, updating the DOM without a full page reload.

---

## Notes

### Flask Setup & Project Structure

```
project/
├── app.py
├── requirements.txt
├── static/
│   ├── styles.css
│   └── script.js
└── templates/
    ├── layout.html
    ├── index.html
    └── login.html
```

```python
from flask import Flask, render_template, request, redirect, session, jsonify
app = Flask(__name__)
```

---

### Routes

```python
@app.route("/")
def index():
    return render_template("index.html")

@app.route("/about")
def about():
    return render_template("about.html")

# Dynamic route
@app.route("/user/<name>")
def user(name):
    return render_template("user.html", name=name)
```

- `@app.route()` maps a URL path to a Python function
- The function must return something — usually `render_template()`

---

### Jinja2 Templating

```html
<!-- Output variable -->
<h1>Hello, {{ name }}!</h1>

<!-- If statement -->
{% if user %}
  <p>Welcome, {{ user }}!</p>
{% else %}
  <p>Please log in.</p>
{% endif %}

<!-- For loop -->
{% for item in items %}
  <li>{{ item }}</li>
{% endfor %}
```

**Template inheritance:**
```html
<!-- layout.html -->
<!DOCTYPE html>
<html>
  <head><title>{% block title %}{% endblock %}</title></head>
  <body>
    {% block content %}{% endblock %}
  </body>
</html>

<!-- index.html -->
{% extends "layout.html" %}
{% block title %}Home{% endblock %}
{% block content %}
  <h1>Welcome!</h1>
{% endblock %}
```

---

### GET vs. POST & Forms

```python
@app.route("/login", methods=["GET", "POST"])
def login():
    if request.method == "POST":
        username = request.form.get("username")
        password = request.form.get("password")
        # validate...
        return redirect("/")
    return render_template("login.html")
```

- **GET** — data in URL: `request.args.get("q")`
- **POST** — data in body: `request.form.get("username")`
- Use GET for searches, POST for passwords/sensitive data
- **PRG pattern** — after POST, always `redirect()` to prevent form resubmission

---

### Flask + SQL

```python
from cs50 import SQL
db = SQL("sqlite:///app.db")

@app.route("/register", methods=["POST"])
def register():
    name = request.form.get("name")
    if not name:
        return "Name required", 400
    db.execute("INSERT INTO users (name) VALUES (?)", name)
    return redirect("/")

@app.route("/users")
def users():
    rows = db.execute("SELECT * FROM users")
    return render_template("users.html", users=rows)
```

- Always validate input **server-side** before touching the database
- Use `?` placeholders — never string concatenate user input into SQL

---

### Sessions & Cookies

```python
from flask import session
app.secret_key = "your-secret-key"  # required for sessions

# Login
@app.route("/login", methods=["POST"])
def login():
    session["user_id"] = 1  # store in session
    return redirect("/")

# Logout
@app.route("/logout")
def logout():
    session.clear()
    return redirect("/login")

# Protected route
@app.route("/dashboard")
def dashboard():
    if not session.get("user_id"):
        return redirect("/login")
    return render_template("dashboard.html")
```

- **Cookie** — small file stored in browser, sent with every request
- **Session** — server-side dictionary tied to a user's cookie
- `session["key"]` — store user-specific data across requests

---

### AJAX & JSON APIs

```python
# Flask — return JSON
@app.route("/api/search")
def search():
    q = request.args.get("q")
    results = db.execute("SELECT name FROM users WHERE name LIKE ?", f"%{q}%")
    return jsonify(results)
```

```javascript
// JavaScript — call Flask route without page reload
async function search() {
  let q = document.querySelector("input").value;
  let response = await fetch(`/api/search?q=${q}`);
  let data = await response.json();
  // update DOM with data
}
```

- **AJAX** — Asynchronous JavaScript And XML — fetch data without reloading
- `fetch()` with `async/await` — modern way to make HTTP requests from JS
- `jsonify()` — converts Python dict/list to JSON response

---

## Cue Column

| Term | Definition |
|---|---|
| **Flask** | Lightweight Python web framework |
| **Route** | URL path mapped to a Python function via `@app.route()` |
| **Jinja2** | Flask's templating engine — embeds Python logic in HTML |
| **Template inheritance** | Child templates extend a base `layout.html` to avoid repetition |
| **GET** | HTTP method that appends data to the URL — use for searches |
| **POST** | HTTP method that hides data in the request body — use for passwords |
| **PRG pattern** | Post-Redirect-Get — redirect after POST to prevent resubmission |
| **Session** | Server-side dictionary storing user state across requests |
| **Cookie** | Small browser file sent with every request to identify the user |
| **`jsonify()`** | Flask function that converts Python data to a JSON HTTP response |
| **AJAX** | Technique for fetching data in the background without page reload |
