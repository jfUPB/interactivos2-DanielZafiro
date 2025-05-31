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
  
  <img src="https://github.com/user-attachments/assets/73088bfc-cd27-4485-9348-2f175b92d913" width=800>



  <img src="https://i.imgur.com/iKdGU2T.gif" width=800>
