<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Photo QR Generator</title>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: linear-gradient(135deg, #667eea, #764ba2);
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.container {
  width: 100%;
  max-width: 430px;
  background: white;
  padding: 25px;
  border-radius: 20px;
  text-align: center;
  box-shadow: 0 10px 35px rgba(0,0,0,0.25);
}

h1 {
  margin-top: 0;
  color: #333;
}

.subtitle {
  color: #666;
  font-size: 14px;
}

.upload-box {
  border: 2px dashed #667eea;
  border-radius: 15px;
  padding: 25px;
  margin: 20px 0;
  cursor: pointer;
}

.upload-box:hover {
  background: #f5f6ff;
}

input[type="file"] {
  display: none;
}

.preview {
  display: none;
  width: 100%;
  max-height: 300px;
  object-fit: contain;
  border-radius: 12px;
  margin-top: 15px;
}

input[type="url"] {
  width: 100%;
  padding: 13px;
  border: 1px solid #ccc;
  border-radius: 10px;
  font-size: 15px;
  margin: 10px 0;
}

button {
  width: 100%;
  border: none;
  padding: 14px;
  border-radius: 10px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  margin-top: 10px;
}

.generate {
  background: #667eea;
  color: white;
}

.download {
  display: none;
  background: #222;
  color: white;
}

#qrcode {
  margin: 25px auto;
  display: flex;
  justify-content: center;
}

.note {
  font-size: 12px;
  color: #777;
  line-height: 1.5;
}
</style>
</head>

<body>

<div class="container">

  <h1>📸 Photo QR</h1>

  <p class="subtitle">
    Upload your photo and create a QR code for its webpage.
  </p>

  <label class="upload-box" for="photoInput">
    📷<br><br>
    <strong>Choose Photo</strong><br>
    <small>Tap here to select a photo</small>
  </label>

  <input type="file" id="photoInput" accept="image/*">

  <img id="preview" class="preview">

  <input
    type="url"
    id="pageUrl"
    placeholder="Paste your public webpage link"
  >

  <button class="generate" onclick="generateQR()">
    🔲 Generate QR Code
  </button>

  <div id="qrcode"></div>

  <button id="downloadBtn" class="download" onclick="downloadQR()">
    📥 Download QR Code
  </button>

  <p class="note">
    Your photo must be hosted online first.
    The QR code opens the public webpage link you enter.
  </p>

</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>

<script>

const photoInput = document.getElementById("photoInput");
const preview = document.getElementById("preview");

photoInput.addEventListener("change", function() {

  const file = this.files[0];

  if (!file) return;

  const reader = new FileReader();

  reader.onload = function(e) {
    preview.src = e.target.result;
    preview.style.display = "block";
  };

  reader.readAsDataURL(file);
});


function generateQR() {

  const url = document.getElementById("pageUrl").value.trim();
  const qrBox = document.getElementById("qrcode");

  if (!url) {
    alert("Please paste your public webpage link first.");
    return;
  }

  if (!url.startsWith("http://") && !url.startsWith("https://")) {
    alert("Please enter a valid link starting with https://");
    return;
  }

  qrBox.innerHTML = "";

  new QRCode(qrBox, {
    text: url,
    width: 220,
    height: 220,
    correctLevel: QRCode.CorrectLevel.H
  });

  document.getElementById("downloadBtn").style.display = "block";
}


function downloadQR() {

  const canvas = document.querySelector("#qrcode canvas");

  if (!canvas) {
    alert("Generate the QR code first.");
    return;
  }

  const link = document.createElement("a");

  link.download = "photo-qr-code.png";
  link.href = canvas.toDataURL("image/png");

  link.click();
}

</script>

</body>
</html>
