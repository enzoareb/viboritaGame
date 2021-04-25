# viboritaGame

El propósito que nos dan en este trabajo es recrear el juego de la viborita,
también conocida como SNAKE, es un juego clásico de los juegos de consola. La
primera versión apareció en los 70’s y desde entonces se ha reinventado una y
varias veces.
En esta ocasión nos piden la tarea de implementar la viborita en lenguaje
ensamblador ARM. Para este objetivo debemos realizar algunas decisiones de
diseño del juego que nos permitan un versión sencilla y realizable de acuerdo
con nuestro tiempo disponible y a las características propias de la programación
en lenguaje ensamblador.

La @ representara la cabeza de nuestra viborita
Los * representaran el cuerpo de nuestra viborita
La M representara la manzana
El campo donde puede circular la viborita está delimitado por barras verticales | y guiones -
---.
Otra diferencia sustancial con respecto a la viborita original es que nuestra viborita en
ensamblador no sigue de manera continua el movimiento en una dirección sino que hay que
presionar enter para que se deslice cada vez. De todos modos el juego sigue siendo divertido!
Por último, la viborita en nuestra versión no deja un rastro, es decir, que a medida que avanza
la longitud se mantiene
constante, a menos que, la viborita coma una manzana en cuyo caso la longitud pasa a ser
longitud + 1

Una vez que la víbora se comió la manzana se debe crear otra manzana en una posición
aleatoria, que sea válida.
Si se selecciona varias veces la tecla s la viborita avanzará, pero como el mapa es acotado, si
no cambiamos de dirección la viborita chocará y el juego terminará

## Pseudocodigo
```
Main(){
    Jugar() ;
    //luego de jugar, finaliza el programa
    
Método main se encarga de iniciar el juego y finalizarlo
Jugar(){
    //Inicio variables
    Int fila;
    Int colunma;
    Int puntaje;
    Int posColaViborita;
    While(!gameOver)
      ImprimirMapa();
      ingresarDireccion();
      //guardo la posición antes de Actualizarla
      posAnterior=obtenerNuevaPosicion();
      //pedimos que el jugador presione una tecla para moverse
      If(ingresardireccion() == ‘w’){

          Columna--;
          obtenerPosicion();
          haymanzana();
          haypared();
          avanzarNuevaPosicion();
      else If(ingresardireccion() == ‘s’){

          columna++;
          obtenerPosicion();
          haymanzana();
          haypared();
          avanzarNuevaPosicion();
      else If(ingresardireccion() == ‘a’){
          fila--;
          obtenerPosicion();
          haymanzana();
          haypared();
          avanzarNuevaPosicion();
      else If(ingresardireccion() == ‘d’){

          fila++;
          obtenerPosicion();
          haymanzana();
          haypared();
          avanzarNuevaPosicion();

      else If(ingresardireccion() == ‘q’){
      //sale del programa
      //salimos de jugar
```
