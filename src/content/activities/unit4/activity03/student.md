#### Aplicación con p5LiveMedia – "Pintura a Dúo"

<img src="https://i.imgur.com/ilZ7xUF.gif" width="800">

🎨 **1. Descripción de la aplicación propuesta**

**“Pintura a Dúo”** es una aplicación colaborativa que permite a múltiples usuarios dibujar simultáneamente sobre un mismo lienzo compartido en tiempo real, cada uno con su propio color y nombre visible. A través de una interfaz simple e intuitiva, los usuarios pueden:

* Dibujar con el mouse.
* Seleccionar su color preferido.
* Activar un modo de borrador.
* Visualizar en tiempo real el cursor y nombre de los demás participantes.

Esta herramienta está pensada como una base para experiencias artísticas colectivas, juegos colaborativos o incluso como módulo dentro de performances interactivas.

---

🔌 **2. ¿Cómo hace uso de p5LiveMedia?**

La aplicación utiliza la biblioteca `p5LiveMedia` en **modo DATA**, lo que permite compartir información personalizada entre navegadores conectados a la misma “sala”.

* Cada cliente se conecta a una sala llamada `"sala-dibujo"`.
* Cuando un usuario dibuja, su trazo (posición, color, tamaño y nombre) se envía como un objeto JSON a los demás.
* También se actualiza continuamente la posición del cursor de cada usuario para mostrar su nombre junto al puntero.
* La comunicación es **peer-to-peer (P2P)** usando WebRTC, facilitada por `p5LiveMedia`.

---

🌐 **3. Relación con el proyecto de curso**

Esta aplicación puede integrarse como parte del proyecto de curso si se busca **una herramienta expresiva, colaborativa y en tiempo real**. Por ejemplo, en el contexto de una experiencia inmersiva tipo videomapping o performance artística, este lienzo compartido puede permitir que múltiples espectadores de forma remota o local participen generando trazos visuales que se proyectan en vivo como parte del espectáculo.

---

🛠️ **4. Tutorial paso a paso**

**Paso 1: Estructura del proyecto**

Crea una carpeta con los siguientes archivos:

* `index.html`
* `sketch.js`

**Paso 2: Código de `index.html`**

<details>
  <summary>index.html</summary>
  
```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="utf-8" />
    <script src="https://cdn.jsdelivr.net/npm/p5@1.11.0/lib/p5.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/p5@1.11.0/lib/addons/p5.sound.min.js"></script>
    <script src="https://p5livemedia.itp.io/socket.io.js"></script>
    <script src="https://p5livemedia.itp.io/simplepeer.min.js"></script>
    <script src="https://p5livemedia.itp.io/p5livemedia.js"></script>
    <style>
      body { margin: 0; overflow: hidden; }
      canvas { display: block; }
    </style>
  </head>
  <body>
    <script src="sketch.js"></script>
  </body>
</html>
```

</details>

**Paso 3: Código de `sketch.js`**

<details>
  <summary>sketch.js</summary>
  
```javascript
let p5lm;
let otherUsers = {};
let userName;
let currentColor = '#ff0000';
let erasing = false;

function setup() {
  createCanvas(windowWidth, windowHeight);
  background(255);

  userName = prompt("Ingresa tu nombre:");

  // Conexión LiveMedia
  p5lm = new p5LiveMedia(this, "DATA", null, "sala-dibujo");
  p5lm.on('data', gotData);
  p5lm.on('disconnect', (id) => delete otherUsers[id]);

  createColorPickerUI();
}

function draw() {
  background(255);

  // Redibujar trazos acumulados
  for (let id in otherUsers) {
    let u = otherUsers[id];
    for (let d of u.strokes) {
      drawStroke(d);
    }

    // Mostrar nombre y cursor
    fill(0);
    noStroke();
    text(u.name || "Desconocido", u.x + 10, u.y - 10);
    fill(u.color || 'gray');
    ellipse(u.x, u.y, 10, 10);
  }
}

function mouseDragged() {
  let data = {
    x: mouseX,
    y: mouseY,
    prevX: pmouseX,
    prevY: pmouseY,
    color: erasing ? '#ffffff' : currentColor,
    size: erasing ? 30 : 4,
    name: userName
  };

  // Guardar y enviar
  drawStroke(data);
  if (!otherUsers['self']) {
    otherUsers['self'] = { strokes: [], name: userName, color: currentColor, x: mouseX, y: mouseY };
  }
  otherUsers['self'].strokes.push(data);
  otherUsers['self'].x = mouseX;
  otherUsers['self'].y = mouseY;
  otherUsers['self'].color = currentColor;

  p5lm.send(JSON.stringify(data));
}

function gotData(data, id) {
  let d = JSON.parse(data);

  if (!otherUsers[id]) {
    otherUsers[id] = { strokes: [], name: d.name, color: d.color || '#000', x: d.x, y: d.y };
  }

  otherUsers[id].strokes.push(d);
  otherUsers[id].x = d.x;
  otherUsers[id].y = d.y;
  otherUsers[id].color = d.color;
}

function drawStroke(d) {
  stroke(d.color);
  strokeWeight(d.size);
  line(d.prevX, d.prevY, d.x, d.y);
}

function createColorPickerUI() {
  const colorPicker = createColorPicker(currentColor);
  colorPicker.position(10, 10);
  colorPicker.input(() => currentColor = colorPicker.value());

  const eraseBtn = createButton('🧽 Borrar');
  eraseBtn.position(10, 40);
  eraseBtn.mousePressed(() => erasing = true);

  const drawBtn = createButton('🎨 Dibujar');
  drawBtn.position(10, 70);
  drawBtn.mousePressed(() => erasing = false);
}
```
</details>

---

💡 **enlaces**

[Pruebalo](https://editor.p5js.org/DanielZafiro/sketches/_MG2AX-Lp2)
