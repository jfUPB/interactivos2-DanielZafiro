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

<details>
  <summary>¿Qué significa enviar datos en binario con socket.io? (Click aquí)</summary>

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

<details>
  <summary>Solución al error</summary>

```
RangeError: start offset of Float32Array should be a multiple of 4
```

significa que el buffer binario recibido no está alineado correctamente para ser leído como un `Float32Array`, es decir, el inicio del arreglo (`byteOffset`) **no cae en una dirección múltiplo de 4**, lo cual es obligatorio para `Float32Array`.

- Solución recomendada:
  - Asegúrarse de que el **cliente móvil** esté enviando un `Float32Array.buffer` directamente (no un `Uint8Array` o similar!), y que **el servidor no esté transformando ni reinterpretando** esos datos.

Así sería el flujo corregido y seguro:

<details>
  <summary>CLIENTE MOBILE</summary>

 emitiendo binario correctamente

```js
function touchMoved() {
    const data = new Float32Array([mouseX, mouseY]);
    socket.emit('message', data.buffer); // ✅ Se envía el ArrayBuffer directamente
    return false;
}
```
</details>

<details>
  <summary>Servidor</summary>

manejo correcto del buffer binario

```js
io.on('connection', (socket) => {
    console.log('New client connected');

    socket.on('message', (data) => {
        if (data instanceof ArrayBuffer) {
            try {
                // No usar offset ni longitud personalizada
                const floatData = new Float32Array(data);
                console.log(`Received binary: x=${floatData[0]} y=${floatData[1]}`);

                // Retransmitir el mismo buffer sin modificar
                socket.broadcast.emit('message', data);
            } catch (e) {
                console.error("Error parsing binary data:", e);
            }
        } else {
            console.log("Non-binary message:", data);
        }
    });

    socket.on('disconnect', () => {
        console.log('Client disconnected');
    });
});
```
</details>

</details>

- **¿Por qué funciona esta solución?**
    - `new Float32Array(data)` es más seguro que pasar manualmente `byteOffset` y `byteLength` si no tienes garantías de alineación.
    - Esto funciona bien **si el emisor usa `Float32Array(buffer)` directamente**, como hicimos en el `touchMoved()` del móvil.

</details>

<img src="https://i.imgur.com/OVoaDoq.gif" width="800">

🧪 **Resultado emitiendo y recibiendo en binario**

- Flujo completo de datos en binario, desde el celular hasta el escritorio.
- Menor peso que usar JSON.
- Alta eficiencia para enviar datos de sensores, gestos o coordenadas.

**Así quedarian los scripts:**

<details>
  <summary>CLiente Mobile Sketch.js</summary>
  
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

function draw() {
    background(220);
    fill(255, 128, 0);
    textAlign(CENTER, CENTER);
    textSize(24);
    text('Touch to move the circle', width / 2, height / 2);
}

function touchMoved() {
    const data = new Float32Array([mouseX, mouseY]);
    socket.emit('message', data.buffer);

    //socket.emit('message', buffer); // Envío binario
    return false; // Previene el scroll en móviles
}

```
</details>

<details>
  <summary>Server server.js</summary>
  
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
            const floatData = new Float32Array(data);
            console.log(`Received binary coordinates:`, floatData);

            // Reenviar a otros clientes como binario
            socket.broadcast.emit('message', data);
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

<details>
  <summary>Cliente Desktop Sketch.js</summary>
  
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
            const floatData = new Float32Array(data);
            circleX = floatData[0];
            circleY = floatData[1];
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
    fill(255, 0, 0);
    ellipse(circleX, circleY, 50, 50);
}

```
</details>

---

#### Experimento 3:

- modificar los datos que estamos enviando... entonces para este experimento simplemente quise cambiar el color del circulo con cada nueva posicion, para esto tuve que modificar el código para que el cliente mobile envíe un color aleatorio (en formato RGB) junto con las coordenadas, y que el cliente desktop lo reciba y lo use para pintar el círculo.

<img src="https://i.imgur.com/QFUzFI7.gif" width="800">

<details>
  <summary>Cliente mobile</summary>

se agrega una función para generar colores aleatorios y envía r, g, b:
```js
let socket;
let lastTouchX = null;
let lastTouchY = null;
const threshold = 5;

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

function draw() {
    background(220);
    fill(255, 128, 0);
    textAlign(CENTER, CENTER);
    textSize(24);
    text('Touch to move', width / 2, height / 2);
}

function touchMoved() {
    if (socket && socket.connected) {
        let dx = abs(mouseX - lastTouchX);
        let dy = abs(mouseY - lastTouchY);

        if (dx > threshold || dy > threshold || lastTouchX === null) {
            let touchData = {
                type: 'touch',
                x: mouseX,
                y: mouseY,
                r: random(255),
                g: random(255),
                b: random(255)
            };
            socket.emit('message', JSON.stringify(touchData));

            lastTouchX = mouseX;
            lastTouchY = mouseY;
        }
    }
    return false;
}

```
</details>

<details>
  <summary>Cliente desktop</summary>

se agrega la recepción de color
```js
let socket;
let circleX = 200;
let circleY = 200;
let circleColor = [255, 0, 0];

function setup() {
    createCanvas(300, 400);
    background(220);
    socket = io();

    socket.on('connect', () => {
        console.log('Connected to server');
    });

    socket.on('message', (data) => {
        try {
            let parsedData = JSON.parse(data);
            if (parsedData.type === 'touch') {
                circleX = parsedData.x;
                circleY = parsedData.y;
                if (parsedData.r !== undefined && parsedData.g !== undefined && parsedData.b !== undefined) {
                    circleColor = [parsedData.r, parsedData.g, parsedData.b];
                }
            }
        } catch (e) {
            console.error("Error parsing received JSON:", e);
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
    fill(circleColor[0], circleColor[1], circleColor[2]);
    ellipse(circleX, circleY, 50, 50);
}

```
</details>

- No es necesario modificar el server.js puesto que solo retransmite el mensaje tal cual y el procesamiento(intepretacion) lo hace cada browser

---

#### Experimento 4:

- Notar el `type` de mensaje que se envía y hacer pruebas para entenderlo, eliminando el type `touch`...

- **¿Qué representa `"type": "touch"`?**
    - Ese `"type"` es **una etiqueta** que tú mismo defines para que el receptor sepa **cómo interpretar el contenido del mensaje**. En el caso actual:

    ```js
    {
      type: "touch",
      x: 123,
      y: 456
    }
    ```

→ Significa: "Esto es un mensaje que trae las coordenadas de un toque en pantalla".

<details>
  <summary>Cuando usar o no type (Click aquí)</summary>
  
- **¿Por qué no hay más `"type"` por ahora?**
    - Porque **solo estás enviando un tipo de información**: la posición del toque (x, y). Entonces **no necesitamos más tipos**, porque todo lo que llega se interpreta igual.


- **¿Cuándo se necesita más de un `"type"`?**
    - Cuando el servidor o el receptor deben **distinguir entre diferentes tipos de acciones o efectos**, por ejemplo:
    
    ```js
    // Mensaje 1: posición de toque
    {
      type: "touch",
      x: 100,
      y: 200
    }
    
    // Mensaje 2: cambio de color
    {
      type: "color",
      r: 255,
      g: 0,
      b: 0
    }
    
    // Mensaje 3: mensaje de texto
    {
      type: "chat",
      text: "Hola desde el móvil"
    }
    ```

Cada uno de estos tiene un propósito **diferente**. Si los envías todos por el mismo evento `"message"`, **el campo `type` es necesario** para que el receptor sepa qué hacer con cada uno.


- **Alternativa: usar eventos personalizados**
  - En lugar de tener un solo canal `"message"` y revisar `"type"`, puedes **crear eventos específicos**:

    ```js
    socket.emit("touch", { x: 100, y: 200 });
    socket.emit("colorChange", { r: 255, g: 0, b: 0 });
    socket.emit("chatMessage", { text: "Hola desde el móvil" });
    ```

    Y en el receptor:
    
    ```js
    socket.on("touch", handleTouch);
    socket.on("colorChange", handleColor);
    socket.on("chatMessage", handleChat);
    ```

Esto elimina la necesidad del campo `"type"` por completo, porque el **evento ya dice qué tipo de mensaje es**.

</details>

| Caso                                                   | Necesario o no                                | 
| ------------------------------------------------------ | --------------------------------------------- |
| ¿Enviás solo una cosa?                                 | No necesitas `"type"`                         |
| ¿Enviás muchas cosas distintas por el mismo canal?     | Necesitás `"type"`                            |
| ¿Preferís eventos separados para cada tipo de mensaje? | No necesitás `"type"`, usás nombres distintos |

---

**Pongamoslo a prueba usando el experimento 3:**

- integrando dos tipos de mensajes en el sistema actual: "touch" y "color".
  - Implica modificar el cliente móvil para enviar dos tipos de mensajes y el cliente de escritorio para interpretar los mensajes según el `type`

<img src="https://i.imgur.com/MB1Ybwt.gif" width="800">

<details>
  <summary>Cliente mobile</summary>

```js
let socket;
let lastTouchX = null;
let lastTouchY = null;
const threshold = 5;

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

function draw() {
    background(220);
    fill(255, 128, 0);
    textAlign(CENTER, CENTER);
    textSize(18);
    text('Toca para mover\nPresiona una tecla para color', width / 2, height / 2);
}

function touchMoved() {
    if (socket && socket.connected) {
        let dx = abs(mouseX - lastTouchX);
        let dy = abs(mouseY - lastTouchY);

        if (dx > threshold || dy > threshold || lastTouchX === null) {
            let touchData = {
                type: 'touch',
                x: mouseX,
                y: mouseY
            };
            socket.emit('message', JSON.stringify(touchData));

            lastTouchX = mouseX;
            lastTouchY = mouseY;
        }
    }
    return false;
}

function keyPressed() {
    if (socket && socket.connected) {
        // Cambiar aleatoriamente el color
        let r = floor(random(0, 256));
        let g = floor(random(0, 256));
        let b = floor(random(0, 256));

        let colorData = {
            type: 'color',
            r: r,
            g: g,
            b: b
        };

        socket.emit('message', JSON.stringify(colorData));
    }
}

```
</details>

el server.js queda igual porque se encarga de reenviar los mensajes

<details>
  <summary>Cliente desktop</summary>

```js
let socket;
let circleX = 200;
let circleY = 200;
let circleColor = [255, 0, 0]; // color inicial rojo

function setup() {
    createCanvas(300, 400);
    background(220);

    socket = io();

    socket.on('connect', () => {
        console.log('Connected to server');
    });

    socket.on('message', (data) => {
        console.log(`Received message: ${data}`);
        try {
            let parsedData = JSON.parse(data);

            if (parsedData.type === 'touch') {
                circleX = parsedData.x;
                circleY = parsedData.y;
            } else if (parsedData.type === 'color') {
                circleColor = [parsedData.r, parsedData.g, parsedData.b];
            }
        } catch (e) {
            console.error("Error parsing received JSON:", e);
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
    fill(circleColor[0], circleColor[1], circleColor[2]);
    ellipse(circleX, circleY, 50, 50);
}

```
</details>

---

#### Aprendizajes experimentando

<details>
  <summary>Aprendizajes</summary>

**Experimento 1: Ejecutar dos clientes en el mismo computador**

**¿Qué hice?**  
Corrí la aplicación en dos navegadores diferentes (uno como cliente móvil y otro como cliente de escritorio), ambos desde el mismo computador.

**¿Qué aprendí?**
- Aprendí que puedo simular múltiples clientes sin necesidad de usar dispositivos diferentes.
- Descubrí cómo ver en tiempo real los mensajes enviados y recibidos en la consola del navegador y del servidor.
- Confirmé cómo `socket.broadcast.emit()` permite enviar mensajes a todos los clientes excepto al emisor.

---

**Experimento 2: Cambiar el tipo de mensajes emitidos**

**¿Qué hice?**  
Modifiqué el tipo de datos emitidos por el cliente, probando diferentes estructuras, incluyendo la posibilidad de enviar datos binarios (aunque no lo mantuve por ahora).

**¿Qué aprendí?**
- Entendí que no es necesario enviar datos como JSON con `JSON.stringify()`, también puedo enviar directamente objetos.
- Aprendí que enviar datos en binario es más eficiente en ciertos casos, pero requiere cuidado con la alineación de memoria (por ejemplo, en el uso de `Float32Array`).
- Confirmé que puedo personalizar el mensaje según lo que necesite enviar, manteniendo claridad en el receptor.

---

**Experimento 3: Modificar los datos enviados**

**¿Qué hice?**  
Agregué un nuevo conjunto de datos al mensaje, específicamente valores RGB para modificar el color del círculo en el cliente de escritorio.

**¿Qué aprendí?**
- Aprendí a extender el mensaje original para transmitir múltiples parámetros a la vez.
- Practiqué cómo estructurar mensajes más completos (por ejemplo, con propiedades `x`, `y`, `r`, `g`, `b`).
- Descubrí cómo condicionar el receptor para que reaccione solo cuando se reciben ciertos datos.

---

**Experimento 4: Eliminar el campo `type` y modificar el mensaje**

**¿Qué hice?**  
Eliminé el campo `"type"` en el mensaje emitido desde el cliente móvil y observé cómo esto afectaba la lógica del receptor.

**¿Qué aprendí?**
- Comprendí que el campo `type` es clave cuando se manejan múltiples tipos de mensajes.
- Aprendí que, sin este campo, el receptor pierde contexto y puede interpretar mal los datos o fallar al procesarlos.
- Valoré la importancia de mantener mensajes estructurados y autoexplicativos para facilitar la escalabilidad del sistema.


</details>
