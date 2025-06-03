#### p5LiveMedia

📚 **¿Qué es p5LiveMedia?**

**p5LiveMedia** es una biblioteca para [p5.js](https://p5js.org/) que facilita la implementación de comunicaciones en tiempo real entre navegadores utilizando **WebRTC**. Permite compartir:

* **Audio y video** en vivo desde la cámara y el micrófono.
* **El canvas de p5.js** como un stream de video.
* **Datos personalizados** entre pares conectados.

Esta biblioteca simplifica la creación de experiencias interactivas como videollamadas, performances colaborativas y juegos en red, sin necesidad de configurar manualmente las complejidades de WebRTC.

🚀 **¿Qué posibilidades ofrece p5LiveMedia?**

<details>
  <summary>Click aquí</summary>

- Las principales funcionalidades de p5LiveMedia incluyen:
  - **1. Compartir audio y video en vivo**
    - Permite capturar y compartir streams de audio y video entre usuarios conectados en una misma sala.

  - **2. Compartir el canvas de p5.js**
    - Es posible transmitir el contenido del canvas como si fuera un video en vivo, permitiendo que otros usuarios vean en tiempo real lo que se dibuja o genera en el canvas.

  - **3. Compartir datos personalizados**
    - Facilita el envío de datos (como coordenadas, estados o cualquier información en formato JSON) entre los usuarios, habilitando interacciones más complejas y sincronizadas.

📝 **Consideraciones para implementar p5LiveMedia**

Al trabajar con p5LiveMedia, ten en cuenta lo siguiente:

* **Servidor de señalización**: La biblioteca utiliza un servidor de señalización para establecer las conexiones entre pares. Puedes usar el servidor público proporcionado (`https://p5livemedia.itp.io`) o configurar uno propio ejecutando `server.js` desde el repositorio de GitHub.

* **Inclusión de scripts**: Asegúrate de incluir las siguientes bibliotecas en tu HTML antes de usar p5LiveMedia:

  ```html
  <script src="https://p5livemedia.itp.io/simplepeer.min.js"></script>
  <script src="https://p5livemedia.itp.io/socket.io.js"></script>
  <script src="https://p5livemedia.itp.io/p5livemedia.js"></script>
  ```

* **Permisos del navegador**: Para capturar audio o video, el navegador solicitará permisos al usuario. Asegúrate de manejar adecuadamente estos permisos en tu aplicación.

* **Gestión de salas**: Los usuarios deben unirse a la misma sala (identificada por un nombre único) para compartir medios o datos entre ellos.

* **Eventos disponibles**: p5LiveMedia proporciona eventos como `'stream'`, `'data'` y `'disconnect'` para manejar la recepción de medios, datos y la desconexión de usuarios, respectivamente.


📚 **Recursos adicionales**

* Repositorio oficial de p5LiveMedia: [https://github.com/vanevery/p5LiveMedia](https://github.com/vanevery/p5LiveMedia)
* Documentación y ejemplos: [https://github.com/vanevery/p5LiveMedia#examples](https://github.com/vanevery/p5LiveMedia#examples)
* Tutorial sobre p5LiveMedia: [https://www.walking-productions.com/notslop/2020/11/18/make-video-chat-fun-again-live-audio-video-canvas-data-streaming-in-p5/](https://www.walking-productions.com/notslop/2020/11/18/make-video-chat-fun-again-live-audio-video-canvas-data-streaming-in-p5/)

</details>

---

**Ejemplos:**

<img src="https://i.imgur.com/eJ3LfZF.gif" width="500">

**Compartir el CANVAS**

Este ejemplo transmite el contenido del canvas a otros usuarios en la misma sala.

<details>
  <summary>Detalles (click aquí)</summary>

<details>
  <summary>sketch.js</summary>

```js
let p5lm;
let otherCanvas;

function setup() {
  let myCanvas = createCanvas(400, 400);
  p5lm = new p5LiveMedia(this, "CANVAS", myCanvas, "salaCanvas123");
  p5lm.on('stream', gotStream);
}

function draw() {
  background(220);
  fill(255, 0, 0);
  ellipse(mouseX, mouseY, 50, 50);
}

function gotStream(stream, id) {
  // Este es el canvas recibido como video
  let video = stream;
  video.position(width + 10, 0); // Muestra el canvas remoto al lado
  video.size(400, 400);
}

```
</details>

<details>
  <summary>index.html</summary>

```html
...
<script src="https://p5livemedia.itp.io/simplepeer.min.js"></script>
<script src="https://p5livemedia.itp.io/socket.io.js"></script>
<script src="https://p5livemedia.itp.io/p5livemedia.js"></script>
<script src="sketch.js"></script>
</body>
</html>

```
</details>


- ✅ ¿Qué debería pasar al ejecutar el sketch?
1. **Al ejecutar el código en un navegador**, p5LiveMedia creará un **canvas interactivo** donde puedes mover el mouse y se dibuja un círculo rojo.
2. Cuando abres **otra instancia del mismo sketch en una nueva pestaña o navegador diferente**, ambas sesiones se **conectan automáticamente a través de la misma “sala”** (`"salaCanvas123"` en este caso).
3. Lo que tú dibujas en tu canvas local **se transmite como video** al otro navegador. Entonces, verás **tu canvas duplicado**:
   * Uno es el tuyo.
   * El otro es el canvas remoto que llega como un video.
4. El canvas remoto aparece justo **a la derecha del original** (`video.position(width + 10, 0)`).


- ❗ ¿Qué es importante para que funcione?
  * **Debe haber al menos dos usuarios conectados a la misma sala** 
  * Necesitas tener habilitado **WebRTC** (que ya viene por defecto en navegadores modernos como Chrome o Firefox).
  * Debes haber agregado correctamente las **tres librerías** al `index.html`.



</details>

---

<img src="https://i.imgur.com/8TE4y63.gif" width="500">

**Compartir el audio y video en tiempo real entre navegadores usando WebRTC**

Este ejemplo tiene como objetivo experimentar con la biblioteca `p5LiveMedia` para establecer una conexión directa peer-to-peer (P2P) entre dos navegadores y así compartir audio y video en tiempo real usando el protocolo **WebRTC**.


<details>
  <summary>Detalles (click aquí)</summary>



- ⚙️ **¿Cómo funciona?**
  * Se utiliza `createCapture({ video: true, audio: true })` para obtener acceso a la cámara y al micrófono del usuario.
  * Se instancia un objeto `p5LiveMedia` indicando:
    * El sketch de p5 (`this`)
    * El tipo de stream (`"CAPTURE"`)
    * El stream de audio/video capturado
    * Un nombre de sala compartido para que los pares se encuentren.
  * Al recibir un stream remoto, el evento `'stream'` permite mostrar en el canvas el video del otro usuario.


🧪 **Código base (`sketch.js`)**

```javascript
let myVideo;
let otherVideo;
let livemedia;

function setup() {
  createCanvas(640, 480);

  myVideo = createCapture({ video: true, audio: true }, function(stream) {
    livemedia = new p5LiveMedia(this, "CAPTURE", stream, "mi_sala_videochat");
    livemedia.on('stream', gotStream);
  });

  myVideo.elt.muted = true;  // evitar feedback de audio
  myVideo.hide();            // opcional: ocultar video local en HTML
}

function gotStream(stream, id) {
  console.log("Recibiendo stream de:", id);

  otherVideo = stream; // ya es un objeto tipo video
}

function draw() {
  background(0);

  if (myVideo) {
    image(myVideo, 0, 0, width / 2, height);
    text("📷 Mi video", 10, 20);
  }

  if (otherVideo) {
    image(otherVideo, width / 2, 0, width / 2, height);
    text("🌐 Remoto", width / 2 + 10, 20);
  }
}
```


- ✅ **Consideraciones importantes**
  * Ambos usuarios deben abrir el sketch en ventanas distintas y **usar el mismo nombre de sala** para conectarse (por ejemplo, `"mi_sala_videochat"`).
  * El navegador puede pedir permisos para usar la cámara y el micrófono.
  * `myVideo.elt.muted = true` es esencial para evitar retroalimentación de sonido (feedback).
  * La conexión se establece mediante el servidor de señalización público de `p5LiveMedia` (`https://p5livemedia.itp.io`).
  * No se requiere servidor backend propio.


- 🔍 **Hallazgos**
  * La biblioteca `p5LiveMedia` simplifica notablemente la implementación de WebRTC en proyectos creativos con `p5.js`.
  * El callback de `createCapture` es crucial: **solo desde ahí se debe inicializar `p5LiveMedia`** para que funcione correctamente.
  * La transmisión de video es fluida en redes locales y relativamente estable usando el servidor de señalización público.
  * Es posible combinar esta funcionalidad con otras capacidades interactivas de `p5.js` (como canvas compartido o envío de datos).


</details>


---

<img src="https://i.imgur.com/gcLpdMW.gif" width="500">

**Compartir datos entre navegadores con WebRTC (p5LiveMedia)**

Este ejemplo demuestra cómo usar p5LiveMedia para compartir datos personalizados (en este caso, coordenadas del mouse) entre dos navegadores conectados por WebRTC, permitiendo sincronizar comportamientos o representaciones visuales en tiempo real.

<details>
  <summary>Detalles (click aquí)</summary>

⚙️ **¿Cómo funciona?**
  
* Se crea una instancia de `p5LiveMedia` con el tipo `"DATA"` y un nombre de sala.
* Cada vez que se mueve el mouse, se envían sus coordenadas a los otros pares.
* Se recibe el mensaje en formato JSON y se actualizan variables para representar visualmente la interacción del otro usuario.


🧪 **Código base (`sketch.js`)**

```javascript
let p5lm;
let otherX = 0;
let otherY = 0;

function setup() {
  createCanvas(600, 400);
  background(255);

  // Inicializar LiveMedia para datos
  p5lm = new p5LiveMedia(this, "DATA", null, "sala_datos_compartidos");

  // Evento al recibir datos
  p5lm.on('data', gotData);
}

function draw() {
  background(240);

  // Dibujo local
  fill(0, 100, 255);
  ellipse(mouseX, mouseY, 50, 50);
  text("Yo", mouseX + 30, mouseY);

  // Dibujo del otro usuario
  fill(255, 0, 0);
  ellipse(otherX, otherY, 50, 50);
  text("Otro usuario", otherX + 30, otherY);
}

function mouseMoved() {
  // Enviar las coordenadas en formato JSON
  let dataToSend = { x: mouseX, y: mouseY };
  p5lm.send(JSON.stringify(dataToSend));
}

function gotData(data, id) {
  // Parsear los datos recibidos
  let parsed = JSON.parse(data);
  otherX = parsed.x;
  otherY = parsed.y;
}
```


- ✅ **Consideraciones importantes**
  * Solo se permite enviar **strings**, por eso se usa `JSON.stringify()` para enviar objetos complejos.
  * El método `p5lm.send(data)` envía los datos a todos los otros pares conectados en la misma sala.
  * Se recomienda siempre usar `try-catch` al parsear JSON si se espera mayor robustez.
  * No se usa `createCapture()` ni video en este ejemplo: el canal WebRTC está dedicado exclusivamente al **canal de datos**.


- 🔍 **Hallazgos**
  * `p5LiveMedia` permite establecer un canal de datos confiable y en tiempo real entre navegadores.
  * Es ideal para enviar coordenadas, estados de objetos, mensajes de texto o cualquier información serializada como string.
  * Permite diseñar experiencias colaborativas como juegos multijugador, control distribuido de animaciones o sistemas interactivos conectados.




</details>
