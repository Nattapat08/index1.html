# index1.html
<!DOCTYPE html>
<html>
<head>
  <title>Servo Control</title>
  <script src="https://unpkg.com/mqtt/dist/mqtt.min.js"></script>

  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(135deg, #74ebd5, #ACB6E5);
      height: 100vh;
      margin: 0;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .card {
      background: white;
      padding: 30px;
      border-radius: 15px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.2);
      text-align: center;
      width: 300px;
    }

    h2 {
      margin-bottom: 20px;
    }

    #timer {
      font-size: 40px;
      font-weight: bold;
      margin: 20px 0;
      color: #333;
    }

    button {
      padding: 12px 20px;
      margin: 10px;
      border: none;
      border-radius: 8px;s
      font-size: 16px;
      cursor: pointer;
      transition: 0.2s;
      width: 100px;
    }

    .onBtn {
      background-color: #4CAF50;
      color: white;
    }

    .onBtn:hover {
      background-color: #43a047;
    }

    .offBtn {
      background-color: #f44336;
      color: white;
    }

    .offBtn:hover {
      background-color: #d32f2f;
    }
  </style>
</head>

<body>

<div class="card">
  <h2>Servo Control</h2>

  <div id="timer">30:00</div>

  <button class="onBtn" onclick="sendOn()">ON</button>
  <button class="offBtn" onclick="sendOff()">OFF</button>
</div>

<script>
const client = mqtt.connect("wss://test.mosquitto.org:8081");

client.on("connect", function () {
  console.log("Connected MQTT");
});

let countdown;
let totalSeconds = 1800; // 30 นาที

function updateDisplay() {
  let minutes = Math.floor(totalSeconds / 60);
  let seconds = totalSeconds % 60;

  document.getElementById("timer").innerText =
    String(minutes).padStart(2,'0') + ":" +
    String(seconds).padStart(2,'0');
}

function startTimer() {
  clearInterval(countdown);
  totalSeconds = 1800;
  updateDisplay();

  countdown = setInterval(() => {
    if (totalSeconds > 0) {
      totalSeconds--;
      updateDisplay();
    } else {
      clearInterval(countdown);
    }
  }, 1000);
}

function resetTimer() {
  clearInterval(countdown);
  totalSeconds = 1800;
  updateDisplay();
}

function sendOn(){
  client.publish("visawa/servo/control","ON");
  startTimer();
}

function sendOff(){
  client.publish("visawa/servo/control","OFF");
  resetTimer();
}

// แสดงค่าเริ่มต้น
updateDisplay();

</script>

</body>
</html>
