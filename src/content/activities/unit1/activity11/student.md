#### "Poema.p5js"

Este código genera un "poema" de varias líneas, donde en cada línea se generan de 3 a 7 palabras seleccionadas aleatoriamente:

<details>

  <summary>codigo (Click Aquí)</summary>

  ```js
  // Array de palabras para generar el texto
  let words = [
    "amor", "vida", "sueño", "cielo", "sol", "luz", "sombra", 
    "tiempo", "eco", "paz", "alma", "fuego", "viento", "esperanza", "silencio"
  ];
  
  function setup() {
    createCanvas(800, 600);
    background(220);
    textAlign(CENTER, CENTER);
    // Opcional: cambiar el modo de dibujo para una apariencia diferente
    noLoop(); // Se dibuja una sola vez hasta que se presione una tecla o se reinicie
  }
  
  function draw() {
    background(220);
    let numLines = 5;              // Número de líneas (o "versos")
    let lineHeight = height / numLines;
  
    // Generar cada línea del "poema"
    for (let i = 0; i < numLines; i++) {
      let sentence = "";
      let numWords = floor(random(3, 8)); // Cada línea tendrá entre 3 y 7 palabras
      
      // Generar la frase combinando palabras aleatorias
      for (let j = 0; j < numWords; j++) {
        sentence += random(words) + " ";
      }
      
      // Configurar un tamaño de fuente aleatorio para esta línea
      let fSize = random(16, 48);
      textSize(fSize);
      
      // Establecer un color de texto aleatorio
      fill(random(255), random(255), random(255));
      
      // Opcional: cambiar estilo (puedes experimentar con textStyle(BOLD), textStyle(ITALIC), etc.)
      // Por ejemplo: textStyle(random([BOLD, ITALIC, NORMAL]));
      textStyle(random([BOLD, ITALIC, NORMAL]));
      
      // Dibujar la línea en el centro horizontal y en la posición correspondiente verticalmente
      text(sentence, width / 2, lineHeight / 2 + i * lineHeight);
    }
  }
  
  // Al presionar una tecla se genera un nuevo "poema"
  function keyPressed() {
    redraw();
  }

  ```
  
</details>

**Contenido del codigo**

- **Array de Palabras:**
Se define un array `words` con términos que pueden ser combinados para generar frases.

- **Setup:**
Se crea un canvas de 800×600 píxeles y se establece la alineación central para el texto. Se utiliza `noLoop()` para que el dibujo se realice una sola vez y se pueda redibujar manualmente con una tecla (en este caso, cualquier tecla).

- **Draw:**
Se divide el canvas en 5 líneas (o "versos"). En cada línea se genera una frase compuesta de entre 3 y 7 palabras elegidas aleatoriamente.
Para cada línea, se configura un tamaño de fuente aleatorio (entre 16 y 48 píxeles), un color de relleno aleatorio y se aplica un estilo de texto aleatorio (BOLD, ITALIC o NORMAL). Finalmente, la frase se dibuja en el centro horizontal del canvas.

- **Interactividad:**
Al presionar cualquier tecla se llama a `redraw()`, generando un nuevo conjunto de frases con diferentes combinaciones y estilos.

<img src="https://github.com/user-attachments/assets/f308cc84-2c11-42a2-965f-7160a1c5138a" width="350">



