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

Juego() inicializa las variables fila y columna con la posición de la cabeza (@) de la viborita,
inicia el puntaje en 4 y la posición donde termina el cuerpo de la viborita
Luego por medio de un ciclo analiza los datos ingresados por teclado y ejecuta la subrutina
correspondiente
 Si es ‘w’ resta 1 a la columna
 Si es ‘s’ suma 1 a la columna
 Si es ‘a’ resta 1 a la fila
 Si es ‘d’ suma 1 a la fila
 Si es ‘q’ sale del ciclo y finaliza el método.
Dentro de cada condición ejecuta las rutinas correspondientes

```
obtenerPosicion(){
    posiciónActual= (fila*cantDeColumnasTotales)+columna;
    return posiciónActual;
}
```

Toma el valor de la variable fila y lo multiplica por la cantidad de columnas totales del
“entorno” luego le suma la variable columna y asi se obtiene la posición donde se quiere
avanzar

```
haymanzana(){
    if posiciónActual==’M’
    comerManzana();
}
```

Analiza si en la posición hay una manzana(’M’) llama ala función comerManzana()

```
comerManzana(){
    manzanaActual=null;
    puntaje++;
    largoviborita++;
    generarManzana();
}
```

La manzana actual la desaparece, aumenta el puntaje y el largo de la viborita en 1, genera
una nueva manzana en una posición random valida

```
hayPared(){
    if (posición==’|’ || posición==’-’ || posición==’*’)
        imprimirgameover()
        //findelJuego
}
```

Analiza si en la posición actual hay una pared(‘|’) , un piso/techo(‘-’) o esta el cuerpo de la
viborita (‘*’), en ese caso imprime mensaje gameOver y finaliza el juego

```
avanzarNuevaPosicion(){
    posicionActual=’@’;
    posicionAnterior=’*’;
    posicionAnteriorColaViborita=’ ’;
    if (posicionAnteriorColaViborita.izquierda==’*’)
        posicionColaViborita= posicionAnteriorColaViborita-1;
    if (posicionAnteriorColaViborita.derecha==’*’)
        posicionColaViborita= posicionAnteriorColaViborita+1;
    if (posicionAnteriorColaViborita.arriba==’*’)
        posicionColaViborita= posicionAnteriorColaViborita-51;
    if (posicionAnteriorColaViborita.abajo==’*’)
        posicionColaViborita= posicionAnteriorColaViborita+51;
}
```

A la posición a ocupar se le agrega un ’@’ y a la posición donde estaba la cabeza se la cambia
por un ‘*’ que simula el cuerpo de la viborita.
Luego a la posición de la cola de la viborita se la cambio por un espacio en blanco y se busca
en los alrededores la posición que tenga un asterisco. La misma se asigna como la nueva
posición de la Cola de la Viborita.

```
generarManzana(){
    int numero = posicionActual*posicionColaViborita;
    int resto = numero % 458;
    int nroRandom = resto + 255;
    while (posicion(nroRandom)!=’ ’)
        nroRandom+10;
        posManzana = posicion(nroRandom);
}
```

Para generar una manzana en una posición random valida, tomamos la posicion de la cola
de la viborita y la multiplicamos por la posición de la manzana actual. Luego hacemos las
restas sucesivas para obtener el resto de esta multiplicación con el numero 458 (este numero
representa la cantidad de posiciones que tiene el recuadro del juego). Luego al resto le
sumamos 255 (este numero representa la primera posicion del recuadro del juego donde se
puede mover la viborita). Y evaluamos si en la posicion del numero obtenido hay objetos, si
es asi sumamos 10 unidades al numero obtenido hasta encontrar una posicion valida.
Finalmente colocamos la manzana en el lugar adecuado.
