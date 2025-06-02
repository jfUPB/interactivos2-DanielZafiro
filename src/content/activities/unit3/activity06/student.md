#### Aplicación de transferencia de imagenes entre clientes / Comunicación Cliente-Servidor y Cliente-Cliente

<img src="https://i.imgur.com/gdhyKrf.gif" width="800">

**Demostración de funcionamiento localmente (el mismo pc)**

---

**Demostración de funcionamiento Remotamente(tuneles) (PC y otro dispositivo macbook)**

cliente mobile remoto(macbook)
<img src="https://github.com/user-attachments/assets/96612bd4-83f9-4514-93b0-be75f95096ff" width="500">

cliente desktop local(PC) recibe
<img src="https://i.imgur.com/mgjXpJu.gif" width="500">

---

**Codigos**

<details>
  <summary>mobile client</summary>

<details>
  <summary>mobile index.html</summary>

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Mobile Client</title>
  <style>
    body { font-family: sans-serif; text-align: center; padding: 20px; }
    #preview { max-width: 100%; max-height: 300px; margin: 10px 0; }
    video { width: 100%; max-width: 300px; }
    button { margin: 5px; padding: 10px 20px; }
  </style>
</head>
<body>

  <h2>Enviar imagen</h2>

  <input type="file" accept="image/*" id="fileInput"><br>

  <button id="openCamera">Usar cámara</button><br>

  <video id="video" autoplay playsinline hidden></video><br>
  <canvas id="canvas" hidden></canvas>

  <img id="preview" src="" alt="Vista previa"><br>

  <button id="sendBtn" disabled>Enviar</button>
  <button id="discardBtn" disabled>Eliminar imagen</button>

  <p id="feedbackResult" style="font-size: 18px; margin-top: 20px;"></p>


  <script src="/socket.io/socket.io.js"></script>
  <script src="sketch.js"></script>

</body>
</html>

```
---
</details>

<details>
  <summary>mobile sketch.js</summary>

```js
const socket = io();

const fileInput = document.getElementById('fileInput');
const openCameraBtn = document.getElementById('openCamera');
const sendBtn = document.getElementById('sendBtn');
const discardBtn = document.getElementById('discardBtn');
const preview = document.getElementById('preview');
const video = document.getElementById('video');
const canvas = document.getElementById('canvas');

let currentImageBlob = null;
let stream = null;

// Seleccionar imagen desde archivos
fileInput.addEventListener('change', () => {
    const file = fileInput.files[0];
    if (file && file.type.startsWith('image/')) {
        const reader = new FileReader();
        reader.onload = (e) => {
            preview.src = e.target.result;
            preview.hidden = false;
            currentImageBlob = dataURLtoBlob(e.target.result);
            enableButtons();
        };
        reader.readAsDataURL(file);
    }
});

// Usar cámara
openCameraBtn.addEventListener('click', async () => {
    stream = await navigator.mediaDevices.getUserMedia({ video: true });
    video.srcObject = stream;
    video.hidden = false;

    const captureBtn = document.createElement('button');
    captureBtn.textContent = 'Capturar foto';
    document.body.appendChild(captureBtn);

    captureBtn.onclick = () => {
        canvas.width = video.videoWidth;
        canvas.height = video.videoHeight;
        canvas.getContext('2d').drawImage(video, 0, 0);

        const dataURL = canvas.toDataURL('image/png');
        preview.src = dataURL;
        preview.hidden = false;
        currentImageBlob = dataURLtoBlob(dataURL);

        video.hidden = true;
        stream.getTracks().forEach(track => track.stop());
        document.body.removeChild(captureBtn);
        enableButtons();
    };
});

// Enviar imagen
sendBtn.addEventListener('click', () => {
    if (currentImageBlob) {
        const reader = new FileReader();
        reader.onload = () => {
            // Mostrar mensaje de "esperando feedback"
            document.getElementById('feedbackResult').innerText = "⌛ Esperando feedback...";
            socket.emit('image', reader.result);
            alert("Imagen enviada correctamente");
            resetState();
        };
        reader.readAsArrayBuffer(currentImageBlob);
    }
});

// Eliminar imagen
discardBtn.addEventListener('click', () => {
    resetState();
});

function enableButtons() {
    sendBtn.disabled = false;
    discardBtn.disabled = false;
}

function resetState() {
    preview.src = '';
    preview.hidden = true;
    currentImageBlob = null;
    sendBtn.disabled = true;
    discardBtn.disabled = true;
    fileInput.value = '';
}

function dataURLtoBlob(dataURL) {
    const byteString = atob(dataURL.split(',')[1]);
    const mimeString = dataURL.split(',')[0].split(':')[1].split(';')[0];

    const ab = new ArrayBuffer(byteString.length);
    const ia = new Uint8Array(ab);
    for (let i = 0; i < byteString.length; i++) {
        ia[i] = byteString.charCodeAt(i);
    }

    return new Blob([ab], { type: mimeString });
}

socket.on('feedback', (data) => {
    const feedbackText = data.type === 'like' 
        ? '👍 ¡Recibieron tu imagen con agrado!' 
        : '👎 No gustó tu imagen';

    document.getElementById('feedbackResult').innerText = feedbackText;
});

```
</details>

---

</details>

<details>
  <summary>server.js</summary>

```js

const express = require('express');
const http = require('http');
const socketIO = require('socket.io');
const app = express();
const server = http.createServer(app);
const io = socketIO(server);
const port = 3000;

app.use(express.static('public'));

io.on('connection', (socket) => {
    console.log('Client connected');

    // socket.on('message', (message) => {
    //     console.log(`Mensaje recibido => ${message}`);
    //     socket.broadcast.emit('message', message);
    // });

    socket.on('image', (data) => {
        console.log('Image received, broadcasting...');
        socket.broadcast.emit('image', data); // Enviar a todos menos al emisor
    });

    socket.on('feedback', (data) => {
    console.log(`Feedback recibido: ${data.type}`);
    socket.broadcast.emit('feedback', data);
    });

    socket.on('disconnect', () => {
        console.log('Client disconnected');
    });
});

server.listen(port, () => {
    console.log(`Server running at http://localhost:${port}`);
});

```
---
</details>

<details>
  <summary>desktop client</summary>

<details>
  <summary>desktop index.html</summary>

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Desktop Receiver</title>
  <style>
    body { font-family: sans-serif; text-align: center; padding: 20px; }
    img { max-width: 90%; max-height: 400px; margin-top: 20px; }
  </style>
</head>
<body>

  <h2>Esperando imagen del cliente móvil...</h2>
  <div id="imageContainer">
    <img id="receivedImage" src="" alt="Imagen recibida">
  </div>
  <div id="feedbackSection" style="margin-top: 20px; display: none;">
  <p>¿Te gustó la imagen?</p>
  <button onclick="sendFeedback('like')">👍 Like</button>
  <button onclick="sendFeedback('dislike')">👎 Dislike</button>
</div>


  <script src="/socket.io/socket.io.js"></script>
  <script src="sketch.js"></script>

</body>
</html>

```
---
</details>

<details>
  <summary>desktop sketch.js</summary>

```js
const socket = io();
const receivedImage = document.getElementById('receivedImage');
const feedbackSection = document.getElementById('feedbackSection');

socket.on('connect', () => {
    console.log('Conectado al servidor');
});

socket.on('image', (arrayBuffer) => {
    console.log('Imagen recibida');
    
    const blob = new Blob([arrayBuffer], { type: 'image/png' });
    const imageUrl = URL.createObjectURL(blob);

    receivedImage.src = imageUrl;
    feedbackSection.style.display = 'block'; // Mostrar opciones de feedback
});

function sendFeedback(type) {
    socket.emit('feedback', { type });
    feedbackSection.style.display = 'none'; // Ocultar después de dar feedback
}

socket.on('disconnect', () => {
    console.log('Desconectado del servidor');
});

```
</details>

---

</details>

---

#### ¿Cómo se comunican los clientes con el servidor?

Los clientes se comunican con el servidor mediante **WebSockets**, usando la librería `socket.io`. Esta tecnología permite establecer una conexión persistente y bidireccional entre cada cliente (mobile o desktop) y el servidor.

#### ¿Cómo se comunican los clientes entre sí?

Los clientes no se comunican directamente entre ellos. Toda la comunicación pasa por el servidor. El servidor actúa como intermediario: cuando un cliente envía un mensaje, el servidor lo recibe y luego lo retransmite al otro cliente mediante `socket.broadcast.emit()` o `socket.to(...).emit()`.

---

#### Tipo de mensajes, datos y eventos

- ¿Qué tipo de mensajes se envían?
  - Se envían dos tipos principales de mensajes:
    - `image`: mensaje que contiene los datos binarios de la imagen capturada o seleccionada por el cliente mobile.
    - `feedback`: mensaje que contiene la reacción (like o dislike) enviada por el cliente desktop al recibir la imagen.

- ¿Qué tipo de datos se envían?
  - **Datos binarios**: el contenido de la imagen se envía como un `ArrayBuffer`, lo cual permite transferir archivos eficientemente.
  - **Datos JSON**: el feedback del cliente desktop se envía como un objeto JSON simple que incluye la propiedad `type` con el valor `'like'` o `'dislike'`.

-  ¿Qué tipo de eventos se generan?
    -  `connect` y `disconnect`: eventos automáticos de conexión/desconexión de cada cliente.
    -  `image`: evento personalizado generado por el cliente mobile para enviar una imagen al servidor.
    -  `feedback`: evento personalizado generado por el cliente desktop para enviar una reacción.
    -  `feedback` (también): evento que el servidor reenvía al cliente mobile con la reacción recibida.

---

#### Flujo de datos

- ¿Cómo es el flujo de datos entre los clientes y el servidor?

1. El cliente mobile selecciona o captura una imagen.
2. Envía la imagen al servidor mediante el evento `image`.
3. El servidor recibe el evento `image` y retransmite los datos al cliente desktop.
4. El cliente desktop muestra la imagen y espera una reacción del usuario.
5. Al seleccionar like/dislike, el cliente desktop emite un evento `feedback`.
6. El servidor recibe el feedback y lo reenvía al cliente mobile.

- ¿Cómo es el flujo de datos entre los clientes?

Aunque no existe comunicación directa entre clientes, el flujo entre ellos es el siguiente:

1. **Cliente Mobile → Servidor → Cliente Desktop**: flujo de datos de la imagen.
2. **Cliente Desktop → Servidor → Cliente Mobile**: flujo del feedback.

Ambos clientes dependen del servidor como puente para intercambiar mensajes y datos.



