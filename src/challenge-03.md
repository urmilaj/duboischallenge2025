---
title: Land Owned By Negroes In Georgia, U.S.A 1870-1900
---

Challenge 03 - Feb 17 (Made this super late, Feb 23)

```js
const georgiaMap = FileAttachment("data/challenge_03/georgia.json").json();

const mapData = FileAttachment("data/challenge_03/ga1899.csv").csv({ typed: true });
```

```js
const getValues = (name, mapInfo) => {
  const value = mapInfo.filter((d) => String(d.County1890).toLowerCase() == name);
  const color = d3.scaleOrdinal().domain(["blue", "brown", "green", "lightgray", "pink", "red", "yellow"]).range(["#647AA5", "#A8856C", "#3F5F52", "#C1BCA9", "#E48485", "#C21432", "#EFB400"])
  return value && value[0] ? color(value[0].color) : "black";
};

const getValues2 = (name, mapInfo) => {
  const value = mapInfo.filter((d) => String(d.County1890).toLowerCase() == name);
  return value && value[0] ? value[0]["Acres 1899"] : "hi";
};
```

```js
const map = Plot.plot({
  className: "custom-plot",
  width: width,
  height: width / 1.2,
  axis: null,
  style: {
    fontFamily: "sans-serif",
    textAlign: "center",
    backgroundColor: "#E4D4C4"
  },
  marks: [
    Plot.geo(georgiaMap, {fill: (d) => getValues(d.properties.NHGISNAM.toLowerCase(), mapData), stroke:"black", strokeWidth:0.5}),
    Plot.text(georgiaMap.features, Plot.centroid({
        text: (d) => getValues2(d.properties.NHGISNAM.toLowerCase(), mapData),
        textAnchor: "middle",
        fill: "black",
      })),
  ]
})
```

<div class="container">
  <h2>${"Land Owned By Negroes In Georgia, U.S.A 1870-1900.".toUpperCase()}</h2>
  <div>${map}</div>
</div>

<hr/>

## Process of creating the visualization

1. Use [mapshaper.org](https://mapshaper.org/) to convert shapefiles to geojson.

2. Import all the data and geojson files.

```
const georgiaMap = FileAttachment("data/challenge_03/georgia.json").json();

const mapData = FileAttachment("data/challenge_03/ga1899.csv").csv({ typed: true });
```

3. Create functions to retrieve values for each county

```
const getValues = (name, mapInfo) => {
  const value = mapInfo.filter((d) => String(d.County1890).toLowerCase() == name);
  const color = d3.scaleOrdinal().domain(["blue", "brown", "green", "lightgray", "pink", "red", "yellow"]).range(["#647AA5", "#A8856C", "#3F5F52", "#C1BCA9", "#E48485", "#C21432", "#EFB400"])
  return value && value[0] ? color(value[0].color) : "black";
};

const getValues2 = (name, mapInfo) => {
  const value = mapInfo.filter((d) => String(d.County1890).toLowerCase() == name);
  return value && value[0] ? value[0]["Acres 1899"] : "null";
};
```

4. Use Observable Plot to visualize your map and data

```
const map = Plot.plot({
  className: "custom-plot",
  width: width,
  height: width / 1.2,
  axis: null,
  style: {
    fontFamily: "sans-serif",
    textAlign: "center",
    backgroundColor: "#E4D4C4"
  },
  marks: [
    Plot.geo(georgiaMap, {fill: (d) => getValues(d.properties.NHGISNAM.toLowerCase(), mapData), stroke:"black", strokeWidth:0.5}),
    Plot.text(georgiaMap.features, Plot.centroid({
        text: (d) => getValues2(d.properties.NHGISNAM.toLowerCase(), mapData),
        textAnchor: "middle",
        fill: "black",
      })),
  ]
})
```

5. Finally call your plot function and Voila! you have your map.

```
<div class="container">
  <h2>${"Land Owned By Negroes In Georgia, U.S.A 1870-1900.".toUpperCase()}</h2>
  <div>${map}</div>
</div>
```

<style>
  * {
    font-family: sans-serif;
  }

  .container {
    background: #E4D4C4;
    display: flex;
    flex-direction: column;
    padding: 10px;
  }

  h2 {
    text-align: center;
    max-width: 100%;
    margin: auto;
    padding: 0;
    display: inline-block;
  }

  .container div {
    transform: rotate(6deg) scale(0.9)
  }
</style>
