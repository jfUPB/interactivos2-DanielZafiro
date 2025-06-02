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

<img src="https://i.imgur.com/ZF.gif" width="500">

**Compartir el audio**

Este ejemplo transmite el contenido del canvas a otros usuarios en la misma sala.

<details>
  <summary>Detalles (click aquí)</summary>





</details>

---

<img src="https://i.imgur.com/ZF.gif" width="500">

**Compartir el video**

Este ejemplo transmite el contenido del canvas a otros usuarios en la misma sala.

<details>
  <summary>Detalles (click aquí)</summary>





</details>

---

<img src="https://i.imgur.com/ZF.gif" width="500">

**Compartir datos**

Este ejemplo transmite el contenido del canvas a otros usuarios en la misma sala.

<details>
  <summary>Detalles (click aquí)</summary>





</details>
