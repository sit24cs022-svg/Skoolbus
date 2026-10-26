<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>GPS School Bus Monitoring System</title>

<link rel="stylesheet"
href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css">

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #f1f5f9;
  color: #1e293b;
}

header {
  background: #123b69;
  color: white;
  text-align: center;
  padding: 22px 12px;
}

header h1 {
  margin: 0 0 10px;
  font-size: 26px;
}

main {
  max-width: 1100px;
  margin: 25px auto;
  padding: 0 15px;
}

.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 15px;
}

.card, .panel {
  background: white;
  padding: 20px;
  margin-bottom: 18px;
  border-radius: 12px;
  box-shadow: 0 3px 12px #00000010;
}

.card h3 {
  color: #64748b;
  font-size: 15px;
}

.card p {
  font-size: 20px;
  font-weight: bold;
  color: #123b69;
  overflow-wrap: anywhere;
}

.panel h2 {
  color: #123b69;
}

#map {
  height: 380px;
  width: 100%;
  border-radius: 10px;
}

button {
  padding: 12px 18px;
  border: none;
  border-radius: 7px;
  background: #dc2626;
  color: white;
  font-size: 15px;
  cursor: pointer;
}

button:hover {
  background: #b91c1c;
}

.note {
  color: #64748b;
  font-size: 13px;
  line-height: 1.6;
}

#alertMessage {
  font-weight: bold;
  line-height: 1.5;
}

footer {
  background: #123b69;
  color: white;
  text-align: center;
  padding: 18px;
}

@media (max-width: 600px) {
  header h1 {
    font-size: 22px;
  }
  #map {
    height: 300px;
  }
}
</style>
</head>

<body>

<header>
  <h1>🚌 School Transport Monitor</h1>
  <p>GPS Driven Intelligent School Transport Monitoring System</p>
</header>

<main>

  <section class="cards">
    <div class="card">
      <h3>Bus Number</h3>
      <p id="busNumber">TN 01 AB 1234</p>
    </div>

    <div class="card">
      <h3>Bus Status</h3>
      <p id="status">On Route</p>
    </div>

    <div class="card">
      <h3>Estimated Arrival</h3>
      <p id="eta">10 minutes</p>
    </div>

    <div class="card">
      <h3>Speed</h3>
      <p id="speed">25 km/h</p>
    </div>
  </section>

  <section class="panel">
    <h2>📍 Bus Location Map</h2>
    <p id="location">Loading bus location...</p>
    <div id="map"></div>
    <p class="note">
      Demo mode: bus movement is simulated using sample Chennai
      coordinates. Actual GPS hardware is not connected.
    </p>
  </section>

  <section class="panel">
    <h2>🛡️ Student Safety</h2>
    <button id="alertButton">Generate Emergency Alert</button>
    <p id="alertMessage" role="status"></p>
  </section>

  <section class="panel">
    <h2>📋 Recent Updates</h2>
    <ul id="updates">
      <li>Monitoring dashboard initialized.</li>
      <li>Bus tracking demo is ready.</li>
    </ul>
  </section>

</main>

<footer>
  <p>GPS School Transport Monitoring System</p>
  <p>Student Safety | Smart Tracking | Reliable Transport</p>
</footer>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
// Bus details
const bus = {
  number: "TN 01 AB 1234",
  status: "On Route"
};

// Sample Chennai coordinates for demonstration
const route = [
  [13.0827, 80.2707],
  [13.0870, 80.2750],
  [13.0910, 80.2800],
  [13.0960, 80.2850],
  [13.1000, 80.2900]
];

let routeIndex = 0;

document.getElementById("busNumber").textContent = bus.number;
document.getElementById("status").textContent = bus.status;

// Initialize interactive map
const map = L.map("map").setView(route[0], 14);

L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
  attribution: "&copy; OpenStreetMap contributors"
}).addTo(map);

// Display sample route
L.polyline(route, {
  color: "blue",
  weight: 5
}).addTo(map);

// Display bus marker
const marker = L.marker(route[0]).addTo(map);
marker.bindPopup("School Bus: " + bus.number).openPopup();

function addUpdate(message) {
  const item = document.createElement("li");
  item.textContent = message;

  const list = document.getElementById("updates");
  list.prepend(item);

  while (list.children.length > 6) {
    list.lastElementChild.remove();
  }
}

// Simulate bus movement every five seconds
function updateBus() {
  routeIndex = (routeIndex + 1) % route.length;

  const position = route[routeIndex];

  marker.setLatLng(position);
  map.panTo(position);

  document.getElementById("location").textContent =
    "Latitude: " + position[0] +
    " | Longitude: " + position[1];

  document.getElementById("speed").textContent =
    (20 + Math.floor(Math.random() * 16)) + " km/h";

  document.getElementById("eta").textContent =
    (10 - routeIndex * 2) + " minutes";

  addUpdate(
    "Bus location updated at " + new Date().toLocaleTimeString()
  );
}

document.getElementById("location").textContent =
  "Latitude: " + route[0][0] +
  " | Longitude: " + route[0][1];

setInterval(updateBus, 5000);

// Emergency alert demonstration
document.getElementById("alertButton").addEventListener("click", function() {
  document.getElementById("alertMessage").textContent =
    "Demo emergency alert generated. No message was sent to parents or school authorities.";

  addUpdate(
    "Demo emergency alert generated at " +
    new Date().toLocaleTimeString()
  );
});
</script>

</body>
</html>
