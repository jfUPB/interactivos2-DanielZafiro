#### 1. "Beijing Lucky Numbers" by Banyapon Poolsawas

<img src="https://raw.githubusercontent.com/jfUPB/interactivos2-DanielZafiro/refs/heads/main/src/assets/pictures/Screen%20Shot%202025-02-04%20at%2010.40.31%20AM.png" width="350">

[Link de OpenProcessing](https://openprocessing.org/sketch/2456871)

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


<img src="../../../../assets/pictures/Screen Shot 2025-02-04 at 10.39.26 AM.png" width="350">


-  Modifiqué la velocidad en la que caen las columnas de números entre min 1 max 3.
-  Modifiqué la cantidad de de numeros que se generen por columna de min 5 y max 8.
-  Modifiqué el color del fondo de rojo a verde.
-  Modofiqué los caracteres que forman las columnas de chinos a 1 y 0.


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

---

#### "Generative Art" por Hamza Khan

<img src="../../../../assets/pictures/Screen Shot 2025-02-04 at 11.10.58 AM.png" width="350">

[Link de OpenProcessing](https://openprocessing.org/sketch/1917943)

**Descripción del código:** Genera una serie de arcos concéntricos y huecos que forman patrones circulares complejos. Utiliza funciones trigonométricas para calcular las posiciones de los puntos y dibujar los arcos, creando una estética hipnótica y detallada. El uso de colores en el modo HSB y la mezcla de colores mediante `blendMode(ADD)` contribuyen a la riqueza visual de la obra.

<details>
  <summary>Código fuente</summary>
  
  ```js
  let global_n = 0;

function setup() {
	createCanvas(800, 800);
	colorMode(HSB, 360, 100, 100, 100);
	angleMode(DEGREES);
}

function draw() {
	blendMode(BLEND);
	background(0, 0, 20);
	randomSeed(0);
	blendMode(ADD);
	// randomSeed(random(10000));
	// drawingContext.shadowColor = color(0, 0, 0, 15);
	// drawingContext.shadowBlur = width / 30;
	// drawingContext.shadowOffsetX = width / 30 / 2;
	// drawingContext.shadowOffsetY = width / 30 / 2;
	consecutiveHollowedOutArc(
		width / 2,
		height / 2,
		50,
		300,
		0,
		360
	);
	// noLoop();
}

function consecutiveHollowedOutArc(
	center_x,
	center_y,
	r_min,
	r_max,
	start_angle,
	end_angle
) {
	push();
	translate(center_x, center_y);
	let angle = start_angle;
	let angle_step;
	let mode = random() > 0.5;
	let r = r_max;
	let r_step = 10;
	while (r > r_min) {
		if (mode == false) {
			r_step = int(random(3, 10)) * 3;
		} else {
			r_step = int(random(3, 10)) * 10;
		}
		angle = start_angle;
		while (angle < end_angle) {
			if (mode == true) {
				angle_step = int(random(random()) * 4 + 1) * 5;
			} else {
				angle_step = int(random(1, 5)) * 15;
			}
			if (random() > 0.95) mode = !mode;
			if (angle + angle_step > end_angle) angle_step = end_angle - angle;
			// arc(0, 0, r_max, r_max, angle, angle + angle_step,PIE);
			hollowedOutArc(
				0,
				0,
				r,
				max(r / 4, r - r_step),
				angle,
				angle + angle_step,
				true,
				1
			);
			angle += angle_step;
		}
		r -= r_step;
	}

	pop();
}

function hollowedOutArc(
	x,
	y,
	maxD,
	minD,
	startAngle,
	endAngle,
	bool,
	angleStep = 1
) {
	let dir = random() > 0.5 ? -1 : 1;
	push();
	translate(x, y);
	global_n++ % 2 == 0 ? stroke(0, 0, 50) : stroke(0, 0, 100);
	noFill();
	if (bool) {
		if (global_n++ % 2 == 0) {
			let angle = min(startAngle, endAngle);
			let angle_plus =
				(max(endAngle, startAngle) - min(endAngle, startAngle)) /
				int(random([1, 3, 5, 7, 9, 11, 13]));
			while (angle < endAngle) {
				hollowedOutArc(0, 0, maxD, minD, angle, angle + angle_plus, false, 1);
				angle += angle_plus;
			}
		} else {
			let d = minD;
			let d_plus = (maxD - minD) / int(random([1, 3, 5, 7, 9, 11, 13]));
			while (d < maxD) {
				hollowedOutArc(0, 0, d, d + d_plus, startAngle, endAngle, false, 1);
				d += d_plus;
			}
		}
	} else {
		let t =
			(1 + ((maxD + startAngle / 360 + (dir * frameCount) / 100) % 1)) % 1;
		t = easeInOutCirc(t);
		let sw = max(maxD, minD) - min(maxD, minD);
		strokeCap(SQUARE);
		strokeWeight((1 - t) * sw);
		let d = minD + sw / 2;
		let ld = (2 * d * PI * (endAngle - startAngle)) / 360;
		drawingContext.setLineDash([ld]);
		drawingContext.lineDashOffset = ld * t * 2;
		beginShape();
		for (let a = startAngle; a <= endAngle; a += angleStep) {
			vertex(cos(a) * d, sin(a) * d);
		}
		endShape();
	}
	pop();
}

function easeInOutCirc(x) {
	return x < 0.5 ?
		(1 - Math.sqrt(1 - Math.pow(2 * x, 2))) / 2 :
		(Math.sqrt(1 - Math.pow(-2 * x + 2, 2)) + 1) / 2;
}

function easeInOutElastic(x) {
	const c5 = (2 * Math.PI) / 4.5;

	return x === 0 ?
		0 :
		x === 1 ?
		1 :
		x < 0.5 ?
		-(Math.pow(2, 20 * x - 10) * Math.sin((20 * x - 11.125) * c5)) / 2 :
		(Math.pow(2, -20 * x + 10) * Math.sin((20 * x - 11.125) * c5)) / 2 + 1;
}
  ```

</details>


**Modificación del código:** 


<img src="../../../../assets/pictures/Screen Shot 2025-02-04 at 1.03.11 PM.png" width="350">


-  Modifiqué el uso de BlendMode(ADD) para que se pudieran colorear las formas generadas usando RGB en vez de HSL y definiendo los colores. 
-  Modifiqué el codigo de manera que detectara el mouse en el eje x para que girara segun la posicion del cursor.
-  Modifiqué el modo de color ajustandola a una paleta de colores personalizada con un arreglo de colores vibrantes.
-  Modifiqué la velocidad de rotación basandose en la posicion del cursor
  
<details>
  <summary>Código modificado</summary>
  
  ```js
  let global_n = 0;
let rotationSpeed = 0; // Variable para controlar la velocidad de rotación
let colors = []; // Arreglo para almacenar la paleta de colores

function setup() {
  createCanvas(800, 800);
  // Definir una paleta de colores vibrantes
  colors = [
    color(255, 0, 0),    // Rojo
    color(0, 255, 0),    // Verde
    color(0, 0, 255),    // Azul
    color(255, 255, 0),  // Amarillo
    color(0, 255, 255),  // Cian
    color(255, 0, 255),  // Magenta
    color(255, 165, 0),  // Naranja
    color(75, 0, 130)    // Índigo
  ];
  angleMode(DEGREES);
}

function draw() {
  background(20);
  randomSeed(0);

  // Calcular la velocidad de rotación basada en la posición del mouse
  rotationSpeed = map(mouseX, 0, width, -0.5, 0.5);

  // Aplicar rotación al lienzo
  translate(width / 2, height / 2);
  rotate(frameCount * rotationSpeed);
  translate(-width / 2, -height / 2);

  consecutiveHollowedOutArc(width / 2, height / 2, 50, 300, 0, 360);
}

function consecutiveHollowedOutArc(center_x, center_y, r_min, r_max, start_angle, end_angle) {
  push();
  translate(center_x, center_y);
  let angle = start_angle;
  let angle_step;
  let mode = random() > 0.5;
  let r = r_max;
  let r_step = 10;
  while (r > r_min) {
    if (mode == false) {
      r_step = int(random(3, 10)) * 3;
    } else {
      r_step = int(random(3, 10)) * 10;
    }
    angle = start_angle;
    while (angle < end_angle) {
      if (mode == true) {
        angle_step = int(random(random()) * 4 + 1) * 5;
      } else {
        angle_step = int(random(1, 5)) * 15;
      }
      if (random() > 0.95) mode = !mode;
      if (angle + angle_step > end_angle) angle_step = end_angle - angle;
      hollowedOutArc(0, 0, r, max(r / 4, r - r_step), angle, angle + angle_step, true, 1);
      angle += angle_step;
    }
    r -= r_step;
  }
  pop();
}

function hollowedOutArc(x, y, maxD, minD, startAngle, endAngle, bool, angleStep = 1) {
  let dir = random() > 0.5 ? -1 : 1;
  push();
  translate(x, y);
  // Asignar un color aleatorio de la paleta
  stroke(random(colors));
  noFill();
  if (bool) {
    if (global_n++ % 2 == 0) {
      let angle = min(startAngle, endAngle);
      let angle_plus = (max(endAngle, startAngle) - min(endAngle, startAngle)) / int(random([1, 3, 5, 7, 9, 11, 13]));
      while (angle < endAngle) {
        hollowedOutArc(0, 0, maxD, minD, angle, angle + angle_plus, false, 1);
        angle += angle_plus;
      }
    } else {
      let d = minD;
      let d_plus = (maxD - minD) / int(random([1, 3, 5, 7, 9, 11, 13]));
      while (d < maxD) {
        hollowedOutArc(0, 0, d, d + d_plus, startAngle, endAngle, false, 1);
        d += d_plus;
      }
    }
  } else {
    let t = (1 + ((maxD + startAngle / 360 + (dir * frameCount) / 100) % 1)) % 1;
    t = easeInOutCirc(t);
    let sw = max(maxD, minD) - min(maxD, minD);
    strokeCap(SQUARE);
    strokeWeight((1 - t) * sw);
    let d = minD + sw / 2;
    let ld = (2 * d * PI * (endAngle - startAngle)) / 360;
    drawingContext.setLineDash([ld]);
    drawingContext.lineDashOffset = ld * t * 2;
    beginShape();
    for (let a = startAngle; a <= endAngle; a += angleStep) {
      vertex(cos(a) * d, sin(a) * d);
    }
    endShape();
  }
  pop();
}

function easeInOutCirc(x) {
  return x < 0.5 ? (1 - Math.sqrt(1 - Math.pow(2 * x, 2))) / 2 : (Math.sqrt(1 - Math.pow(-2 * x + 2, 2)) + 1) / 2;
}

  ```

</details>

#### Reflexión:

Antes de explorar estos ejemplos de arte generativo, solía pensar que animaciones visualmente atractivas como estas eran creadas exclusivamente en programas como After Effects u otras herramientas de edición digital. Sin embargo, descubrir que este tipo de arte puede generarse a través de código parametrizado me ha abierto una nueva perspectiva sobre la creatividad y la programación.

En el caso de Beijing Lucky Numbers, aprendí cómo la simple repetición de elementos numéricos, combinada con reglas algorítmicas de disposición y color, puede producir una estética poderosa y culturalmente significativa(Matrix). Me impresionó cómo el uso de parámetros controlados y aleatoriedad permite obtener composiciones que evocan tradiciones visuales, pero con una ejecución completamente generativa. 

Por otro lado, Generative Art de Hamza Khan me mostró el increíble potencial de los algoritmos para crear piezas abstractas y envolventes. La manera en que los arcos concéntricos se generan en base a cálculos trigonométricos, convierte al código en una herramienta de expresión artística con posibilidades casi ilimitadas.


