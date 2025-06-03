#### Diseño preliminar

🔄 ¿Qué pasa antes de la experiencia?

Los visitantes llegan a la plaza principal al caer la tarde. El entorno está iluminado discretamente con tonos cálidos y música ambiente instrumental. Se reparten códigos QR discretamente ubicados para acceder a la webapp desde sus dispositivos móviles mostrando un "stand by" con instruccion de mover el dedo por la pantalla. No se requiere instalación previa y cuando estén listos se iniciaria la secuencia (de 8 minutos ya implementada en unity).

---

🌟 ¿Qué pasa durante la experiencia?

- La experiencia inicia con una secuencia audiovisual de 9 minutos dividida en tres momentos:

- Durante Cuaresma, los visitantes pueden deslizar su dedo en sus celulares y generar visuales de incienso proyectado en tiempo real.

  <img src="https://github.com/user-attachments/assets/e2d474fe-8816-4646-a438-3f9fa3b59b27" width="300">
  <img src="https://github.com/user-attachments/assets/26e54341-ee13-4435-aec8-4ed9b8d39d01" width="300">



- Durante Semana Santa, los toques en pantalla podrían activar la aparición de velas con flores que acompañan visualmente los pasos procesionales en las proyecciones, evocando la tradición de los feligreses que acompañan las procesiones con velas encendidas. Estas velas se proyectarán de manera sutil alrededor de las figuras procesionales, respetando la atmósfera de recogimiento y reforzando la narrativa devocional.

  <img src="https://github.com/user-attachments/assets/7952a923-1a5f-4431-a02d-35969d61cc53" width="300">
  <img src="https://github.com/user-attachments/assets/33476620-060a-44b7-b7c3-c5c3cbe34626" width="300">



- Durante Pascua, los movimientos de los dedos generan flores (la clavelilla, claveles, lirios, crisantemos y hortensias) que brotan en fachadas y casas, en una atmósfera de celebración y esperanza.

  <img src="https://www.plantasonya.com.br/wp-content/img/lirios8.gif" width="300">
  <img src="https://i.pinimg.com/originals/af/e5/ba/afe5bac67a1eedee24c74ee2b39807c1.gif" width="200">

Las luces y el sonido se sincronizan automáticamente con la narrativa audiovisual.

---

🔁 ¿Qué pasa después de la experiencia?

Una vez termina el evento, la plaza vuelve a su estado de calma con una música suave de cierre y proyecciones de la arquitectura de la fachada sin movimientos bruscos o cambios de color abruptos y permitiendo a los visitantes escribir su nombre para que en la fachada se decore con un lirio con su nombre. Desde la webapp,  Además, se podría mostrar un resumen visual de la participación colectiva durante el evento y la invitación al museo de arte religioso.

<img src="https://lacasadelasflores.com.mx/img/loading.gif" width="200">

---

🧵 ¿Cómo se conecta la narrativa con la experiencia?

Cada interacción está pensada para respetar el simbolismo del momento litúrgico: incienso como purificación (Cuaresma), velas con flores como procesión (Semana Santa), flores como resurrección (Pascua), nombres en flores como devoción. La interacción potencia el relato espiritual sin descontextualizarlo.

---

📲 ¿Qué tipo de dispositivos vas a utilizar?

- Servidor central (PC o laptop) con Node.js.

- Dispositivos móviles del público (celulares/tablets).

- Dispositivo móvil de control (facilitador, guía o técnico).

- Equipos de proyección y luces programables.

---

🧾 ¿Qué tipo de datos vas a capturar?

- Posiciones del dedo en pantalla.

- Cantidad de toques de la pantalla

- Tiempos de interacción por usuario.

Opcional: ubicación relativa si se integra con sensores.

---

🔗 ¿Cómo se conectan esos datos con la narrativa?

Cada dato de interacción (posición, momento, frecuencia) determina el tipo de visual que se activa. Esto permite que cada visitante influya sutilmente en la narrativa, transformándola sin romper su coherencia.

---

🎛 ¿Qué tipo de control remoto vas a utilizar?

Un celular o tablet en modo facilitador, conectado al sistema por WebSocket, permitirá iniciar la secuencia, pausarla, reiniciarla o lanzar efectos especiales, ademas de visualizar cantidad de personas conectadas.

---

🧩 ¿Qué tipo de interacción en tiempo real te gustaría tener?

Interacciones visuales proyectadas colectivamente, activadas desde dispositivos móviles, con respuesta visual inmediata en la fachada y las casas. Además de posibles efectos de sonido activados por acumulación de acciones como aplauso individual + aplausos de todos los visitantes escuchandose en los parlantes.

---

🎥 ¿Qué tipo de contenido en tiempo real te gustaría generar?

Visuales generativos sincronizados con la narrativa (incienso, velas, flores creciendo), adaptados a la arquitectura de la iglesia y su entorno. También posibilidad de mostrar en tiempo real métricas de participación o reacciones poéticas de los asistentes.


---

**Repositorio del proyecto:**

[SFI_2_Proyecto_Curso_Santa_Pasion](https://github.com/DannieLudens/SFI_2_Proyecto_Curso_Santa_Pasion/tree/main)

Visualizacion preliminar:

![OmenCommandCenterBackground_KCaeLAIHhs](https://github.com/user-attachments/assets/1c607b42-933f-43ce-b78c-fbcbc7a9bdc2)


