#### Deconstrucción y Reconstrucción

**Deconstrucción tomado de la pagina**

**De la sección P.1. Color "P_1_2_3_05" :**

- [Link al ejemplo original](http://www.generative-gestaltung.de/2/sketches/?01_P/P_1_2_3_05)

<img src="https://github.com/user-attachments/assets/25aa244e-1607-4c7f-b006-9786c2090ba3" width="350">

Este ejemplo presenta un patrón generativo basado en líneas. Se generan múltiples líneas con posiciones, longitudes, ángulos y colores aleatorios. El objetivo es crear una composición visual dinámica que explora la interacción del azar con la geometría simple, resultando en patrones únicos en cada ejecución.

**Reconstrucción en p5.js**

mi código en p5.js que recrea la idea del ejemplo seleccionado, utilizando mi propia interpretación basada en el proceso de Deconstruction/Reconstruction 

<img src="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExeXZsb3FmN2N0Znl2a3JzZ25vb2wyOXZvYWVyMmR1cTNiYXlibnN0dSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/o7ei3UzxSSC4MbIKWG/giphy.gif" width="350">

<details>
  <summary>Sketch.js (Click aquí)</summary>
  
  ```js
  function setup() {
  createCanvas(800, 600);
  background(255);
  noLoop(); // Dibujar una sola vez para obtener un patrón fijo
}

function draw() {
  // Generamos 100 líneas aleatorias
  for (let i = 0; i < 100; i++) {
    // Posición de inicio aleatoria
    let x = random(width);
    let y = random(height);
    
    // Ángulo aleatorio entre 0 y 2*PI
    let angle = random(TWO_PI);
    
    // Longitud aleatoria entre 20 y 150 píxeles
    let len = random(20, 150);
    
    // Color aleatorio para el trazo
    stroke(random(255), random(255), random(255));
    
    // Grosor del trazo aleatorio entre 1 y 5
    strokeWeight(random(1, 5));
    
    // Dibujar la línea
    line(x, y, x + cos(angle) * len, y + sin(angle) * len);
  }
}

// Al presionar el mouse se redibuja el patrón
function mousePressed() {
  redraw();
}

  ```
  
</details>

En mi reconstrucción, cada línea se dibuja en una posición aleatoria, con un ángulo, longitud y color aleatorios.

