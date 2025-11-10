# Interfaz-II
##### Introduccion a processing y arduino para el desarrollo de una interfaz interactiva humano-maquina como pieza artistica.
1. [Hola mundo](#ejercicio-n1-hola-mundo) <br>
2. [Led parpadeante](#ejercicio-n2-led-parpadeante) <br>
3. [Control pulsador](#ejercicio-n3-control-pulsador) <br>
4. [Led potenciometro](#ejercicio-n4-led-potenciometro) <br>
5. [Semaforo](#ejercicio-n5-semaforo) <br>
6. [Elipse interactivo](#ejercicio-n6-elipse-interactivo) <br>
7. [Arduino-boton](#ejercicio-n7-arduino-boton-processing) <br>
8. [Arduino-boton-potenciometro](#ejercicio-n8-arduino-boton-potenciometro-processing) <br>
9. [Estructuras de control en arduino](#ejercicio-n9-estructuras-de-control-en-arduino) <br>
10. [Botonera](#ejercicio-n10-botonera) <br>
11. [Evalución nota N°1: Alteración elipse interactivo](#evaluaci%C3%B3n-nota-1-alteracion-elipse-interactivo) <br>
12. [Sensor de distancia visual](#ejercicio-n11-sensor-distancia-visual) <br>
13. [Sensor de humedad](#ejercicio-n12-sensor-humedad) <br>
14. [Promedio de imagenes carpeta](#ejercicio-n14-promedio-de-imagenes-carpeta) <br>
                                                             
# Ejercicio N°1 Hola mundo
```js
void setup() {
  Serial.begin(9600); // Inicia la comunicación serie a 9600 bps
  Serial.println("Hola, Mundo!"); // Envía "Hola, Mundo!" al monitor serie
}

void loop() {
  // No es necesario poner nada en el loop para este ejemplo
}
```
<img src="https://raw.githubusercontent.com/pato-ta/Interfaz-II/refs/heads/main/imagen/Holamundo.png">/

# Ejercicio N°2 LED parpadeante
```js
void setup() {  // Configuración inicial (ej: pines como entrada/salida)
  pinMode(13, OUTPUT);  // Pin 13 como salida
}

void loop() {   // Se repite infinitamente
  digitalWrite(13, HIGH);  // Encender LED
  delay(1000);             // Esperar 1 segundo
  digitalWrite(13, LOW);   // Apagar LED
  delay(1000);             // Esperar 1 segundo
}
```
<img src="https://raw.githubusercontent.com/pato-ta/Interfaz-II/refs/heads/main/imagen/ledparpadeante.png">/

# Ejercicio N°3 Control pulsador
```js
void setup() {
  pinMode(2, INPUT);  // Botón como entrada
  pinMode(13, OUTPUT);
}
void loop() {
  if (digitalRead(2) == HIGH) {  // Si se presiona el botón
    digitalWrite(13, HIGH);
  } else {
    digitalWrite(13, LOW);
  }
}
```
<img src="https://raw.githubusercontent.com/pato-ta/Interfaz-II/refs/heads/main/imagen/Controlpulsador.png">/

# Ejercicio N°4 LED Potenciometro
```js
void setup() {
  pinMode(9, OUTPUT);  // Pin PWM (símbolo ~)
}
void loop() {
  int valor = analogRead(A0);           // Leer potenciómetro (0-1023)
  int brillo = map(valor, 0, 1023, 0, 255);  // Convertir a rango PWM
  analogWrite(9, brillo);               // Ajustar brillo
}
```
<img src="https://raw.githubusercontent.com/pato-ta/Interfaz-II/refs/heads/main/imagen/Ledpotenciometro.png">/
<img src="https://raw.githubusercontent.com/pato-ta/Interfaz-II/refs/heads/main/imagen/circuitopotenciometro.jpg">/

# Ejercicio N°5 Semaforo
```js
// C++ code - Semáforo Autos y Peatones

// Definición de pines
int LED_1 = 6;  // Luz roja autos
int LED_2 = 7;  // Luz amarilla autos
int LED_3 = 8;  // Luz verde autos
int LED_4 = 9;  // Luz verde peatones
int LED_5 = 10; // Luz roja peatones

void setup() {
  // Configuramos todos los pines como salida
  pinMode(LED_1, OUTPUT);
  pinMode(LED_2, OUTPUT);
  pinMode(LED_3, OUTPUT);
  pinMode(LED_4, OUTPUT);
  pinMode(LED_5, OUTPUT);
}

void loop() {
  // 🚦 Fase 1: Autos en verde, peatones en rojo
  digitalWrite(LED_1, LOW);   // Rojo autos apagado
  digitalWrite(LED_2, LOW);   // Amarillo autos apagado
  digitalWrite(LED_3, HIGH);  // Verde autos encendido
  digitalWrite(LED_4, LOW);   // Verde peatones apagado
  digitalWrite(LED_5, HIGH);  // Rojo peatones encendido
  delay(5000); // 5 segundos

  // 🚦 Fase 2: Amarillo autos, peatones siguen en rojo
  digitalWrite(LED_3, LOW);   // Verde autos apagado
  digitalWrite(LED_2, HIGH);  // Amarillo autos encendido
  delay(2000); // 2 segundos
  digitalWrite(LED_2, LOW);   // Amarillo autos apagado

  // 🚦 Fase 3: Rojo autos, verde peatones
  digitalWrite(LED_1, HIGH);  // Rojo autos encendido
  digitalWrite(LED_5, LOW);   // Rojo peatones apagado
  digitalWrite(LED_4, HIGH);  // Verde peatones encendido
  delay(5000); // 5 segundos

  // 🚦 Fase 4: Rojo autos, rojo peatones (tiempo intermedio)
  digitalWrite(LED_4, LOW);   // Verde peatones apagado
  digitalWrite(LED_5, HIGH);  // Rojo peatones encendido
  delay(2000); // 2 segundos
}
```
<img src="https://raw.githubusercontent.com/pato-ta/Interfaz-II/refs/heads/main/imagen/Semaforo.png">/
<img src="https://raw.githubusercontent.com/pato-ta/Interfaz-II/refs/heads/main/imagen/Circuito.jpg">/

# Ejercicio N°6 elipse interactivo
```js
import processing.serial.*;

Serial myPort;  // Crear objeto de la clase Serial
static String val;    // Datos recibidos desde el puerto serial
int sensorVal = 0;

void setup()
{
  background(0); 
  //fullScreen(P3D);
   size(1080, 720);
   noStroke();
  noFill();
  String portName = "COM3";// Cambia el número (en este caso) para que coincida con el puerto correspondiente conectado a tu Arduino. 

  //myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  myPort = new Serial(this, Serial.list()[0], 9600);

}

void draw()
{
  if ( myPort.available() > 0) {  // Si hay datos disponibles,
  val = myPort.readStringUntil('\n'); 
  try {
   sensorVal = Integer.valueOf(val.trim());
  }
  catch(Exception e) {
  ;
  }
  println(sensorVal); // léelos y guárdalos en vals!
  }  
 //background(0);
  // Escala el valor de mouseX de 0 a 640 a un rango entre 0 y 175
  float c = map(sensorVal, 0, width, 0, 400);
  // Escala el valor de mouseX de 0 a 640 a un rango entre 40 y 300
  float d = map(sensorVal, 0, width, 40,500);
  fill(255, c, 0);
  ellipse(width/2, height/2, d, d);   
}
```
<img src="https://raw.githubusercontent.com/pato-ta/Interfaz-II/refs/heads/main/imagen/Arduino%20Processing.png">/
<img src="https://raw.githubusercontent.com/pato-ta/Interfaz-II/refs/heads/main/imagen/elipse.png">/

# Ejercicio N°7 arduino-boton-processing
### Codigo arduino
```js
int buttonPin = 2;  // Pin del botón
int buttonState = 0;

void setup() {
  pinMode(buttonPin, INPUT_PULLUP); // Botón con resistencia interna
  Serial.begin(9600);
}

void loop() {
  buttonState = digitalRead(buttonPin);

  if (buttonState == HIGH) {   // Botón presionado
    Serial.println(1);        // Enviar un "1" a Processing
    delay(200);               // Evitar rebotes
  }
}
```
### Codigo processing
```js
import processing.serial.*;

Serial myPort;
ArrayList<PVector> circles; 

void setup() {
  size(1920, 1080);
  background(0);
  
  // Ajusta el nombre del puerto según tu Arduino
  println(Serial.list());
  myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  //myPort = new Serial(this, Serial.list()[0], 9600);
  
  circles = new ArrayList<PVector>();
}

void draw() {
  //background(0);
  
  // Dibujar círculos almacenados
  fill(0, 0, 0);
  //noStroke();
  stroke(255, 0, 0);
  for (PVector c : circles) {
    ellipse(c.x, c.y, 30, 30);
  }
  
  // Revisar si llega algo de Arduino
  if (myPort.available() > 0) {
    String val = myPort.readStringUntil('\n');
    if (val != null) {
      val = trim(val);
      if (val.equals("1")) {
        // Cada vez que se aprieta el botón, agregar un círculo en posición aleatoria
        circles.add(new PVector(random(width), random(height)));
      }
    }
  }
}
```
# Ejercicio N°8 Arduino-boton-potenciometro-processing
### Codigo arduino
```js
int buttonPin = 2;       // Pin del botón
int potPin = A0;         // Pin del potenciómetro
int buttonState = 0;

void setup() {
  pinMode(buttonPin, INPUT_PULLUP); // Botón con resistencia interna
  Serial.begin(9600);
}

void loop() {
  buttonState = digitalRead(buttonPin);

  if (buttonState == HIGH) {   // Botón presionado
    int potValue = analogRead(potPin);   // 0 - 1023
    Serial.print("BTN,");     // etiqueta para Processing
    Serial.println(potValue); // mando el valor junto con el evento
    delay(200);               // debounce simple
  }
}
```
### Codigo processing
```js
import processing.serial.*;

Serial myPort;
ArrayList<PVector> circles; 

void setup() {
  size(1920, 1080);
  background(0);
  
  // Ajusta el nombre del puerto según tu Arduino
  println(Serial.list());
  myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  //myPort = new Serial(this, Serial.list()[0], 9600);
  
  circles = new ArrayList<PVector>();
}

void draw() {
  //background(0);
  
  // Dibujar círculos almacenados
  fill(0, 0, 0);
  //noStroke();
  stroke(255, 0, 0);
  for (PVector c : circles) {
    ellipse(c.x, c.y, 30, 30);
  }
  
  // Revisar si llega algo de Arduino
  if (myPort.available() > 0) {
    String val = myPort.readStringUntil('\n');
    if (val != null) {
      val = trim(val);
      if (val.equals("1")) {
        // Cada vez que se aprieta el botón, agregar un círculo en posición aleatoria
        circles.add(new PVector(random(width), random(height)));
      }
    }
  }
}
```
# Ejercicio N°9 Estructuras de control en arduino
### for
```js
void setup() {
  Serial.begin(9600);   // Inicia la comunicación serial
}

void loop() {
  for (int i = 0; i < 5; i++) {
    Serial.println(i);  // imprime 0,1,2,3,4
    delay(500);         // medio segundo entre cada número
  }
}
```
### if-else
```js
int valor;  // aquí guardaremos la lectura del sensor

void setup() {
  Serial.begin(9600);   // Inicia la comunicación serial
}

void loop() {
  valor = analogRead(A0);   // lee el pin analógico A0

  if (valor < 200) {
    Serial.println("Muy bajo");
  } else if (valor < 500) {
    Serial.println("Medio");
  } else {
    Serial.println("Alto");
  }

  delay(500); // medio segundo entre lecturas
}
```
# Ejercicio N°10 Botonera
### Codigo arduino
```js
// --- Configuración de botones ---
const int numButtons = 3;
const int buttonPins[numButtons] = {2, 4, 7};
const int ledButtonPins[numButtons] = {9, 10, 11}; // LEDs botones

// --- Configuración de potenciómetros ---
const int numPots = 2;
const int potPins[numPots] = {A0, A1};
const int ledPotPins[numPots] = {3, 5}; // LEDs PWM

// Variables de estados previos
int lastButtonState[numButtons];
int lastPotValue[numPots];

void setup() {
  Serial.begin(9600);

  // Configurar botones y LEDs
  for (int i = 0; i < numButtons; i++) {
    pinMode(buttonPins[i], INPUT_PULLUP);
    pinMode(ledButtonPins[i], OUTPUT);
    lastButtonState[i] = digitalRead(buttonPins[i]);
  }

  // Configurar LEDs de potenciómetros
  for (int i = 0; i < numPots; i++) {
    pinMode(ledPotPins[i], OUTPUT);
    lastPotValue[i] = analogRead(potPins[i]);
  }
}

void loop() {
  // Leer y enviar botones
  for (int i = 0; i < numButtons; i++) {
    int buttonState = digitalRead(buttonPins[i]);

    // LED se enciende cuando botón está presionado
    digitalWrite(ledButtonPins[i], buttonState == LOW ? HIGH : LOW);

    if (buttonState != lastButtonState[i]) {  // enviar cambios
      Serial.print("B");
      Serial.print(i); 
      Serial.print(":");
      Serial.println(buttonState);
      lastButtonState[i] = buttonState;
    }
  }

  // Leer y enviar potenciómetros
  for (int i = 0; i < numPots; i++) {
    int potValue = analogRead(potPins[i]); // 0–1023
    int pwmValue = potValue / 4;           // 0–255

    // Ajustar LED según valor
    analogWrite(ledPotPins[i], pwmValue);

    if (abs(pwmValue - lastPotValue[i]) > 2) { // evitar ruido
      Serial.print("P");
      Serial.print(i);
      Serial.print(":");
      Serial.println(pwmValue);
      lastPotValue[i] = pwmValue;
    }
  }

  delay(10);
}
```
### Codigo processing
```js
// Importamos librería para comunicación serial
import processing.serial.*;
// Importamos librería Minim para manejar audio
import ddf.minim.*;

// Declaramos el objeto serial para comunicarnos con Arduino
Serial myPort;
// Objeto principal de Minim
Minim minim;
// Array de reproductores de audio (3 pistas)
AudioPlayer[] players;
// Variable para guardar el índice de la pista que está sonando
int currentTrack = -1;  // -1 significa que no hay pista activa al inicio

void setup() {
  size(400, 200); // Ventana de 400x200 píxeles
  
  // --- Configuración del puerto serial ---
  printArray(Serial.list()); // Muestra en consola la lista de puertos disponibles
  myPort = new Serial(this, Serial.list()[0], 9600); // Abrimos el primer puerto de la lista a 9600 baudios
  
  // --- Configuración de audio ---
  minim = new Minim(this); // Inicializamos Minim
  players = new AudioPlayer[3]; // Creamos un array de 3 reproductores
  
  // Cargamos los 3 archivos de audio desde la carpeta "data"
  players[0] = minim.loadFile("audio1.mp3", 2048); 
  players[1] = minim.loadFile("audio2.mp3", 2048); 
  players[2] = minim.loadFile("audio3.mp3", 2048); 
}

void draw() {
  background(0); // Fondo negro
  fill(255);     // Color blanco para el texto
  textSize(16);  // Tamaño del texto
  
  // Mostramos en pantalla qué botón está activo
  text("Botón actual: " + (currentTrack == -1 ? "ninguno" : currentTrack), 20, 40);
}

void serialEvent(Serial myPort) {
  // Leemos la cadena que llega desde Arduino hasta el salto de línea
  String inString = trim(myPort.readStringUntil('\n'));
  
  // Si no llega nada, salimos
  if (inString == null) return;

  // --- Si el mensaje recibido empieza con "B" significa que es un botón ---
  if (inString.startsWith("B")) {
    // Quitamos la letra "B" y separamos el mensaje en partes (ejemplo "0:0")
    String[] parts = split(inString.substring(1), ':');
    
    // Si realmente recibimos dos partes (índice y estado)
    if (parts.length == 2) {
      int buttonIndex = int(parts[0]); // Número del botón (0,1,2)
      int state = int(parts[1]);       // Estado del botón (0 = presionado, 1 = suelto)
      
      // Si el botón fue presionado (LOW = 0 en Arduino)
      if (state == 0) { 
        playTrack(buttonIndex); // Llamamos a la función para reproducir la pista correspondiente
      }
    }
  }
}

// --- Función que reproduce una pista según el botón ---
void playTrack(int index) {
  // Si ya había una pista sonando, la pausamos y la rebobinamos al inicio
  if (currentTrack != -1 && players[currentTrack].isPlaying()) {
    players[currentTrack].pause();
    players[currentTrack].rewind();
  }
  
  // Reproducimos en bucle la pista seleccionada
  players[index].loop();
  
  // Actualizamos la variable para saber cuál es la pista activa
  currentTrack = index;
}
```
# Evaluación nota N°1 alteracion elipse interactivo
### Codigo arduino
```js
unsigned int ADCValue;
void setup(){
    Serial.begin(9600);
}

void loop(){

 int val = analogRead(0);
   val = map(val, 0, 300, 0, 255);
    Serial.println(val);
delay(50);
}
```
### Codigo processing
```js
import processing.serial.*;

Serial myPort;  // Crear objeto de la clase Serial
static String val;    // Datos recibidos desde el puerto serial
int sensorVal = 0;

void setup()
{
  background(0); 
  //fullScreen(P3D);
   size(1080, 720);
   noStroke();
  noFill();
  String portName = "COM3";// Cambia el número (en este caso) para que coincida con el puerto correspondiente conectado a tu Arduino. 

  //myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  myPort = new Serial(this, Serial.list()[0], 9600);

}

void draw()
{
  if ( myPort.available() > 0) {  // Si hay datos disponibles,
  val = myPort.readStringUntil('\n'); 
  try {
   sensorVal = Integer.valueOf(val.trim());
  }
  catch(Exception e) {
  ;
  }
  println(sensorVal); // léelos y guárdalos en vals!
  }  
 //background(0);
  // Escala el valor de mouseX de 0 a 640 a un rango entre 0 y 175
  float c = map(sensorVal, 0, width, 0, 400);
  // Escala el valor de mouseX de 0 a 640 a un rango entre 40 y 300
  float d = map(sensorVal, 0, width, 40,500);
  fill(255, c, 0);
  ellipse(width/2, height/2, d, d);   
}
```
### Primera alteración de codigo processing 
### Pregunta a IA: Puedes cambiar este codigo para que el circulo sea un cuadrado?
```js
import processing.serial.*;
Serial myPort;  // Crear objeto de la clase Serial
static String val;    // Datos recibidos desde el puerto serial
int sensorVal = 0;

void setup(){
  background(0); 
  //fullScreen(P3D);
   size(1080, 720);
   noStroke();
  noFill();
  String portName = "COM3";// Cambia el número (en este caso) para que coincida con el puerto correspondiente conectado a tu Arduino. 

  //myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  // Se ha ajustado para usar el primer puerto disponible, como ya lo tenías.
  myPort = new Serial(this, Serial.list()[0], 9600);
}

void draw(){
  if ( myPort.available() > 0) {  // Si hay datos disponibles,
  val = myPort.readStringUntil('\n'); 
  try {
   sensorVal = Integer.valueOf(val.trim());
  }
  catch(Exception e) {
  ;
  }
  println(sensorVal); // léelos y guárdalos en vals!
  }  
  
 // Se mantiene el fondo en negro para que el nuevo cuadrado se dibuje sobre la posición anterior.
 // Para ver solo el cuadrado actual, descomenta la línea de abajo.
 // background(0); 

  // Escala el valor de sensorVal a un rango entre 0 y 400 para el color
  // Usaremos 'sensorVal' en lugar de 'width' en el mapeo, asumiendo que 'sensorVal'
  // variará en un rango que queremos normalizar (ej. 0 a 1023 para un sensor Arduino)
  // Si no sabes el rango máximo, déjalo como 1023 (o el valor máximo de tu sensor).
  // Si mapeas 'sensorVal' usando 'width' como máximo, el mapeo será inconsistente.
  // Asumiremos un rango máximo de **1023** para un sensor analógico típico.
  float c = map(sensorVal, 0, 1023, 0, 400); // Mapeo para el color
  
  // Escala el valor de sensorVal a un rango entre 40 y 500 para el tamaño del cuadrado
  float d = map(sensorVal, 0, 1023, 40, 500); // Mapeo para el tamaño (ancho y alto)
  
  fill(255, c, 0); // Color del cuadrado
  
  // Para dibujar un cuadrado centrado:
  // La función rect() toma (posición X de la esquina superior izquierda, posición Y de la esquina superior izquierda, ancho, alto)
  // Para centrarlo: (width/2 - d/2, height/2 - d/2, d, d)
  rect(width/2 - d/2, height/2 - d/2, d, d); 
}
```
<img src="https://raw.githubusercontent.com/pato-ta/Interfaz-II/refs/heads/main/imagen/elipse%20interactivo%20cuadrado.png">/

### Segunda alteración de codigo processing
### Pregunta a IA: Puedes modificar el codigo para que la figura sea una estrella?
```js
import processing.serial.*;

Serial myPort;  // Crear objeto de la clase Serial
static String val;    // Datos recibidos desde el puerto serial
int sensorVal = 0;
int numPuntas = 5; // Número de puntas para la estrella (puedes cambiarlo)

void setup(){
  background(0); 
  //fullScreen(P3D);
   size(1080, 720);
   noStroke();
  // noFill(); // Quitamos esto para que la estrella se pinte, la pondremos en draw
  
  // Asumiendo que Serial.list()[0] es el puerto correcto. 
  // Si tienes problemas, cambia el índice [0] o usa el nombre del puerto como en tu código original.
  String portName = "COM3"; // Esto es solo un recordatorio
  try {
     myPort = new Serial(this, Serial.list()[0], 9600);
  } catch (Exception e) {
    println("Error al abrir el puerto serial: " + e.getMessage());
    println("Puertos disponibles: " + join(Serial.list(), ", "));
    // Puedes comentar la línea de arriba y descomentar la siguiente para usar un puerto específico si Serial.list()[0] falla
    // myPort = new Serial(this, "COM3", 9600); 
  }
}

void draw(){
  // --- Lectura Serial ---
  if ( myPort.available() > 0) {  // Si hay datos disponibles,
    val = myPort.readStringUntil('\n'); 
    try {
      sensorVal = Integer.valueOf(val.trim());
    }
    catch(Exception e) {
      // Ignorar datos no válidos
    }
    println(sensorVal); // léelos y guárdalos en sensorVal!
  }  
  
  background(0); // Limpia la pantalla en cada frame
  
  // --- Mapeo de Variables ---
  // Escala el valor del sensor (asume de 0 a 1023 si es de un Arduino con 10 bits)
  // Mapeamos a un rango de color (por ejemplo, el componente verde de 0 a 255)
  float colorG = map(sensorVal, 0, 1023, 0, 255);
  
  // Mapeamos el valor para los radios de la estrella
  float radioExterior = map(sensorVal, 0, 1023, 50, 300); // Tamaño de las puntas
  // El radio interior se mantiene proporcional, por ejemplo, un 40% del radio exterior
  float radioInterior = radioExterior * 0.4; 

  // --- Dibujo de la Estrella ---
  fill(255, colorG, 0); // Color: Rojo fijo, Verde basado en sensor, Azul fijo a 0
  noStroke(); // Sin contorno
  
  // Dibuja la estrella en el centro de la pantalla
  // star(centroX, centroY, radioInterior, radioExterior, numPuntas);
  star(width/2, height/2, radioInterior, radioExterior, numPuntas);
}

// ----------------------------------------------------------------------------------

/**
 * Función para dibujar una estrella.
 * Basada en la geometría polar para calcular los vértices.
 */
void star(float x, float y, float radius1, float radius2, int npoints) {
  float angle = TWO_PI / npoints;
  float halfAngle = angle / 2.0;
  
  pushMatrix(); // Guarda el estado de transformación actual
  translate(x, y); // Mueve el origen al centro de la estrella
  
  beginShape();
  for (float a = 0; a < TWO_PI; a += angle) {
    // Vértice de la punta (radio exterior)
    float sx = cos(a) * radius2;
    float sy = sin(a) * radius2;
    vertex(sx, sy);
    
    // Vértice de la hendidura (radio interior)
    sx = cos(a + halfAngle) * radius1;
    sy = sin(a + halfAngle) * radius1;
    vertex(sx, sy);
  }
  endShape(CLOSE);
  
  popMatrix(); // Restaura el estado de transformación anterior
}
````
<img src="https://raw.githubusercontent.com/pato-ta/Interfaz-II/refs/heads/main/imagen/elipse%20interactivo%20estrella.png">/

### Tercera alteración de codigo processing
### Pregunta a IA: Puedes modificarlo para que el azul y el rosado vayan en degrade dentro de la misma estrella?
```
import processing.serial.*;

Serial myPort;  // Crear objeto de la clase Serial
static String val;    // Datos recibidos desde el puerto serial
int sensorVal = 0;
int numPuntas = 5; // Número de puntas para la estrella (puedes cambiarlo)

void setup(){
  background(0); 
  size(1080, 720);
  noStroke();
  
  // Configuración del puerto serial
  String portName = "COM3"; // Recordatorio
  try {
     myPort = new Serial(this, Serial.list()[0], 9600);
  } catch (Exception e) {
    println("Error al abrir el puerto serial: " + e.getMessage());
    println("Puertos disponibles: " + join(Serial.list(), ", "));
  }
}

void draw(){
  // --- Lectura Serial ---
  if ( myPort.available() > 0) {  // Si hay datos disponibles,
    val = myPort.readStringUntil('\n'); 
    try {
      sensorVal = Integer.valueOf(val.trim());
    }
    catch(Exception e) {
      // Ignorar datos no válidos
    }
    println(sensorVal); // léelos y guárdalos en sensorVal!
  }  
  
  background(0); // Limpia la pantalla en cada frame
  
  // --- Mapeo de Variables para Tamaño ---
  float radioExterior = map(sensorVal, 0, 1023, 50, 300); // Tamaño de las puntas
  
  // --- Definir colores de inicio y fin del degradado ---
  // Color Azul (inicio del degradado)
  color azulInicio = color(0, 0, 255); 
  // Color Rosado (fin del degradado)
  color rosadoFin = color(255, 100, 200); // Un rosado con algo de verde y azul
  
  // --- Dibujo de la Estrella con Degradado por Capas ---
  noStroke(); // Sin contorno para las capas
  
  int numCapas = 20; // Número de estrellas superpuestas para el degradado
  
  for (int i = numCapas; i >= 0; i--) {
    // Interpolar el color entre azul y rosado
    float t = i / (float)numCapas; // t va de 1 a 0
    color capaColor = lerpColor(azulInicio, rosadoFin, t);
    
    // Interpolar el tamaño
    float currentRadioExterior = lerp(0, radioExterior, t);
    float currentRadioInterior = currentRadioExterior * 0.4;
    
    fill(capaColor);
    star(width/2, height/2, currentRadioInterior, currentRadioExterior, numPuntas);
  }
}

// ----------------------------------------------------------------------------------

/**
 * Función para dibujar una estrella.
 * Basada en la geometría polar para calcular los vértices.
 */
void star(float x, float y, float radius1, float radius2, int npoints) {
  float angle = TWO_PI / npoints;
  float halfAngle = angle / 2.0;
  
  pushMatrix(); // Guarda el estado de transformación actual
  translate(x, y); // Mueve el origen al centro de la estrella
  
  beginShape();
  for (float a = 0; a < TWO_PI; a += angle) {
    // Vértice de la punta (radio exterior)
    float sx = cos(a) * radius2;
    float sy = sin(a) * radius2;
    vertex(sx, sy);
    
    // Vértice de la hendidura (radio interior)
    sx = cos(a + halfAngle) * radius1;
    sy = sin(a + halfAngle) * radius1;
    vertex(sx, sy);
  }
  endShape(CLOSE);
  
  popMatrix(); // Restaura el estado de transformación anterior
}
```
<img src="https://raw.githubusercontent.com/pato-ta/Interfaz-II/refs/heads/main/imagen/elipse%20interactivo%20azul%20rosado.png">/

### Cuarta alteración de codigo processing
### Pregunta a IA: Puedes modificarlo para que las mismas estrellas en degrade azul y rosado se conviertan en tres estrellas individuales una tras otra en degrade azul y rosado?
```js
import processing.serial.*;

Serial myPort;  // Crear objeto de la clase Serial
static String val;    // Datos recibidos desde el puerto serial
int sensorVal = 0;
int numPuntas = 5; // Número de puntas para la estrella
float estrellaBaseSize = 250; // Tamaño fijo de las estrellas para esta demostración

void setup(){
  background(0); 
  size(1080, 720);
  noStroke();
  
  // Configuración del puerto serial
  String portName = "COM3"; // Recordatorio
  try {
     myPort = new Serial(this, Serial.list()[0], 9600);
  } catch (Exception e) {
    println("Error al abrir el puerto serial: " + e.getMessage());
    println("Puertos disponibles: " + join(Serial.list(), ", "));
  }
}

void draw(){
  // --- Lectura Serial ---
  if ( myPort.available() > 0) {  // Si hay datos disponibles,
    val = myPort.readStringUntil('\n'); 
    try {
      sensorVal = Integer.valueOf(val.trim());
    }
    catch(Exception e) {
      // Ignorar datos no válidos
    }
    // sensorVal se usará para controlar la posición, por lo que imprimimos para seguimiento
    println(sensorVal); 
  }  
  
  background(0); // Limpia la pantalla en cada frame
  
  // --- Definir colores de inicio y fin del degradado ---
  color azulInicio = color(0, 0, 255);  // Azul puro
  color rosadoFin = color(255, 100, 200); // Tono rosado
  
  // --- Mapeo del Sensor a Posición Horizontal ---
  // Mapeamos el sensor (0-1023) a la posición horizontal (0 a width)
  float xPosMap = map(sensorVal, 0, 1023, 0, width);
  
  // Definimos las posiciones de las 3 estrellas
  float yCenter = height / 2;
  float separation = width / 4; // Separación entre estrellas (se colocan en 1/4, 2/4, 3/4)
  
  // Array de posiciones X de las estrellas.
  // Usaremos el valor mapeado del sensor para desplazar estas posiciones.
  float[] starXPositions = {
    separation, 
    width / 2, 
    width - separation
  };
  
  // El tamaño de la estrella es fijo para simplificar, pero puedes mapearlo también.
  float radioExterior = estrellaBaseSize;
  float radioInterior = radioExterior * 0.4;
  
  // --- Dibujo de las 3 Estrellas ---
  noStroke();
  
  // Bucle para dibujar las tres estrellas
  for (int i = 0; i < starXPositions.length; i++) {
    float starX = starXPositions[i];
    
    // Llamamos a la función que dibuja una estrella con degradado.
    // El tamaño y la paleta de colores son fijos, solo cambia la posición (starX).
    drawGradientStar(starX, yCenter, radioInterior, radioExterior, numPuntas, azulInicio, rosadoFin);
  }
}

// ----------------------------------------------------------------------------------

/**
 * Nueva función que encapsula la lógica de la estrella con degradado por capas.
 * Esto hace el código de draw() más limpio y reutilizable.
 */
void drawGradientStar(float x, float y, float r1, float r2, int npoints, color c1, color c2) {
  int numCapas = 20; // Número de estrellas superpuestas para el degradado
  
  for (int i = numCapas; i >= 0; i--) {
    // Interpolar el color (de c1 a c2)
    float t = i / (float)numCapas; // t va de 1 a 0
    color capaColor = lerpColor(c1, c2, t);
    
    // Interpolar el tamaño (de 0 a r2)
    float currentRadioExterior = lerp(0, r2, t);
    float currentRadioInterior = currentRadioExterior * (r1 / r2); // Mantiene la proporción r1/r2
    
    fill(capaColor);
    star(x, y, currentRadioInterior, currentRadioExterior, npoints);
  }
}

// ----------------------------------------------------------------------------------

/**
 * Función base para dibujar una estrella simple.
 * Basada en la geometría polar para calcular los vértices.
 */
void star(float x, float y, float radius1, float radius2, int npoints) {
  float angle = TWO_PI / npoints;
  float halfAngle = angle / 2.0;
  
  pushMatrix(); 
  translate(x, y); 
  
  beginShape();
  for (float a = 0; a < TWO_PI; a += angle) {
    // Vértice de la punta (radio exterior)
    float sx = cos(a) * radius2;
    float sy = sin(a) * radius2;
    vertex(sx, sy);
    
    // Vértice de la hendidura (radio interior)
    sx = cos(a + halfAngle) * radius1;
    sy = sin(a + halfAngle) * radius1;
    vertex(sx, sy);
  }
  endShape(CLOSE);
  
  popMatrix();
}
```
<img src="https://raw.githubusercontent.com/pato-ta/Interfaz-II/refs/heads/main/imagen/elipse%20interactivo%20tres%20estrellas.png">/

### Quinta alteracion a codigo de processing
### Pregunta a IA: Puedes modificarlo para que estas tres estrellas cambien de tamaño cuando muevo el boton conectado al arduino?
```js
import processing.serial.*;

Serial myPort;  // Crear objeto de la clase Serial
static String val;    // Datos recibidos desde el puerto serial
int sensorVal = 0;
int numPuntas = 5; // Número de puntas para la estrella
// Eliminamos la variable de tamaño fijo, ahora será dinámico

void setup(){
  background(0); 
  size(1080, 720);
  noStroke();
  
  // Configuración del puerto serial
  String portName = "COM3"; // Recordatorio
  try {
     myPort = new Serial(this, Serial.list()[0], 9600);
  } catch (Exception e) {
    println("Error al abrir el puerto serial: " + e.getMessage());
    println("Puertos disponibles: " + join(Serial.list(), ", "));
  }
}

void draw(){
  // --- Lectura Serial ---
  if ( myPort.available() > 0) {  // Si hay datos disponibles,
    val = myPort.readStringUntil('\n'); 
    try {
      sensorVal = Integer.valueOf(val.trim());
    }
    catch(Exception e) {
      // Ignorar datos no válidos
    }
    println(sensorVal); // Imprimimos el valor del sensor
  }  
  
  background(0); // Limpia la pantalla en cada frame
  
  // --- Mapeo de Variables para TAMAÑO (Controlado por el Sensor) ---
  // Mapeamos el valor del sensor (0-1023) al radio exterior deseado (ej: 50 a 350 píxeles)
  float radioExterior = map(sensorVal, 0, 1023, 50, 350); 
  float radioInterior = radioExterior * 0.4; // El radio interior sigue siendo proporcional
  
  // --- Definir colores de inicio y fin del degradado ---
  color azulInicio = color(0, 0, 255);  // Azul puro
  color rosadoFin = color(255, 100, 200); // Tono rosado
  
  // --- Posiciones de las 3 Estrellas ---
  float yCenter = height / 2;
  float separation = width / 4; // Separación entre estrellas
  
  // Posiciones X de las estrellas (fijas)
  float[] starXPositions = {
    separation, 
    width / 2, 
    width - separation
  };
  
  // --- Dibujo de las 3 Estrellas ---
  noStroke();
  
  // Bucle para dibujar las tres estrellas
  for (int i = 0; i < starXPositions.length; i++) {
    float starX = starXPositions[i];
    
    // NOTA: Se pasan los radios DINÁMICOS (radioInterior, radioExterior) a la función.
    drawGradientStar(starX, yCenter, radioInterior, radioExterior, numPuntas, azulInicio, rosadoFin);
  }
}

// ----------------------------------------------------------------------------------

/**
 * Función que encapsula la lógica de la estrella con degradado por capas.
 * Usa los radios r1 y r2 que le son pasados (ahora dinámicos).
 */
void drawGradientStar(float x, float y, float r1, float r2, int npoints, color c1, color c2) {
  int numCapas = 20; // Número de estrellas superpuestas para el degradado
  float ratio = r1 / r2; // Calculamos la proporción una sola vez

  for (int i = numCapas; i >= 0; i--) {
    // Interpolar el color (de c1 a c2)
    float t = i / (float)numCapas; // t va de 1 a 0
    color capaColor = lerpColor(c1, c2, t);
    
    // Interpolar el tamaño (de 0 a r2)
    float currentRadioExterior = lerp(0, r2, t);
    float currentRadioInterior = currentRadioExterior * ratio; // Mantiene la proporción r1/r2
    
    fill(capaColor);
    star(x, y, currentRadioInterior, currentRadioExterior, npoints);
  }
}

// ----------------------------------------------------------------------------------

/**
 * Función base para dibujar una estrella simple.
 */
void star(float x, float y, float radius1, float radius2, int npoints) {
  float angle = TWO_PI / npoints;
  float halfAngle = angle / 2.0;
  
  pushMatrix(); 
  translate(x, y); 
  
  beginShape();
  for (float a = 0; a < TWO_PI; a += angle) {
    // Vértice de la punta (radio exterior)
    float sx = cos(a) * radius2;
    float sy = sin(a) * radius2;
    vertex(sx, sy);
    
    // Vértice de la hendidura (radio interior)
    sx = cos(a + halfAngle) * radius1;
    sy = sin(a + halfAngle) * radius1;
    vertex(sx, sy);
  }
  endShape(CLOSE);
  
  popMatrix();
}
```
# Ejercicio N°11 Sensor distancia visual
### Codigo Arduino
```js
// Definir el pin del sensor Sharp
int sharpPin = A0;

void setup() {
  Serial.begin(9600); // Iniciar comunicación serial
}

void loop() {
  int sensorValue = analogRead(sharpPin); // Leer valor del sensor
  Serial.println(sensorValue); // Enviar valor a Processing
  delay(100); // Esperar un momento
}
```
### Codigo Processing
```js
import processing.serial.*;

Serial myPort;  // Create object from Serial class
static String val;    // Data received from the serial port
int sensorVal = 0;

void setup()
{
  background(0); 
  //fullScreen(P3D);
   size(1080, 720);
   noStroke();
  noFill();
  String portName = "COM5";// Change the number (in this case ) to match the corresponding port number connected to your Arduino. 

  myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
}

void draw()
{
  if ( myPort.available() > 0) {  // If data is available,
  val = myPort.readStringUntil('\n'); 
  try {
   sensorVal = Integer.valueOf(val.trim());
  }
  catch(Exception e) {
  ;
  }
  println(sensorVal); // read it and store it in vals!
  }  
 //background(0);
  // Scale the mouseX value from 0 to 640 to a range between 0 and 175
  float c = map(sensorVal, 0, width, 0, 400);
  // Scale the mouseX value from 0 to 640 to a range between 40 and 300
  float d = map(sensorVal, 0, width, 40,500);
  fill(255, c, 0);
  ellipse(width/2, height/2, d, d);   

}
```
<img src="https://raw.githubusercontent.com/pato-ta/Interfaz-II/refs/heads/main/imagen/SENSOR%20DISTANCIA.jpg">/
<img src="https://raw.githubusercontent.com/pato-ta/Interfaz-II/refs/heads/main/imagen/DISTANCIA%20SENSOR.png">/

# Ejercicio N°12 Sensor humedad
### Codigo Arduino
```js
void setup()
{
  Serial.begin(9600);// abre el puerto serial y Establece la velocidad en baudios a 9600 bps
}
void loop()
{
  int sensorValue;
  sensorValue = analogRead(0);   //conectar el sensor de humedad al pin analogo 0
  Serial.println(sensorValue); //imprime el valor a serial.
  delay(200);
}
```
# Ejercicio N°13 Promedio de imagenes
### Codigo arduino
```js
void setup() {
  Serial.begin(9600);
}

void loop() {
  int potValue = analogRead(A0);
  Serial.println(potValue);
  delay(20);
}
```
### Codigo processing 
```js
import processing.serial.*;

Serial myPort;
PImage[] imgs;
int numImages = 3;
PImage avgImg;
float mixAmount = 0;

void setup() {
  size(800, 600);
  println(Serial.list());
  
  //Cambia el índice según tu puerto (0, 1, 2, etc.)
  myPort = new Serial(this, Serial.list()[0], 9600);
  //myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  myPort.bufferUntil('\n');

  // Cargar imágenes
  imgs = new PImage[numImages];
  imgs[0] = loadImage("img1.jpg");
  imgs[1] = loadImage("img2.jpg");
  imgs[2] = loadImage("img3.jpg");

  avgImg = createImage(imgs[0].width, imgs[0].height, RGB);
}

void draw() {
  // Dibujar la imagen promedio según el valor del potenciómetro
  background(0);
  calcAverage(mixAmount);
  image(avgImg, 0, 0, width, height);
  
  fill(255);
  textSize(20);
  text("Mezcla: " + nf(mixAmount, 1, 2), 20, height - 20);
}

void serialEvent(Serial p) {
  String val = p.readStringUntil('\n');
  if (val != null) {
    val = trim(val);
    float sensor = float(val);
    mixAmount = map(sensor, 0, 1023, 0, 1); // 0 a 1
  }
}

void calcAverage(float t) {
  avgImg.loadPixels();

  for (int i = 0; i < avgImg.pixels.length; i++) {
    color c1 = imgs[0].pixels[i];
    color c2 = imgs[1].pixels[i];
    color c3 = imgs[2].pixels[i];

    // Promedio ponderado según el potenciómetro
    float r = red(c1)*(1-t) + red(c2)*t*0.5 + red(c3)*t*0.5;
    float g = green(c1)*(1-t) + green(c2)*t*0.5 + green(c3)*t*0.5;
    float b = blue(c1)*(1-t) + blue(c2)*t*0.5 + blue(c3)*t*0.5;

    avgImg.pixels[i] = color(r, g, b);
  }
  avgImg.updatePixels();
}
```
<img src="https://raw.githubusercontent.com/pato-ta/Interfaz-II/refs/heads/main/imagen/sensor%20%20humedad.jpg">/

# Ejercicio N°14 Promedio de imagenes carpeta 
### Codigo arduino
```js
void setup() {
  Serial.begin(9600);
}

void loop() {
  int potValue = analogRead(A0);
  Serial.println(potValue);
  delay(20);
}
```
### Codigo processing
```js
// --- Librerías necesarias ---
// Importa la librería de comunicación serial para conectar con Arduino
import processing.serial.*;
// Importa la clase File de Java para listar archivos y carpetas
import java.io.File;

// --- Comunicación serial con Arduino ---
// Variable que contendrá el objeto de puerto serial (conexión con Arduino)
Serial myPort;
// Variable que guarda el valor leído del potenciómetro (0..1023)
float potValue = 0;

// --- Variables de imágenes ---
// Arreglo dinámico que contendrá todas las imágenes cargadas desde la carpeta
PImage[] imgs;
// Imagen donde se almacenará el resultado del promedio/interpolación
PImage avgImg;

// --- Configuración inicial ---
void setup() {
  // Define el tamaño de la ventana de Processing (ancho, alto)
  size(745, 1024);
  
  // Cargar imágenes desde carpeta "data/imagenes"
  // Llama a la función que busca todas las imágenes dentro de esa carpeta
  imgs = loadImagesFromFolder("imagenes");
  // Imprime en la consola cuántas imágenes se cargaron (útil para debug)
  println("Imágenes cargadas: " + imgs.length);
  
  // Redimensionar todas las imágenes al tamaño del lienzo para que coincidan pixel a pixel
  for (int i = 0; i < imgs.length; i++) {
    imgs[i].resize(width, height); // redimensiona cada imagen al ancho y alto de la ventana
  }
  
  // Crea una imagen vacía del tamaño del lienzo donde guardaremos el promedio
  avgImg = createImage(width, height, RGB);
  
  // Conectar con Arduino (ver lista de puertos)
  // Muestra en consola la lista de puertos seriales disponibles (para identificar cuál usar)
  printArray(Serial.list());
  // Alternativa automática (comentada): abrir el primer puerto disponible a 9600 baudios
  // myPort = new Serial(this, Serial.list()[0], 9600);
  // Abrir un puerto específico (ejemplo para macOS). Ajusta según el puerto real en tu sistema.
  myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  // Nota: si no funciona el puerto, revisa la salida de printArray(Serial.list()) y usa el nombre correcto.
}

// --- Bucle principal ---
// draw() se ejecuta continuamente (aprox. 60 veces por segundo)
void draw() {
  // Pinta el fondo de negro en cada frame
  background(0);
  // Llama a la función que lee datos desde el puerto serial (actualiza potValue)
  readSerial();
  
  // Si no hay imágenes o sólo hay una, no hacemos nada (necesitamos al menos 2 para interpolar)
  if (imgs == null || imgs.length < 2) return;
  
  // Mapear el valor del potenciómetro (0..1023) al rango de índices entre 0 y imgs.length-1
  // Esto permite moverse a lo largo de la secuencia de imágenes
  float mixValue = map(potValue, 0, 1023, 0, imgs.length - 1);
  
  // Calcular el promedio/interpolación entre las dos imágenes vecinas según mixValue
  avgImagesWeighted(mixValue);
  
  // Mostrar la imagen promedio resultante en la pantalla, en la posición (0,0)
  image(avgImg, 0, 0);
  
  // Mostrar texto con el valor actual del potenciómetro en la esquina inferior izquierda
  fill(255); // color blanco para el texto
  text("Valor pot: " + nf(potValue, 1, 0), 10, height - 10); // nf para formatear el número
}

// --- Función que calcula el promedio ponderado entre imágenes ---
// mix es un valor flotante que indica la posición entre imágenes (ej. 2.3 -> entre img2 e img3)
void avgImagesWeighted(float mix) {
  // Accede al arreglo de píxeles de avgImg para poder modificarlos directamente
  avgImg.loadPixels();
  
  // Asegura que mix esté dentro del rango válido [0, imgs.length - 1]
  mix = constrain(mix, 0, imgs.length - 1);
  
  // i1 es el índice de la imagen "inferior" (por ejemplo 2 en 2.3)
  int i1 = floor(mix);
  // i2 es la imagen siguiente (i1 + 1), pero sin pasarse del último índice
  int i2 = min(i1 + 1, imgs.length - 1);
  // t es la fracción entre i1 e i2 (por ejemplo, 0.3 si mix es 2.3)
  float t = mix - i1;
  
  // Cargar los píxeles de las dos imágenes que vamos a mezclar
  imgs[i1].loadPixels();
  imgs[i2].loadPixels();
  
  // Recorre todos los píxeles de la imagen objetivo
  for (int i = 0; i < avgImg.pixels.length; i++) {
    // Coge el color del píxel i de la imagen i1
    color c1 = imgs[i1].pixels[i];
    // Coge el color del píxel i de la imagen i2
    color c2 = imgs[i2].pixels[i];
    
    // Interpola por separado cada componente de color (rojo, verde, azul)
    // red(c1) obtiene la componente roja del color c1
    float r = lerp(red(c1), red(c2), t);
    // green(c1) obtiene la componente verde del color c1
    float g = lerp(green(c1), green(c2), t);
    // blue(c1) obtiene la componente azul del color c1
    float b = lerp(blue(c1), blue(c2), t);
    
    // Crea un nuevo color a partir de las componentes interpoladas y lo asigna al píxel i
    avgImg.pixels[i] = color(r, g, b);
  }
  
  // Aplica los cambios realizados en el arreglo de píxeles a la imagen avgImg
  avgImg.updatePixels();
}

// --- Leer valor del potenciómetro desde Arduino ---
// Lee datos desde el puerto serial hasta encontrar saltos de línea y los convierte a número
void readSerial() {
  // Mientras el puerto exista y tenga bytes disponibles para leer...
  while (myPort != null && myPort.available() > 0) {
    // Lee una línea completa hasta '\n' (salto de línea)
    String val = myPort.readStringUntil('\n');
    if (val != null) {
      // Elimina espacios y caracteres de control al inicio/final
      val = trim(val);
      // Si la cadena no está vacía, la convierte a float y la asigna a potValue
      if (val.length() > 0) {
        potValue = float(val);
      }
    }
  }
}

// --- Cargar todas las imágenes desde una carpeta ---
// Devuelve un arreglo PImage[] con todas las imágenes JPG/PNG encontradas en data/folderName
PImage[] loadImagesFromFolder(String folderName) {
  // Construye la ruta absoluta a la carpeta dentro de la carpeta data del sketch
  String path = sketchPath("data/" + folderName);
  // Crea un objeto File apuntando a esa carpeta
  File folder = new File(path);
  // Lista todos los archivos dentro de la carpeta (puede devolver null si no existe)
  File[] files = folder.listFiles();
  
  // Si files es null, la carpeta no existe o no tiene permisos -> avisar y devolver null
  if (files == null) {
    println("Carpeta no encontrada: " + path);
    return null;
  }
  
  // Crea una lista dinámica para almacenar las PImage cargadas
  ArrayList<PImage> loaded = new ArrayList<PImage>();
  // Recorre cada archivo encontrado en la carpeta
  for (File f : files) {
    // Obtiene el nombre del archivo y lo convierte a minúsculas para comparar extensiones
    String fname = f.getName().toLowerCase();
    // Si termina en .jpg o .png, lo cargamos
    if (fname.endsWith(".jpg") || fname.endsWith(".png")) {
      // loadImage busca en data/folderName el archivo y devuelve un PImage
      PImage img = loadImage(folderName + "/" + f.getName());
      // Si la imagen se cargó correctamente, la agregamos a la lista
      if (img != null) loaded.add(img);
    }
  }
  
  // Convierte la ArrayList a un arreglo PImage[] y lo retorna
  return loaded.toArray(new PImage[loaded.size()]);
}
```
# Evaluación nota N°2: Desasosiego
### Promedio de imagenes carpeta
Realizaremos una secuencia de fotografías en blanco y negro que serán intervenidas posteriormente de forma análoga a partir de la tecnica rotoscopia. Haremos uso del ejercicio “promedio de Imágenes” a través de los programas Arduino IDE y Processing, y de un circuito que incluye un potenciómetro.
### Codigo arduino
```js
void setup() {
  Serial.begin(9600);
}

void loop() {
  int potValue = analogRead(A0);
  Serial.println(potValue);
  delay(20);
}
```
### Codigo processing
```js
// --- Librerías necesarias ---
// Importa la librería de comunicación serial para conectar con Arduino
import processing.serial.*;
// Importa la clase File de Java para listar archivos y carpetas
import java.io.File;

// --- Comunicación serial con Arduino ---
// Variable que contendrá el objeto de puerto serial (conexión con Arduino)
Serial myPort;
// Variable que guarda el valor leído del potenciómetro (0..1023)
float potValue = 0;

// --- Variables de imágenes ---
// Arreglo dinámico que contendrá todas las imágenes cargadas desde la carpeta
PImage[] imgs;
// Imagen donde se almacenará el resultado del promedio/interpolación
PImage avgImg;

// --- Configuración inicial ---
void setup() {
  // Define el tamaño de la ventana de Processing (ancho, alto)
  size(745, 1024);
  
  // Cargar imágenes desde carpeta "data/imagenes"
  // Llama a la función que busca todas las imágenes dentro de esa carpeta
  imgs = loadImagesFromFolder("imagenes");
  // Imprime en la consola cuántas imágenes se cargaron (útil para debug)
  println("Imágenes cargadas: " + imgs.length);
  
  // Redimensionar todas las imágenes al tamaño del lienzo para que coincidan pixel a pixel
  for (int i = 0; i < imgs.length; i++) {
    imgs[i].resize(width, height); // redimensiona cada imagen al ancho y alto de la ventana
  }
  
  // Crea una imagen vacía del tamaño del lienzo donde guardaremos el promedio
  avgImg = createImage(width, height, RGB);
  
  // Conectar con Arduino (ver lista de puertos)
  // Muestra en consola la lista de puertos seriales disponibles (para identificar cuál usar)
  printArray(Serial.list());
  // Alternativa automática (comentada): abrir el primer puerto disponible a 9600 baudios
  // myPort = new Serial(this, Serial.list()[0], 9600);
  // Abrir un puerto específico (ejemplo para macOS). Ajusta según el puerto real en tu sistema.
  myPort = new Serial(this, "/dev/cu.usbmodem1101", 9600);
  // Nota: si no funciona el puerto, revisa la salida de printArray(Serial.list()) y usa el nombre correcto.
}

// --- Bucle principal ---
// draw() se ejecuta continuamente (aprox. 60 veces por segundo)
void draw() {
  // Pinta el fondo de negro en cada frame
  background(0);
  // Llama a la función que lee datos desde el puerto serial (actualiza potValue)
  readSerial();
  
  // Si no hay imágenes o sólo hay una, no hacemos nada (necesitamos al menos 2 para interpolar)
  if (imgs == null || imgs.length < 2) return;
  
  // Mapear el valor del potenciómetro (0..1023) al rango de índices entre 0 y imgs.length-1
  // Esto permite moverse a lo largo de la secuencia de imágenes
  float mixValue = map(potValue, 0, 1023, 0, imgs.length - 1);
  
  // Calcular el promedio/interpolación entre las dos imágenes vecinas según mixValue
  avgImagesWeighted(mixValue);
  
  // Mostrar la imagen promedio resultante en la pantalla, en la posición (0,0)
  image(avgImg, 0, 0);
  
  // Mostrar texto con el valor actual del potenciómetro en la esquina inferior izquierda
  fill(255); // color blanco para el texto
  text("Valor pot: " + nf(potValue, 1, 0), 10, height - 10); // nf para formatear el número
}

// --- Función que calcula el promedio ponderado entre imágenes ---
// mix es un valor flotante que indica la posición entre imágenes (ej. 2.3 -> entre img2 e img3)
void avgImagesWeighted(float mix) {
  // Accede al arreglo de píxeles de avgImg para poder modificarlos directamente
  avgImg.loadPixels();
  
  // Asegura que mix esté dentro del rango válido [0, imgs.length - 1]
  mix = constrain(mix, 0, imgs.length - 1);
  
  // i1 es el índice de la imagen "inferior" (por ejemplo 2 en 2.3)
  int i1 = floor(mix);
  // i2 es la imagen siguiente (i1 + 1), pero sin pasarse del último índice
  int i2 = min(i1 + 1, imgs.length - 1);
  // t es la fracción entre i1 e i2 (por ejemplo, 0.3 si mix es 2.3)
  float t = mix - i1;
  
  // Cargar los píxeles de las dos imágenes que vamos a mezclar
  imgs[i1].loadPixels();
  imgs[i2].loadPixels();
  
  // Recorre todos los píxeles de la imagen objetivo
  for (int i = 0; i < avgImg.pixels.length; i++) {
    // Coge el color del píxel i de la imagen i1
    color c1 = imgs[i1].pixels[i];
    // Coge el color del píxel i de la imagen i2
    color c2 = imgs[i2].pixels[i];
    
    // Interpola por separado cada componente de color (rojo, verde, azul)
    // red(c1) obtiene la componente roja del color c1
    float r = lerp(red(c1), red(c2), t);
    // green(c1) obtiene la componente verde del color c1
    float g = lerp(green(c1), green(c2), t);
    // blue(c1) obtiene la componente azul del color c1
    float b = lerp(blue(c1), blue(c2), t);
    
    // Crea un nuevo color a partir de las componentes interpoladas y lo asigna al píxel i
    avgImg.pixels[i] = color(r, g, b);
  }
  
  // Aplica los cambios realizados en el arreglo de píxeles a la imagen avgImg
  avgImg.updatePixels();
}

// --- Leer valor del potenciómetro desde Arduino ---
// Lee datos desde el puerto serial hasta encontrar saltos de línea y los convierte a número
void readSerial() {
  // Mientras el puerto exista y tenga bytes disponibles para leer...
  while (myPort != null && myPort.available() > 0) {
    // Lee una línea completa hasta '\n' (salto de línea)
    String val = myPort.readStringUntil('\n');
    if (val != null) {
      // Elimina espacios y caracteres de control al inicio/final
      val = trim(val);
      // Si la cadena no está vacía, la convierte a float y la asigna a potValue
      if (val.length() > 0) {
        potValue = float(val);
      }
    }
  }
}

// --- Cargar todas las imágenes desde una carpeta ---
// Devuelve un arreglo PImage[] con todas las imágenes JPG/PNG encontradas en data/folderName
PImage[] loadImagesFromFolder(String folderName) {
  // Construye la ruta absoluta a la carpeta dentro de la carpeta data del sketch
  String path = sketchPath("data/" + folderName);
  // Crea un objeto File apuntando a esa carpeta
  File folder = new File(path);
  // Lista todos los archivos dentro de la carpeta (puede devolver null si no existe)
  File[] files = folder.listFiles();
  
  // Si files es null, la carpeta no existe o no tiene permisos -> avisar y devolver null
  if (files == null) {
    println("Carpeta no encontrada: " + path);
    return null;
  }
  
  // Crea una lista dinámica para almacenar las PImage cargadas
  ArrayList<PImage> loaded = new ArrayList<PImage>();
  // Recorre cada archivo encontrado en la carpeta
  for (File f : files) {
    // Obtiene el nombre del archivo y lo convierte a minúsculas para comparar extensiones
    String fname = f.getName().toLowerCase();
    // Si termina en .jpg o .png, lo cargamos
    if (fname.endsWith(".jpg") || fname.endsWith(".png")) {
      // loadImage busca en data/folderName el archivo y devuelve un PImage
      PImage img = loadImage(folderName + "/" + f.getName());
      // Si la imagen se cargó correctamente, la agregamos a la lista
      if (img != null) loaded.add(img);
    }
  }
  
  // Convierte la ArrayList a un arreglo PImage[] y lo retorna
  return loaded.toArray(new PImage[loaded.size()]);
}
```
