---
layout: post
title: "Identifying active catalyst states"
date: 2022-06-01
img: spectroscopy.jpeg
---

<p style="text-align: justify;">
Method for deconvoluting the spectro-electrochemical signal of iridium catalysts for water oxidation. </p><!--more-->

<p style="text-align: justify; margin-bottom: 0;">
<strong>Redox-state kinetics in water-oxidation IrO<sub>x</sub> electrocatalysts measured by operando spectroelectrochemistry</strong>.<em><a href="https://pubs.acs.org/doi/full/10.1021/acscatal.1c03290" target="_blank"> ACS Catal., 2021, 11, 24, 15013–15025</a></em>; <em><a href="https://chemrxiv.org/engage/chemrxiv/article-details/60ed85bc882582263da7bf81" target="_blank">ChemRxiv</a></em>.
</p>
<p style="text-align: justify; margin-top: 0;">
<strong>Spectroelectrochemistry of water oxidation kinetics in molecular versus heterogeneous oxide iridium electrocatalysts</strong>.<em><a href="https://pubs.acs.org/doi/full/10.1021/jacs.2c02006" target="_blank"> J. Am. Chem. Soc. 2022, 144, 19, 8454–8459</a></em>.
</p>

<p style="text-align: justify; margin-top: 30px;">
This approach deconvolves the ultraviolet–visible absorption signal measured as a function of the applied electric potential. The oxidation states of the catalyst across the potential range are modeled as a sum of cumulative Gaussian distributions, and their absorption is calculated from one of the spectra following the Lambert-Beer law. The interactive plots below allow you to explore the raw data, simulated spectra, fitting errors, and the extracted absorption coefficients and concentration gradients. You can further test the fitting process by:
</p>

<ol style="text-align: justify;">
  <li>Modifying the initial guesses and boundaries of the Gaussian functions</li>
  <li>Editing the Python script directly</li>
  <li>Running the Python script</li>
  <li>Uploading your own two-dimensional dataset</li>
  <li>Downloading the results</li>
</ol>


<link href="https://cdn.jsdelivr.net/npm/prismjs@1.29.0/themes/prism-tomorrow.min.css" rel="stylesheet" />
<script src="https://cdn.jsdelivr.net/npm/prismjs@1.29.0/prism.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/prismjs@1.29.0/components/prism-python.min.js"></script>
<script src="https://cdn.plot.ly/plotly-2.35.2.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/chroma-js/2.1.0/chroma.min.js"></script>

<style>
  .row-top {
    display: flex;
    justify-content: center;
    gap: 10px;
    flex-wrap: wrap;
    margin-bottom: 10px;
  }

  .row-top > div {
    width: 100%;
    max-width: 400px;
    flex-shrink: 0;
  }

  table {
    background-color: #f0f0f0 !important;
    border-collapse: collapse;
    width: 100%;
    margin-top: 10px;
    font-family: inherit;
    font-size: 1rem;
    overflow-x: auto;
  }
  table tr, table td, table th {
    background-color: #f0f0f0 !important;
    text-align: left;
  }
  th, td {
    border: 1px solid #ccc;
    padding: 8px;
    text-align: center;
    font-size: 1rem !important;
    font-weight: normal !important;
  }
  label {
    font-family: inherit;
    font-size: 1rem;
    font-weight: normal;
  }

  #py-code {
    margin-top: 10px;
    border: 1px solid #e5e7eb;
    border-radius: 10px;
    padding: 14px;
    background: #1e1e1e;
    color: #eee;
    font-size: 14px;
    overflow: auto;
  }
  #py-code code {
    display: block;
    white-space: pre;
    outline: none;
    font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
  }

  #downloads {
    margin-top: 10px;
    display: grid;
    grid-template-columns: repeat(auto-fit,minmax(220px,1fr));
    gap: 10px;
  }
  .dl-section {
    border: 1px solid #e5e7eb;
    border-radius: 10px;
    padding: 12px;
  }
  .dl-section h4 {
    margin: 0 0 8px 0;
    font-size: 14px;
  }
  .dl-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: 8px;
  }
  .dl-btn {
    display: inline-block;
    padding: 10px 12px;
    border: 1px solid #e5e7eb;
    border-radius: 10px;
    text-decoration: none;
    font-weight: 600;
  }
  .dl-btn[aria-disabled="true"] {
    opacity: .55;
    pointer-events: none;
  }


  @media (max-width: 768px) {
    .row-top {
      flex-direction: column;
      gap: 10px;
    }

    .row-top > div {
      width: 100%;
      max-width: 100%;
      flex-grow: 1;
    }

    #plot-data, #plot-simulated, #plot-error, #plot-coefficients, #plot-concentrations {
      width: 100%;
      max-width: 100%;
    }

    table {
      width: 100%;
      overflow-x: auto;
      display: block;
      font-size: 0.9rem;
    }

    button, input[type="number"], input[type="file"] {
      width: 100%;
      max-width: 300px;
      margin-bottom: 10px;
    }

    #downloads {
      display: block;
      margin-top: 20px;
    }

    .dl-btn {
      width: 100%;
      text-align: center;
      padding: 12px;
    }

    #py-code {
      font-size: 12px;
      padding: 10px;
    }

    #status {
      font-size: 14px;
      padding: 10px;
    }
  }

  @media (max-width: 480px) {
    table {
      font-size: 0.85rem;
    }

    .dl-btn {
      font-size: 14px;
    }

    #py-code {
      font-size: 10px;
      padding: 8px;
    }

    .row-top {
      flex-direction: column;
      gap: 0px;
    }

    .row-top > div {
      width: 100%;
      max-width: 100%;
      flex-grow: 10;
    }

    label, input, button {
      width: 100%;
    }

    #downloads {
      margin-top: 20px;
    }

    .dl-btn {
      font-size: 14px;
      padding: 8px;
      margin-bottom: 8px;
    }

    #mlcatalysis-app {
      padding: 12px;
    }
  }
</style>

<div class="row-top">
  <div id="plot-data"></div>
  <div id="plot-simulated"></div>
  <div id="plot-error"></div>
</div>

<div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 0px; margin-top: 0px;">
  <div id="plot-coefficients"></div>
  <div id="plot-concentrations"></div>
</div>

<script>

const runtimeCSVBlobs = {
};

const filesDefault = [
  { id: "plot-data",           file: "{{ site.baseurl }}/assets/data/data_default.csv",          showLegend: true, group: 'group1' },
  { id: "plot-simulated",      file: "{{ site.baseurl }}/assets/data/simulated_default.csv",     showLegend: true, group: 'group1' },
  { id: "plot-error",          file: "{{ site.baseurl }}/assets/data/error_default.csv",         showLegend: true, group: 'group1' },
  { id: "plot-coefficients",   file: "{{ site.baseurl }}/assets/data/coefficients_default.csv",  showLegend: true, group: 'group2' },
  { id: "plot-concentrations", file: "{{ site.baseurl }}/assets/data/gradients_default.csv",     showLegend: true, group: 'group2' }
];

const colormaps = { group1: [], group2: [] };

const getHeaderLength = (rows) => {
  for (let i = 0; i < rows.length; i++) {
    if (rows[i].length > 1) return rows[i].length - 1;
  }
  return 1;
};

async function renderAllPlots() {
  const files = filesDefault.map(row => ({
    ...row,
    file: runtimeCSVBlobs[row.id] || row.file
  }));

  const texts = await Promise.all(files.map(f => fetch(f.file).then(r => r.text())));
  const rowsMap = {};
  files.forEach((f, i) => {
    rowsMap[f.id] = texts[i].trim().split('\n').map(r => r.split(','));
  });

  const seriesCount = (rows) => getHeaderLength(rows);
  const firstG1 = rowsMap['plot-data'] || rowsMap[files.find(f => f.group === 'group1')?.id];
  const firstG2 = rowsMap['plot-concentrations'] || rowsMap[files.find(f => f.group === 'group2')?.id];
  const numSamplesGroup1 = firstG1 ? seriesCount(firstG1) : 1;
  const numSamplesGroup2 = firstG2 ? seriesCount(firstG2) : 1;

  colormaps.group1 = {
    'plot-data':      chroma.scale('Spectral').colors(numSamplesGroup1),
    'plot-simulated': chroma.scale('Spectral').colors(numSamplesGroup1),
    'plot-error':     chroma.scale('Spectral').colors(numSamplesGroup1),
  };
  colormaps.group2 = {
    'plot-coefficients':   chroma.scale(['#00008B', '#0000FF', '#ADD8E6']).colors(numSamplesGroup2),
    'plot-concentrations': chroma.scale(['#004d00', '#00a000', '#90EE90']).colors(numSamplesGroup2),
  };

  for (const { id, showLegend, group } of files) {
    const rows = rowsMap[id];
    const xLabel = rows[1][0];
    const yLabel = rows[0][1];
    const headers = rows[2].slice(1);
    const x = rows.slice(3).map(r => parseFloat(r[0]));
    const colors = colormaps[group][id];

    const traces = headers.map((header, i) => {
      const y = rows.slice(3).map(r => parseFloat(r[i + 1]));
      return {
        x, y, type: 'scatter', mode: 'lines', name: header,
        line: { color: colors[i % colors.length] }
      };
    });

    const layout = { xaxis: { title: xLabel }, yaxis: { title: yLabel }, showlegend: showLegend };
    if (id === 'plot-data' || id === 'plot-simulated' || id === 'plot-error') {
      layout.title = id.replace('plot-', '').replace(/_/g, ' ').toUpperCase();
      layout.hovermode = false;
    }
    Plotly.newPlot(id, traces, layout);
  }
}

document.addEventListener('DOMContentLoaded', renderAllPlots);
</script>

<div>
  <label for="numGaussians">Number of Gaussians:</label>
  <input type="number" id="numGaussians" min="1">
  <button onclick="generateFields()">Update Fields</button>
</div>

<form id="gaussianForm"></form>


<script>
  const numInput = document.getElementById("numGaussians");
  const formEl = document.getElementById("gaussianForm");

  function buildCSVFromForm() {
    const num = parseInt(numInput.value) || 0;
    let csv = "Gaussian,Reference Spectrum,Variable,Initial Guess,Lower Boundary,Upper Boundary\n";
    for (let i = 0; i < num; i++) {
      const reference = (document.querySelector(`input[name=reference${i}]`)?.value ?? "").trim();
      for (const v of ["center", "width", "amplitude"]) {
        const guess = (document.querySelector(`input[name=${v}_guess${i}]`)?.value ?? "").trim();
        const lower = (document.querySelector(`input[name=${v}_lower${i}]`)?.value ?? "").trim();
        const upper = (document.querySelector(`input[name=${v}_upper${i}]`)?.value ?? "").trim();
        csv += `${i + 1},${reference},${v},${guess},${lower},${upper}\n`;
      }
    }
    window.generatedGaussiansCSV = csv;
    return csv;
  }

  function attachLiveSync() {
    formEl.addEventListener("input", buildCSVFromForm);
  }

  function generateFields() {
    const num = parseInt(numInput.value) || 0;
    formEl.innerHTML = "";

    for (let i = 0; i < num; i++) {
      const group = document.createElement("div");
      group.innerHTML = `
        <h4>Gaussian ${i + 1}</h4>
        <label style="display:block;margin-bottom:10px;">Reference Spectrum: <input type="text" name="reference${i}"></label>
        <table>
          <tr>
            <th> </th>
            <th>Initial Guess</th>
            <th>Lower Boundary</th>
            <th>Upper Boundary</th>
          </tr>
          <tr>
            <td style="font-size:1rem;">Center</td>
            <td><input type="text" name="center_guess${i}"></td>
            <td><input type="text" name="center_lower${i}"></td>
            <td><input type="text" name="center_upper${i}"></td>
          </tr>
          <tr>
            <td style="font-size:1rem;">Width</td>
            <td><input type="text" name="width_guess${i}"></td>
            <td><input type="text" name="width_lower${i}"></td>
            <td><input type="text" name="width_upper${i}"></td>
          </tr>
          <tr>
            <td style="font-size:1rem;">Amplitude</td>
            <td><input type="text" name="amplitude_guess${i}"></td>
            <td><input type="text" name="amplitude_lower${i}"></td>
            <td><input type="text" name="amplitude_upper${i}"></td>
          </tr>
        </table><br>
      `;
      formEl.appendChild(group);
    }

    attachLiveSync();
    buildCSVFromForm();
  }

  async function loadDefaultGaussians() {
    try {
      const resp = await fetch("{{ site.baseurl }}/assets/data/gaussians_default.csv");
      const text = await resp.text();

      const rows = text.trim().split("\n");
      if (rows.length <= 1) return;
      const dataRows = rows.slice(1).map(r => r.split(","));

      const gaussIds = [...new Set(dataRows.map(r => r[0]))].map(Number).sort((a,b)=>a-b);
      const n = gaussIds.length;
      numInput.value = String(n);

      generateFields();

      for (let g = 0; g < n; g++) {
        const gId = (g + 1).toString();
        const rowsForG = dataRows.filter(r => r[0] === gId);

        const ref = rowsForG?.[0]?.[1] ?? "";
        const refInput = document.querySelector(`input[name=reference${g}]`);
        if (refInput) refInput.value = ref;

        for (const v of ["center", "width", "amplitude"]) {
          const row = rowsForG.find(r => r[2].trim().toLowerCase() === v);
          if (!row) continue;
          const [, , , guess, lower, upper] = row;
          const gInput = document.querySelector(`input[name=${v}_guess${g}]`);
          const lInput = document.querySelector(`input[name=${v}_lower${g}]`);
          const uInput = document.querySelector(`input[name=${v}_upper${g}]`);
          if (gInput) gInput.value = guess ?? "";
          if (lInput) lInput.value = lower ?? "";
          if (uInput) uInput.value = upper ?? "";
        }
      }

      buildCSVFromForm();
    } catch (e) {
      console.warn("Could not load /assets/data/gaussians_default.csv:", e);
      if (!numInput.value) {
        numInput.value = "1";
        generateFields();
      }
    }
  }

  document.addEventListener("DOMContentLoaded", loadDefaultGaussians);
</script>

<div id="mlcatalysis-app" style="border:1px solid #e5e7eb;border-radius:14px;padding:16px;margin:20px 0;">
  <div style="display:flex;gap:12px;flex-wrap:wrap;align-items:center;">
    <button id="run-btn" disabled style="padding:8px 14px;border-radius:10px;border:0;background:#111827;color:#fff;cursor:not-allowed;">Loading Python…</button>
    <label for="csv-input" style="font-weight:600;">Upload your data (.csv):</label>
    <input id="csv-input" type="file" accept=".csv" />
  </div>

  <details style="margin-top:14px;">
    <summary style="cursor:pointer;font-weight:600;">Show / edit Python script</summary>
    <p style="margin:8px 0 0 0;color:#6b7280;">Your script should read from <code>data.csv</code> and save out CSVs (e.g. <code>results_*.csv</code>). Any <code>.csv</code> files it writes will become downloadable.</p>

    <pre id="py-code" class="language-python">
<code class="language-python" contenteditable="true">import pandas as pd
import numpy as np
import math
import os
from scipy.optimize import differential_evolution

raw_data = pd.read_csv('data.csv', header=None)
gaussian_data = pd.read_csv('gaussians.csv')

def closest_i(target, vector):
    closest_index = min(range(len(vector)), key=lambda i: abs(vector.iloc[i] - target))
    return closest_index

def sum_of_gaussians(data, sample_data, ref, *properties):
    num_gaussians = len(properties) // 3
    concentrations = []
    gradients = []
    for i in range(num_gaussians):
        amp, mean, std = properties[3*i], properties[3*i+1], properties[3*i+2]
        gradient = amp * np.exp(-0.5 * ((sample_data - mean) / std) ** 2)
        gradients.append(gradient)
        concentrations.append(np.cumsum(gradient))

    coefficients = []
    residual = data.iloc[:, ref[0]].values
    for i in range(num_gaussians):
        if i == 0:
            coeff = residual / concentrations[i][ref[0]]
        else:
            previous = sum(coefficients[j] * concentrations[j][ref[i]] for j in range(i))
            coeff = (data.iloc[:, ref[i]].values - previous) / concentrations[i][ref[i]]
        coefficients.append(coeff)
    return coefficients, concentrations, gradients

def simulation (data, sample_data, ref, *properties):
    num_gaussians = len(properties) // 3
    coefficients, concentrations, gradients = sum_of_gaussians(data, sample_data, ref, *properties)
    simulated_data = sum(np.outer(coefficients[i], concentrations[i]) for i in range(num_gaussians))
    if any(np.any(c &lt; 0) for c in coefficients):
        simulated_data *= 2
    return simulated_data

def error(data, sample_data, ref, *properties):
    simulated_data = simulation (data, sample_data, ref, *properties)
    diff = np.array(data.values) - simulated_data
    error = np.sum(diff**2)
    return error

def optimize_properties(data, sample_data, ref, initial_guess, b):
    iteration = {'count': 0}
    def callback(xk, convergence):
        iteration['count'] += 1
        print(f"Iteration {iteration['count']}: Current error = {error(data, sample_data, ref, *xk):.6f}")

    result = differential_evolution(
        func=lambda props: error(data, sample_data, ref, *props),
        bounds=b,
        strategy='best1bin',
        maxiter=200,
        popsize=20,
        mutation=(0.5, 1.0),
        recombination=0.7,
        tol=1e-5,
        callback=callback,
        polish=True
    )
    return result

x_label = raw_data.iloc[1, 0]
y_label = raw_data.iloc[0, 1]
sample_label = raw_data.iloc[1, 1]
x_data = raw_data.iloc[3:, 0].astype(float)
y_data = raw_data.iloc[3:, 1:88].astype(float)
sample_data = raw_data.iloc[2, 1:88].astype(float)
y_data = y_data.reset_index(drop=True)
x_data = x_data.reset_index(drop=True)
sample_data = sample_data.reset_index(drop=True)

n_gauss = int(len(gaussian_data)/3)
ref = []
initial_guess = []
boundaries = []
for i in range(n_gauss):
  r_val = float(gaussian_data['Reference Spectrum'].iloc[i*3])
  r = closest_i(r_val, sample_data)
  ref = ref + [r]
  for j in range (3):
    initial_guess = initial_guess + [float(gaussian_data['Initial Guess'].iloc[i*3+j])]
    boundaries = boundaries + [(float(gaussian_data['Lower Boundary'].iloc[i*3+j]), float(gaussian_data['Upper Boundary'].iloc[i*3+j]))]

optimized = optimize_properties (y_data, sample_data, ref, initial_guess, boundaries)
simulated_data = simulation(y_data, sample_data, ref, *optimized.x)
diff = np.array(y_data.astype(float).values) - simulated_data
coefficients, concentrations, gradients = sum_of_gaussians(y_data, sample_data, ref, *optimized.x)

header_row = [''] + [y_label] + ['']* (y_data.shape[1]-1)
label_row = [x_label, sample_label] + [''] * (y_data.shape[1] - 1)
sample_row = [''] + sample_data.tolist()

data_rows = pd.concat([x_data, pd.DataFrame(simulated_data)], axis=1)
data_rows.columns = range(data_rows.shape[1])
simulated = pd.DataFrame([header_row, label_row, sample_row])
simulated = pd.concat([simulated, data_rows], ignore_index=True)
simulated.to_csv("simulated.csv", index=False, header=False)

data_rows = pd.concat([x_data, pd.DataFrame(diff)], axis=1)
data_rows.columns = range(data_rows.shape[1])
error = pd.DataFrame([header_row, label_row, sample_row])
error = pd.concat([error, data_rows], ignore_index=True)
error.to_csv("error.csv", index=False, header=False)

d = pd.DataFrame(coefficients).transpose()
s_label = list(range(1, n_gauss+1))
header_row = [''] + [y_label] + ['']* (d.shape[1]-1)
label_row = [x_label, 'Gaussian number'] + [''] * (d.shape[1] - 1)
sample_row = [''] + s_label
data_rows = pd.concat([x_data, d], axis=1)
data_rows.columns = range(data_rows.shape[1])
coeff = pd.DataFrame([header_row, label_row, sample_row])
coeff = pd.concat([coeff, data_rows], ignore_index=True)
coeff.to_csv("coefficients.csv", index=False, header=False)

d = pd.DataFrame(gradients).transpose()
header_row = [''] + ['Concentration gradient'] + ['']* (d.shape[1]-1)
label_row = [sample_label, 'Gaussian number'] + [''] * (d.shape[1] - 1)
data_rows = pd.concat([sample_data, d], axis=1)
data_rows.columns = range(data_rows.shape[1])
grad = pd.DataFrame([header_row, label_row, sample_row])
grad = pd.concat([grad, data_rows], ignore_index=True)
grad.to_csv("gradients.csv", index=False, header=False)

optimized_gaussians = gaussian_data[['Gaussian', 'Reference Spectrum', 'Variable']].copy()
optimized_gaussians['Optimized'] = optimized.x
optimized_gaussians.to_csv("optimized_gaussians.csv", index=False)
</code>
    </pre>
  </details>

  <div id="status" style="margin-top:14px;font-family:system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial, 'Apple Color Emoji','Segoe UI Emoji';white-space:pre-wrap;"></div>

  <div id="downloads">
    <div class="dl-section" id="dl-latest">
      <h4>Latest run (this session)</h4>
      <div class="dl-grid">
        <a id="dl-latest-data"           class="dl-btn" aria-disabled="true">Download data.csv</a>
        <a id="dl-latest-simulated"      class="dl-btn" aria-disabled="true">Download simulated.csv</a>
        <a id="dl-latest-error"          class="dl-btn" aria-disabled="true">Download error.csv</a>
        <a id="dl-latest-coefficients"   class="dl-btn" aria-disabled="true">Download coefficients.csv</a>
        <a id="dl-latest-gradients"      class="dl-btn" aria-disabled="true">Download gradients.csv</a>
        <a id="dl-latest-optimized"      class="dl-btn" aria-disabled="true">Download optimized_gaussians.csv</a>
        <a id="dl-latest-gaussians"      class="dl-btn" aria-disabled="true">Download gaussians.csv</a>
      </div>
    </div>
    <div class="dl-section" id="dl-defaults">
      <h4>
        Published data (<a href="https://pubs.acs.org/doi/abs/10.1021/acscatal.1c03290" target="_blank" rel="noopener noreferrer">ACS Catal. 2021, 11, 24</a>)
      </h4>
      <div class="dl-grid">
        <a class="dl-btn" href="{{ site.baseurl }}/assets/data/data_default.csv" download>Download data.csv</a>
        <a class="dl-btn" href="{{ site.baseurl }}/assets/data/simulated_default.csv" download>Download simulated.csv</a>
        <a class="dl-btn" href="{{ site.baseurl }}/assets/data/error_default.csv" download>Download error.csv</a>
        <a class="dl-btn" href="{{ site.baseurl }}/assets/data/coefficients_default.csv" download>Download coefficients.csv</a>
        <a class="dl-btn" href="{{ site.baseurl }}/assets/data/gradients_default.csv" download>Download gradients.csv</a>
        <a class="dl-btn" href="{{ site.baseurl }}/assets/data/gaussians_default.csv" download>Download gaussians.csv</a>
      </div>
    </div>
  </div>
</div>

<script>
  document.addEventListener('DOMContentLoaded', () => {
    if (window.Prism && Prism.highlightAll) Prism.highlightAll();
    const det = document.querySelector('#mlcatalysis-app details');
    if (det) {
      det.addEventListener('toggle', () => {
        if (det.open && window.Prism && Prism.highlightAllUnder) {
          Prism.highlightAllUnder(det);
        }
      });
    }
  });
</script>

<script src="https://cdn.jsdelivr.net/pyodide/v0.26.0/full/pyodide.js"></script>
<script>
  (function () {
    const fileInput   = document.getElementById('csv-input');
    const runBtn      = document.getElementById('run-btn');
    const statusEl    = document.getElementById('status');

    const codeEl      = document.querySelector('#py-code code');

    const latestAnchors = {
      data:         document.getElementById('dl-latest-data'),
      simulated:    document.getElementById('dl-latest-simulated'),
      error:        document.getElementById('dl-latest-error'),
      coefficients: document.getElementById('dl-latest-coefficients'),
      gradients:    document.getElementById('dl-latest-gradients'),
      optimized:    document.getElementById('dl-latest-optimized'),
      gaussians:    document.getElementById('dl-latest-gaussians'),
    };

    let pyodide = null;
    let packagesLoaded = false;
    let lastRunTimestamp = 0;

    function log(msg) {
      statusEl.textContent += (statusEl.textContent ? "\n" : "") + msg;
    }
    function clearLog() { statusEl.textContent = ""; }
    function setRunning(running) {
      runBtn.textContent = running ? "Running…" : "Run script";
      runBtn.style.cursor = running ? "wait" : "pointer";
      runBtn.disabled = running;
      runBtn.style.opacity = running ? "0.7" : "1";
    }
    function setReady() {
      runBtn.disabled = false;
      runBtn.textContent = "Run script";
      runBtn.style.cursor = "pointer";
    }

    async function boot() {
      try {
        pyodide = await loadPyodide({ fullStdLib: true });
        log("Python runtime loaded.");
        const needed = ['pandas', 'numpy', 'scipy', 'scikit-learn'];
        await pyodide.loadPackage(needed);
        log("Packages ready: " + needed.join(", "));
        packagesLoaded = true;
        log("Packages ready: pandas");
        setReady();
      } catch (err) {
        console.error(err);
        log("❌ Failed to load Python runtime or packages.");
      }
    }

    async function writeUploadToFS() {
      const file = fileInput.files?.[0];
      if (file) {
        const content = new Uint8Array(await file.arrayBuffer());
        pyodide.FS.writeFile('data.csv', content);
        return file.name + " (uploaded)";
      } else {
        const resp = await fetch('/assets/data/data_default.csv');
        const text = await resp.text();
        const enc = new TextEncoder();
        pyodide.FS.writeFile('data.csv', enc.encode(text));
        return "data_default.csv (fallback)";
      }
    }

    async function writeGaussiansToFS() {
      if (window.generatedGaussiansCSV) {
        const enc = new TextEncoder();
        pyodide.FS.writeFile('gaussians.csv', enc.encode(window.generatedGaussiansCSV));
        return "generated on page";
      }
      const gaussInput = document.getElementById('gauss-input');
      if (gaussInput && gaussInput.files?.[0]) {
        const buf = new Uint8Array(await gaussInput.files[0].arrayBuffer());
        pyodide.FS.writeFile('gaussians.csv', buf);
        return gaussInput.files[0].name;
      }
      throw new Error('No gaussians.csv found. Generate it above or upload one.');
    }

    function setLatestDownload(name, anchor, mime = "text/csv") {
      try {
        const bytes = pyodide.FS.readFile(name);
        const blob  = new Blob([bytes], { type: mime });
        const url   = URL.createObjectURL(blob);
        anchor.href = url;
        anchor.download = name;
        anchor.setAttribute('aria-disabled', 'false');
        return url;
      } catch (e) {
        console.warn("Could not set latest download for", name, e);
        return null;
      }
    }

    function pushRuntimeCSVsToPlots() {
      const mapping = {
        "plot-data":           "data.csv",
        "plot-simulated":      "simulated.csv",
        "plot-error":          "error.csv",
        "plot-coefficients":   "coefficients.csv",
        "plot-concentrations": "gradients.csv"
      };

      for (const [plotId, fsName] of Object.entries(mapping)) {
        const url = setLatestDownload(
          fsName,
          fsName === "data.csv"           ? latestAnchors.data :
          fsName === "simulated.csv"      ? latestAnchors.simulated :
          fsName === "error.csv"          ? latestAnchors.error :
          fsName === "coefficients.csv"   ? latestAnchors.coefficients :
          fsName === "gradients.csv"      ? latestAnchors.gradients :
                                            document.createElement('a')
        );
        if (url) runtimeCSVBlobs[plotId] = url;
      }

      setLatestDownload("optimized_gaussians.csv", latestAnchors.optimized);
      setLatestDownload("gaussians.csv", latestAnchors.gaussians);

      renderAllPlots();
    }

    runBtn.addEventListener('click', async () => {
      document.querySelector('#downloads').scrollIntoView({ behavior: 'smooth', block: 'nearest' });
      statusEl.textContent = "";
      if (!pyodide || !packagesLoaded) { log("Python not ready yet."); return; }

      try {
        setRunning(true);
        const uploadedName = await writeUploadToFS();
        log(`Prepared "${uploadedName}" as data.csv`);
        const gName = await writeGaussiansToFS();
        log(`Prepared "${gName}" as gaussians.csv`);
        lastRunTimestamp = Date.now();

        const code = codeEl.textContent;
        await pyodide.runPythonAsync(code);

        log("✅ Script finished.");

        pushRuntimeCSVsToPlots();
      } catch (err) {
        console.error(err);
        log("❌ Error while running your script:\n" + (err?.message || String(err)));
      } finally {
        setRunning(false);
      }
    });

    fileInput.addEventListener('change', () => {
      if (fileInput.files?.length) log(`Selected file: ${fileInput.files[0].name}`);
    });

    boot();
  })();
</script>
