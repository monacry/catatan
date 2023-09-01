## theme mode 2

###### html

```html
<a class="btn rounded-pill btn-light ms-3 fw-semibold" href="javascript:void(0)" onclick="darkMode('terang')">terang</a>
<a class="btn rounded-pill btn-light ms-3 fw-semibold" href="javascript:void(0)" onclick="darkMode('gelap')">gelap</a>
```

---

###### javascript

```javascript
// NIGHTMODE NAVBAR==================================
if (localStorage.getItem("theme") == "dark") darkMode("gelap");
if (localStorage.getItem("theme") == "terang") darkMode("terang");

function darkMode(isDark) {
  if (isDark == "gelap") {
    document.getElementById("bootstrap-theme").href = "/bootstrap-5.3.1/dist/css/bootstrap-main.css";
    localStorage.setItem("theme", "dark");
  } else if (isDark == "terang") {
    document.getElementById("bootstrap-theme").href = "/bootstrap-5.3.1/dist/css/bootstrap.css";
    localStorage.setItem("theme", "terang");
  } else {
    localStorage.removeItem("theme");
  }
}
```
