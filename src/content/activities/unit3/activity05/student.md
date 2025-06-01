#### Experimento 1:

- En vez de correr un cliente en el smartphone correr los dos clientes en el mismo PC en diferentes browser:

  <img src="https://i.imgur.com/iKdGU2T.gif" width=800>

- Abrir la consola de desarrollo(inspeccionar elemento) en ambos browser desktop y mobile y observar las consolas tanto del servidor como los mensajes que se envian y se reciben:

  <img src="https://i.imgur.com/toAATKu.gif" width=800>

- Esto demostró que no es obligatorio tener un dispositivo móvil; es posible testear los eventos simulando dos clientes desde la misma máquina, lo cual facilita el desarrollo y la depuración

---

#### Experimento 2:

- Enviar diferentes tipos de mensaje al hacer `socket.emit`

- Se me ocurrió que enviar datos en binario en lugar de texto JSON como esta implementado actualmente sería algo muy util para mejorar el rendimiento de la comunicación, sin embargo esto tiene ciertas implicaciones, una de ellas esque no es practico a la hora de realizar pruebas y ver en consola cual es el contenido del mensaje que se esta emitiendo, pero una vez todas las pruebas hayan sido realizadas con exito y tenga que conectar a muchos clientes el uso de la comunicacion en binario seria mucho mejor por rendimiento.

- ¿Qué significa enviar datos en binario con `socket.io`?
  - Socket.IO permite enviar tipos de datos binarios como:
    - `ArrayBuffer`
    - `Buffer` (en Node.js)
    - `Blob` (en navegadores)
    - `TypedArrays` (`Uint8Array`, `Float32Array`, etc.)


  <details>
      
  <summary>Ejemplo (cliente)</summary>
      
    ```js
    const buffer = new ArrayBuffer(8);
    const view = new Float32Array(buffer);
    view[0] = mouseX;
    view[1] = mouseY;
    socket.emit('message', buffer);

    ```
      
  </details>

  <details>
      
  <summary>Ejemplo (servidor)</summary>
      
    ```js
    socket.on('message', (buffer) => {
    const view = new Float32Array(buffer);
    console.log(`X: ${view[0]}, Y: ${view[1]}`);
    });

    ```
      
  </details>

**¿Cuándo conviene usar binario?**

| Situación                                           | ¿Conviene usar binario? | Razón                                  |
| --------------------------------------------------- | ----------------------- | -------------------------------------- |
| Comunicación simple con pocos datos (x, y)          | ❌ No                    | JSON es más fácil de manejar y leer    |
| Alto volumen de datos (p.ej., sensores o streaming) | ✅ Sí                    | Menor sobrecarga, más eficiente        |
| Precisión estricta (p.ej., `Float32`)               | ✅ Sí                    | Control total del tipo de dato         |
| Facilidad de depuración y pruebas                   | ❌ No                    | Es más difícil inspeccionar en consola |


**¿Afecta enviar los datos en binario?**

Sí, **afecta de forma positiva en rendimiento**, **pero negativa en legibilidad y simplicidad**. Veamos cómo:

**Pros**

- Reduces el **tamaño del mensaje** (sin strings, sin claves tipo `"x": ...`).
- Evitas el **parseo de JSON**, que es costoso en tiempo de CPU si se envían muchos mensajes.
- Puedes transmitir más datos por segundo (útil en instalaciones o visualizaciones intensivas).

**Contras**

- Más **complejo de depurar**.
- Requiere **conversión manual** en ambos extremos (buffer ↔ datos).
- Puede ser excesivo para proyectos simples o de prototipado rápido.

> [!TIP]
> **¿Qué es lo mejor?**
>
> - Para esta **fase de experimentación y aprendizaje**, lo mejor es **usar JSON** por su **claridad**.
> - Para proyectos finales donde se espera **mayor carga de mensajes**, **retos de rendimiento** o **uso de dispositivos con hardware limitado**, podrías migrar a binario.

> [!IMPORTANT]
> **Así podría comprobar el rendimiento con muchos mas clientes conectados:**
> 
> compararando estas dos versiones de emision:
> 
> 1. Emitiendo con `socket.emit('message', JSON.stringify({...}))`
> 2. Emitiendo con `socket.emit('message', new Float32Array([x, y]).buffer)`
> 
> Y medir en consola:
> 
> - Tamaño de mensaje (`console.log(data.byteLength)`)
> - Rendimiento (frames por segundo, tiempos de respuesta)
> - Facilidad de mantenimiento

Ejemplo implementando la emision en binario

<details>
  <summary>SERVIDOR (server.js)</summary>
  
Recepción y lectura del buffer
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
    console.log('New client connected');

    socket.on('message', (data) => {
        if (Buffer.isBuffer(data)) {
            // Convertir buffer a Float32Array
            const floatData = new Float32Array(data.buffer, data.byteOffset, data.byteLength / 4);
            const x = floatData[0];
            const y = floatData[1];
            console.log(`Received binary coordinates: x=${x}, y=${y}`);

            // Reenviar a otros clientes como binario
            socket.broadcast.emit('message', data);
        } else {
            console.log(`Received non-binary message:`, data);
        }
    });

    socket.on('disconnect', () => {
        console.log('Client disconnected');
    });
});

server.listen(port, () => {
    console.log(`Server is listening on http://localhost:${port}`);
});

```
</details>

<img src="https://i.imgur.com/a9fD8Dw.png" width="800">

<details>
  <summary>CLIENTE (mobile/sketch.js)</summary>

envío en binario con `Float32Array`

```js
let socket;

function setup() {
    createCanvas(300, 400);
    background(220);
    socket = io();

    socket.on('connect', () => {
        console.log('Connected to server');
    });

    socket.on('disconnect', () => {
        console.log('Disconnected from server');
    });

    socket.on('connect_error', (error) => {
        console.error('Socket.IO error:', error);
    });
}

function touchMoved() {
    // Creamos un buffer binario para enviar X e Y como float de 32 bits
    const buffer = new ArrayBuffer(8); // 2 * 4 bytes = 8 bytes
    const view = new Float32Array(buffer);
    view[0] = mouseX;
    view[1] = mouseY;

    socket.emit('message', buffer); // Envío binario
    return false; // Previene el scroll en móviles
}

```
  
</details>

<img src="https://i.imgur.com/tHRaFHr.png" width="800">

<details>
  <summary>CLIENTE (desktop/sketch.js)</summary>
  
- Se verifica si lo recibido es un ArrayBuffer (tipo de dato binario).
- Se interpreta como Float32Array, asumiendo que los dos primeros valores son `x` e `y`.
- Se actualiza la posición del círculo y se muestra en consola.
  
Lectura de datos binarios
```js
let socket;
let circleX = 200;
let circleY = 200;

function setup() {
    createCanvas(300, 400);
    background(220);
    socket = io();

    socket.on('connect', () => {
        console.log('Connected to server');
    });

    socket.on('message', (data) => {
        if (data instanceof ArrayBuffer) {
            const view = new Float32Array(data);
            circleX = view[0];
            circleY = view[1];
            console.log(`Received binary data: x=${circleX}, y=${circleY}`);
        } else {
            console.log("Received non-binary data:", data);
        }
    });

    socket.on('disconnect', () => {
        console.log('Disconnected from server');
    });

    socket.on('connect_error', (error) => {
        console.error('Socket.IO error:', error);
    });
}

function draw() {
    background(220);
    fill(0, 0, 255);
    ellipse(circleX, circleY, 50, 50);
}

```
</details>

<img src="https://i.imgur.com/aD3m0Vm.png" width="800">

> [!WARNING]
>
> Esto genera un error en consola ya explicaré que es lo que pasó
> 
> <img src="https://i.imgur.com/TRD5of2.png" width="800">




🧪 **Resultado**

- Flujo completo de datos en binario, desde el celular hasta el escritorio.
- Menor peso que usar JSON.
- Alta eficiencia para enviar datos de sensores, gestos o coordenadas.
