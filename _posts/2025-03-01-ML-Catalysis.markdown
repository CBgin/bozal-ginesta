---
layout: post
title: Machine learning in heterogeneous catalysis
date: 2025-03-01
img: catalyst.png
---

<p style="text-align: justify;">Literature review of scientific articles on heterogeneous catalysis using machine learning or high-throughput synthesis and characterization.</p><!--more-->
<p style="text-align: justify; margin-top: 0;"><strong>Developing machine learning for heterogeneous catalysis with experimental and computational data</strong><br><em>Link to <a href="https://www.nature.com/articles/s41570-025-00740-4" target="_blank">Nature Reviews Chemistry</a> and <a href="https://www.doi.org/10.26434/chemrxiv-2025-6v1sf" target="_blank">ChemRxiv</a></em></p> 

<p style="text-align: justify; margin-top: 30px;">The review systematically compares published studies in terms of machine learning methods, input and output features, dataset size, reaction type, and reported accuracy. We identified 41 scientific articles on heterogeneous catalysis that report unitless R<sup>2</sup> values for machine learning models, allowing for an objective comparison. The data compiled from these papers is displayed in the interactive plots below and in Figure 3 of the review. The label <em>CO and CO2 reactivity</em> includes any catalysis with CO<sub>2</sub> and CO as substrates, like syngas reactions, except where methane is involved; <em>Methane reactivity</em> includes all the studies where methane is either reactant or product; <em>Water splitting</em> includes hydrogen and oxygen evolution reactions and oxygen reduction reaction.</p>

<script src="https://cdn.plot.ly/plotly-2.27.0.min.js"></script>

<div id="plot" style="width: 100%; height: 600px;"></div>

<p>
  X-axis:
  <select id="x-axis-select">
    <option value="Input">Input</option>
    <option value="Output">Output</option>
    <option value="Reaction type">Reaction type</option>
    <option value="Data type">Data type</option>
  </select>

  Y-axis:
  <select id="y-axis-select">
    <option value="Counts">Counts</option>
    <option value="Average R2">Average R2</option>
    <option value="Average number of datapoints">Average number of datapoints</option>
  </select>
</p>

<div id="histogram" style="width: 100%; height: 600px;"></div>

<script>
function parseTSV(text) {
  const lines = text.trim().split('\n');
  const headers = lines[0].split(',').map(h => h.trim());
  const keepCols = [
    "Data type",
    "Reaction type",
    "Average number of datapoints",
    "Average R2",
    "Input",
    "Output"
  ];

  const indices = keepCols.map(h => headers.indexOf(h));
  const result = {};
  keepCols.forEach(h => result[h] = []);

  lines.slice(1).forEach(line => {
    const parts = line.split(',');
    if (parts.length < Math.max(...indices) + 1) return;

    indices.forEach((idx, i) => {
      const key = keepCols[i];
      let val = parts[idx];

      if (!val) return result[key].push(null); // leave missing

      val = val.trim();

      if (key.includes("Average")) {
        val = val.replace(",", ".");
        const parsed = parseFloat(val);
        result[key].push(isNaN(parsed) ? null : parsed);
      } else {
        result[key].push(val);
      }
    });
  });
  console.log("Parsed data:", result);
  return result;
}

function encodeCategorical(values) {
  const categories = [...new Set(values)];
  const map = Object.fromEntries(categories.map((c, i) => [c, i]));
  return {
    numeric: values.map(v => map[v]),
    labels: categories
  };
}

function plotParallelCoords(data) {
  const dimensions = [];

  ["Data type", "Average R2", "Reaction type", "Average number of datapoints"].forEach(key => {
    if (key === "Data type" || key === "Reaction type") {
      const { numeric, labels } = encodeCategorical(data[key]);
      dimensions.push({
        label: key,
        tickvals: labels.map((_, i) => i),
        ticktext: labels,
        values: numeric
      });
    } else if (key === "Average number of datapoints") {
      dimensions.push({
        label: key,
        values: data[key].map(v => v > 0 ? Math.log10(v) : null),
        tickvals: [1, 2, 3, 4, 5],
        ticktext: ["10", "100", "1000", "10000", "100000"]
      });
    } else {
      dimensions.push({
        label: key,
        values: data[key]
      });
    }
  });

  Plotly.newPlot('plot', [{
    type: 'parcoords',
    line: {
      color: data["Average R2"],
      colorscale: 'Jet',
      width: 20
    },
    dimensions
  }]);
}

function plotHistogram(data, xKey, yKey) {
  const rawX = data[xKey] || [];
  let xValues = [];
  let yValues = [];

  for (let i = 0; i < rawX.length; i++) {
    const raw = rawX[i];
    const xItems = (xKey === "Input" || xKey === "Output")
      ? (raw ? raw.trim().split(/[\s,;]+/).filter(Boolean) : [])
      : [raw];

    xItems.forEach(item => {
      xValues.push(item);
      if (yKey === "Counts") {
        yValues.push(1);
      } else {
        const yVal = data[yKey]?.[i];
        yValues.push(isNaN(yVal) ? null : yVal);
      }
    });
  }

  // Aggregate
  const grouped = {};
  for (let i = 0; i < xValues.length; i++) {
    const key = xValues[i];
    const y = yValues[i];
    if (!grouped[key]) grouped[key] = [];
    if (y != null) grouped[key].push(y);
  }

  const x = [];
  const y = [];
  const errorY = [];

  for (const key in grouped) {
    x.push(key);

    if (yKey === "Counts") {
      y.push(grouped[key].length);
      errorY.push(0);  // No error bars for counts
    } else {
      const values = grouped[key];
      const n = values.length;
      const mean = values.reduce((a, b) => a + b, 0) / n;
      const variance = values.reduce((sum, val) => sum + Math.pow(val - mean, 2), 0) / n;
      const stddev = Math.sqrt(variance);

      y.push(mean);
      errorY.push(stddev);
    }
  }

  const validY = y.filter(v => v != null && isFinite(v));
  let minY = Math.min(...validY);
  let maxY = Math.max(...validY);

  let yRange = null;
  if (yKey !== "Counts" && yKey !== "Average number of datapoints") {
    const padding = (maxY - minY) * 2 || 1;
    const lower = Math.max(0.0001, minY - padding);
    const upper = maxY + padding;
    yRange = [lower, upper];
  }

  Plotly.newPlot('histogram', [{
    type: 'bar',
    x: x,
    y: y,
    marker: { color: 'rgba(192,192,192,0.6)' },
    error_y: {
      type: 'data',
      array: errorY,
      visible: yKey !== "Counts",
      color: 'rgba(100,100,100,0.8)',
      thickness: 1.5,
      width: 4
    }
  }], {
    margin: { t: 40, b: 100 },
    xaxis: {
      title: xKey,
      tickangle: -45,
      automargin: true
    },
    yaxis: {
      title: yKey,
      type: yKey === "Average number of datapoints" ? 'log' : 'linear',
      range: yRange
    }
  });
}

// Load default TSV file from /assets/
fetch('{{ "/assets/data/ML_catalysis_data.csv" | relative_url }}')
  .then(res => res.text())
  .then(text => {
    const data = parseTSV(text);
    plotParallelCoords(data);

    const xSelect = document.getElementById('x-axis-select');
    const ySelect = document.getElementById('y-axis-select');
    plotHistogram(data, xSelect.value, ySelect.value);

    xSelect.addEventListener('change', () => plotHistogram(data, xSelect.value, ySelect.value));
    ySelect.addEventListener('change', () => plotHistogram(data, xSelect.value, ySelect.value));
  });

// Handle user upload
document.addEventListener("DOMContentLoaded", function () {
  const uploadInput = document.getElementById('csv-upload');
  if (!uploadInput) return;

  uploadInput.addEventListener('change', function(e) {
    const file = e.target.files[0];
    if (!file) return;

    const reader = new FileReader();
    reader.onload = function(evt) {
      const data = parseTSV(evt.target.result);
      plotParallelCoords(data);
    };
    reader.readAsText(file);
  });
});
</script>

<p>Download the data: <a href='{{ "/assets/data/ML_catalysis_data.csv" | relative_url }}' download><button>Download</button></a><br>
Upload your own data, keeping the same labels and format: <input type="file" id="csv-upload" accept=".csv" /></p>