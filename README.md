<html>
<head>
  <meta charset="utf-8">
  <title>Dashboard Monitoring ESP32</title>
  <!-- Import library MQTT.js -->
  <script src="https://unpkg.com/mqtt/dist/mqtt.min.js"></script>
</head>
<body>
  <h2>📡 Dashboard Monitoring ESP32</h2>
  <p>Tegangan: <span id="voltage">-</span> V</p>

  <script>
    // Alamat broker (pakai HiveMQ public broker dengan WebSocket)
    const host = "wss://broker.hivemq.com:8884/mqtt";
    const options = {
      clientId: "webclient_" + Math.random().toString(16).substr(2, 8),
    };

    // Koneksi ke broker
    const client = mqtt.connect(host, options);

    client.on("connect", () => {
      console.log("✅ Connected to MQTT broker");
      // Subscribe ke topic dari ESP32
      client.subscribe("esp32/tegangan");
    });

    client.on("message", (topic, message) => {
      if (topic === "esp32/tegangan") {
        document.getElementById("voltage").innerText = message.toString();
        console.log("📩 Data diterima:", message.toString());
      }
    });

    client.on("error", (err) => {
      console.error("❌ Connection error: ", err);
    });
  </script>
</body>
</html>
