## Chart.Js

## ini adalah rangkuman design chart saya menggunakan framework Chart.Js

---

> **Html**

```html
<div class="card shadow mb-4">
  <div class="card-body">
    <canvas id="myChart" style="min-height: 150px; height: 150px; max-height: 150px; max-width: 100%"></canvas>
  </div>
</div>
```

> **Javascript**

```javascript
var ctx = document.getElementById("myChart").getContext("2d");
var gradient1 = ctx.createLinearGradient(0, 0, 0, 300);
gradient1.addColorStop(0, "rgba(255, 0, 0, 0.7)");
gradient1.addColorStop(1, "rgba(255, 255, 255, 1)");

var gradient2 = ctx.createLinearGradient(0, 0, 0, 300);
gradient2.addColorStop(0, "rgba(0, 0, 255, 0.7)");
gradient2.addColorStop(1, "rgba(255, 255, 255, 1)");

var chartOptions = {
  maintainAspectRatio: false,
  responsive: true,
  scales: {
    y: {
      beginAtZero: true,
      grid: {
        display: false,
      },
    },
    x: {
      grid: {
        display: false,
      },
    },
  },
  plugins: {
    tooltip: {
      mode: "index",
      intersect: false,
    },
  },
};

var data1 = {
  labels: ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"],
  datasets: [
    {
      label: "Contoh dua",
      data: [28, 48, 40, 19, 86, 27, 90, 40, 19, 86, 27, 90],
      backgroundColor: gradient1,
      borderColor: "rgba(255, 0, 0, 1)",
      borderWidth: 2,
      pointRadius: 0,
      fill: true,
      tension: 0.4,
    },
    {
      label: "Contoh dua",
      data: [65, 59, 80, 81, 56, 55, 40, 86, 27, 90, 40, 10],
      backgroundColor: gradient2,
      borderColor: "rgba(0, 0, 255, 1)",
      borderWidth: 2,
      pointRadius: 0,
      fill: true,
      tension: 0.4,
    },
  ],
};

var myChart = new Chart(ctx, {
  type: "line",
  data: data1,
  options: chartOptions,
});
```
