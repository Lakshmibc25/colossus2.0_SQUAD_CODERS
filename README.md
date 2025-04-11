# colossus2.0_SQUAD_CODERS
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>GMIT Davangere Navigation</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #ffffff;
    }
    header {
      background: #0f172a;
      color: white;
      padding: 1rem;
      text-align: center;
      font-size: 1.5rem;
    }
    #controls {
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 1rem;
      gap: 1rem;
      background: #e2e8f0;
    }
    select, button {
      padding: 0.5rem;
      font-size: 1rem;
    }
    button {
      background-color: #1d4ed8;
      color: white;
      border: none;
      border-radius: 0.25rem;
      cursor: pointer;
    }
    #map {
      height: 90vh;
      width: 100vw;
    }
  </style>
</head>
<body>
  <header>GMIT Davangere Campus Navigation</header>
  <div id="controls">
    <select id="start"></select>
    <select id="end"></select>
    <button onclick="navigate()">Navigate</button>
  </div>
  <div id="map"></div>

  <script>
    const locations = {
      "Main Entrance": [14.4924, 75.9065],
      "Admin Block": [14.4926, 75.9069],
      "Library": [14.4928, 75.9071],
      "CSE Department": [14.4930, 75.9074],
      "ECE Department": [14.4932, 75.9077],
      "Mechanical Block": [14.4927, 75.9068],
      "Hostel": [14.4935, 75.9072],
      "Canteen": [14.4929, 75.9066],
      "Seminar Hall": [14.4931, 75.9070],
      "Parking Area": [14.4925, 75.9067],
      "Sports Ground": [14.4936, 75.9073],
      "Placement Cell": [14.4927, 75.9070],
      "CSE Lab - Floor 1": [14.49305, 75.90745],
      "CSE Lab - Floor 2": [14.49305, 75.90742],
      "Library Reading Room": [14.49282, 75.90712]
    };

    const addressLookup = {
      "Main Entrance": "GMIT Main Gate, Davangere",
      "Admin Block": "GMIT Admin Office, Davangere",
      "Library": "GMIT Central Library, Davangere",
      "CSE Department": "Computer Science Block, GMIT, Davangere",
      "ECE Department": "Electronics Dept., GMIT, Davangere",
      "Mechanical Block": "Mechanical Engg. Block, GMIT, Davangere",
      "Hostel": "GMIT Boys/Girls Hostel, Davangere",
      "Canteen": "GMIT Canteen Area, Davangere",
      "Seminar Hall": "Seminar Hall, GMIT Campus, Davangere",
      "Parking Area": "GMIT Vehicle Parking, Davangere",
      "Sports Ground": "GMIT Playground, Davangere",
      "Placement Cell": "Placement Office, GMIT, Davangere",
      "CSE Lab - Floor 1": "CSE Lab Floor 1, GMIT, Davangere",
      "CSE Lab - Floor 2": "CSE Lab Floor 2, GMIT, Davangere",
      "Library Reading Room": "Reading Room, Library Block, GMIT"
    };

    const startSelect = document.getElementById('start');
    const endSelect = document.getElementById('end');

    Object.keys(locations).forEach(loc => {
      const optionStart = document.createElement('option');
      optionStart.text = loc;
      const optionEnd = optionStart.cloneNode(true);
      startSelect.add(optionStart);
      endSelect.add(optionEnd);
    });

    const map = L.map('map').setView([14.4928, 75.9070], 18);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: ''
    }).addTo(map);

    let startMarker, endMarker, routeLine, userMarker;

    Object.entries(locations).forEach(([name, coords]) => {
      L.circleMarker(coords, { radius: 6, color: '#1e40af' })
        .bindPopup(<strong>${name}</strong><br>${addressLookup[name]}<br>Lat: ${coords[0]}<br>Lng: ${coords[1]})
        .addTo(map);
    });

    function navigate() {
      const start = locations[startSelect.value];
      const end = locations[endSelect.value];

      if (startMarker) map.removeLayer(startMarker);
      if (endMarker) map.removeLayer(endMarker);
      if (routeLine) map.removeLayer(routeLine);

      startMarker = L.circleMarker(start, { radius: 8, color: 'green' }).addTo(map);
      endMarker = L.circleMarker(end, { radius: 8, color: 'red' }).addTo(map);

      routeLine = L.polyline([start, end], { color: '#2563eb', weight: 4 }).addTo(map);
      map.fitBounds([start, end], { padding: [50, 50] });
    }

    if (navigator.geolocation) {
      navigator.geolocation.watchPosition(
        (pos) => {
          const lat = pos.coords.latitude;
          const lng = pos.coords.longitude;
          if (userMarker) map.removeLayer(userMarker);
          userMarker = L.circleMarker([lat, lng], {
            radius: 10,
            color: '#facc15',
            fillColor: '#facc15',
            fillOpacity: 1
          }).bindPopup("You are here").addTo(map);
        },
        (err) => console.error(err),
        { enableHighAccuracy: true }
      );
    }
  </script>
</body>
</html>
