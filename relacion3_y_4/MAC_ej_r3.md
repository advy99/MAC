EJ10:

Vemos como este programa con variables dada una entrada en X la copia en Y


Como hemos visto en teoria, el programa post-turing almacenará las variables de la siguiente forma:

#X1#...#XM#Z1#...#Zk#Y#


#01001##: #X1001#0####: #XY001#01####
 ^               ^              ^

Luego la siguiente máquina post-turing es equivalente:


					IF 0 GOTO INI_COPIA0
					IF 1 GOTO INI_COPIA1
					HALT

[COPIA0]			PRINT x
					RIGHT TO NEXT BLANK
					RIGHT TO NEXT BLANK
					PRINT 0
					LEFT TO NEXT BLANK  // vamos al primer blanco a la izq (el separador)
					LEFT UNTIL XY // vamos hasta la primera X o Y
					RIGHT // nos posicionamos en el primer simbolo
					IF 0 GOTO COPIA0 // si es 0
					IF 1 GOTO COPIA1 // si es 1
					ACABAR // si acaba la cadena, paramos

[COPIA1]			PRINT y
					RIGHT TO NEXT BLANK
					RIGHT TO NEXT BLANK
					PRINT 1
					LEFT TO NEXT BLANK
					LEFT UNTIL XY
					RIGHT
					IF 0 GOTO COPIA0
					IF 1 GOTO COPIA1
					ACABAR

[ACABAR]			LEFT
					IF X GOTO PONER0
					IF Y GOTO PONER1
					HALT

[PONER0]			PRINT 0
					GOTO ACABAR

[PONER1]			PRINT 1
					GOTO ACABAR

MACROS:

LEFT UNTIL XY:

	[A]			IF X GOTO E
					IF Y GOTO E
					LEFT
					GOTO A




EJ11: Esta máquina post-turing elimina los ceros de la cadena, dejando en el mismo sitio los unos.

(q_0, 0) -> (q_0, #, D)
(q_0, 1) -> (q_0, 1, D)
(q_0, #) -> (q_F, #, S)

Siendo q_F un estado final







EJ8: Le damos el valor 0 y b el valor 1 para al usar nuestra función Z para pasar de cadena a entero, de forma que simularemos la suma en binario con los caracteres a y b como si se comportaran como 0 y 1. De esta forma a + b en binario, pasado a naturales tendrán el mismo valor.


[SIN_ACARREO]	IF X1 ENDS a GOTO X1-0
					IF X1 ENDS b GOTO X1-1
					IF X2 ENDS a GOTO 0+0
					IF X2 ENDS b GOTO 1+0
					HALT


[CON_ACARREO]	IF X1 ENDS a GOTO X1-1
					IF X1 ENDS b GOTO AC-X1-1
					IF X2 ENDS a GOTO 1+0
					IF X2 ENDS b GOTO 1+1
					HALT


[X1-0]			X1 <- X1-
					IF X2 ENDS a GOTO 0+0
					IF X2 ENDS b GOTO 1+0
					Y <- a
					GOTO SIN_ACARREO

[X1-1]			X1 <- X1-
					IF X2 ENDS a GOTO 1+0
					IF X2 ENDS b GOTO 1+1
					Y <- b
					GOTO SIN_ACARREO


[AC-X1-1]		X1 <- X1-
					IF X2 ENDS a GOTO 1+1
					IF X2 ENDS b GOTO 1+1+1
					GOTO 1+1


[0+0]				X2 <- X2-
					Y <- aY
					GOTO SIN_ACARREO


[1+0]				X2 <- X2-
					Y <- bY
					GOTO SIN_ACARREO

[1+1]				X2 <- X2-
					Y <- aY
					GOTO CON_ACARREO


[1+1+1]			X2 <- X2-
					Y <- aY
					GOTO ACARREO
















EJ9: Considerar un lenguage Post Turing para programas con varias cintas. Hay un número finito de cintas y en cada momento una de ellas está activa, inicialmente la primera. Hay dos instrucciones UP and DOWN que se mueven a la cinta superior e inferior respectivamente. Demostrar que todo cálculo realizado por un programa Post Turing con varias cintas, puede realizarse con un programa Post Turing con una sola cinta.


- 1 símbolo extra por cada símbolo (para marcar por donde nos hemos quedado en una cinta, por ejemplo A y B para el 0 y 1 respectivamente). Cuando pasemos de una cinta a otra, ejecutaremos una macro.
- 1 símbolo para marcar el fin de una cinta e inicio de otra (por ejemplo la C).
- Cambiaremos el símbolo # por el símbolo #_1. Solo habrá símbolos # antes de la primera cinta y al final de la última.
- Para añadir símbolos al final de una "cinta", desplazaremos toda la cinta uno a la derecha y volveremos a la posición añadida con una macro.
- Símbolo del alfabeto de entrada X, marca fin de todas las cintas.


Macro inicial, al darnos #0101##X -> #0101 #_1 C #_1 B0101#_1 C #_1Z#:

[INI]			IF # GOTO NUEVACINTA
				IF X GOTO FIN
				RIGHT
				GOTO INI


[PRIMER_V]	RIGHT
				IF 0 GOTO PONERA
				IF 1 GOTO PONERB
				IF # GOTO PONER#_1

[PONERA]		PRINT A
				GOTO INI

[PONERB]		PRINT B
				GOTO INI

[PONER#_1]	INSERT#
				PRINT #_1
				GOTO INI

[NUEVACINTA]PRINT #_1
				RIGHT
				INSERT#
				PRINT C
				RIGHT
				INSERT#
				PRINT #_1
				GOTO PRIMER_V

[FIN]			PRINT #
				VOLVER

[VOLVER]		LEFT TO NEXT BLANK
				RIGHT
				GOTO E





Macro INSERT#:   #00010101101#X# :
							^

				IF 0 GOTO PA_1
				IF 1 GOTO PB_1
				IF # GOTO PZ_1
[C]			RIGHT TO NEXT X

				RIGHT
				PRINT X
[I]			LEFT
				LEFT
				IF 0 GOTO MOV0
				IF 1 GOTO MOV1
				IF # GOTO MOV#

				IF A_1 GOTO FIN_A
				IF B_1 GOTO FIN_B
				IF Z_1 GOTO FIN_#


[MOV0]		RIGHT
				PRINT 0
				GOTO I

[MOV1]		RIGHT
				PRINT 1
				GOTO I

[MOV#]		RIGHT
				PRINT #
				GOTO I

[PA_1]		PRINT A_1
				GOTO C

[PB_1]		PRINT B_1
				GOTO C
[PZ_1]		PRINT Z_1
				GOTO C



[FIN_A]		RIGHT
				PRINT 0
				LEFT
				PRINT #
				GOTO E

[FIN_B]		RIGHT
				PRINT 1
				LEFT
				PRINT #
				GOTO E

[FIN_#]		RIGHT
				PRINT #
				LEFT
				PRINT #
				GOTO E





Pasar de una cinta a otra UP (o DOWN):

[INI]			IF 0 GOTO CINTA0
				IF 1 GOTO CINTA1
				[MOVEUP, MOVEDOWN]

[CINTA0] 	PRINT A
				GOTO INI

[CINTA1] 	PRINT B
				GOTO INI



Macro MOVEDOWN (o MOVEUP, cambiando todos los RIGHT por LEFT):

				RIGHT TO NEXT C
[MOVE]		IF A GOTO FIN_A
				IF B GOTO FIN_B
				RIGHT
				GOTO MOVE

[FIN_A]		PRINT 0
				GOTO E

[FIN_B]		PRINT 1
				GOTO E


Macro RIGHT TO NEXT C:

[A]			IF C GOTO E
				RIGHT
				GOTO A
