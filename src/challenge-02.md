---
title: Acres Of Land Owned By Negroes In Georgia
---

Challenge 02 - Feb 10

```js
const acresOfLand = FileAttachment("data/challenge_02_data.csv").csv({ typed: true });
```

```js
const margin = { top: 80, left: 80, bottom: 50, right: 100 };

const parseYear = d3.timeParse("%Y");
const formatYear = d3.timeFormat("%Y");

const height = 800;

const x = d3
  .scaleLinear()
  .domain([d3.min(acresOfLand, (d) => d.Land), d3.max(acresOfLand, (d) => d.Land)])
  .range([margin.left, width - margin.right]);

const y = d3
  .scaleTime()
  .domain([parseYear(1874), parseYear(1899)])
  .range([margin.top, height - margin.bottom]);
```

```js
svg`<svg width="${width}" height="${height}" style="background-color: #EADDCE;">
<text x=${width/2} y=40 text-anchor="middle" style="font-weight:bold;">ACRES OF LAND OWNED BY NEGROES IN GEORGIA.</text>
<g transform="translate(${margin.left}, 0)">
${acresOfLand.map(d=>svg`<text x=0 dx="-10" y=${y(parseYear(d.Date))} text-anchor="end" alignment-baseline="middle">${d.Date}</text>
<g transform="translate(0, -12)"><rect x=0 y=${y(parseYear(d.Date))} width=${x(d.Land)} height=20 fill="#DD1C3D"/></g>`)}</g>
<text x=${x(acresOfLand[0].Land)} y=${y(parseYear(1874))} dx=5 alignment-baseline="middle">${acresOfLand[0].Land.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</text>
<text x=${x(acresOfLand[acresOfLand.length-1].Land)/2} y=${y(parseYear(1899))} dx=5 text-anchor="start" alignment-baseline="middle">${acresOfLand[acresOfLand.length-1].Land.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</text>
</svg>`
```


<style>
  * {
    font-family: sans-serif;
  }
</style>
