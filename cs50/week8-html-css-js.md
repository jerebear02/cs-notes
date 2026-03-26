# CS50 Week 8 — HTML, CSS & JavaScript

## Summary
HTML (HyperText Markup Language) is the skeleton of every webpage, defining structure through nested tags. CSS (Cascading Style Sheets) controls the visual presentation — colors, fonts, layout, spacing. JavaScript is the programming language of the browser, enabling dynamic behavior and interactivity without page reloads. The DOM (Document Object Model) is the browser's in-memory representation of the HTML tree, which JavaScript can read and modify in real time. Event listeners attach JavaScript functions to user actions like clicks, keypresses, and form submissions. The Geolocation API and other browser APIs expose device capabilities to JavaScript. Together, HTML, CSS, and JavaScript form the complete frontend stack.

---

## Notes

### HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Page Title</title>
    <link rel="stylesheet" href="styles.css">
  </head>
  <body>
    <h1>Heading</h1>
    <p>Paragraph</p>
    <script src="script.js"></script>
  </body>
</html>
```

**Common tags:**
- `<h1>`–`<h6>` — headings
- `<p>` — paragraph
- `<a href="...">` — link
- `<img src="..." alt="...">` — image
- `<ul>` / `<ol>` / `<li>` — lists
- `<div>` — generic block container
- `<span>` — generic inline container
- `<form>`, `<input>`, `<button>` — forms

---

### HTML Forms

```html
<form action="/submit" method="post">
  <input type="text"     name="username" placeholder="Username">
  <input type="password" name="password" placeholder="Password">
  <input type="email"    name="email"    placeholder="Email">
  <button type="submit">Submit</button>
</form>
```

- `method="get"` — data appended to URL (visible)
- `method="post"` — data in request body (hidden)

---

### CSS

**Selectors:**
```css
/* Element */
p { color: red; }

/* Class */
.card { background: white; }

/* ID */
#header { font-size: 24px; }

/* Descendant */
.card p { margin: 0; }
```

**Box model:** every element is a box — `content → padding → border → margin`

**Flexbox (layout):**
```css
.container {
  display: flex;
  justify-content: center;  /* horizontal */
  align-items: center;      /* vertical */
  gap: 16px;
}
```

**Common properties:**
- `color`, `background-color`, `font-size`, `font-family`
- `margin`, `padding`, `border`, `border-radius`
- `width`, `height`, `max-width`
- `display: flex`, `display: grid`, `display: none`

---

### JavaScript Basics

```javascript
// Variables
let x = 5;       // mutable
const y = 10;    // immutable

// Functions
function greet(name) {
  return `Hello, ${name}!`;
}

// Arrow functions
const greet = (name) => `Hello, ${name}!`;

// Arrays
let nums = [1, 2, 3];
nums.push(4);
nums.forEach(n => console.log(n));

// Objects
let person = { name: "Alice", age: 21 };
person.name   // Alice
```

---

### The DOM

The **DOM** (Document Object Model) is the browser's tree representation of the HTML page. JavaScript can read and modify it.

```javascript
// Selecting elements
document.querySelector("h1")           // first h1
document.querySelector("#id")          // by ID
document.querySelector(".class")       // by class
document.querySelectorAll("p")         // all p tags

// Modifying elements
let el = document.querySelector("h1");
el.textContent = "New heading";
el.style.color = "red";
el.classList.add("active");
el.classList.remove("active");

// Creating elements
let p = document.createElement("p");
p.textContent = "Hello";
document.body.appendChild(p);
```

---

### Event Listeners

```javascript
// Wait for DOM to load
document.addEventListener("DOMContentLoaded", function() {

  // Click event
  document.querySelector("button").addEventListener("click", function() {
    alert("Clicked!");
  });

  // Form submission — prevent default reload
  document.querySelector("form").addEventListener("submit", function(e) {
    e.preventDefault();
    let input = document.querySelector("input").value;
    console.log(input);
  });

  // Keydown event
  document.addEventListener("keydown", function(e) {
    console.log(e.key);
  });

});
```

- `DOMContentLoaded` — fires when HTML is fully parsed
- `e.preventDefault()` — stops default browser behavior (form reload, link follow)

---

### DOM Manipulation Examples

```javascript
// Toggle visibility
let el = document.querySelector(".panel");
el.style.display = el.style.display === "none" ? "block" : "none";

// Interval — repeat every N ms
setInterval(function() {
  console.log("tick");
}, 1000);

// Timeout — run once after N ms
setTimeout(function() {
  console.log("done");
}, 2000);
```

---

### Geolocation API

```javascript
navigator.geolocation.getCurrentPosition(function(position) {
  let lat = position.coords.latitude;
  let lng = position.coords.longitude;
  console.log(lat, lng);
});
```

---

## Cue Column

| Term | Definition |
|---|---|
| **HTML** | HyperText Markup Language — defines webpage structure |
| **CSS** | Cascading Style Sheets — controls visual presentation |
| **JavaScript** | Browser programming language enabling interactivity |
| **DOM** | Document Object Model — browser's in-memory tree of the HTML page |
| **`querySelector`** | Selects first matching element using CSS selector syntax |
| **Event listener** | Function that fires when a user action (click, keypress) occurs |
| **`e.preventDefault()`** | Stops the browser's default action for an event |
| **`DOMContentLoaded`** | Event fired when HTML is fully parsed and DOM is ready |
| **Flexbox** | CSS layout model for aligning and distributing elements |
| **`setInterval`** | Repeatedly calls a function at a set time interval |
