#### ¿Cómo funciona WebRTC?

---

#### 🔍 Exploración del protocolo WebRTC y su relación con mi aplicación

En esta actividad investigué los conceptos técnicos fundamentales del protocolo **WebRTC** y cómo están presentes en la aplicación colaborativa de dibujo que desarrollé con **p5LiveMedia**. A continuación, presento algunos de mis hallazgos:

---

#### 📌 ¿Qué es WebRTC?

**WebRTC (Web Real-Time Communication)** es un conjunto de APIs y protocolos que permite a navegadores y aplicaciones establecer comunicación en tiempo real, de forma directa y sin necesidad de servidores intermedios para transmitir datos, audio o video. Es decir, habilita conexiones **peer-to-peer (P2P)** entre clientes.

> 🔧 En mi aplicación: WebRTC es la base que permite a los usuarios compartir en tiempo real los datos de dibujo y movimiento del cursor a través de p5LiveMedia, sin requerir un servidor central para reenviar esa información.

---

#### 📌 ¿Cómo funciona WebRTC?

WebRTC establece una conexión P2P en tres etapas:

1. **Señalización (signaling):** Intercambio inicial de información para establecer la conexión (como direcciones IP, puertos, y capacidades).
2. **Descubrimiento de red (ICE):** Localización de rutas posibles entre pares.
3. **Establecimiento de canales seguros:** Se abren canales para transmisión de datos, audio o video.

> 🔧 En mi aplicación: p5LiveMedia abstrae estos pasos, pero en el fondo usa WebRTC para gestionar las conexiones entre pares y transmitir la información de dibujo en tiempo real.

---

#### 📌 ¿Qué es un *peer connection*?

Es la conexión directa que se establece entre dos navegadores usando WebRTC. Permite enviar y recibir flujos de datos, audio o video entre ellos.

> 🔧 En mi aplicación: cada cliente se conecta con los demás mediante un `peer connection` gestionado por la biblioteca `simplepeer` utilizada por p5LiveMedia.

---

#### 📌 ¿Qué es un *data channel*?

Es un canal de comunicación que WebRTC abre entre los peers para el envío de **datos arbitrarios (no audio ni video)** como texto, coordenadas, o comandos.

> 🔧 En mi aplicación: el canal de datos se utiliza para enviar información como la posición del mouse, color seleccionado, tamaño del pincel, y nombre de usuario. Esto permite que todos los usuarios vean las acciones del otro en tiempo real.

---

#### 📌 ¿Qué es un *media stream*?

Es un objeto que representa una transmisión de **video, audio o ambas**, ya sea desde una cámara, micrófono o desde el mismo canvas.

> 🔧 En mi aplicación: aunque se enfoca en el canal de datos, p5LiveMedia también puede usar media streams para compartir video (por ejemplo, con `createCapture()` y `p5LiveMedia("CAPTURE")`).

---

#### 📌 ¿Qué es un *ICE server*?

ICE (Interactive Connectivity Establishment) es un protocolo que ayuda a los navegadores a descubrir cómo conectarse entre sí a través de redes complicadas (NATs, firewalls). Un ICE server coordina el descubrimiento de rutas disponibles.

> 🔧 En mi aplicación: p5LiveMedia usa ICE por debajo para que los clientes puedan conectarse incluso si están en redes distintas. Todo esto se maneja automáticamente.

---

#### 📌 ¿Qué es un *STUN server*?

Un **STUN (Session Traversal Utilities for NAT)** server ayuda a los navegadores a conocer su dirección pública y los puertos accesibles desde el exterior de su red local. Es esencial para que dos peers puedan encontrarse.

> 🔧 En mi aplicación: se usa un servidor STUN público detrás de cámaras. p5LiveMedia usa este servidor para obtener la IP pública de los clientes y facilitar el enlace P2P.

---

#### 📌 ¿Qué es un *TURN server*?

Un **TURN (Traversal Using Relays around NAT)** server se utiliza cuando la conexión directa entre peers falla (por ejemplo, por restricciones de firewall). En ese caso, TURN actúa como intermediario para retransmitir el tráfico.

> 🔧 En mi aplicación: si la conexión directa no es posible, WebRTC puede recurrir a TURN para asegurar que la comunicación no falle. Esto también es manejado internamente por p5LiveMedia.

---

#### 📌 ¿Qué es un *signaling server*?

El **servidor de señalización** es necesario para intercambiar información entre pares al inicio de la conexión (como claves, direcciones, capacidades). WebRTC no define un protocolo de señalización, por lo tanto, se suele implementar con socket.io o WebSocket.

> 🔧 En mi aplicación: p5LiveMedia utiliza un servidor de señalización basado en **Socket.io**, alojado en `https://p5livemedia.itp.io`, que facilita el primer contacto entre clientes en la misma sala antes de que se establezca la conexión P2P.


