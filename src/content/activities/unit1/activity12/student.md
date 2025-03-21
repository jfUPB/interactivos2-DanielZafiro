#### Manipulación de pixeles de imagenes

Este código carga una imagen, la redimensiona y crea una versión "pixelada" en la que cada bloque de píxeles se dibuja como un círculo. 

El tamaño de cada círculo varía en función de la distancia al mouse: cuanto más cerca esté el mouse, más pequeño será el círculo. 

Además, para lograr un efecto de aberración cromática, se dibujan tres círculos por bloque, desplazando ligeramente los canales rojo y azul respecto al canal verde. Todo esto se actualiza en tiempo real, generando una interacción visual dinámica.

<img src="https://github.com/user-attachments/assets/153fb2b3-24fe-40e1-a219-ef0765168c32" width="400">


<details>

  <summary>Codigo (Click Aquí)</summary>

  ```js
  let img;
  let blockSize = 10; // Tamaño base de cada bloque de píxeles
  
  function preload() {
    // Asegúrate de tener la imagen en la carpeta junto a los otros scripts o dentro de la carpeta "data" para mantener el orden y usar el nombre correcto.
    img = loadImage('AstroBoy.jpg');
  }
  
  function setup() {
    createCanvas(800, 600);
    pixelDensity(1);         // Reduce la densidad de píxeles para mejorar el rendimiento en pantallas de alta resolución
    img.resize(400, 0);       // Reducir la resolución de la imagen para procesar menos píxeles
    // Elimina noLoop() para actualizar en tiempo real
  }
  
  function draw() {
    background(255);
    
    // Calcular la escala para adaptar la imagen al canvas
    let scaleX = width / img.width;
    let scaleY = height / img.height;
    
    // Parámetros para la modulación del tamaño basado en la distancia al mouse
    let influenceRadius = 100; // Radio de influencia (en píxeles)
    let minScale = 0.2;        // Escala mínima (20% del tamaño base) cuando el mouse está muy cerca
    
    // Recorremos la imagen en pasos de blockSize
    for (let y = 0; y < img.height; y += blockSize) {
      for (let x = 0; x < img.width; x += blockSize) {
        
        // Obtener el color del píxel de la imagen
        let c = img.get(x, y);
        let r = red(c);
        let g = green(c);
        let b = blue(c);
        
        // Calcular la posición en el canvas
        let posX = x * scaleX;
        let posY = y * scaleY;
        
        // Calcular la distancia entre el mouse y el centro del "pixel"
        let d = dist(mouseX, mouseY, posX, posY);
        
        // Mapear la distancia a un factor de escala
        let factor = map(d, 0, influenceRadius, minScale, 1, true);
        let effectiveSize = blockSize * scaleX * factor;
        
        noStroke();
        
        // Dibujar canal verde en la posición original (base)
        fill(0, g, 0, 200);
        ellipse(posX, posY, effectiveSize, effectiveSize);
        
        // Dibujar canal rojo desplazado a la izquierda para aberración cromática
        fill(r, 0, 0, 150);
        ellipse(posX - 2, posY, effectiveSize, effectiveSize);
        
        // Dibujar canal azul desplazado a la derecha
        fill(0, 0, b, 150);
        ellipse(posX + 2, posY, effectiveSize, effectiveSize);
      }
    }
  }

  ```

  
</details>

**Aplicaciones Potenciales**

- Instalaciones Interactivas:
    - Ideal para proyecciones o pantallas interactivas donde el efecto responde en tiempo real al movimiento del espectador.

- Diseño Multimedia:
    - Puede utilizarse para crear efectos visuales dinámicos en videos o presentaciones digitales.

- Arte Generativo:
    - Transformar imágenes en composiciones abstractas y "pop-up" que cambian en función de la interacción del usuario.

---

> [!TIP]
> Este punto me quedo en la mente sonando durante dias ya que la propuesta que habia propuesto no me encajaba en la mente que fuera generativa dados los cocneptos que ya hemos venido trabajando.
>
> Es interesante que se reinterprete una imagen x para volverla en estilo pop-up y que además se le disponga una averración cromatica como filtro estilizado, sin embargo estas caracteristicas solamente generan un producto reactivo pero y donde esta lo gnerativo..

Para hacer que el efecto sea generativo, incorporamos elementos que evolucionen de forma autónoma con el tiempo, además de la interacción del mouse.

- **Añadiré un factor basado en el tiempo:**  
  Por ejemplo, usar `frameCount` o `noise()` en función del tiempo para modificar el tamaño o el color de los círculos. Así, incluso si el mouse está quieto, la imagen cambiará continuamente.

Entonces esta es la nueva versión modificada del código quitando la averracion cromatica e incorporando un factor generativo basado en `noise()` y `frameCount` para que, además de la reactividad, haya una evolución autónoma del patrón:

**Generativo + Reactivo**

<img src="https://github.com/user-attachments/assets/7ff8042e-ecb0-4d30-850d-a2026f2d86cd" width="400">


<details>
  
<summary>Código en p5.js (Click Aquí)</summary>
  
```javascript
let img;
let blockSize = 10; // Tamaño base de cada bloque de píxeles

function preload() {
  // Asegúrate de tener la imagen en la carpeta "data" o en el mismo directorio
  img = loadImage('data/AstroBoy.jpg');
}

function setup() {
  createCanvas(800, 600);
  pixelDensity(1);         // Reduce la densidad para mejorar el rendimiento en pantallas de alta resolución
  img.resize(400, 0);       // Reducir la resolución de la imagen para procesar menos píxeles
  frameRate(30);
}

function draw() {
  background(255);
  
  // Calcular la escala para adaptar la imagen redimensionada al canvas
  let scaleX = width / img.width;
  let scaleY = height / img.height;
  
  // Parámetros para la modulación del tamaño basado en la distancia al mouse y el tiempo
  let influenceRadius = 100; // Radio de influencia (en píxeles)
  let minScale = 0.2;        // Escala mínima (20% del tamaño base) cuando el mouse está muy cerca
  
  // Factor generativo que evoluciona con el tiempo usando noise
  let timeFactor = noise(frameCount * 0.02);
  
  // Recorremos la imagen en pasos de blockSize
  for (let y = 0; y < img.height; y += blockSize) {
    for (let x = 0; x < img.width; x += blockSize) {
      
      // Obtener el color del píxel de la imagen
      let c = img.get(x, y);
      
      // Calcular la posición en el canvas
      let posX = x * scaleX;
      let posY = y * scaleY;
      
      // Calcular la distancia entre el mouse y el centro del "pixel"
      let d = dist(mouseX, mouseY, posX, posY);
      
      // Mapear la distancia a un factor de escala basado en el mouse
      let mouseFactor = map(d, 0, influenceRadius, minScale, 1, true);
      
      // Combinar el factor del mouse con un factor generativo basado en el tiempo y noise
      let factor = lerp(mouseFactor, 1 - timeFactor, 0.5);
      let effectiveSize = blockSize * scaleX * factor;
      
      noStroke();
      fill(c, 200);
      ellipse(posX, posY, effectiveSize, effectiveSize);
    }
  }
}
```

</details>

**Descripción**

Este código en p5.js carga una imagen ("AstroBoy.jpg") y la redimensiona para reducir la cantidad de píxeles a procesar. La imagen se transforma en un efecto "pixelado" en el que cada bloque se representa como un círculo. El tamaño de cada círculo se modula de dos maneras:

- **Reactividad al mouse:**  
  Los círculos se hacen más pequeños si están cerca del mouse (usando `dist(mouseX, mouseY, posX, posY)`).

- **Evolución generativa:**  
  Se introduce un factor generativo basado en `noise(frameCount * 0.02)` que hace que el tamaño de los círculos evolucione de forma autónoma con el tiempo. Se combina este factor con el efecto del mouse utilizando `lerp()`, de manera que el patrón cambia continuamente incluso sin interacción directa.

El resultado es un efecto visual híbrido, que combina **elementos reactivos** y **generativos** para ofrecer una experiencia dinámica y en constante evolución.


**Aplicaciones Potenciales**

- **Reacción generativa con recepción de audio**
  La imagen se transforma dependiendo del audio captado por un microfono

- **Instalaciones interactivas:**  
  La imagen se transforma en tiempo real en respuesta al movimiento del espectador, creando una experiencia inmersiva.

- **Visualizaciones de arte digital:**  
  Puede usarse para generar fondos y efectos visuales que evolucionen de forma autónoma, útiles en presentaciones y proyecciones.

- **Experiencias multimedia:**  
  Combina elementos generativos y reactivos, ideal para aplicaciones en videojuegos o aplicaciones interactivas donde la estética evoluciona con el tiempo.

