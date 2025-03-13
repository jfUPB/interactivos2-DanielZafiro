#### Manipulación de pixeles de imagenes

Este código carga una imagen, la redimensiona y crea una versión "pixelada" en la que cada bloque de píxeles se dibuja como un círculo. 

El tamaño de cada círculo varía en función de la distancia al mouse: cuanto más cerca esté el mouse, más pequeño será el círculo. 

Además, para lograr un efecto de aberración cromática, se dibujan tres círculos por bloque, desplazando ligeramente los canales rojo y azul respecto al canal verde. Todo esto se actualiza en tiempo real, generando una interacción visual dinámica.

<img src="https://github.com/user-attachments/assets/153fb2b3-24fe-40e1-a219-ef0765168c32" width="350">

<details>

  <summary>Codigo (Click Aquí)</summary>

  ```js
  let img;
  let blockSize = 10; // Tamaño base de cada bloque de píxeles
  
  function preload() {
    // Asegúrate de tener la imagen en la carpeta "data" y usar el nombre correcto.
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
