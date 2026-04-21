---
glightbox: false
hide:
  - toc
---

# Briefings

Here you will find aerodrome briefings for the airports within the Philippines! To start, select an Aerodrome highlighted in red below.
<div class="aero-map-outer">
  <div class="aero-wrap" id="aeroWrap">
    <img id="aeroImg" src="/assets/img/aerodromes.png" alt="Philippines Aerodrome Index Chart" data-no-lightbox />
  </div>
</div>
<style>
.aero-map-outer {
  width: 100%;
  overflow-x: auto;
}
.aero-wrap {
  position: relative;
  display: block;
  width: 100%;
}
.aero-wrap img {
  display: block;
  width: 100%;
  height: auto;
  border-radius: 6px;
}
.aero-spot {
  position: absolute;
  transform: translate(-50%, -50%);
  cursor: pointer;
  z-index: 5;
  text-decoration: none;
}
.aero-ring {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: rgba(220, 50, 50, 0.55);
  border: 1.5px solid rgba(220, 50, 50, 0.85);
  transition: background 0.12s, border-color 0.12s, transform 0.12s;
}
.aero-spot:hover .aero-ring {
  background: rgba(220, 50, 50, 0.85);
  border-color: rgba(220, 50, 50, 1);
  transform: scale(1.7);
}
.aero-tip {
  position: absolute;
  bottom: calc(100% + 7px);
  left: 50%;
  transform: translateX(-50%);
  background: #1a0808;
  border: 1px solid rgba(220, 80, 80, 0.7);
  color: #ffcccc;
  font-size: 11px;
  font-weight: 600;
  padding: 5px 10px 4px;
  border-radius: 5px;
  white-space: nowrap;
  pointer-events: none;
  opacity: 0;
  transition: opacity 0.12s;
  z-index: 20;
  line-height: 1.4;
}
.aero-tip .icao-code {
  display: block;
  font-size: 10px;
  font-weight: 400;
  color: #ff9999;
  margin-top: 1px;
}
.aero-tip::after {
  content: "";
  position: absolute;
  top: 100%; left: 50%;
  transform: translateX(-50%);
  border: 5px solid transparent;
  border-top-color: rgba(220, 80, 80, 0.7);
}
.aero-spot:hover .aero-tip { opacity: 1; }
.aero-caption {
  font-size: 11px;
  color: var(--md-default-fg-color--light);
  margin-top: 6px;
  font-style: italic;
  text-align: center;
}
</style>

<script>
(function() {
  const spots = [
    { name: "Ninoy Aquino Intl",  icao: "RPLL", x: 40.2, y: 42.3 },
    { name: "Clark Intl",         icao: "RPLC", x: 37.0, y: 38.8 },
    { name: "Caticlan",           icao: "RPVE", x: 46.5, y: 55.4 },
    { name: "Kalibo",             icao: "RPVK", x: 49.5, y: 56.6 },
    { name: "Roxas",              icao: "RPVR", x: 52.2, y: 57.0 },
    { name: "Mactan-Cebu Intl",   icao: "RPVM", x: 61.0, y: 63.5 },
    { name: "Francisco Bangoy",   icao: "RPMD", x: 73.7, y: 79.7 },
  ];
  function build() {
    const wrap = document.getElementById('aeroWrap');
    if (!wrap) return;
    wrap.querySelectorAll('.aero-spot').forEach(function(e) { e.remove(); });
    const W = wrap.offsetWidth;
    const img = document.getElementById('aeroImg');
    const H = img.offsetHeight;
    if (!H) return;

    spots.forEach(function(s) {
      const a = document.createElement('a');
      a.className = 'aero-spot';
      a.href = 'https://learn.vatphil.com/briefings/' + s.icao;
      a.style.left = (s.x / 100 * W) + 'px';
      a.style.top  = (s.y / 100 * H) + 'px';

      const ring = document.createElement('div');
      ring.className = 'aero-ring';

      const tip = document.createElement('div');
      tip.className = 'aero-tip';
      tip.innerHTML = s.name + '<span class="icao-code">' + s.icao + '</span>';

      a.appendChild(ring);
      a.appendChild(tip);
      wrap.appendChild(a);
    });
  }

  function init() {
    const img = document.getElementById('aeroImg');
    if (!img) return;
    if (img.complete && img.naturalHeight) {
      build();
    } else {
      img.onload = build;
    }
    window.addEventListener('resize', build);
  }

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', init);
  } else {
    init();
  }
})();
</script>

<style>
  .grid.cards p strong {
    color: #000000;
  }
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

-   **Mindinao**

    ---
    [RPMD →](https://learn.vatphil.com/briefings/RPMD/)

-   **RPHI**

    ---

    [FIR →](https://learn.vatphil.com/briefings/RPHI/)

</div>

## RPHI Briefing

<figure markdown="span">
  ![RNAV Routes](../../assets/img/RP_ENR_6-1.2.png){ width="800" }
  <figcaption>RNAV Routes Within RPHI</figcaption>
</figure>

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