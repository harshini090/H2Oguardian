<script>
    import { onMount } from "svelte";
    import L from "leaflet";
  
    let map;
  
    onMount(() => {
      map = L.map("map").setView([40.7128, -74.006], 3);
  
      L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
        attribution: '© OpenStreetMap contributors',
      }).addTo(map);
  
      // Example markers
      const locations = [
        { name: "New York", lat: 40.7128, lng: -74.006, quality: "Safe" },
        { name: "Los Angeles", lat: 34.0522, lng: -118.2437, quality: "Unsafe" },
      ];
  
      locations.forEach((location) => {
        const marker = L.marker([location.lat, location.lng]).addTo(map);
        marker.bindPopup(`<b>${location.name}</b><br>Water Quality: ${location.quality}`);
        marker.on("mouseover", () => marker.openPopup());
        marker.on("mouseout", () => marker.closePopup());
      });
    });
  </script>
  
  <div id="map" class="map-container"></div>
  
  <style>
    .map-container {
      height: 400px;
      margin: 20px 0;
      border-radius: 8px;
    }
  </style>
  