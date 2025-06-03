#### 📚 **Revisión de las actividades y conceptos aprendidos**

#### **Actividad 1: Configuración inicial**

* **Conceptos:**

  * *Servidor local con Node.js*: Entendí cómo iniciar un servidor con `express` y `socket.io` en `server.js`.
  * *WebSockets*: Protocolo para mantener una conexión abierta en tiempo real entre el cliente y el servidor.
  * *Cliente y servidor HTML + JS*: Se crean dos clientes independientes (mobile y desktop) que se conectan al servidor.

#### **Actividad 2: Envío de imágenes**

* **Conceptos:**

  * *getUserMedia + canvas*: Permite capturar una imagen desde la cámara o seleccionar un archivo.
  * *Conversión de imágenes a ArrayBuffer*: Para enviar la imagen por WebSocket.
  * *socket.emit()*: Se usa para emitir eventos con datos binarios al servidor.
  * *socket.on()*: Escucha eventos entrantes tanto del servidor como del otro cliente.

#### **Actividad 3: Comunicación visual**

* **Conceptos:**

  * *Renderización de imágenes en el cliente receptor (desktop)*.
  * *Mostrar notificación de imagen recibida*.
  * *Diferenciar entre imágenes de cámara y archivos seleccionados*.

#### **Actividad 4: Feedback**

* **Conceptos:**

  * *Feedback visual interactivo (like/dislike)*.
  * *Envío de feedback del cliente receptor al emisor* usando eventos de socket.
  * *Sincronización de estados visuales entre clientes*.

---

#### ✅ ¿Qué conceptos nuevos aprendí en esta unidad?

* Crear un servidor WebSocket local usando `socket.io` y `express`.
* Conectar múltiples clientes al mismo servidor y hacer que intercambien datos en tiempo real.
* Capturar imágenes desde la cámara con JavaScript (`getUserMedia`) y transferirlas a otro navegador.
* Gestionar eventos personalizados (`'image'`, `'feedback'`) y flujos de entrada/salida entre clientes y servidor.
* Diferenciar comportamiento según tipo de cliente (mobile como emisor, desktop como receptor).

---

#### 😵 ¿Qué conceptos me resultaron más difíciles de entender?

* **Conversión de datos binarios (imágenes)**: Convertir de `Blob` a `ArrayBuffer`, y viceversa, y enviarlos correctamente por socket.
* **Sincronización de estados**: Asegurar que el cliente mobile vea el estado actualizado del feedback sin confusión ni errores de secuencia.
* **Evitar condiciones de carrera**: Cuando el usuario mobile envía muchas imágenes seguidas sin esperar el feedback anterior.

---

#### 🧠 Análisis de la aplicación construida (Actividad 6)

**Descripción:**
Una aplicación compuesta por dos clientes (mobile y desktop) conectados a un servidor local `server.js`. El cliente mobile puede enviar imágenes (por cámara o archivos), y el cliente desktop puede dar feedback (👍 o 👎), que se refleja en el emisor.

#### **Conceptos aplicados en el código**

| Concepto                                     | Aplicación en el código                                                         |
| -------------------------------------------- | ------------------------------------------------------------------------------- |
| `express` y `socket.io`                      | Servidor local (`server.js`) para recibir conexiones y transmitir eventos       |
| WebSockets (`socket.emit`, `socket.on`)      | Envío de imágenes (`'image'`), recepción (`'display'`), feedback (`'feedback'`) |
| Captura de imagen (`getUserMedia`)           | En el cliente mobile para obtener imágenes desde la cámara                      |
| Conversión de datos (`ArrayBuffer` y `Blob`) | Transformación de imágenes para enviarlas y reconstruirlas en el receptor       |
| Interfaz dinámica (DOM API)                  | Mostrar previsualizaciones, botones, mensajes de espera y feedback              |
| Control de flujo                             | Esperar feedback antes de enviar una nueva imagen                               |

#### **Fragmentos relevantes del código**

```js
// Cliente mobile: enviar imagen
socket.emit('image', reader.result); // Enviar ArrayBuffer
```

```js
// Servidor: retransmitir imagen
socket.on('image', (data) => {
  socket.broadcast.emit('display', data);
});
```

```js
// Cliente desktop: feedback
likeBtn.onclick = () => {
  socket.emit('feedback', { type: 'like' });
};
```

```js
// Cliente mobile: recibir feedback
socket.on('feedback', (data) => {
  document.getElementById('feedbackResult').innerText = ...;
});
```

---

#### ⚠️ Limitaciones al aplicar el modelo input-procesamiento-output

* **Falta de persistencia**: Si se recarga la página, se pierde el historial de imágenes y feedback.
* **No hay lógica avanzada de procesamiento**: El servidor es un simple puente, no analiza ni procesa los datos.
* **No hay autenticación ni identificación clara**: No se asocia un usuario a un nombre o sesión única.
* **Comunicación secuencial**: Se asume un flujo controlado de un emisor y un receptor. No escala bien a múltiples usuarios simultáneos.

---

#### 🚀 ¿Qué tendría que aprender para superar estas limitaciones?

* **Almacenamiento temporal o en base de datos** (por ejemplo, usar MongoDB o Firebase).
* **Lógica de procesamiento y validación en el servidor**, por ejemplo, filtrar o analizar contenido.
* **Sistemas de autenticación de usuarios** (tokens, nombres, sesiones).
* **Manejo de múltiples clientes y sincronización avanzada**, posiblemente con sockets con salas o IDs únicos.
* **Manejo de errores y reconexión automática en el socket**.


