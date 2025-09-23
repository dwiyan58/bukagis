---
layout: single
title: Peta Lokasi
permalink: /map/
---
<div id="map" style="height:70vh"></div>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin=""/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"
integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>
<script>
const map = L.map('map').setView([-2.87, 107.95], 11);
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
  maxZoom: 19, attribution: '&copy; OpenStreetMap contributors'
}).addTo(map);

fetch('{{ "/data/lokasi.geojson" | relative_url }}')
  .then(r => r.json())
  .then(geo => {
    const layer = L.geoJSON(geo, {
      onEachFeature: (f, l) => l.bindPopup(f.properties?.nama || 'Lokasi')
    }).addTo(map);
    try { map.fitBounds(layer.getBounds(), {padding:[20,20]}); } catch(e){}
  })
  .catch(err => console.error('Gagal memuat GeoJSON:', err));
</script>
