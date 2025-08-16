---
---

<style>
    .wetter-box {
      border: 2px solid #ccc;
      border-radius: 12px;
      padding: 20px;
      width: 300px;
      text-align: center;
      font-family: sans-serif;
      background: #f0f8ff;
      margin: 20px auto;
    }
  
    .wetter-box h2 {
      margin: 0 0 10px 0;
    }
  
    .wetter-info p {
      margin: 5px 0;
      font-size: 16px;
    }
  
    .wetter-icon {
      font-size: 40px;
      margin-bottom: 10px;
    }
  </style>
  
  <div class="wetter-box">
    <h2>Collingwood, Ontario</h2>
    <div class="wetter-icon" id="wetter-icon">☀️</div>
    <div class="wetter-info" id="wetter-info">Lade Wetter...</div>
  </div>
  
  <script type="module">
    async function ladeWetter() {
      try {
        const latitude = 44.5;
        const longitude = -80.22;
  
        const url = `https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&current_weather=true&timezone=auto`;
  
        const res = await fetch(url);
        const daten = await res.json();
  
        const temp = daten.current_weather.temperature;
        const wind = daten.current_weather.windspeed;
        const code = daten.current_weather.weathercode;
  
        // Einfaches Symbol für Wetter
        let icon = "☀️"; // Sonne als Standard
        if ([61, 63, 65, 66, 67].includes(code)) icon = "🌧️"; // Regen
        else if ([71, 73, 75].includes(code)) icon = "❄️"; // Schnee
        else if ([80, 81, 82].includes(code)) icon = "🌦️"; // Schauer
        else if ([95, 96, 99].includes(code)) icon = "⛈️"; // Gewitter
        else if ([45, 48].includes(code)) icon = "🌫️"; // Nebel
  
        document.getElementById("wetter-icon").innerText = icon;
        document.getElementById("wetter-info").innerHTML = `
          <p>Temperatur: ${temp} °C</p>
          <p>Wind: ${wind} km/h</p>
        `;
      } catch (error) {
        document.getElementById("wetter-info").innerText = "Fehler beim Laden des Wetters.";
      }
    }
  
    ladeWetter();
  </script>