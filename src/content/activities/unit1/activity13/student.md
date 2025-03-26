#### Sonido generativo báscio con p5.sound

Este programa en p5.js utiliza la biblioteca p5.sound para generar un oscilador cuya forma de onda puede cambiar en tiempo real. 

La frecuencia y la amplitud se controlan mediante la posición del mouse, mientras que la tecla "1" activa una onda senoidal, la tecla "2" una onda triangular y la tecla "3" una onda cuadrada. 

Además, se utiliza p5.FFT para analizar la señal de audio y dibujar la forma de onda en la parte inferior del canvas, permitiendo visualizar cómo varía la señal a medida que se modifican los parámetros.


<img src="https://github.com/user-attachments/assets/b57c2df6-a84b-49ff-b26e-daefd84bd58b" width="350">
<img src="https://github.com/user-attachments/assets/d0307d1f-e48c-4bd3-871a-782b9ceaad33" width="350">
<img src="https://github.com/user-attachments/assets/7c2af75d-4c58-41c8-828a-4cdc0b29d1e4" width="350">


<details>

<summary>index.html incluimos p5.sound (Click aquí)</summary>

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Sonido Generativo Básico</title>
    <!-- Incluir p5.js -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.6.0/p5.min.js"></script>
    <!-- Incluir p5.sound -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.6.0/addons/p5.sound.min.js"></script>
  </head>
  <body>
    <!-- Tu sketch se carga desde un archivo -->
    <script src="sketch.js"></script>
  </body>
</html>
```

</details>

<details>

<summary>Sketch.js (Click aquí)</summary>

```js
/*
  Este sketch utiliza la biblioteca p5.sound para generar un oscilador cuyo
  tipo de onda se puede cambiar con las teclas 1 (senoidal), 2 (triangular) y 3 (cuadrada).
  La frecuencia y la amplitud del sonido se controlan con la posición del mouse.
  Además, se utiliza p5.FFT para analizar la señal de audio y se dibuja la forma
  de onda en la parte inferior del canvas en tiempo real.
*/

let osc; // Oscilador que genera el sonido
let fft; // Objeto para analizar la señal de audio (Transformada Rápida de Fourier)

function setup() {
  createCanvas(800, 600);   // Crear un canvas de 800x600 píxeles
  background(220);          // Establecer un fondo gris claro
  // Crear un oscilador inicial de forma senoidal
  osc = new p5.Oscillator('sine');
  osc.freq(220);            // Frecuencia inicial en 220 Hz
  osc.amp(0.5);             // Amplitud inicial en 0.5
  osc.start();              // Iniciar el oscilador
  
  // Inicializar el objeto FFT para analizar la señal de audio
  fft = new p5.FFT();
}

function draw() {
  background(220); // Actualizar el fondo en cada frame
  
  // Mapear la posición del mouse para controlar la frecuencia:
  // Cuando el mouse se mueve de izquierda a derecha, la frecuencia varía de 100 Hz a 1000 Hz.
  let freq = map(mouseX, 0, width, 100, 1000);
  // Mapear la posición del mouse para controlar la amplitud:
  // Cuando el mouse se mueve de arriba hacia abajo, la amplitud varía de 1 (arriba) a 0 (abajo).
  let amp = map(mouseY, 0, height, 1, 0);
  // Actualizar la frecuencia y la amplitud del oscilador con una transición de 0.1 segundos.
  osc.freq(freq);
  osc.amp(amp, 0.1);
  
  // Mostrar información en pantalla:
  fill(0);                // Color de texto negro
  textSize(16);           // Tamaño del texto de 16 píxeles
  textAlign(CENTER, TOP); // Alinear el texto al centro horizontalmente y arriba verticalmente
  // Mostrar el tipo de oscilador actual y las instrucciones para cambiarlo
  text("Oscillator: " + osc.getType() + " (Presiona 1: sine, 2: triangle, 3: square)", width / 2, 10);
  // Mostrar la frecuencia y la amplitud actuales
  text("Frecuencia: " + nf(freq, 0, 2) + " Hz, Amplitud: " + nf(amp, 0, 2), width / 2, 30);
  
  // Obtener la forma de onda actual de la señal de audio
  let waveform = fft.waveform();
  
  // Dibujar la forma de onda en la parte inferior del canvas:
  noFill();             // No se rellena la forma; solo se dibuja el contorno
  stroke(255, 0, 0);    // Color de la línea: rojo
  strokeWeight(2);      // Grosor de la línea: 2 píxeles
  
  let waveHeight = 200; // Altura del área donde se dibuja la onda
  push();               // Guardar el estado actual del canvas
  translate(0, height - waveHeight); // Mover la referencia al área inferior
  beginShape();         // Comenzar a dibujar la forma de la onda
  // Recorrer cada valor del arreglo de la forma de onda
  for (let i = 0; i < waveform.length; i++) {
    // Mapear el índice a una posición horizontal
    let x = map(i, 0, waveform.length, 0, width);
    // Mapear el valor del waveform (entre -1 y 1) a una posición vertical en el área asignada
    let y = map(waveform[i], -1, 1, waveHeight, 0);
    vertex(x, y);       // Agregar un vértice a la forma
  }
  endShape();           // Finalizar la forma
  pop();                // Restaurar el estado previo del canvas
}

function keyPressed() {
  // Cambiar el tipo de onda del oscilador según la tecla presionada:
  if (key === '1') {
    osc.setType('sine');     // Onda senoidal
  } else if (key === '2') {
    osc.setType('triangle'); // Onda triangular
  } else if (key === '3') {
    osc.setType('square');   // Onda cuadrada
  }
}

```

</details>
