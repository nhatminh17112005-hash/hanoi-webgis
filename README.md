<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"/>
<title>Hanoi Population Density - Minh Nguyen</title>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>

<style>
#map {
height: 100vh;
}

.legend {
background:white;
padding:10px;
line-height:1.5em;
}
</style>
</head>

<body>

<h2 style="text-align:center">
Hanoi Population Density 2023  
Student ID: XXXXX  
Name: Minh Nguyen
</h2>

<div id="map"></div>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>

var map = L.map('map').setView([21.03,105.85],10);

L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{
maxZoom:19
}).addTo(map);

function getColor(d){
return d > 10000 ? '#800026' :
d > 8000 ? '#BD0026' :
d > 6000 ? '#E31A1C' :
d > 4000 ? '#FC4E2A' :
d > 2000 ? '#FD8D3C' :
d > 1000 ? '#FEB24C' :
'#FFEDA0';
}

function style(feature){
return {
fillColor: getColor(feature.properties.density),
weight:1,
color:'white',
fillOpacity:0.7
};
}

function onEachFeature(feature,layer){
layer.bindTooltip(feature.properties.name);
}

fetch("hanoi_districts.geojson")
.then(res=>res.json())
.then(data=>{
L.geoJSON(data,{
style:style,
onEachFeature:onEachFeature
}).addTo(map);
});

var legend = L.control({position:'topright'});

legend.onAdd=function(map){

var div=L.DomUtil.create('div','legend');

div.innerHTML += "<b>Population Density</b><br>";

div.innerHTML +=
'<i style="background:#800026;width:18px;height:18px;display:inline-block"></i> >10000<br>';

div.innerHTML +=
'<i style="background:#FC4E2A;width:18px;height:18px;display:inline-block"></i> 4000-10000<br>';

div.innerHTML +=
'<i style="background:#FEB24C;width:18px;height:18px;display:inline-block"></i> <4000<br>';

return div;
};

legend.addTo(map);

</script>

</body>
</html>
