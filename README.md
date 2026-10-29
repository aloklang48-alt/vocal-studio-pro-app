# vocal-studio-pro-app
Professional Vocal Recording Studio App
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Vocal Studio Pro</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #10111b;
      color: white;
      text-align: center;
      padding: 25px 15px;
    }
    h1 { color: #b794ff; }
    .studio {
      max-width: 400px;
      margin: 25px auto;
      padding: 22px;
      background: #202235;
      border-radius: 18px;
    }
    button {
      padding: 14px 20px;
      margin: 8px;
      border: none;
      border-radius: 10px;
      font-size: 16px;
      color: white;
      background: #7948ff;
    }
    #status { color: #a9f5c8; }
  </style>
</head>
<body>
  <h1>🎙️ Vocal Studio Pro</h1>
  <p>Your Personal Recording Studio</p>

  <div class="studio">
    <h2>Recording Studio</h2>
    <p id="status">Ready to record</p>
    <button onclick="startRecording()">🔴 Start</button>
    <button onclick="stopRecording()">⏹ Stop</button>
    <audio id="playback" controls></audio>
  </div>

  <script>
    let recorder;
    let chunks = [];

    async function startRecording() {
      try {
        const stream =
          await navigator.mediaDevices.getUserMedia({
            audio: true
          });
        chunks = [];
        recorder = new MediaRecorder(stream);

        recorder.ondataavailable = event => {
          if (event.data.size > 0) chunks.push(event.data);
        };

        recorder.onstop = () => {
          const audioBlob = new Blob(chunks, {
            type: recorder.mimeType
          });
          document.getElementById("playback").src =
            URL.createObjectURL(audioBlob);
          stream.getTracks().forEach(track => track.stop());
        };

        recorder.start();
        document.getElementById("status").textContent =
          "🔴 Recording...";
      } catch (error) {
        document.getElementById("status").textContent =
          "Please allow microphone access.";
      }
    }

    function stopRecording() {
      if (recorder && recorder.state === "recording") {
        recorder.stop();
        document.getElementById("status").textContent =
          "Recording stopped. Play your audio below.";
      }
    }
  </script>
</body>
</html>
