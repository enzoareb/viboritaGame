.data
 /* Definicion de datos */
mapa: .asciz "+------------------------------------------------+\n|               ****************                 |\n|               *** VIBORITA ***                 |\n|               ****************                 |\n+------------------------------------------------+\n|                                                |\n|                                                |\n|         @***                                   |\n|                                                |\n|                    M                           |\n|                                                |\n|                                                |\n|                                                |\n|                                                |\n+------------------------------------------------+\n| Puntaje:                      Nivel:           |\n+------------------------------------------------+\n"      @ \n enter
longitud = . - mapa
movimiento: .byte ''
gameover: .asciz "                    __GAMEOVER__                    \n"
cls: .asciz "\x1b[H\x1b[2J"
lencls = .-cls
manzana: .byte 'M'
fila: .byte 7
columnas: .byte 10
largovibora: .byte 4
puntaje: .asciz "  "
filapuntaje: .byte 16

.text
/*
r0= direcciones
r5= puntaje
r3=fila
r4=columna
r6=posicionActual
r9=posicionAnterior
r11=cola de la viborita
*/

entradaConsola:
    .fnstart
            push {lr}
            mov r7,#3                       /*ingreso x teclado*/
            mov r0,#0                       /*lectura cadena*/
            mov r2,#4                       /*cantidad de caracteres*/
            ldr r1,=movimiento                  /*guardar*/
            swi 0
            pop {lr}
            bx lr
    .fnend

obtenerPosicionpuntaje:
	.fnstart
		push {lr}
		ldr r0, =filapuntaje
		ldrb r8,[r0]
		mul r12,r4,r8
		add r12,#13
		pop {lr}
		bx lr
	.fnend
	
asignarPuntajeinicial:
	.fnstart
		push {lr}
		bl obtenerPosicionpuntaje
		ldr r0,=mapa
	    add r0,r12
       	mov r5,#4
       	strb r5,[r0]
		pop {lr}
	.fnend

pasarATexto:
	.fnstart	
		push {lr}
		add r5,#0x30
		ldr r12, =puntaje
		strb r3,[r12]
		pop {lr}
	.fnend

obtenerNuevaPosicion:
    .fnstart
        push {lr}
        /*Obtengo las coordenadas (fila,columna) de la posición del mapa a 
        donde tiene que avanzar la viborita*/
        mov r6,#0
        mov r8,#0
        mov r8,#51
	    mul r6,r3,r8
        add r6,r4
        pop {lr}
        bx lr
    .fnend

hayManzana:
    .fnstart
        push {lr}
    	/*Verifica si en la nueva posición hay una ‘M’*/
        ldr r0,=mapa
        add r0,r6
        ldrb r1,[r0]
        cmp r1,#'M'
        beq siHay
        bal finHayManzana
    siHay:
        bl comerManzana
    finHayManzana:
        pop {lr}
        bx lr
    .fnend

comerManzana:	
    .fnstart
        push {lr}
        /*Si en la nueva posición hay una manzana comer 
        (incrementar tamaño de la viborita y puntajes)*/ 
            //aumento puntaje
            add r5,#1
            //incremento tamaño viborita donde alla espacio en blanco
            //izquierda
            ldr r0,=mapa
            mov r10,#0
            sub r10,r11,#1
            add r0,r10
            ldrb r7,[r0]
            cmp r7,#''
            beq aumentar
            //derecha
            ldr r0,=mapa
            mov r10,#0
            add r10,r11,#1
            add r0,r10
            ldrb r7,[r0]
            cmp r7,#' '
            beq aumentar
            //arriba
            ldr r0,=mapa
            mov r10,#0
            sub r10,r11,#51
            add r0,r10
            ldrb r7,[r0]
            cmp r7,#' '
            beq aumentar
            //abajo
            ldr r0,=mapa
            mov r10,#0
            add r10,r11,#51
            add r0,r10
            ldrb r7,[r0]
            cmp r7,#' '
            beq aumentar

    aumentar:
            mov r7,#'*'
            strb r7,[r0]
            mov r11,r10
            //generar nueva manzana
            bl generarManzana
        pop {lr}
        bx lr
    .fnend

hayPared:
    .fnstart
        push {lr}
    	/*Verifica si en la nueva posición hay una pared, ‘-’,‘|’ o el cuerpo de la viborita '*' */
        ldr r0,=mapa
        add r0,r6
        ldrb r1,[r0]
        cmp r1,#'-'
        beq hayPiso
        bal finHayPiso
    hayPiso:
        bl imprimirGameOver
        mov r7,#1
        swi 0
    finHayPiso:
        ldr r0,=mapa
        add r0,r6
        ldrb r1,[r0]
        cmp r1,#'|'
        beq hayLateral
        bal finHayPared
    hayLateral:
        bl imprimirGameOver
        mov r7,#1
        swi 0
    finHayPared:
        ldr r0,=mapa
        add r0,r6
        ldrb r1,[r0]
        cmp r1,#'*'
        beq hayCuerpoViborita
        bal noHaynada
    hayCuerpoViborita:
        bl imprimirGameOver
        mov r7,#1
        swi 0
    noHaynada:
        pop {lr}
        bx lr
    .fnend

imprimirGameOver:
    .fnstart
        push {lr}
    	/*Imprime el mensaje de Game Over*/
        mov r7,#4                       /*salida x pantalla*/
        mov r0,#1                       /*salida cadena*/
        mov r2,#60                     /*cant de caracteres*/
        ldr r1,=gameover
        swi 0
        pop {lr}
        bx lr
    .fnend

avanzarANuevaPosicionViborita:
    .fnstart
            push {lr}
            //vieja posicion de la cabeza
            ldr r0,=mapa
            add r0,r9
            mov r7,#'*'
            strb r7,[r0]       
        
            //nueva posisicon de la cabeza
            ldr r0,=mapa
            add r0,r6
            mov r8,#'@'
            strb r8,[r0]  

            // vieja posicion cola
            ldr r0,=mapa
            add r0,r11
            mov r7,#' '
            strb r7,[r0]

            //nueva posicion cola
            //izquierda
            ldr r0,=mapa
            mov r10,#0
            sub r10,r11,#1
            add r0,r10
            ldrb r7,[r0]
            cmp r7,#'*'
            beq finavanzar
            //derecha
            ldr r0,=mapa
            mov r10,#0
            add r10,r11,#1
            add r0,r10
            ldrb r7,[r0]
            cmp r7,#'*'
            beq finavanzar
            //arriba
            ldr r0,=mapa
            mov r10,#0
            sub r10,r11,#51
            add r0,r10
            ldrb r7,[r0]
            cmp r7,#'*'
            beq finavanzar
            //abajo
            ldr r0,=mapa
            mov r10,#0
            add r10,r11,#51
            add r0,r10
            ldrb r7,[r0]
            cmp r7,#'*'
            beq finavanzar

    finavanzar:
            mov r11,r10
            pop {lr}
            bx lr
    .fnend

generarManzana:	
    .fnstart
        push {lr}
        mul r12,r11,r6
        mov r1,#458
    ciclo:
        sub r12,r12,r1
        cmp r12,r1
        blt sumar
        bal ciclo
    sumar:
        mov r1,#255
        add r12,r1
    continuar:
        ldr r0,=mapa
        add r0,r12
        ldrb r7,[r0]
        cmp r7,#' '
        beq fingenerarManzana
        add r12,#10
        bal continuar
        
    fingenerarManzana:
        ldr r0,=mapa
        add r0,r12
        mov r7,#'M'
        strb r7,[r0] 
        
        pop {lr}
        bx lr
    .fnend

actualizarPuntaje:
    .fnstart
        push {lr}
        /*Actualizar el mapa con los nuevos puntajes, basados en la longitud de la viborita.*/
            ldr r0,=mapa
            add r0,#776
            strb r5,[r0]
        pop {lr}
        bx lr
    .fnend

borrarpantalla:
        .fnstart
            push {lr}
            mov r0,#1
            ldr r1,=cls
            ldr r2,=lencls
            mov r7,#4
            swi 0
            pop {lr}
            bx lr
        .fnend

ingresarDireccion:
	.fnstart
    	push {lr}
	    bl entradaConsola
	    ldr r0,=movimiento
	    ldrb r1,[r0]
	    pop {lr}
	    bx lr
	.fnend
imprimirMapa:
        .fnstart
            push {lr}
            mov r7, #4	                /* Salida por pantalla*/  
    	    mov r0, #1                  /* Indicamos a SWI que será una cadena*/
            ldr r2, =longitud           /* Tamaño de la cadena*/
    	    ldr r1, =mapa               /* Cargamos en r1 la dirección del mensaje*/
 	        swi 0		                /* SWI, Software interrupt*/
            pop {lr}
            bx lr
        .fnend
jugar:
    .fnstart
        push {lr}
        // coordenada inicial cabeza viborita 
        ldr r0, =fila
        ldrb r3,[r0]
        ldr r0, =columnas
        ldrb r4,[r0]
        //posicion inicial cola viborita
        mov r11,#370  
        
        bl asignarPuntajeinicial

         /* while(!gameover)*/
    seguir:
        bl borrarpantalla
        bl imprimirMapa
        bl ingresarDireccion
        bl actualizarPuntaje
        bl obtenerNuevaPosicion
        mov r9,r6 /*guardo la posicion actual de la viborita en r9*/
        //chequeo movimiento del jugador
        cmp r1,#'w'
        beq arriba
        bal finArriba
    arriba:
        sub r3,#1
        bl obtenerNuevaPosicion
        bl hayManzana
        bl hayPared
        bl avanzarANuevaPosicionViborita
        bal seguir
    finArriba:
        cmp r1,#'s'
        beq abajo
        bal finAbajo
    abajo:
        add r3,#1
        bl obtenerNuevaPosicion
        bl hayManzana
        bl hayPared
        bl avanzarANuevaPosicionViborita
        bal seguir
    finAbajo:
        cmp r1,#'a'
        beq izq
        bal finIzq
    izq:
        sub r4,#1
        bl obtenerNuevaPosicion
        bl hayManzana
        bl hayPared
        bl avanzarANuevaPosicionViborita
        bal seguir
    finIzq:
        cmp r1,#'d'
        beq der
        bal finDer
    der:
        add r4,#1
        bl obtenerNuevaPosicion
        bl hayManzana
        bl hayPared
        bl avanzarANuevaPosicionViborita
        bal seguir
    finDer:
        cmp r1,#'q'
        beq finJugar

    finJugar:
        pop {lr}
        bx lr
    .fnend

.global main
main:
        bl jugar

        mov r7,#1
        swi #0
    
