#### Pruebas Locales

> [!NOTE]
> Para este actividad realicé todo el proceso desde Windows

**1. Clonar el repositorio [sfiSocketioDesktopMobile](https://github.com/juanferfranco/sfiSocketioDesktopMobile)** 

- Este repositorio contiene todos los elementos necesarios para habilitar un servidor web de manera local con un experimento que permite ver la posición de un circulo en un cliente(desktop) y controlar dicha posición mediante otro cliente(mobile) esto gracias a socket.io que permite la comunicación entre los clientes pero toda esta comunicación mediada por el servidor local, y visualmente usando la libreria de p5js

- Realicé un fork al repositorio brindado por el profe y abri el fork desde mi VSC iniciando sesion con github y abriendo el repositorio clonado, esto con el fin de poder hacer mis propias modificaciones y presonalizaciones

  <img src="https://github.com/user-attachments/assets/04d7885b-f397-4771-a8fa-ce4a9c65174b" width=550>


**2. Instalar NodeJS**

- Muy importante para poder encender el servidor es necesario tener instalado el Node JS antes de poder ejercutar los comandos en terminal Bash de `npm install` y `npm start` del VSC, de lo contrario habrán errores de comandos no encontrados.

  <img src="https://github.com/user-attachments/assets/4f41a967-d1e9-4716-b852-d67aabf01cb0" width=400>


- una vez instalado el `npm` se podrá encender el servidor localmente con el comando `npm start` mediante una linea de codigo en el script del servidor `server.js` se imprime en consola que el servidor esta escuchando a la espera de clientes que se conecten a el mediante el puerto asignado en este caso `http://localhost:3000` el cual haciendo pruebas solo funciona en el computador localmente
  
  <img src="https://i.imgur.com/iKdGU2T.gif" width=800>

- Hay una manera de hacer que estos accesos locales de los clientes puedan hacerse remotamente y es con los tuneles de puerto que es un servicio de Microsoft en Github para activarlos hay que abrirlos enviarlos y esta es la descripción:

  ![image](https://github.com/user-attachments/assets/e41b1dea-e399-4f39-bb7e-dec5051d6607)
  
  ![image](https://github.com/user-attachments/assets/d96346a3-ad54-4bd2-85ab-e0fea45dbfa1)

- En la pestaña de puertos se creó un enlace al puerto 3000 desde el cual puede ser abierto en cualquier otro dispositivo en diferente conexión wifi siempre y cuando se inicie sesión en github por la privacidad establecida en privado, es una manera de amplificar la experimentación entre los clientes y servidor

  ![image](https://github.com/user-attachments/assets/f69edcbf-772a-4c07-b5d0-834814ed424b)

  ```markdown
  https://dr71n378-3000.use2.devtunnels.ms/ (Tunel del puerto 3000 local creado y conectado a GitHub)
  https://dr71n378-3000.use2.devtunnels.ms/desktop (Tunel de acceso para el cliente desktop)
  https://dr71n378-3000.use2.devtunnels.ms/mobile (Tunel de acceso para el cliente mobile)
  ```

- En la `Terminal > OUTPUT > Port Forwarding` Podemos observar lo que ocurrio cuando reenviamos el puerto 3000 reenvia el puerto local a traves del servicio de tuneles de desarrollo y lo establece privado.

  ![image](https://github.com/user-attachments/assets/99549602-a06d-4566-88e0-cf3cd0608f8f)

- A continuación se puede observar como estoy controlando el circulo rojo mediante los enlaces de los tuneles del puerto 3000 mediante el buscador del iPhone utilizando los datos moviles y no estando conectado al mismo wifi, 

  <img src="https://i.imgur.com/EOzuC1S.gif" width=800>


