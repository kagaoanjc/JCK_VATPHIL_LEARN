---
glightbox: false
hide:
  - toc
---

# Briefings

Here you will find aerodrome briefings for the airports within the Philippines. Select a highlighted aerodrome on the map to open its briefing or select a sector to see its area of responsibility.

<div id="vatphil-map"></div>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.css" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.js"></script>

<style>
#vatphil-map {
  width: 100%;
  height: 540px;
  border-radius: 8px;
  margin-top: 1rem;
  margin-bottom: 1.5rem;
  border: 1px solid rgba(255,255,255,0.08);
  position: relative;
  z-index: 0;
}

.vp-tooltip {
  background: #1a1a1a !important;
  border: 1px solid rgba(220, 80, 80, 0.5) !important;
  border-radius: 6px !important;
  box-shadow: 0 4px 20px rgba(0,0,0,0.5) !important;
  padding: 0 !important;
  white-space: nowrap;
}
.vp-tooltip::before { display: none !important; }
.vp-tt-inner { padding: 10px 13px 9px; }
.vp-tt-icao { font-family: monospace; font-size: 11px; color: #ff9999; letter-spacing: 0.08em; margin-bottom: 2px; }
.vp-tt-name { font-size: 13px; font-weight: 600; color: #f0f0f0; margin-bottom: 3px; line-height: 1.3; }
.vp-tt-type { font-size: 10px; color: #888; text-transform: uppercase; letter-spacing: 0.05em; margin-bottom: 7px; }
.vp-tt-hint { font-size: 10px; color: rgba(255,150,150,0.6); font-style: italic; }


.vp-right-controls {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.vp-sl-title {
  font-size: 9px;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: #666;
  margin-bottom: 6px;
  border-bottom: 1px solid #333;
  padding-bottom: 4px;
}
.vp-sl-empty {
  color: #444;
  font-size: 11px;
  font-style: italic;
}

.vp-sl-entry {
  margin-bottom: 8px;
  padding-bottom: 8px;
  border-bottom: 1px solid rgba(255,255,255,0.06);
  animation: vp-fadein 0.2s ease;
}
.vp-sl-entry:last-child {
  margin-bottom: 0;
  padding-bottom: 0;
  border-bottom: none;
}
@keyframes vp-fadein {
  from { opacity: 0; transform: translateY(-3px); }
  to   { opacity: 1; transform: translateY(0); }
}
.vp-sl-entry-primary .vp-sl-entry-id { color: #ff9999; }
.vp-sl-entry-overlap  .vp-sl-entry-id { color: #7dc4ff; }

.vp-sl-entry-id {
  font-family: monospace;
  font-size: 11px;
  font-weight: bold;
  letter-spacing: 0.08em;
  margin-bottom: 1px;
}
.vp-sl-entry-name {
  font-size: 11px;
  color: #bbb;
  margin-bottom: 4px;
}
.vp-sl-freq-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 10px;
  gap: 8px;
}
.vp-sl-freq-role { color: #666; }
.vp-sl-freq-val {
  font-family: monospace;
  color: #a8d4ff;
  font-weight: 600;
  cursor: pointer;
  border-radius: 3px;
  padding: 1px 3px;
  transition: background 0.15s;
}
.vp-sl-freq-val:hover {
  background: rgba(168,212,255,0.15);
}
.vp-sl-freq-val:hover::after {
  content: ' ✦';
  font-size: 8px;
  opacity: 0.5;
}


.vp-layer-control {
  background: #1a1a1a;
  border: 1px solid rgba(255,255,255,0.12);
  border-radius: 6px;
  padding: 8px 10px;
  font-size: 11px;
  color: #ccc;
  line-height: 1.8;
  min-width: 150px;
}
.vp-layer-control label { display: flex; align-items: center; gap: 7px; cursor: pointer; user-select: none; }
.vp-layer-control input { cursor: pointer; accent-color: #dc3232; }
.vp-layer-control .vp-swatch { display: inline-block; width: 18px; height: 3px; border-radius: 2px; flex-shrink: 0; }


.vp-daynight-control {
  background: #1a1a1a;
  border: 1px solid rgba(255,255,255,0.12);
  border-radius: 6px;
  overflow: hidden;
}
.vp-daynight-btn {
  display: flex; align-items: center; padding: 7px 11px; font-size: 11px;
  font-family: inherit; color: #ccc; background: transparent; border: none;
  cursor: pointer; transition: background 0.2s, color 0.2s; width: 100%;
}
.vp-daynight-btn:hover { background: rgba(255,255,255,0.07); color: #fff; }


.vp-copy-toast {
  position: fixed;
  bottom: 24px;
  left: 50%;
  transform: translateX(-50%) translateY(12px);
  background: #1a1a1a;
  border: 1px solid rgba(168,212,255,0.35);
  color: #a8d4ff;
  font-size: 11px;
  font-family: monospace;
  padding: 6px 14px;
  border-radius: 20px;
  pointer-events: none;
  opacity: 0;
  transition: opacity 0.2s, transform 0.2s;
  z-index: 9999;
}
.vp-copy-toast.show {
  opacity: 1;
  transform: translateX(-50%) translateY(0);
}

/* On-map sector labels */
.vp-sector-label {
  background: transparent;
  border: none;
  pointer-events: none;
}

.vp-sector-label.vp-overlap-label {
  pointer-events: auto !important;
  cursor: pointer;
}
.vp-sector-label-inner {
  text-align: center;
  line-height: 1.4;
  pointer-events: none;
  white-space: nowrap;
  padding: 5px 9px 6px;
  border-radius: 5px;
  backdrop-filter: blur(2px);
  transition: background 0.15s, border-color 0.15s;
}
.vp-sector-label-inner.primary {
  background: rgba(30, 8, 8, 0.72);
  border: 1px solid rgba(255, 100, 100, 0.45);
}
.vp-sector-label-inner.overlap {
  background: rgba(8, 18, 38, 0.72);
  border: 1px solid rgba(74, 158, 255, 0.45);
}
/* Hover state for overlap labels */
.vp-sector-label-inner.overlap.hovered {
  background: rgba(20, 50, 100, 0.85);
  border-color: rgba(100, 180, 255, 0.85);
  box-shadow: 0 0 10px rgba(74, 158, 255, 0.4);
}
.vp-sector-label-inner.primary .vp-lbl-id { color: #ff9999; }
.vp-sector-label-inner.overlap  .vp-lbl-id { color: #7dc4ff; }
.vp-sector-label-inner.overlap.hovered .vp-lbl-id { color: #a8d4ff; }
.vp-lbl-id {
  font-family: monospace;
  font-size: 11px;
  font-weight: bold;
  letter-spacing: 0.07em;
}
.vp-lbl-name {
  font-size: 10px;
  color: #ccd8ee;
}
.vp-lbl-freq {
  font-size: 10px;
  color: #a8d4ff;
  font-family: monospace;
  margin-top: 1px;
}
.vp-lbl-freq-role {
  color: #7a9abb;
  font-family: sans-serif;
}


.vp-marker-wrap {
  position: relative; width: 22px; height: 22px;
  display: flex; align-items: center; justify-content: center;
}
.vp-pulse {
  position: absolute; top: 50%; left: 50%;
  width: 12px; height: 12px; margin: -6px 0 0 -6px;
  border-radius: 50%; background: rgba(220, 50, 50, 0.5);
  animation: vp-pulse 2.2s ease-out infinite; pointer-events: none;
}
@keyframes vp-pulse {
  0%   { transform: scale(1);   opacity: 0.6; }
  70%  { transform: scale(2.4); opacity: 0; }
  100% { transform: scale(2.4); opacity: 0; }
}
</style>


<div id="vp-copy-toast" class="vp-copy-toast"></div>

<script>
(function () {
  var aerodromes = [
    { icao: "RPLL", name: "Ninoy Aquino Intl",  type: "International",     lat: 14.5086, lon: 121.0197 },
    { icao: "RPLC", name: "Clark Intl",          type: "International",     lat: 15.1860, lon: 120.5600 },
    { icao: "RPVE", name: "Caticlan",             type: "Principal Class 1", lat: 11.9246, lon: 121.9530 },
    { icao: "RPVK", name: "Kalibo",               type: "Principal Class 1", lat: 11.6795, lon: 122.3760 },
    { icao: "RPVR", name: "Roxas",                type: "Principal Class 1", lat: 11.5977, lon: 122.7517 },
    { icao: "RPVM", name: "Mactan-Cebu Intl",     type: "International",     lat: 10.3075, lon: 123.9794 },
    { icao: "RPMD", name: "Francisco Bangoy",     type: "International",     lat:  7.1255, lon: 125.6458 },
  ];

  var SECTOR_INFO = {
    "MNL_C":     { name: "Manila Inner Combined",            freqs: [{ role: "Primary", mhz: "132.075" }] },
    "MNL_CTR":   { name: "Manila",                  freqs: [{ role: "Primary", mhz: "119.300" }] },
    "MNL_N_CTR": { name: "Manila North Center",     freqs: [{ role: "Primary", mhz: "126.575" }] },
    "MNL_S_CTR": { name: "Manila South Center",     freqs: [{ role: "Primary", mhz: "133.500" }] },
    "MNL_2_CTR": { name: "Manila Split South 2",    freqs: [{ role: "Primary", mhz: "124.950" }] },
    "MNL_N1":    { name: "Manila Combined North",   freqs: [{ role: "Primary", mhz: "129.000" }] },
    "MNL_S1":    { name: "Manila Combined South",   freqs: [{ role: "Primary", mhz: "131.500" }] },
    "MNL_N":     { name: "Manila North",            freqs: [{ role: "Primary", mhz: "126.575" }] },
    "MNL_S":     { name: "Manila South",            freqs: [{ role: "Primary", mhz: "133.500" }] },
    "MNL_2":     { name: "Manila Split South",      freqs: [{ role: "Primary", mhz: "124.950" }] },
    "MNL_NW":    { name: "Manila North West",       freqs: [{ role: "Primary", mhz: "128.700" }] },
    "MNL_NE":    { name: "Manila North East",       freqs: [{ role: "Primary", mhz: "132.500" }] },
    "MNL_CN":    { name: "Manila Central North",    freqs: [{ role: "Primary", mhz: "120.500" }] },
    "MNL_CE":    { name: "Manila Central East",     freqs: [{ role: "Primary", mhz: "128.750" }] },
    "MNL_CS":    { name: "Manila Central South",    freqs: [{ role: "Primary", mhz: "125.700" }] },
    "MNL_CW":    { name: "Manila Central West",     freqs: [{ role: "Primary", mhz: "132.700" }] },
    "MNL_W":     { name: "Manila West",             freqs: [{ role: "Primary", mhz: "118.900" }] },
    "MNL_SW":    { name: "Manila South West",       freqs: [{ role: "Primary", mhz: "124.900" }] },
    "MNL_SE":    { name: "Manila South East",       freqs: [{ role: "Primary", mhz: "125.750" }] }
  };

  var sectors = [
    {"type":"Feature","properties":{"id":"MNL_N1"},"geometry":{"type":"MultiPolygon","coordinates":[[[[121.021722,14.507972],[120.397448,14.636917],[119.74649,14.84913],[117.994167,16.335556],[119.70646,17.76008],[120.0,18.0],[120.887532,17.806853],[122.6,17.411111],[125.353333,16.743611],[124.692222,13.864722],[122.037395,14.3389],[121.021722,14.507972]]]]}},
    {"type":"Feature","properties":{"id":"MNL_S1"},"geometry":{"type":"MultiPolygon","coordinates":[[[[121.021722,14.507972],[122.037395,14.3389],[124.692222,13.864722],[124.372217,12.3719],[124.256944,12.371944],[123.630556,12.369167],[122.748611,12.364444],[122.409853,12.368143],[121.697456,12.364799],[121.001387,12.35862],[120.7,12.35],[120.5425,11.180278],[120.281389,10.512222],[119.203611,10.679444],[117.256389,11.708333],[116.762778,13.725556],[116.461667,15.0],[117.994167,16.335556],[119.74649,14.84913],[120.397448,14.636917],[121.021722,14.507972]]]]}},
    {"type":"Feature","properties":{"id":"MNL_2"},"geometry":{"type":"MultiPolygon","coordinates":[[[[121.021722,14.507972],[122.037395,14.3389],[124.692222,13.864722],[130.0,13.0],[130.0,7.0],[132.533333,4.0],[124.645833,4.0],[121.25,4.0],[120.0,4.0],[117.9,7.0],[120.281389,10.512222],[120.5425,11.180278],[120.7,12.35],[120.8724,13.51758],[121.021722,14.507972]]]]}},
    {"type":"Feature","properties":{"id":"MNL_CE"},"geometry":{"type":"MultiPolygon","coordinates":[[[[122.6,17.411111],[125.353333,16.743611],[124.692222,13.864722],[122.037395,14.3389],[121.021722,14.507972],[121.49422,15.40169],[122.6,17.411111]]]]}},
    {"type":"Feature","properties":{"id":"MNL_CN"},"geometry":{"type":"MultiPolygon","coordinates":[[[[120.0,18.0],[120.887532,17.806853],[122.6,17.411111],[121.49422,15.40169],[121.021722,14.507972],[120.397448,14.636917],[119.74649,14.84913],[117.994167,16.335556],[119.70646,17.76008],[120.0,18.0]]]]}},
    {"type":"Feature","properties":{"id":"MNL_CS"},"geometry":{"type":"MultiPolygon","coordinates":[[[[121.021722,14.507972],[122.037395,14.3389],[124.692222,13.864722],[124.372217,12.3719],[124.256944,12.371944],[123.630556,12.369167],[122.748611,12.364444],[122.409853,12.368143],[121.697456,12.364799],[121.001387,12.35862],[120.7,12.35],[120.8724,13.51758],[121.021722,14.507972]]]]}},
    {"type":"Feature","properties":{"id":"MNL_CW"},"geometry":{"type":"MultiPolygon","coordinates":[[[[121.021722,14.507972],[120.8724,13.51758],[120.7,12.35],[120.5425,11.180278],[120.281389,10.512222],[119.203611,10.679444],[117.256389,11.708333],[116.762778,13.725556],[116.461667,15.0],[117.994167,16.335556],[119.74649,14.84913],[120.397448,14.636917],[121.021722,14.507972]]]]}},
    {"type":"Feature","properties":{"id":"MNL_N"},"geometry":{"type":"MultiPolygon","coordinates":[[[[117.5,21.0],[118.189722,21.0],[121.3,21.0],[121.5,21.0],[124.0,21.0],[126.852778,21.000444],[130.0,21.0],[130.0,13.0],[124.692222,13.864722],[122.037395,14.3389],[121.021722,14.507972],[120.397448,14.636917],[119.74649,14.84913],[117.994167,16.335556],[116.461667,15.0],[114.0,12.5],[113.996608,15.734407],[114.0,16.666667],[115.537683,18.598851],[116.123518,19.32391],[117.5,21.0]]]]}},
    {"type":"Feature","properties":{"id":"MNL_NE"},"geometry":{"type":"MultiPolygon","coordinates":[[[[124.692222,13.864722],[125.353333,16.743611],[122.6,17.411111],[124.0,21.0],[126.852778,21.000444],[130.0,21.0],[130.0,13.0],[124.692222,13.864722]]]]}},
    {"type":"Feature","properties":{"id":"MNL_NW"},"geometry":{"type":"MultiPolygon","coordinates":[[[[114.0,12.5],[113.996608,15.734407],[114.0,16.666667],[115.537683,18.598851],[116.123518,19.32391],[117.5,21.0],[118.189722,21.0],[121.3,21.0],[121.5,21.0],[124.0,21.0],[122.6,17.411111],[120.887532,17.806853],[120.0,18.0],[119.70646,17.76008],[117.994167,16.335556],[116.461667,15.0],[114.0,12.5]]]]}},
    {"type":"Feature","properties":{"id":"MNL_SE"},"geometry":{"type":"MultiPolygon","coordinates":[[[[122.409853,12.368143],[122.748611,12.364444],[123.630556,12.369167],[124.256944,12.371944],[124.372217,12.3719],[124.692222,13.864722],[130.0,13.0],[130.0,7.0],[132.533333,4.0],[124.645833,4.0],[124.492099,5.505],[124.072222,7.963333],[124.0,9.028611],[123.962389,9.159639],[123.988296,10.313568],[123.471191,10.965559],[123.144311,11.417421],[122.409853,12.368143]]]]}},
    {"type":"Feature","properties":{"id":"MNL_W"},"geometry":{"type":"MultiPolygon","coordinates":[[[[116.461667,15.0],[116.762778,13.725556],[117.256389,11.708333],[119.203611,10.679444],[120.281389,10.512222],[117.9,7.0],[117.5,7.5],[116.5,8.416667],[116.194461,8.670099],[114.0,10.5],[114.0,12.366667],[114.0,12.5],[116.461667,15.0]]]]}},
    {"type":"Feature","properties":{"id":"MNL_S"},"geometry":{"type":"MultiPolygon","coordinates":[[[[124.692222,13.864722],[122.037395,14.3389],[121.021722,14.507972],[120.397448,14.636917],[119.74649,14.84913],[117.994167,16.335556],[116.461667,15.0],[114.0,12.5],[114.0,12.366667],[114.0,10.5],[116.194461,8.670099],[116.5,8.416667],[117.5,7.5],[117.9,7.0],[120.0,4.0],[121.25,4.0],[124.645833,4.0],[132.533333,4.0],[130.0,7.0],[130.0,13.0],[124.692222,13.864722]]]]}},
    {"type":"Feature","properties":{"id":"MNL_SW"},"geometry":{"type":"MultiPolygon","coordinates":[[[[120.7,12.35],[121.001387,12.35862],[121.697456,12.364799],[122.409853,12.368143],[123.144311,11.417421],[123.471191,10.965559],[123.988296,10.313568],[123.962389,9.159639],[124.0,9.036111],[124.071667,7.972222],[124.4921,5.505],[124.645833,4.0],[121.25,4.0],[120.0,4.0],[117.9,7.0],[120.281389,10.512222],[120.5425,11.180278],[120.7,12.35]]]]}}  ,
    {"type":"Feature","properties":{"id":"MNL_C"},"geometry":{"type":"MultiPolygon","coordinates":[[[[120.0,18.0],[122.6,17.411111],[125.353333,16.743611],[124.692222,13.864722],[124.372217,12.3719],[124.256944,12.371944],[123.630556,12.369167],[122.748611,12.364444],[122.409853,12.368143],[121.697456,12.364799],[121.001387,12.35862],[120.7,12.35],[120.5425,11.180278],[120.281389,10.512222],[119.203611,10.679444],[117.256389,11.708333],[116.762778,13.725556],[116.461667,15.0],[117.994167,16.335556],[120.0,18.0]]]]}}
  ];

  var TILES = {
    night: { url: 'https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png',             label: 'Day mode' },
    day:   { url: 'https://{s}.basemaps.cartocdn.com/rastertiles/voyager/{z}/{x}/{y}{r}.png', label: 'Night mode' }
  };
  var ATT = '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> &copy; <a href="https://carto.com/">CARTO</a>';

  function makeIcon() {
    var s = 22;
    var svg = '<svg xmlns="http://www.w3.org/2000/svg" width="' + s + '" height="' + s + '" viewBox="0 0 ' + s + ' ' + s + '" style="display:block;">'
      + '<circle cx="' + (s/2) + '" cy="' + (s/2) + '" r="8" fill="rgba(220,50,50,0.9)" stroke="#ff6666" stroke-width="1.5"/>'
      + '<circle cx="' + (s/2) + '" cy="' + (s/2) + '" r="3.5" fill="#fff" opacity="0.95"/>'
      + '</svg>';
    return L.divIcon({
      html: '<div class="vp-marker-wrap"><div class="vp-pulse"></div>' + svg + '</div>',
      className: '',
      iconSize: [s, s],
      iconAnchor: [s/2, s/2],
      tooltipAnchor: [0, -14]
    });
  }

  function pointInRing(latlng, ring) {
    var x = latlng.lng, y = latlng.lat, inside = false;
    for (var i = 0, j = ring.length - 1; i < ring.length; j = i++) {
      var xi = ring[i][0], yi = ring[i][1], xj = ring[j][0], yj = ring[j][1];
      if (((yi > y) !== (yj > y)) && (x < (xj - xi) * (y - yi) / (yj - yi) + xi)) inside = !inside;
    }
    return inside;
  }

  function pointInFeature(latlng, feature) {
    var coords = feature.geometry.coordinates;
    for (var p = 0; p < coords.length; p++)
      for (var r = 0; r < coords[p].length; r++)
        if (pointInRing(latlng, coords[p][r])) return true;
    return false;
  }

  function getArea(feature) {
    var coords = feature.geometry.coordinates;
    var area = 0;
    for (var p = 0; p < coords.length; p++) {
      for (var r = 0; r < coords[p].length; r++) {
        var ring = coords[p][r];
        var sum = 0;
        for (var i = 0; i < ring.length - 1; i++) {
          sum += ring[i][0] * ring[i+1][1] - ring[i+1][0] * ring[i][1];
        }
        area += Math.abs(sum) / 2;
      }
    }
    return area;
  }

  sectors.sort(function(a, b) { return getArea(b) - getArea(a); });

  function getCentroid(feature) {
    var coords = feature.geometry.coordinates;
    var bestRing = null, bestArea = -1;
    for (var p = 0; p < coords.length; p++) {
      for (var r = 0; r < coords[p].length; r++) {
        var ring = coords[p][r];
        var sum = 0;
        for (var i = 0; i < ring.length - 1; i++)
          sum += ring[i][0] * ring[i+1][1] - ring[i+1][0] * ring[i][1];
        var a = Math.abs(sum) / 2;
        if (a > bestArea) { bestArea = a; bestRing = ring; }
      }
    }
    if (!bestRing) return L.latLng(0, 0);
    var sumLon = 0, sumLat = 0;
    for (var k = 0; k < bestRing.length - 1; k++) {
      sumLon += bestRing[k][0];
      sumLat += bestRing[k][1];
    }
    var n = bestRing.length - 1;
    return L.latLng(sumLat / n, sumLon / n);
  }

  function buildPanelHTML(matched) {
    var html = '';
    for (var i = 0; i < matched.length; i++) {
      var id = matched[i].properties.id;
      var info = SECTOR_INFO[id] || { name: id, freqs: [] };
      var isPrimary = (i === 0);
      var entryClass = isPrimary ? 'vp-sl-entry vp-sl-entry-primary' : 'vp-sl-entry vp-sl-entry-overlap';
      var freqRows = '';
      for (var k = 0; k < info.freqs.length; k++) {
        var mhz = info.freqs[k].mhz;
        freqRows += '<div class="vp-sl-freq-row">'
          + '<span class="vp-sl-freq-role">' + info.freqs[k].role + '</span>'
          + '<span class="vp-sl-freq-val" onclick="(function(){' 
          + 'var t=document.getElementById(\'vp-copy-toast\');'
          + 'try{navigator.clipboard.writeText(\'' + mhz + '\').then(function(){'
          + 't.textContent=\'Copied ' + mhz + ' MHz\';t.classList.add(\'show\');'
          + 'setTimeout(function(){t.classList.remove(\'show\');},1800);});}catch(e){'
          + 't.textContent=\'' + mhz + ' MHz\';t.classList.add(\'show\');'
          + 'setTimeout(function(){t.classList.remove(\'show\');},1800);}})();" '
          + 'title="Click to copy">'
          + mhz + ' MHz</span>'
          + '</div>';
      }
      if (!freqRows) freqRows = '<span style="font-size:10px;color:#444">No frequencies listed</span>';
      html += '<div class="' + entryClass + '">'
            + '<div class="vp-sl-entry-id">' + id + '</div>'
            + '<div class="vp-sl-entry-name">' + info.name + '</div>'
            + freqRows
            + '</div>';
    }
    return html;
  }

  function updatePanel(matched) {
    var panel = document.getElementById('vp-sector-entries');
    if (!panel) return;
    if (!matched || matched.length === 0) {
      panel.innerHTML = '<div class="vp-sl-empty">Click a sector</div>';
    } else {
      panel.innerHTML = buildPanelHTML(matched);
    }
  }

  function initMap() {
    if (!window.L) { setTimeout(initMap, 100); return; }

    var map = L.map('vatphil-map', { center: [12.0, 122.5], zoom: 6, zoomControl: true, attributionControl: true });

    var currentMode = 'night';
    var tileLayer = L.tileLayer(TILES.night.url, { attribution: ATT, subdomains: 'abcd', maxZoom: 19 }).addTo(map);

    var highlightLayers = [];
    
    var overlapHighlightMap = {};

    function clearHighlights() {
      for (var i = 0; i < highlightLayers.length; i++) { try { map.removeLayer(highlightLayers[i]); } catch(e){} }
      highlightLayers = [];
      overlapHighlightMap = {};
    }

    map.on('click', function() {
      clearHighlights();
      updatePanel(null);
    });

    var sectorLayerGroup = L.geoJSON({ type: 'FeatureCollection', features: sectors }, {
      style: { color: '#4a9eff', weight: 1, opacity: 0.45, fillColor: '#4a9eff', fillOpacity: 0.03, dashArray: '5, 6' },
      onEachFeature: function(feature, layer) {
        layer.on('mouseover', function() {
          layer.setStyle({ fillOpacity: 0.15, opacity: 0 });
        });
        layer.on('mouseout', function() {
          layer.setStyle({ fillOpacity: 0.03, opacity: 0.45 });
        });
        layer.on('click', function(e) {
          L.DomEvent.stopPropagation(e);
          var pt = e.latlng;

          var matched = sectors.filter(function(f) { return pointInFeature(pt, f); });
          clearHighlights();

          if (matched.length === 0) {
            updatePanel(null);
            return;
          }

          matched.sort(function(a, b) { return getArea(a) - getArea(b); });

          var primaryFeature = matched[0];

          
          matched.forEach(function(f) {
            if (f.properties.id !== primaryFeature.properties.id) {
              var hl = L.geoJSON(f, {
                style: { stroke: true, color: '#4a9eff', weight: 2, fillColor: '#1e3a8a', fillOpacity: 0.35 },
                interactive: false
              }).addTo(map);
              highlightLayers.push(hl);
              overlapHighlightMap[f.properties.id] = hl;
            }
          });

          
          var hlClicked = L.geoJSON(primaryFeature, {
            style: { stroke: true, color: '#ff6666', weight: 2, fillColor: '#dc3232', fillOpacity: 0.45 },
            interactive: false
          }).addTo(map);
          highlightLayers.push(hlClicked);

          // Label placement
          var LBL_PX_W = 160;
          var LBL_PX_H = 58;
          var LBL_PAD  = 8;

          var placedBoxes = [];

          function pxOverlaps(ax, ay, bx, by) {
            return ax < bx + LBL_PX_W + LBL_PAD &&
                   ax + LBL_PX_W + LBL_PAD > bx &&
                   ay < by + LBL_PX_H + LBL_PAD &&
                   ay + LBL_PX_H + LBL_PAD > by;
          }

          function findFreePixelPos(cx, cy) {
            var step = LBL_PX_H + LBL_PAD;
            var attempts = [
              [cx, cy],
              [cx, cy - step],
              [cx, cy + step],
              [cx + LBL_PX_W + LBL_PAD, cy],
              [cx - LBL_PX_W - LBL_PAD, cy],
              [cx, cy - step * 2],
              [cx, cy + step * 2],
              [cx + LBL_PX_W + LBL_PAD, cy - step],
              [cx + LBL_PX_W + LBL_PAD, cy + step],
              [cx - LBL_PX_W - LBL_PAD, cy - step],
              [cx - LBL_PX_W - LBL_PAD, cy + step],
              [cx, cy - step * 3],
              [cx, cy + step * 3],
            ];
            for (var i = 0; i < attempts.length; i++) {
              var tx = attempts[i][0], ty = attempts[i][1];
              var free = true;
              for (var k = 0; k < placedBoxes.length; k++) {
                if (pxOverlaps(tx, ty, placedBoxes[k].x, placedBoxes[k].y)) {
                  free = false; break;
                }
              }
              if (free) return { x: tx, y: ty };
            }
            return { x: attempts[attempts.length - 1][0], y: attempts[attempts.length - 1][1] };
          }

          function pixelToLatLng(px, py) {
            return map.containerPointToLatLng(L.point(px, py));
          }

          function makeLabelMarker(px, py, id, info, isPrimary) {
            var freqLine = info.freqs.length
              ? '<div class="vp-lbl-freq"><span class="vp-lbl-freq-role">' + info.freqs[0].role + '</span> ' + info.freqs[0].mhz + ' MHz</div>'
              : '';
            var cls = isPrimary ? 'primary' : 'overlap';
            var markerClass = isPrimary ? 'vp-sector-label' : 'vp-sector-label vp-overlap-label';
            var marker = L.marker(pixelToLatLng(px, py), {
              icon: L.divIcon({
                className: markerClass,
                html: '<div class="vp-sector-label-inner ' + cls + '" data-sector-id="' + id + '">'
                    + '<div class="vp-lbl-id">' + id + '</div>'
                    + '<div class="vp-lbl-name">' + info.name + '</div>'
                    + freqLine
                    + '</div>',
                iconSize: [LBL_PX_W, LBL_PX_H],
                iconAnchor: [0, 0]
              }),
              
              interactive: !isPrimary
            });

            // Sector fill
            if (!isPrimary) {
              marker.on('mouseover', function() {
                
                var el = marker.getElement();
                if (el) {
                  var inner = el.querySelector('.vp-sector-label-inner');
                  if (inner) inner.classList.add('hovered');
                }
                
                var hl = overlapHighlightMap[id];
                if (hl) {
                  hl.setStyle({ color: '#64b4ff', weight: 2.5, fillColor: '#2a5abf', fillOpacity: 0.55 });
                }
              });
              marker.on('mouseout', function() {
                
                var el = marker.getElement();
                if (el) {
                  var inner = el.querySelector('.vp-sector-label-inner');
                  if (inner) inner.classList.remove('hovered');
                }
                
                var hl = overlapHighlightMap[id];
                if (hl) {
                  hl.setStyle({ color: '#4a9eff', weight: 2, fillColor: '#1e3a8a', fillOpacity: 0.35 });
                }
              });
            }

            return marker;
          }

          // Primary label at click point
          var pInfo = SECTOR_INFO[primaryFeature.properties.id] || { name: primaryFeature.properties.id, freqs: [] };
          var clickPx = map.latLngToContainerPoint(pt);
          var pStartX = clickPx.x - LBL_PX_W / 2;
          var pStartY = clickPx.y - LBL_PX_H / 2;
          var pPos = findFreePixelPos(pStartX, pStartY);
          placedBoxes.push(pPos);
          var pLbl = makeLabelMarker(pPos.x, pPos.y, primaryFeature.properties.id, pInfo, true);
          pLbl.addTo(map);
          highlightLayers.push(pLbl);

          // Overlap labels at each sector's centroid
          matched.forEach(function(f) {
            if (f.properties.id === primaryFeature.properties.id) return;
            var info = SECTOR_INFO[f.properties.id] || { name: f.properties.id, freqs: [] };
            var centroid = getCentroid(f);
            var cPx = map.latLngToContainerPoint(centroid);
            var startX = cPx.x - LBL_PX_W / 2;
            var startY = cPx.y - LBL_PX_H / 2;
            var pos = findFreePixelPos(startX, startY);
            placedBoxes.push(pos);
            var lbl = makeLabelMarker(pos.x, pos.y, f.properties.id, info, false);
            lbl.addTo(map);
            highlightLayers.push(lbl);
          });

          updatePanel(matched);
        });
      }
    }).addTo(map);

    var markerLayer = L.layerGroup();
    aerodromes.forEach(function(a) {
      var marker = L.marker([a.lat, a.lon], { icon: makeIcon() });
      marker.bindTooltip(
        '<div class="vp-tt-inner"><div class="vp-tt-icao">' + a.icao + '</div>'
        + '<div class="vp-tt-name">' + a.name + '</div>'
        + '<div class="vp-tt-type">' + a.type + '</div>'
        + '<div class="vp-tt-hint">Click to open briefing</div></div>',
        { className: 'vp-tooltip', direction: 'top', offset: [0, -4], permanent: false, sticky: false }
      );
      marker.on('click', function() { window.location.href = 'https://learn.vatphil.com/briefings/' + a.icao; });
      markerLayer.addLayer(marker);
    });
    markerLayer.addTo(map);

    // Day/Night toggle
    var DayNightControl = L.Control.extend({
      onAdd: function() {
        var div = L.DomUtil.create('div', 'vp-daynight-control');
        var btn = L.DomUtil.create('button', 'vp-daynight-btn', div);
        btn.textContent = 'Day mode';
        L.DomEvent.disableClickPropagation(div);
        L.DomEvent.disableScrollPropagation(div);
        L.DomEvent.on(btn, 'click', function() {
          var next = currentMode === 'night' ? 'day' : 'night';
          map.removeLayer(tileLayer);
          tileLayer = L.tileLayer(TILES[next].url, { attribution: ATT, subdomains: 'abcd', maxZoom: 19 });
          tileLayer.addTo(map); tileLayer.bringToBack();
          currentMode = next;
          btn.textContent = TILES[next].label;
        });
        return div;
      }
    });
    new DayNightControl({ position: 'topleft' }).addTo(map);

    new CombinedControl({ position: 'topright' }).addTo(map);

    setTimeout(function() {
      document.getElementById('vp-toggle-sectors').addEventListener('change', function(e) {
        e.target.checked ? map.addLayer(sectorLayerGroup) : map.removeLayer(sectorLayerGroup);
      });
      document.getElementById('vp-toggle-aero').addEventListener('change', function(e) {
        e.target.checked ? map.addLayer(markerLayer) : map.removeLayer(markerLayer);
      });
    }, 200);
  }

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', initMap);
  } else {
    initMap();
  }
})();
</script>

<style>
  .grid.cards p strong { color: #000000; }
</style>

<div class="grid cards" style="text-align: center" markdown>

-   **Luzon**

    ---
    [RPLL →](https://learn.vatphil.com/briefings/RPLL/)

    [RPLC →](https://learn.vatphil.com/briefings/RPLC/)

-   **Visayas**

    ---
    [RPVM →](https://learn.vatphil.com/briefings/RPVM/)

    [RPVE →](https://learn.vatphil.com/briefings/RPVE/)

    [RPVK →](https://learn.vatphil.com/briefings/RPVK/)

    [RPVR →](https://learn.vatphil.com/briefings/RPVR/)

-   **Mindanao**

    ---
    [RPMD →](https://learn.vatphil.com/briefings/RPMD/)

-   **RPHI**

    ---

    [FIR →](https://learn.vatphil.com/briefings/RPHI/)

</div>