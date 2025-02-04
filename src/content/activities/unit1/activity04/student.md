#### Beijing Lucky Numbers by Banyapon Poolsawas

[Link de OpenProcessing](https://openprocessing.org/sketch/2456871)

![captura1]()

**Descripción del código:** "Beijing Lucky Numbers" es un proyecto de arte generativo inspirado en los números chinos y su simbolismo cultural. El sketch presenta columnas de números chinos (零 to 一) (del 0 al 1) que caen en cascada, creando un flujo rítmico sobre un fondo rojo vibrante que simboliza prosperidad y buena fortuna. La tipografía alterna entre colores blanco y dorado, evocando la estética del arte tradicional chino fusionado con el diseño digital moderno, **Se puede observar un Programación orientada a objetos en la clase NumberColumn**

<details>
  <summary> Código fuente </summary>
  
  ```js
  let columns = [];
let numColumns = 20;
let minFontSize = 15;
let maxFontSize = 90;

function setup() {
  createCanvas(windowWidth, windowHeight);
  textFont('Courier New');
  for (let i = 0; i < numColumns; i++) {
    let fontSize = random(minFontSize, maxFontSize);
    columns.push(new NumberColumn(i * (width / numColumns), fontSize, random() < 0.5 ? '#fff' : '#ffd700'));
  }
}

function draw() {
  background('#3A533A'); // Red background
  for (let col of columns) {
    col.update();
    col.display();
  }
}

class NumberColumn {
  constructor(x, fontSize, color) {
    this.x = x;
    this.y = random(-200, 0);
    this.speed = random(2, 5);
    this.fontSize = fontSize;
    this.color = color;
    this.numbers = [];
    this.maxLength = floor(random(5, 15));
    for (let i = 0; i < this.maxLength; i++) {
      this.numbers.push(this.randomChineseNumber());
    }
  }

  update() {
    this.y += this.speed;
    if (this.y > height) {
      this.y = random(-200, 0);
      this.numbers = [];
      this.maxLength = floor(random(5, 15));
      for (let i = 0; i < this.maxLength; i++) {
        this.numbers.push(this.randomChineseNumber());
      }
    }
  }

  display() {
    fill(this.color);
    for (let i = 0; i < this.numbers.length; i++) {
      textSize(this.fontSize);
      text(this.numbers[i], this.x, this.y + i * this.fontSize);
    }
  }

  randomChineseNumber() {
    const chineseNumbers = ['0', '1']; // Chinese numbers for 0 and 1
    return chineseNumbers[floor(random(0, chineseNumbers.length))];
  }
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}

  ```
</details>

**Modificación del código:** 
-  Modifiqué la velocidad en la que caen las columnas de números entre min 1 max 3.
-  Modifiqué la cantidad de de numeros que se generen por columna de min 5 y max 8.
-  Modifiqué el color del fondo de rojo a verde.
-  Modofiqué los caracteres que forman las columnas de chinos a 1 y 0.

![captura2]()

<details>
  <summary> Código fuente modificado</summary>
  
  ```js
  let columns = [];
let numColumns = 20;
let minFontSize = 15;
let maxFontSize = 90;

function setup() {
  createCanvas(windowWidth, windowHeight);
  textFont('Courier New');
  for (let i = 0; i < numColumns; i++) {
    let fontSize = random(minFontSize, maxFontSize);
    columns.push(new NumberColumn(i * (width / numColumns), fontSize, random() < 0.5 ? '#fff' : '#ffd700'));
  }
}

function draw() {
  background('#113C0C'); // Red background // Modificación de color de fondo
  for (let col of columns) {
    col.update();
    col.display();
  }
}

class NumberColumn {
  constructor(x, fontSize, color) {
    this.x = x;
    this.y = random(-200, 0);
    this.speed = random(1, 3); // Modificación de velocidad
    this.fontSize = fontSize;
    this.color = color;
    this.numbers = [];
    this.maxLength = floor(random(5, 8));
    for (let i = 0; i < this.maxLength; i++) {
      this.numbers.push(this.randomChineseNumber());
    }
  }

  update() {
    this.y += this.speed;
    if (this.y > height) {
      this.y = random(-200, 0);
      this.numbers = [];
      this.maxLength = floor(random(5, 8)); // Modificación de cantidad de numeros
      for (let i = 0; i < this.maxLength; i++) {
        this.numbers.push(this.randomChineseNumber());
      }
    }
  }

  display() {
    fill(this.color);
    for (let i = 0; i < this.numbers.length; i++) {
      textSize(this.fontSize);
      text(this.numbers[i], this.x, this.y + i * this.fontSize);
    }
  }

  randomChineseNumber() {
    const chineseNumbers = ['0', '1']; // Chinese numbers for 0 and 1 // Modificación de caracteres
    return chineseNumbers[floor(random(0, chineseNumbers.length))];
  }
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}

  ```
</details>

