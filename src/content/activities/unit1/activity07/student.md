#### **Algoritmos Basados en Ruido Perlin**

**📌 Descripción**

El **ruido Perlin** es un algoritmo de generación de ruido **suave y continuo**, desarrollado por **Ken Perlin** en 1983. A diferencia del **ruido aleatorio tradicional**, el ruido Perlin genera **transiciones suaves**, lo que lo hace ideal para la creación de **paisajes naturales, texturas y movimiento orgánico**.

✅ **Genera valores pseudoaleatorios interpolados**, evitando el aspecto caótico del ruido blanco.  
✅ **Se usa en dos o más dimensiones**, lo que permite su aplicación en gráficos y simulaciones.  
✅ **Puede combinarse en capas** para generar detalles más complejos.  


**🎨 Ejemplos de Aplicación**

🔹 **Generación de terrenos y mapas de altura en videojuegos.**  
🔹 **Simulación de humo, nubes y fuego en gráficos computacionales.**  
🔹 **Patrones fluidos en animaciones generativas.**  

🔗 **Ejemplo visual:** 
- [Generación de terreno 2D con Ruido Perlin en p5.js](https://p5js.org/es/examples/repetition-noise/)  
    - <img src="https://github.com/user-attachments/assets/23e7a14f-0e55-4119-839e-d4999dfccc76" width="400">

- [Generación de terreno 3D](https://editor.p5js.org/ld.duran.x/sketches/jWEF4Dyyt)
    - <img src="https://github.com/user-attachments/assets/02fd9aaa-8892-4d32-938f-9403ab546ce8" width="400">

🔗 **Ejemplo de código:** 

terreno 2D:

```javascript
function setup() {
  createCanvas(800, 400);
  noiseDetail(4, 0.5); // Define la escala y suavidad del ruido
}

function draw() {
  background(0);
  stroke(255);
  noFill();
  
  beginShape();
  for (let x = 0; x < width; x++) {
    let y = noise(x * 0.01) * height; // Genera la forma de un paisaje
    vertex(x, y);
  }
  endShape();
}
```

terreno 3D

```js
// Define variables globales
let noiseScale = 0.02;
let noiseVal = 0;
let increment = 0.1;

function setup() {
  createCanvas(800, 800, WEBGL);
}

function draw() {
  background(220);
  noStroke();

  // Crea una malla de cubos para visualizar el ruido de Perlin en 3D
  let rows = 30;
  let cols = 30;
  let cubeSize = 20;
  for (let x = 0; x < rows; x++) {
    for (let y = 0; y < cols; y++) {
      // Calcula el valor de ruido de Perlin para cada posición en la malla
      noiseVal = noise(x * noiseScale, y * noiseScale, increment);
      // Convierte el valor de ruido a un color gris
      let gray = floor(map(noiseVal, 0, 1, 0, 255));
      // Calcula la posición y la altura del cubo
      let xPos = x * cubeSize - (rows * cubeSize) / 2;
      let yPos = y * cubeSize - (cols * cubeSize) / 2;
      let zPos = noiseVal * 100;
      // Dibuja el cubo en la posición correspondiente
      push();
      translate(xPos, yPos, zPos);
      fill(gray);
      box(cubeSize);
      pop();
    }
  }

  // Incrementa el valor de "increment" para animar el ruido de Perlin
  increment += 0.005;
}

function mouseClicked() {
  // Modifica la escala del ruido de Perlin al hacer clic en el canvas
  noiseScale = random(0.01, 0.2);
}
```

**🚀 Potencial en Experiencias Interactivas**

🎮 **Mundos generativos en videojuegos:** Creación de terrenos dinámicos.  
🎨 **Texturas naturales en arte digital:** Simulación de mármol, agua y humo.  
🎭 **Música generativa basada en ruido:** Se puede usar para modulación de sonido.  

---

#### **Algoritmos Basados en Sistemas L (L-Systems)**

**📌 Descripción**

Los **Sistemas L** (Lindenmayer Systems) son **sistemas de reescritura** basados en reglas recursivas que generan **estructuras ramificadas**. Fueron desarrollados por el botánico **Aristid Lindenmayer** para modelar el **crecimiento de plantas**.

✅ **Funciona con reglas de producción**, donde un conjunto de símbolos se transforma iterativamente en otros.  
✅ **Genera estructuras fractales y patrones de crecimiento.**  
✅ **Ideal para representar formas naturales y biológicas.**  

**🎨 Ejemplos de Aplicación**

🔹 **Generación de árboles y vegetación en entornos virtuales.**  
🔹 **Modelado de crecimiento de células y tejidos.**  
🔹 **Creación de patrones gráficos fractales y arte abstracto.**  

🔗 **Ejemplo visual:** [Simulación de árboles con L-Systems](https://editor.p5js.org/codingtrain/sketches/QmTx-Y_UP)  

🔗 **Ejemplo de código:**  

<img src="https://github.com/user-attachments/assets/b6742407-1926-4251-b0b0-752927e154d3" width="400">


```javascript// Daniel Shiffman
// Code for: https://youtu.be/E1B4UoSQMFw

// variables: A B

// axiom: A
// rules: (A → AB), (B → A)
var angle;
var axiom = "F";
var sentence = axiom;
var len = 100;

var rules = [];
rules[0] = {
  a: "F",
  b: "FF+[+F-F-F]-[-F+F+F]"
}

function generate() {
  len *= 0.5;
  var nextSentence = "";
  for (var i = 0; i < sentence.length; i++) {
    var current = sentence.charAt(i);
    var found = false;
    for (var j = 0; j < rules.length; j++) {
      if (current == rules[j].a) {
        found = true;
        nextSentence += rules[j].b;
        break;
      }
    }
    if (!found) {
      nextSentence += current;
    }
  }
  sentence = nextSentence;
  createP(sentence);
  turtle();

}

function turtle() {
  background(51);
  resetMatrix();
  translate(width / 2, height);
  stroke(255, 100);
  for (var i = 0; i < sentence.length; i++) {
    var current = sentence.charAt(i);

    if (current == "F") {
      line(0, 0, 0, -len);
      translate(0, -len);
    } else if (current == "+") {
      rotate(angle);
    } else if (current == "-") {
      rotate(-angle)
    } else if (current == "[") {
      push();
    } else if (current == "]") {
      pop();
    }
  }
}

function setup() {
  createCanvas(400, 400);
  angle = radians(25);
  background(51);
  createP(axiom);
  turtle();
  var button = createButton("generate");
  button.mousePressed(generate);
}
```

**🚀 Potencial en Experiencias Interactivas**

🌿 **Simulación de entornos naturales en videojuegos.**  
🎭 **Arte fractal y visualización abstracta en espectáculos digitales.**  
🔬 **Modelado de crecimiento biológico y animación procedural.**  

---

#### **Algoritmos Basados en Autómatas Celulares**

**📌 Descripción**

Un **Autómata Celular** es un modelo computacional basado en una **rejilla de celdas**, donde cada celda evoluciona según reglas simples que dependen del estado de sus vecinas.  
El más conocido es el **Juego de la Vida de Conway**.

✅ **Cada celda tiene un estado (vivo o muerto).**  
✅ **Las reglas determinan si una celda sobrevive, muere o nace.**  
✅ **Pequeñas reglas pueden generar patrones extremadamente complejos.**  

**🎨 Ejemplos de Aplicación**

🔹 **Simulación de patrones biológicos y de crecimiento.**  
🔹 **Creación de efectos visuales dinámicos en arte generativo.**  
🔹 **Evolución de sistemas interactivos con retroalimentación.**  

🔗 **Ejemplo visual:** [Juego de la Vida en p5.js](https://p5js.org/es/examples/math-and-physics-game-of-life/)  

🔗 **Ejemplo de código:**  

<img src="https://github.com/user-attachments/assets/419e78d0-d479-4268-a9d5-c1a74f1cdbd3" width="400">


```javascript
let cellSize = 20;
let columnCount;
let rowCount;
let currentCells = [];
let nextCells = [];

function setup() {
  // Set simulation framerate to 10 to avoid flickering
  frameRate(10);
  createCanvas(720, 400);

  // Calculate columns and rows
  columnCount = floor(width / cellSize);
  rowCount = floor(height / cellSize);

  // Set each column in current cells to an empty array
  // This allows cells to be added to this array
  // The index of the cell will be its row number
  for (let column = 0; column < columnCount; column++) {
    currentCells[column] = [];
  }

  // Repeat the same process for the next cells
  for (let column = 0; column < columnCount; column++) {
    nextCells[column] = [];
  }

  noLoop();
  describe(
    "Grid of squares that switch between white and black, demonstrating a simulation of John Conway's Game of Life. When clicked, the simulation resets."
  );
}

function draw() {
  generate();
  for (let column = 0; column < columnCount; column++) {
    for (let row = 0; row < rowCount; row++) {
      // Get cell value (0 or 1)
      let cell = currentCells[column][row];

      // Convert cell value to get black (0) for alive or white (255 (white) for dead
      fill((1 - cell) * 255);
      stroke(0);
      rect(column * cellSize, row * cellSize, cellSize, cellSize);
    }
  }
}

// Reset board when mouse is pressed
function mousePressed() {
  randomizeBoard();
  loop();
}

// Fill board randomly
function randomizeBoard() {
  for (let column = 0; column < columnCount; column++) {
    for (let row = 0; row < rowCount; row++) {
      // Randomly select value of either 0 (dead) or 1 (alive)
      currentCells[column][row] = random([0, 1]);
    }
  }
}

// Create a new generation
function generate() {
  // Loop through every spot in our 2D array and count living neighbors
  for (let column = 0; column < columnCount; column++) {
    for (let row = 0; row < rowCount; row++) {
      // Column left of current cell
      // if column is at left edge, use modulus to wrap to right edge
      let left = (column - 1 + columnCount) % columnCount;

      // Column right of current cell
      // if column is at right edge, use modulus to wrap to left edge
      let right = (column + 1) % columnCount;

      // Row above current cell
      // if row is at top edge, use modulus to wrap to bottom edge
      let above = (row - 1 + rowCount) % rowCount;

      // Row below current cell
      // if row is at bottom edge, use modulus to wrap to top edge
      let below = (row + 1) % rowCount;

      // Count living neighbors surrounding current cell
      let neighbours =
        currentCells[left][above] +
        currentCells[column][above] +
        currentCells[right][above] +
        currentCells[left][row] +
        currentCells[right][row] +
        currentCells[left][below] +
        currentCells[column][below] +
        currentCells[right][below];

      // Rules of Life
      // 1. Any live cell with fewer than two live neighbours dies
      // 2. Any live cell with more than three live neighbours dies
      if (neighbours < 2 || neighbours > 3) {
        nextCells[column][row] = 0;
        // 4. Any dead cell with exactly three live neighbours will come to life.
      } else if (neighbours === 3) {
        nextCells[column][row] = 1;
        // 3. Any live cell with two or three live neighbours lives, unchanged, to the next generation.
      } else nextCells[column][row] = currentCells[column][row];
    }
  }

  // Swap the current and next arrays for next generation
  let temp = currentCells;
  currentCells = nextCells;
  nextCells = temp;
}
```

**🚀 Potencial en Experiencias Interactivas**

🕹️ **Generación de patrones vivos en juegos y simulaciones.**  
🎨 **Efectos de visualización interactivos en instalaciones artísticas.**  
🔬 **Simulación de ecosistemas y procesos biológicos.**  

