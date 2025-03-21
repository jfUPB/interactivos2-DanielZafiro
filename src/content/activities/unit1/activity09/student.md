#### Programa que genera formas geometricas con elementos aleatorios

**Descripción del programa:**

El código crea un lienzo de 800 x 600 píxeles y dibuja 20 formas geométricas aleatorias cada vez que se ejecuta el `draw()`. Cada forma es elegida al azar entre un círculo, un cuadrado o un triángulo. Se asignan posiciones, tamaños, colores de relleno (con un nivel de transparencia) y colores de borde de forma aleatoria. Al hacer clic en el canvas, se genera un nuevo conjunto de formas.

<details>
  
  <summary>código de p5js (Click Aquí)</summary>

  sketch.js
  ```js
  function setup() {
    createCanvas(800, 600);
    noLoop(); // Se dibuja una sola vez hasta que se presione el mouse
  }
  
  function draw() {
    background(220); // Fondo gris claro
  
    // Genera 20 formas aleatorias
    for (let i = 0; i < 20; i++) {
      // Elegir un tipo de forma aleatorio: 0 = círculo, 1 = cuadrado, 2 = triángulo
      let shapeType = floor(random(3));
      
      // Posición aleatoria dentro del canvas
      let x = random(width);
      let y = random(height);
      
      // Tamaño aleatorio entre 30 y 100
      let randSize = random(30, 100);
      
      // Color de relleno aleatorio (con transparencia) y color de borde aleatorio
      fill(random(255), random(255), random(255), random(100, 200));
      stroke(random(255), random(255), random(255));
      strokeWeight(random(1, 5));
      
      // Dibujar la forma elegida
      if (shapeType === 0) {
        ellipse(x, y, randSize, randSize); // Círculo
      } else if (shapeType === 1) {
        rect(x, y, randSize, randSize); // Cuadrado
      } else {
        triangle(
          x, y - randSize / 2,          // Punto superior
          x - randSize / 2, y + randSize / 2, // Punto inferior izquierdo
          x + randSize / 2, y + randSize / 2  // Punto inferior derecho
        ); // Triángulo
      }
    }
  }
  
  // Al presionar el mouse, se redibuja el canvas para generar nuevas formas
  function mousePressed() {
    redraw();
  }
  
  ```
  
</details>


**Parámetros clave:**

- `shapeType`: Determina el tipo de forma a dibujar (0 = círculo, 1 = cuadrado, 2 = triángulo).
- `x` y `y`: Coordenadas aleatorias para la posición de cada forma.
- `ranSize`: Tamaño aleatorio asignado a cada forma (entre 30 y 100).
- `fill()` y `stroke()`: Se utilizan para asignar colores aleatorios (incluyendo transparencia para el relleno).
- `strokeWeight()`: Controla el grosor del borde de las formas, también asignado aleatoriamente.

**Funciones de p5.js utilizadas para controlar la aleatoriedad y apariencia:**

- `random()`: Para obtener valores aleatorios para posición, tamaño y color.
- `floor()`: Para convertir valores flotantes en enteros (utilizado al elegir el tipo de forma).
- `noLoop()` y `redraw()`: Para que el dibujo se realice solo una vez y se actualice al presionar el mouse, permitiendo experimentar con diferentes resultados sin sobrecargar el lienzo.

![Screen Shot 2025-03-04 at 10 55 57 AM](https://github.com/user-attachments/assets/78980662-35da-47ec-9274-ecef16760fc6)

![Screen Shot 2025-03-04 at 10 55 42 AM](https://github.com/user-attachments/assets/bcea6413-ff6d-4770-b306-23f157d50c3d)
