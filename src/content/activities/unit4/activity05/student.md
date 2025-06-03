#### mi proceso de aprendizaje en la Unidad 4

✅ **¿Qué conceptos nuevos aprendí en esta unidad?**

En esta unidad aprendí varios conceptos fundamentales relacionados con la comunicación en tiempo real a través del protocolo **WebRTC**. Algunos de los más destacados fueron:

* Qué es y cómo funciona WebRTC.
* Qué son los *peer connections*, *data channels* y *media streams*.
* El rol de los servidores **STUN**, **TURN** e **ICE** en el establecimiento de conexiones P2P.
* Cómo funciona un servidor de señalización (*signaling server*).
* Cómo utilizar la biblioteca **p5LiveMedia** para compartir datos, audio, video o canvas entre navegadores.

---

😊 **¿Qué conceptos de la unidad me resultaron más fáciles de entender?**

Los conceptos que me resultaron más intuitivos y fáciles de entender fueron:

* El uso básico de **p5LiveMedia** para compartir datos con otros usuarios conectados a una misma sala.
* La lógica de envío y recepción de datos en tiempo real mediante eventos (`p5lm.send()` y `p5lm.on('data', ...)`) que me recordaron al emision y recepcion de la unidad anterior con el `socket.emit` y `.on`
* La creación de interfaces sencillas en p5.js, como color pickers, botones y herramientas para dibujar.

---

😵 **¿Qué conceptos de la unidad me resultaron más difíciles de entender?**

Los conceptos más complejos y que requirieren mayor investigación y prueba son:

* El funcionamiento de **WebRTC** a nivel técnico, especialmente el rol de los servidores *STUN* y *TURN* en la negociación de conexiones.
* Los errores relacionados con la gestión de streams (por ejemplo, el error de `getTracks is not a function`), que surgieron cuando se pasaban objetos incorrectos como fuentes de video.
* La sincronización de múltiples clientes y la gestión de eventos de conexión y desconexión.

---

🛠️ **¿Qué habilidades nuevas adquirí en esta unidad?**

Gracias al trabajo práctico y exploratorio de esta unidad, adquirí nuevas habilidades como:

* Integrar WebRTC a través de la biblioteca p5LiveMedia en mis propios sketches.
* Diseñar y programar una aplicación colaborativa en tiempo real con p5.js.
* Implementar interfaces de usuario simples para interacción compartida (dibujar, cambiar color, usar borrador).
* Comprender los fundamentos de comunicación peer-to-peer para el desarrollo de experiencias interactivas distribuidas.
* Diagnosticar errores comunes en WebRTC y entender mejor el flujo de datos entre clientes.

