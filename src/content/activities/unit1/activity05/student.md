#### Ejemplo 1 "P_2_3_3_01":

<img src="https://github.com/user-attachments/assets/57a205bc-5e51-4929-b880-32b21fe41224" width="500">

[Link de Generative Design](http://www.generative-gestaltung.de/2/sketches/?01_P/P_2_3_3_01)

**Descripción:** Este sketch permite dibujar con texto, generando una secuencia de letras que siguen el movimiento del mouse. El tamaño de las letras varía según la distancia recorrida, y la rotación se ajusta en función del ángulo de movimiento.

<details>
  <summary>Código fuente</summary>
  
  ```js
  // P_2_3_3_01
//
// Generative Gestaltung – Creative Coding im Web
// ISBN: 978-3-87439-902-9, First Edition, Hermann Schmidt, Mainz, 2018
// Benedikt Groß, Hartmut Bohnacker, Julia Laub, Claudius Lazzeroni
// with contributions by Joey Lee and Niels Poldervaart
// Copyright 2018
//
// http://www.generative-gestaltung.de
//
// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.

/**
 * draw tool. shows how to draw with dynamic elements.
 *
 * MOUSE
 * drag                : draw with text
 *
 * KEYS
 * del, backspace      : clear screen
 * arrow up            : angle distortion +
 * arrow down          : angle distortion -
 * s                   : save png
 */
'use strict';

var x = 0;
var y = 0;
var stepSize = -5.0;

var font = 'Georgia';
var letters = 'All the world\'s a stage, and all the men and women merely players. They have their exits and their entrances.';
var fontSizeMin = 3;
var angleDistortion = 0.0;

var counter = 0;

function setup() {
  // use full screen size
  createCanvas(displayWidth, displayHeight);
  background(255);
  cursor(CROSS);

  x = mouseX;
  y = mouseY;

  textFont(font);
  textAlign(LEFT);
  fill(0);
}

function draw() {
  if (mouseIsPressed && mouseButton == LEFT) {
    var d = dist(x, y, mouseX, mouseY);
    textSize(fontSizeMin + d / 2);
    var newLetter = letters.charAt(counter);
    stepSize = textWidth(newLetter);

    if (d > stepSize) {
      var angle = atan2(mouseY - y, mouseX - x);

      push();
      translate(x, y);
      rotate(angle + random(angleDistortion));
      text(newLetter, 0, 0);
      pop();

      counter++;
      if (counter >= letters.length) counter = 0;

      x = x + cos(angle) * stepSize;
      y = y + sin(angle) * stepSize;
    }
  }
}

function mousePressed() {
  x = mouseX;
  y = mouseY;
}

function keyReleased() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');
  if (keyCode == DELETE || keyCode == BACKSPACE) background(255);
}

function keyPressed() {
  // angleDistortion ctrls arrowkeys up/down
  if (keyCode == UP_ARROW) angleDistortion += 0.1;
  if (keyCode == DOWN_ARROW) angleDistortion -= 0.1;
}

  ```
  
</details>


**Parámetros:**

- **`stepSize`:** Define la distancia entre caracteres según el movimiento del mouse.
- **`fontSizeMin`:** Establece el tamaño mínimo de la fuente.
- **`angleDistortion`:** Controla la distorsión del ángulo al dibujar los caracteres.
- **`letters`:** Contiene la secuencia de texto utilizada en el dibujo.


<img src="https://github.com/user-attachments/assets/9e5186c6-dd1c-482e-92ae-25e2605858d4" width="500">


**Variaciones:**
1. Variante:
   Modifiqué la función `Draw()` especificamente la linea `textSize(fontSizeMin + d / 2);` por `textSize(fontSizeMin + d / 8);` haciendo que el tamaño del texto aumente más lentamente en función de la distancia recorrida por el mouse. Esto genera un efecto visual donde las letras aparecen con un retraso o delay, ya que el crecimiento del texto es menos agresivo. Esto crea un efecto de caligrafia con pincel dinámico con trazos más suavizados.

   <details>
     <summary>Código</summary>
     
     ```js
    // P_2_3_3_01
    //
    // Generative Gestaltung – Creative Coding im Web
    // ISBN: 978-3-87439-902-9, First Edition, Hermann Schmidt, Mainz, 2018
    // Benedikt Groß, Hartmut Bohnacker, Julia Laub, Claudius Lazzeroni
    // with contributions by Joey Lee and Niels Poldervaart
    // Copyright 2018
    //
    // http://www.generative-gestaltung.de
    //
    // Licensed under the Apache License, Version 2.0 (the "License");
    // you may not use this file except in compliance with the License.
    // You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0
    // Unless required by applicable law or agreed to in writing, software
    // distributed under the License is distributed on an "AS IS" BASIS,
    // WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    // See the License for the specific language governing permissions and
    // limitations under the License.
    
    /**
     * draw tool. shows how to draw with dynamic elements.
     *
     * MOUSE
     * drag                : draw with text
     *
     * KEYS
     * del, backspace      : clear screen
     * arrow up            : angle distortion +
     * arrow down          : angle distortion -
     * s                   : save png
     */
    'use strict';
    
    var x = 0;
    var y = 0;
    var stepSize = -5.0;
    
    var font = 'Georgia';
    var letters = 'All the world\'s a stage, and all the men and women merely players. They have their exits and their entrances.';
    var fontSizeMin = 3;
    var angleDistortion = 0.0;
    
    var counter = 0;
    
    function setup() {
      // use full screen size
      createCanvas(displayWidth, displayHeight);
      background(255);
      cursor(CROSS);
    
      x = mouseX;
      y = mouseY;
    
      textFont(font);
      textAlign(LEFT);
      fill(0);
    }
    
    function draw() {
      if (mouseIsPressed && mouseButton == LEFT) {
        var d = dist(x, y, mouseX, mouseY);
        textSize(fontSizeMin + d / 8); // Modificación realizada.
        var newLetter = letters.charAt(counter);
        stepSize = textWidth(newLetter);
    
        if (d > stepSize) {
          var angle = atan2(mouseY - y, mouseX - x);
    
          push();
          translate(x, y);
          rotate(angle + random(angleDistortion));
          text(newLetter, 0, 0);
          pop();
    
          counter++;
          if (counter >= letters.length) counter = 0;
    
          x = x + cos(angle) * stepSize;
          y = y + sin(angle) * stepSize;
        }
      }
    }
    
    function mousePressed() {
      x = mouseX;
      y = mouseY;
    }
    
    function keyReleased() {
      if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');
      if (keyCode == DELETE || keyCode == BACKSPACE) background(255);
    }
    
    function keyPressed() {
      // angleDistortion ctrls arrowkeys up/down
      if (keyCode == UP_ARROW) angleDistortion += 0.1;
      if (keyCode == DOWN_ARROW) angleDistortion -= 0.1;
    }

     ```
     
   </details>
   
     <img src="https://github.com/user-attachments/assets/4dac3e2e-0120-41b1-bdf7-e2b755b07dba" width="500">


   
2. Variante:
   Modifiqué el codigo para que cada vez que se dibuje una letra cambie de color aleatoriamente, sin embargo me parece que seria mejor si cambia de color cada vez que hay una palabra nueva es decir cada vez que se presenta un espacio. Para cambiar el color cada vez para una sola letra que se dibuje simplemente se agrega esta linea de codigo en la funcion `Draw()` dentro del bloque de codigo antes de generar el texto así:
   ```js
   function Draw(){
   
   // ... resto del codigo
   
    push();
    translate(x, y);
    rotate(angle + random(angleDistortion));
     // Asigna un color aleatorio a cada letra
    fill(random(255), random(255), random(255));  
    text(newLetter, 0, 0);
    pop();  
  
   // ... resto del codigo
   
   }
   ```
 
    <img src="https://github.com/user-attachments/assets/305a4c51-7e62-420b-8991-23ff27e35bc2" width="500">
  

   Sin embargo como me pareció que seria mas chevere si cambiara de color cada palabra y no cada letra entonces la idea la variante es que cambie de color cada vez que se presente un espacio. Primero hay que agregar un una nueva variable global que almacene el color actual de la palabra (`var currentColor;`), luego en la funcion `setup()` debemos asignar un color inicial `currentColor = color(random(255),random(255),random(255));`, antes de dibujar el texto colorear la palabra con el mismo color actual `fill(currentColor);` y dentro de la condición `if (d > stepsize){}` que esta dentro de la funcion `draw()`agregar la condicion que si la letra es un espacio entonces asignar un color aleatorio con `if (newLetter === ' ') {currentColor = color(random(255), random(255), random(255));}`.

   <details>
     <summary>Código</summary>
     
     ```js
     'use strict';
    
    var x = 0;
    var y = 0;
    var stepSize = 5.0;
    
    var font = 'Georgia';
    var letters = 'All the world\'s a stage, and all the men and women merely players.';
    var fontSizeMin = 3;
    var angleDistortion = 0.0;
    var counter = 0;
    
    // Variable para almacenar el color actual de la palabra
    var currentColor;
    
    function setup() {
      createCanvas(windowWidth, windowHeight);
      background(255);
      cursor(CROSS);
    
      textFont(font);
      textAlign(LEFT);
      fill(0);
    
      // Asignar un color inicial
      currentColor = color(random(255), random(255), random(255));
    }
    
    function draw() {
      if (mouseIsPressed && mouseButton == LEFT) {
        var d = dist(x, y, mouseX, mouseY);
        textSize(fontSizeMin + d / 16); // Mantiene el efecto de delay en el pincel
    
        var newLetter = letters.charAt(counter);
        stepSize = textWidth(newLetter);
    
        if (d > stepSize) {
          var angle = atan2(mouseY - y, mouseX - x);
    
          push();
          translate(x, y);
          rotate(angle + random(angleDistortion));
    
          // Asignar el mismo color a toda la palabra actual
          fill(currentColor);
          text(newLetter, 0, 0);
          
          pop();
    
          // Si la letra es un espacio, asignar un nuevo color aleatorio
          if (newLetter === ' ') {
            currentColor = color(random(255), random(255), random(255));
          }
    
          // Actualizar la posición para continuar el trazo en la dirección correcta
          x = x + cos(angle) * stepSize;
          y = y + sin(angle) * stepSize;
    
          counter++;
          if (counter >= letters.length) counter = 0;
        }
      }
    }
    
    function mousePressed() {
      // Asegurar que el trazo empieza en el punto donde el usuario hace clic
      x = mouseX;
      y = mouseY;
    }

     ```
     
   </details>


   <img src="https://github.com/user-attachments/assets/9715b57a-57b3-4e93-9a98-827b8558d715" width="500">

**Aplicación potencial en conexto de entretenimiento digital:**

**Decodificación de frases a partir de palabras**
Los usuarios dibujan en la pantalla y aparecen palabras en negro hasta que encuentran palabras clave en color, que forman parte de una frase oculta. Luego, se van organizando automaicamente esas palabras en una cuadrícula para reconstruir el mensaje.

💡 **Aplicaciones Potenciales**

1️⃣ Educación y Aprendizaje de Idiomas 📝
- Ayuda a estudiantes a descubrir frases en otro idioma de forma interactiva.
- Refuerza la gramática y el vocabulario mediante el juego.
- Ejemplo:
    - 📝 Un estudiante de inglés dibuja palabras en la pantalla y encuentra “the”, “cat”, “is”, “on”, “the”, “table”. Ahora debe arrastrarlas y ordenarlas para formar una frase coherente.

2️⃣ Escape Rooms y Juegos de Misterio 🔍
- Los jugadores encuentran pistas ocultas en un texto y deben organizarlas para resolver acertijos.
- Se pueden integrar palabras engañosas para aumentar la dificultad.

3️⃣ Aprendizaje Histórico o Narrativo 📜
- Se revelan frases importantes de textos históricos o filosóficos de forma interactiva.
- Permite reconstruir discursos o documentos clave.

4️⃣ Aventura y Ciencia Ficción 🚀
- Se usa en juegos donde los jugadores descifran mensajes secretos o códigos alienígenas.
- Agrega una capa de exploración y lógica en historias interactivas.

#### Ejemplo 2 "M_6_1_03":

<img src="../../../../assets/pictures/giphy1.gif" width="350">

[Link de OpenProcessing](https://openprocessing.org/sketch/2456871)

**Descripción:** Este sketch simula una red de nodos conectados por resortes, donde los nodos se repelen entre sí y los resortes intentan mantener una longitud específica. 

<details>
  <summary>Código fuente</summary>
  
  ```js
  
  ```
  
</details>

**Parámetros:** Los parámetros principales incluyen:

- `nodeCount`: Número de nodos en la simulación.
- `nodeDiameter`: Diámetro visual de los nodos.
- `spring.length`: Longitud natural de los resortes.
- `spring.stiffness`: Rigidez de los resortes.

**Variaciones:**
1. Variante:

   <details>
     <summary>Código</summary>
     
     ```js
     
     ```
     
   </details>
   
     <img src="../../../../assets/pictures/giphy2.gif" width="350">

   
2. Variante:

   <details>
     <summary>Codigo html</summary>

     ```html
     <!DOCTYPE html>
      <html>
        <head>
          <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.2/p5.min.js"></script>
          <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.2/addons/p5.dom.min.js"></script>
          <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.2/addons/p5.sound.min.js"></script>
      
          <!-- Generative Design Dependencies -->
          <script src="https://cdn.jsdelivr.net/gh/generative-design/Code-Package-p5.js@master/libraries/gg-dep-bundle/gg-dep-bundle.js"></script>
          <script src="https://cdnjs.cloudflare.com/ajax/libs/opentype.js/0.7.3/opentype.min.js"></script>
          <script src="https://cdnjs.cloudflare.com/ajax/libs/rita/1.3.11/rita-small.min.js"></script>
          <script src="https://cdnjs.cloudflare.com/ajax/libs/chroma-js/1.3.6/chroma.min.js"></script>
          <script src="https://code.jquery.com/jquery-3.3.1.min.js"
            integrity="sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8="
            crossorigin="anonymous"></script>
      
          <script src='Node.js'></script>
          <script src='Spring.js'></script>
      
          <link rel="stylesheet" type="text/css" href="style.css">
        </head>
        <body>
      
          <!-- Botón para agregar y eliminar nodos -->
          <button id="addNodeButton">Agregar Nodo</button>
          <button id="deleteNodeButton">Eliminar Nodo</button>
      
      
          <!-- Script principal -->
          <script src="sketch.js"></script>
      
        </body>
      </html>

     ```

     
   </details>

   <details>
     <summary>Código sketch.js</summary>
     
     ```js
     'use strict';
    
      var sketch = function(p) {
      var nodes = [];
      var springs = [];
      var selectedNode = null; // Nodo activo
      var draggingNode = null; // Nodo que está siendo arrastrado
      var nodeDiameter = 16;
      var nodeNames = [];
      var nodeCounter = 1; // Contador de nodos creados
      var inputBox; // Campo de texto para editar nombres
    
      p.setup = function() {
        p.createCanvas(p.windowWidth, p.windowHeight);
        p.background(255);
        p.noStroke();
        
        // Crear el primer nodo en el centro
        createNode(p.width / 2, p.height / 2, "Nodo " + nodeCounter);
    
        // Crear campo de texto para edición de nombres (junto a los botones)
        inputBox = p.createInput('');
        inputBox.position(20, 50); // Colocar cerca de los botones
        inputBox.size(120);
        inputBox.attribute("placeholder", "Nombre del nodo");
    
        // Evento para actualizar el nombre del nodo cuando se edite el texto
        inputBox.input(() => {
          if (selectedNode) {
            let index = nodes.indexOf(selectedNode);
            if (index !== -1) {
              nodeNames[index] = inputBox.value();
            }
          }
        });
      };
    
      p.draw = function() {
        p.background(255);
    
        for (var i = 0; i < nodes.length; i++) {
          nodes[i].attractNodes(nodes);
        }
        for (var i = 0; i < springs.length; i++) {
          springs[i].update();
        }
        for (var i = 0; i < nodes.length; i++) {
          nodes[i].update();
        }
    
        // Si se está arrastrando un nodo, actualizar su posición con el mouse
        if (draggingNode) {
          draggingNode.x = p.mouseX;
          draggingNode.y = p.mouseY;
        }
    
        // Dibujar resortes
        p.stroke(0, 130, 164);
        p.strokeWeight(2);
        for (var i = 0; i < springs.length; i++) {
          p.line(springs[i].fromNode.x, springs[i].fromNode.y, springs[i].toNode.x, springs[i].toNode.y);
        }
    
        // Dibujar nodos con nombres
        p.noStroke();
        for (var i = 0; i < nodes.length; i++) {
          if (nodes[i] === selectedNode) {
            p.fill(255, 0, 0); // Nodo activo en rojo
          } else {
            p.fill(255);
          }
          p.ellipse(nodes[i].x, nodes[i].y, nodeDiameter, nodeDiameter);
          p.fill(0);
          p.ellipse(nodes[i].x, nodes[i].y, nodeDiameter - 4, nodeDiameter - 4);
    
          // Mostrar nombre del nodo
          p.fill(0);
          p.textAlign(p.CENTER, p.CENTER);
          p.textSize(12);
          p.text(nodeNames[i], nodes[i].x, nodes[i].y - 20);
        }
      };
    
      // Función para crear un nuevo nodo
      function createNode(x, y, name) {
        var newNode = new Node(x, y);
        newNode.minX = nodeDiameter / 2;
        newNode.minY = nodeDiameter / 2;
        newNode.maxX = p.width - nodeDiameter / 2;
        newNode.maxY = p.height - nodeDiameter / 2;
        newNode.radius = 100;
        newNode.strength = -5;
        nodes.push(newNode);
        nodeNames.push(name);
      }
    
      // Evento para seleccionar un nodo y permitir arrastrarlo
      p.mousePressed = function() {
        var maxDist = 20;
        for (var i = 0; i < nodes.length; i++) {
          var checkNode = nodes[i];
          var d = p.dist(p.mouseX, p.mouseY, checkNode.x, checkNode.y);
          if (d < maxDist) {
            selectedNode = checkNode; // Seleccionamos el nodo
            draggingNode = checkNode; // Habilitamos el arrastre
    
            // Mostrar el nombre en el campo de texto
            let index = nodes.indexOf(selectedNode);
            if (index !== -1) {
              inputBox.value(nodeNames[index]);
            }
            return;
          }
        }
      };
    
      // Evento para soltar el nodo cuando se deja de hacer clic
      p.mouseReleased = function() {
        draggingNode = null;
      };
    
      // Evento para agregar un nuevo nodo cuando se presiona un botón
      document.getElementById("addNodeButton").addEventListener("click", function() {
        if (selectedNode) {
          nodeCounter++;
          var newX = selectedNode.x + p.random(-50, 50);
          var newY = selectedNode.y + p.random(-50, 50);
          createNode(newX, newY, "Nodo " + nodeCounter);
    
          // Crear un resorte entre el nodo seleccionado y el nuevo nodo
          var newSpring = new Spring(selectedNode, nodes[nodes.length - 1]);
          newSpring.length = 50;
          newSpring.stiffness = 1;
          springs.push(newSpring);
        }
      });
    
      // Evento para eliminar un nodo seleccionado
      document.getElementById("deleteNodeButton").addEventListener("click", function() {
        if (selectedNode && nodes.length > 1) {
          var index = nodes.indexOf(selectedNode);
          if (index !== -1) {
            // Eliminar todos los resortes que conectan con este nodo
            springs = springs.filter(spring => spring.fromNode !== selectedNode && spring.toNode !== selectedNode);
            
            // Eliminar el nodo y su nombre
            nodes.splice(index, 1);
            nodeNames.splice(index, 1);
            
            // Limpiar el campo de texto
            inputBox.value('');
            selectedNode = null;
          }
        }
      });
    
    };
    
    var myp5 = new p5(sketch);

     ```
     
   </details>
   
    <img src="../../../../assets/pictures/giphy3.gif" width="350">

**Aplicación potencial en conexto de entretenimiento digital:**

#### Ejemplo 3 "M_6_1_03":

<img src="../../../../assets/pictures/Screen Shot 2025-02-04 at 10.31 AM.png" width="350">

[Link de OpenProcessing](https://openprocessing.org/sketch/2456871)

**Descripción:**

<details>
  <summary>Código fuente</summary>
  
  ```js
  
  ```
  
</details>

**Parámetros:**

**Variaciones:**
1. Variante:

   <details>
     <summary>Código</summary>
     
     ```js
     
     ```
     
   </details>
   
     <img src="../../../../assets/pictures/Screen Shot 2025-02-04 at 10.31 AM.png" width="350">

   
2. Variante:

   <details>
     <summary>Código</summary>
     
     ```js
     
     ```
     
   </details>
   
    <img src="../../../../assets/pictures/Screen Shot 2025-02-04 at 10.31 AM.png" width="350">


**Aplicación potencial en conexto de entretenimiento digital:**
