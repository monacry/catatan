## Switch Theme
ini potongan projek laravel 10 saya untuk merubah tema gelap dan terang, fix fungsi merubah tema tapi saat halaman dimuat ulang temanya mereset ke tampilan awal lagi.

> html bagian link css
```html
<link href="{{ asset('dashboard-assets') }}/css/bootstrap-main.css" id="bootstrap-theme" class="theme-opt" rel="stylesheet" type="text/css" />
<link href="{{ asset('dashboard-assets') }}/css/style-main.css" id="style-theme" class="theme-opt" rel="stylesheet" type="text/css" />
```

> html bagian button trigger navbar
```html
<a href="javascript:void(0)" class="dark-version t-dark" id="t-dark" onclick="setTheme('gelap')" style="display: block">
    <div class="btn btn-icon btn-soft-light"><i data-feather="sun" class="fea icon-sm"></i></div>
</a>
<a href="javascript:void(0)" class="light-version t-light" id="t-light" onclick="setTheme('terang')" style="display: none">
    <div class="btn btn-icon btn-soft-light"><i data-feather="moon" class="fea icon-sm"></i></div>
</a>
```



> script javascript
```javascript
document.addEventListener("DOMContentLoaded", function() {
    const savedTheme = localStorage.getItem("theme");
    if (savedTheme === "dark") {
        document.getElementById("bootstrap-theme").href = "{{ asset('dashboard-assets') }}/css/bootstrap-dark-main.css";
        document.getElementById("style-theme").href = "{{ asset('dashboard-assets') }}/css/style-dark-main.css";
        document.getElementById("t-dark").style.display = "none";
        document.getElementById("t-light").style.display = "block";
    } else {
        document.getElementById("bootstrap-theme").href = "{{ asset('dashboard-assets') }}/css/bootstrap-main.css";
        document.getElementById("style-theme").href = "{{ asset('dashboard-assets') }}/css/style-main.css";
        document.getElementById("t-dark").style.display = "block";
        document.getElementById("t-light").style.display = "none";
    }
});

function setTheme(tema) {
    if (tema === 'gelap') {
        document.getElementById("t-dark").style.display = "none";
        document.getElementById("t-light").style.display = "block";
        document.getElementById("bootstrap-theme").href = "{{ asset('dashboard-assets') }}/css/bootstrap-dark-main.css";
        document.getElementById("style-theme").href = "{{ asset('dashboard-assets') }}/css/style-dark-main.css";
        localStorage.setItem("theme", "dark");
    } else {
        document.getElementById("t-dark").style.display = "block";
        document.getElementById("t-light").style.display = "none";
        document.getElementById("bootstrap-theme").href = "{{ asset('dashboard-assets') }}/css/bootstrap-main.css";
        document.getElementById("style-theme").href = "{{ asset('dashboard-assets') }}/css/style-main.css";
        localStorage.setItem("theme", "light");
    }
}
```
