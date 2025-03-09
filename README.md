# Rom-ntico-date
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nosso Amor</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Quando começou a história de vocês?</h1>
        <input type="date" id="startDate">
        <button onclick="saveDate()">Próximo</button>
    </div>
    <script src="script.js"></script>
</body>
</html>

<!-- Segundo Arquivo - upload.html -->
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Upload de Foto</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Escolha uma foto de vocês</h1>
        <input type="file" id="photoInput" accept="image/*">
        <button onclick="uploadPhoto()">Gerar QR Code</button>
        <div id="qrcode"></div>
    </div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <script src="script.js"></script>
</body>
</html>

<!-- Terceiro Arquivo - count.html -->
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Estamos Juntos Há...</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Estamos juntos há:</h1>
        <p id="timeTogether"></p>
        <img id="couplePhoto" alt="Foto do casal">
    </div>
    <script src="script.js"></script>
</body>
</html>

<!-- Arquivo de Lógica - script.js -->
<script>
function saveDate() {
    let date = document.getElementById("startDate").value;
    if (date) {
        localStorage.setItem("startDate", date);
        window.location.href = "upload.html";
    } else {
        alert("Por favor, insira uma data.");
    }
}

function uploadPhoto() {
    let fileInput = document.getElementById("photoInput");
    if (fileInput.files.length > 0) {
        let reader = new FileReader();
        reader.onload = function(event) {
            localStorage.setItem("photo", event.target.result);
            generateQRCode();
        };
        reader.readAsDataURL(fileInput.files[0]);
    } else {
        alert("Escolha uma foto.");
    }
}

function generateQRCode() {
    let qrDiv = document.getElementById("qrcode");
    qrDiv.innerHTML = "";
    let url = window.location.origin + "/count.html";
    new QRCode(qrDiv, url);
}

function calculateTime() {
    let startDate = localStorage.getItem("startDate");
    let photo = localStorage.getItem("photo");

    if (startDate) {
        let start = new Date(startDate);
        let now = new Date();
        let diff = now - start;

        let days = Math.floor(diff / (1000 * 60 * 60 * 24));
        let hours = Math.floor((diff / (1000 * 60 * 60)) % 24);
        let minutes = Math.floor((diff / (1000 * 60)) % 60);
        let seconds = Math.floor((diff / 1000) % 60);

        document.getElementById("timeTogether").innerText = `${days} dias, ${hours} horas, ${minutes} minutos e ${seconds} segundos.`;

        if (photo) {
            document.getElementById("couplePhoto").src = photo;
        }
    }
}

if (window.location.pathname.includes("count.html")) {
    setInterval(calculateTime, 1000);
}
</script>

<!-- Arquivo de Estilos - styles.css -->
<style>
body {
    font-family: Arial, sans-serif;
    text-align: center;
    background-color: #ffe6e6;
    color: #d63384;
    margin: 0;
    padding: 0;
}

.container {
    margin-top: 50px;
}

h1 {
    font-size: 24px;
}

input, button {
    margin: 10px;
    padding: 10px;
    font-size: 18px;
}

#qrcode {
    margin-top: 20px;
}

img {
    width: 200px;
    border-radius: 10px;
    margin-top: 20px;
}
</style>
