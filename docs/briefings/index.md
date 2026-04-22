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

  var tmas = [
    {"type":"Feature","properties":{"id":"RPLL","name":"Manila Approach"},"geometry":{"type":"MultiPolygon","coordinates":[[[[119.882434,14.059693],[119.734521,14.618617],[119.674722,14.818611],[119.74649,14.84913],[120.008056,14.956944],[120.251111,15.156389],[120.398004,15.308399],[120.398004,15.392084],[120.483145,15.445909],[120.452317,15.684286],[120.721079,15.687253],[120.817625,15.492158],[120.907409,15.505684],[120.998085,15.511586],[121.088951,15.509814],[121.179306,15.500383],[121.268448,15.483365],[121.355698,15.458891],[121.440369,15.42715],[121.49422,15.40169],[121.521828,15.388387],[121.599434,15.3429],[121.672599,15.291041],[121.740768,15.233209],[121.803413,15.169847],[121.860061,15.101444],[121.910278,15.028524],[121.916529,15.008392],[122.501426,14.765149],[122.501426,14.665156],[122.052517,14.496893],[122.04779,14.41],[122.037401,14.338892],[122.035278,14.32382],[122.015068,14.23903],[121.987328,14.156265],[121.952286,14.076154],[121.910202,13.999304],[121.861404,13.926294],[121.806267,13.857677],[121.745216,13.793972],[121.678711,13.735657],[121.607261,13.683173],[121.53141,13.636915],[121.451721,13.597232],[121.368797,13.564421],[121.283272,13.538731],[121.195785,13.520353],[121.106994,13.509427],[121.017563,13.506034],[120.928169,13.510199],[120.8724,13.51758],[120.839478,13.521893],[120.752159,13.541026],[120.666878,13.567455],[120.584259,13.600981],[120.504936,13.641352],[120.429504,13.688264],[120.358536,13.741363],[120.339167,13.756897],[120.051421,13.756895],[119.998088,13.843556],[120.103786,14.038161],[119.882434,14.059693]]]]}},
    {"type":"Feature","properties":{"id":"RPLI","name":"Laoag Approach"},"geometry":{"type":"MultiPolygon","coordinates":[[[[120.774269,19.068987],[120.742050,18.637783],[120.783577,18.618612],[120.823181,18.596075],[120.860542,18.570345],[120.895393,18.541620],[120.927452,18.510118],[120.956482,18.476084],[120.982261,18.439775],[121.004600,18.401472],[121.023323,18.361463],[121.038299,18.320059],[121.049408,18.277571],[121.056564,18.234325],[121.059730,18.190653],[121.058876,18.146883],[121.054008,18.103351],[121.045174,18.060388],[121.032440,18.018320],[121.015899,17.977466],[120.995697,17.938135],[120.971969,17.900627],[120.944916,17.865227],[120.914726,17.832199],[120.886976,17.806853],[120.881645,17.801798],[120.845917,17.774250],[120.807808,17.749763],[120.787788,17.742382],[120.881966,17.316666],[120.802131,17.293268],[120.720482,17.276567],[120.637634,17.266691],[120.554199,17.263712],[120.470802,17.267654],[120.388084,17.278486],[120.306648,17.296129],[120.227119,17.320448],[120.150085,17.351261],[120.076126,17.388338],[120.005798,17.431400],[119.939644,17.480123],[119.878151,17.534139],[119.821819,17.593046],[119.770996,17.656397],[119.726150,17.723715],[119.706460,17.760080],[119.687607,17.794491],[119.655655,17.868193],[119.630546,17.944259],[119.612488,18.022118],[119.601616,18.101175],[119.598030,18.180834],[119.601761,18.260484],[119.612793,18.339525],[119.631058,18.417349],[119.656418,18.493364],[119.688690,18.566988],[119.727638,18.637659],[119.772972,18.704834],[119.824348,18.767998],[119.881386,18.826666],[119.943641,18.880384],[120.010651,18.928741],[120.081894,18.971363],[120.156822,19.007921],[120.234871,19.038134],[120.315429,19.061768],[120.397880,19.078640],[120.481583,19.088619],[120.565895,19.091629],[120.650162,19.087644],[120.733726,19.076700],[120.774269,19.068987]]]]}},
    {"type":"Feature","properties":{"id":"RPLC","name":"Clark Approach"},"geometry":{"type":"MultiPolygon","coordinates":[[[[120.103786,14.038161],[119.882434,14.059693],[119.734521,14.618617],[119.674722,14.818611],[119.746490,14.849130],[120.008056,14.956944],[120.251111,15.156389],[120.398004,15.308399],[120.398004,15.392084],[120.483145,15.445909],[120.452317,15.684286],[120.721079,15.687253],[120.817625,15.492158],[120.817625,15.143624],[120.817625,15.117865],[120.722345,14.949378],[120.398004,14.794499],[120.396354,14.784292],[120.397026,14.769802],[120.396378,14.755310],[120.398004,14.636917],[120.398004,14.577861],[120.336860,14.466649],[120.103786,14.038161]]]]}},
    {"type":"Feature","properties":{"id":"RPLK","name":"Bicol Approach"},"geometry":{"type":"MultiPolygon","coordinates":[[[[123.630556,12.369167],[122.436944,13.668333],[122.584167,13.729722],[122.778056,13.816667],[122.995833,13.910833],[123.310150,14.046633],[124.377700,13.660767],[124.372217,12.371900],[124.257222,12.372222],[123.630556,12.369167]]]]}},
    {"type":"Feature","properties":{"id":"RPMD","name":"Davao Approach"},"geometry":{"type":"MultiPolygon","coordinates":[[[[125.432222,6.715278],[124.921111,6.904722],[124.847500,7.261944],[124.819258,7.968560],[124.973611,7.968611],[125.698328,7.968560],[126.153724,7.277650],[126.153724,6.715406],[125.432222,6.715278]]]]}},
    {"type":"Feature","properties":{"id":"RPME","name":"Butuan Approach"},"geometry":{"type":"MultiPolygon","coordinates":[[[[126.501404,8.529500],[125.526944,8.525833],[124.960558,8.467239],[124.756152,8.855566],[124.752900,8.878858],[124.749664,8.944118],[124.752197,9.009410],[124.760483,9.074234],[124.774460,9.138099],[124.794029,9.200517],[124.819038,9.261011],[124.849312,9.319122],[124.884605,9.374405],[124.924660,9.426439],[124.969177,9.474825],[125.017807,9.519194],[125.070183,9.559207],[125.125908,9.594557],[125.184562,9.624975],[125.245689,9.650228],[125.308815,9.670120],[125.373474,9.684501],[125.381134,9.684712],[125.385217,10.102717],[126.501404,10.090117],[126.501404,8.529500]]]]}},
    {"type":"Feature","properties":{"id":"RPMR","name":"Tambler Approach"},"geometry":{"type":"MultiPolygon","coordinates":[[[[125.163056,5.498611],[124.811850,5.498611],[124.547778,6.229444],[124.525867,6.290217],[124.564567,6.621967],[124.613056,6.668889],[124.921111,6.904722],[125.432222,6.715278],[125.469433,6.701700],[125.595233,5.498611],[125.163056,5.498611]]]]}},
    {"type":"Feature","properties":{"id":"RPMY","name":"Laguindingan Approach"},"geometry":{"type":"MultiPolygon","coordinates":[[[[124.000000,9.031667],[124.666667,9.025278],[124.756152,8.855566],[124.960558,8.467239],[125.023333,8.343333],[124.973611,7.968611],[124.819258,7.968560],[124.071667,7.965000],[124.000000,9.031667]]]]}},
    {"type":"Feature","properties":{"id":"RPVA","name":"Tacloban Approach"},"geometry":{"type":"MultiPolygon","coordinates":[[[[124.257222,12.372222],[124.257500,11.860278],[124.240929,11.100704],[124.310379,11.075983],[124.377357,11.045401],[124.441353,11.009194],[124.501877,10.967638],[124.558456,10.921052],[124.610672,10.869792],[124.658119,10.814252],[124.700447,10.754855],[124.737320,10.692056],[125.109444,10.821667],[125.777222,10.938333],[125.924450,10.943483],[125.924450,12.363933],[125.360000,12.367778],[124.372217,12.371900],[124.257222,12.372222]]]]}},
    {"type":"Feature","properties":{"id":"RPVB","name":"Bacolod Approach"},"geometry":{"type":"MultiPolygon","coordinates":[[[[122.985000,9.790556],[122.384728,9.998803],[122.068426,10.765417],[122.350940,11.162088],[123.144311,11.417431],[123.471191,10.965559],[123.414801,10.918746],[123.362808,10.867275],[123.315590,10.811543],[123.273514,10.751977],[123.236901,10.689031],[123.206024,10.623185],[123.181114,10.554943],[123.162361,10.484825],[123.149894,10.413367],[123.143814,10.341113],[123.144167,10.268611],[123.102222,10.142778],[122.985000,9.790556]]]]}},
    {"type":"Feature","properties":{"id":"RPVK","name":"Kalibo Approach"},"geometry":{"type":"MultiPolygon","coordinates":[[[[121.697456,12.364244],[122.409853,12.369254],[123.144310,11.417426],[122.351051,11.162088],[121.700108,11.816320],[121.697456,12.364244]]]]}},
    {"type":"Feature","properties":{"id":"RPVM","name":"Mactan Approach"},"geometry":{"type":"MultiPolygon","coordinates":[[[[123.703850,9.081767],[123.301111,9.116944],[122.884000,9.463650],[122.985000,9.790556],[123.102222,10.142778],[123.144167,10.268611],[123.143814,10.341113],[123.149894,10.413367],[123.162361,10.484826],[123.181114,10.554943],[123.206024,10.623185],[123.236900,10.689031],[123.273514,10.751977],[123.315590,10.811543],[123.362808,10.867275],[123.414802,10.918745],[123.471192,10.965558],[123.531540,11.007358],[123.595382,11.043824],[123.662231,11.074676],[123.731540,11.099688],[123.802894,11.118635],[123.875633,11.131446],[123.949226,11.137891],[124.023124,11.138039],[124.096741,11.131851],[124.169532,11.119374],[124.240929,11.100704],[124.310379,11.075983],[124.377357,11.045401],[124.441353,11.009194],[124.501877,10.967638],[124.558456,10.921052],[124.610672,10.869792],[124.658119,10.814252],[124.700447,10.754855],[124.737320,10.692056],[124.768478,10.626336],[124.793671,10.558195],[124.812721,10.488154],[124.825477,10.416747],[124.831863,10.344517],[124.831817,10.271877],[124.825355,10.199794],[124.812530,10.128399],[124.793434,10.058378],[124.768226,9.990258],[124.737100,9.924559],[124.700287,9.861777],[124.658081,9.802389],[124.610794,9.746846],[124.558792,9.695567],[124.502472,9.648942],[124.442261,9.607322],[124.378608,9.571022],[124.312004,9.540317],[124.243000,9.515440],[124.172000,9.496578],[124.099000,9.483873],[124.046944,9.478056],[124.054167,9.374722],[123.962389,9.159639],[123.703850,9.081767]]]]}},
    {"type":"Feature","properties":{"id":"RPVP","name":"Puerto Princesa Approach"},"geometry":{"type":"MultiPolygon","coordinates":[[[[118.664848,9.080119],[118.722626,9.082659],[118.781379,9.082128],[118.839951,9.086645],[118.897911,9.096177],[118.954811,9.110652],[119.010223,9.129959],[119.063721,9.153953],[119.114914,9.182451],[119.163399,9.215239],[119.208816,9.252068],[119.250816,9.292658],[119.289085,9.336701],[119.323334,9.383865],[119.353287,9.433792],[119.378723,9.486102],[119.399452,9.540400],[119.415306,9.596270],[119.426170,9.653290],[119.431953,9.711026],[119.432609,9.769039],[119.428131,9.826888],[119.418556,9.884133],[119.403946,9.940336],[119.384414,9.995071],[119.360099,10.047919],[119.209930,9.971013],[119.188492,10.009060],[119.163765,10.045120],[119.135933,10.078919],[119.105209,10.110198],[119.071831,10.138718],[119.174515,10.270417],[119.126571,10.304185],[119.075806,10.333706],[119.022606,10.358752],[118.967384,10.379133],[118.910561,10.394693],[118.852569,10.405313],[118.793861,10.410911],[118.734871,10.411444],[118.676063,10.406908],[118.617889,10.397339],[118.560783,10.382808],[118.505188,10.363428],[118.451530,10.339348],[118.400223,10.310750],[118.351654,10.277854],[118.306190,10.240912],[118.264183,10.200207],[118.225952,10.156049],[118.191780,10.108775],[118.161941,10.058747],[118.136650,10.006348],[118.116096,9.951976],[118.107361,9.914931],[118.583816,9.740868],[118.583748,9.738962],[118.584465,9.724477],[118.586456,9.709997],[118.589714,9.695966],[118.594208,9.682156],[118.599907,9.668786],[118.606766,9.655955],[118.614731,9.643763],[118.623741,9.632301],[118.633736,9.621657],[118.644630,9.611912],[118.656341,9.603139],[118.668785,9.595406],[118.681862,9.588771],[118.695480,9.583284],[118.709526,9.578988],[118.723900,9.575915],[118.738483,9.574082],[118.753154,9.573522],[118.767876,9.574221],[118.782448,9.576178],[118.664848,9.080119]]]]}}
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

    // TMA layer — yellow, display only, non-interactive
    var tmaLayerGroup = L.geoJSON({ type: 'FeatureCollection', features: tmas }, {
      style: {
        color: '#ffdd00',
        weight: 1.5,
        opacity: 0.7,
        fillColor: '#ffdd00',
        fillOpacity: 0.04,
        dashArray: '4, 5'
      },
      interactive: false
    }).addTo(map);

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
      var tmaToggle = document.getElementById('vp-toggle-tma');
      if (tmaToggle) {
        tmaToggle.addEventListener('change', function(e) {
          e.target.checked ? map.addLayer(tmaLayerGroup) : map.removeLayer(tmaLayerGroup);
        });
      }
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

## RPHI Briefing

## Frequencies

| Callsign | Frequency |
|---|---|
| MNL_CTR | 119.300 |
| MNL_C_CTR | 132.075 |
| MNL_N_CTR | 126.575 |
| MNL_S_CTR | 133.500 |
| MNL_2_CTR | 124.950 |
| MNL_N1_CTR | 129.000 |
| MNL_S1_CTR | 131.500 |
| MNL_NW_CTR | 128.700 |
| MNL_NE_CTR | 132.500 |
| MNL_CN_CTR | 120.500 |
| MNL_CE_CTR | 128.750 |
| MNL_CS_CTR | 125.700 |
| MNL_CW_CTR | 132.700 |
| MNL_W_CTR | 118.900 |
| MNL_SW_CTR | 124.900 |
| MNL_SE_CTR | 125.750 |

## Manila ACC
![RPHI](../../assets/img/RPHI/7.png)
## North ACC Combined and South ACC Combined
![RPHI](../../assets/img/RPHI/5.png)
##  North and South Central ACC Combined
![RPHI](../../assets/img/RPHI/6.png)
## ACC Split Sectors
![RPHI](../../assets/img/RPHI/1.png)
## Central ACC Combined
![RPHI](../../assets/img/RPHI/8.png)
## Manila ACC South Combined
![RPHI](../../assets/img/RPHI/9.png)

## Strategic Lateral Offset Procedures (SLOP)

### Aircraft Navigation Performance and Airspace Safety

Air Traffic Control applies separation minima, including lateral route spacing, based on the assumption that aircraft operate on the center line of a route. In general, unauthorized deviations from this requirement could compromise safety. However, the use of highly accurate navigation systems [such as Global Navigation Satellite System (GNSS)] reduces the magnitude of lateral deviations from the route center line and consequently increases the probability of a collision if a loss of vertical separation between aircraft on the same route occurs.

By using offsets to provide lateral spacing between aircraft, the effect of this reduction in random
lateral deviations can be mitigated, thereby reducing the risk of collision.

### Strategic Lateral Offsets in Oceanic Airspace

- Offsets are only applied in the oceanic airspace in the Manila FIR.
- Offsets are applied only by aircraft with automatic offset tracking capability.
- The following requirements apply to the use of the offset:
    - The decision to apply a strategic lateral offset is the responsibility of the flight crew.
    - The offset shall be established at a distance of one (1) or two (2) nautical miles to the right of the center line relative to the direction of flight.
    - The strategic lateral offset procedure has been designed to include offsets to mitigate the effects of wake turbulence of preceding aircraft. If wake turbulence needs to be avoided, one of the three available options (center line, 1 NM or 2 NM right offset) shall be used.
    - In airspace where the use of lateral offsets has been authorized, pilots are not required to inform ATC that an offset is being applied.
    - Aircraft transiting areas of radar coverage in airspace where offset tracking is permitted may initiate or continue an offset.

## Phraseology

### Manila Radio

Currently the only radio that can be implemented by Manila Control is MNL_NE_CTR, otherwise known as Manila Oceanic. Under Manila Radio, you can still expect [[RVSM](rvsm.md).](https://learn.vatphil.com/classroom/rvsm/).

When contacting Manila Radio, keep in mind that they will not be able to see you and are purely going off what you give them. It is important to have atleast these four information at hand.

1. Aircraft identification.
2. Position and time
3. Level.
4. Next position and ETA.

!!! phraseology "Phraseology"

    Manila Radio, PAL123, Over BISIG 1300z, FL320, Next EXOMI at 1330z

*[MNL_NE_CTR]: Manila Radio or Manila Control

!!! warning "Warning"

    There are areas within the FIR where Controllers and Pilots will not hear each other due to radio wave propagation