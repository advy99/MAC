% Relación 3
% Ejercicio 9

Considerar un lenguage Post Turing para programas con varias cintas. Hay un número finito de cintas y en cada momento una de ellas está activa, inicialmente la primera. Hay dos instrucciones UP and DOWN que se mueven a la cinta superior e inferior respectivamente. Demostrar que todo cálculo realizado por un programa Post Turing con varias cintas, puede realizarse con un programa Post Turing con una sola cinta.


- 1 símbolo extra por cada símbolo (para marcar por donde nos hemos quedado en una cinta, por ejemplo A y B para el 0 y 1 respectivamente). Cuando pasemos de una cinta a otra, ejecutaremos una macro.
- 1 símbolo para marcar el fin de una cinta e inicio de otra (por ejemplo la C).
- Para añadir símbolos al final de una "cinta", desplazaremos toda la cinta uno a la derecha y volveremos a la posición añadida con una macro.
- Símbolo del alfabeto de entrada X, marca inicio y fin global (inicio de la primera cinta y final de la última).

Los caracteres c y X formarán parte del alfabeto de entrada, luego una entrada tendrá la siguiente forma: ##X100101cccX###

Siendo 100101 la entrada en la primera cinta, las tres siguientes c nos marcan que hay otras tres cintas vacías. Al inicio tenemos que marcar por donde vamos en cada una de las cintas, que al principio, al estar vacías, insertaremos un símbolo en blanco y lo marcaremos con su símbolo auxiliar (una Z).

```
[INI]			IF c GOTO NUEVACINTA
				IF X GOTO FIN
				RIGHT
				GOTO INI

[NUEVACINTA]	RIGHT
				INSERT#
				PRINT Z
				GOTO INI

[FIN]			LEFT TO NEXT X
				RIGHT
				GOTO E

```



Macro INSERT#, básicamente marcamos la posición donde llamamos a la macro, nos vamos al final (marcado con una X), y desplazamos todo una posición a la derecha hasta que movamos a la derecha el símbolo marcado, dejando un símbolo en blanco en la posición insertada:

```
				// marcamos el primer símbolo
				IF 0 GOTO PA_1
				IF 1 GOTO PB_1
				IF # GOTO PZ_1
[C]				RIGHT TO NEXT X

				// movemos la X uno a la derecha, nos movemos dos a la
				// izquierda, y movemos ese símbolo uno a la derecha
				// esto lo repetimos hasta que encontremos el símbolo
				// marcado, que indica que ya hemos desplazado todo lo que queríamos
				RIGHT
				PRINT X
[I]				LEFT
				LEFT
				IF 0 GOTO MOV0
				IF 1 GOTO MOV1
				IF # GOTO MOV#

				IF A_1 GOTO FIN_A
				IF B_1 GOTO FIN_B
				IF Z_1 GOTO FIN_#


[MOV0]			RIGHT
				PRINT 0
				GOTO I

[MOV1]			RIGHT
				PRINT 1
				GOTO I

[MOV#]			RIGHT
				PRINT #
				GOTO I

[PA_1]			PRINT A_1
				GOTO C

[PB_1]			PRINT B_1
				GOTO C
[PZ_1]			PRINT Z_1
				GOTO C


				// movemos el símbolo marcado, y escribimos
				// un símbolo en blanco en su posición antigua,
				// finalizando la macro
[FIN_A]			RIGHT
				PRINT 0
				LEFT
				PRINT #
				GOTO E

[FIN_B]			RIGHT
				PRINT 1
				LEFT
				PRINT #
				GOTO E

[FIN_#]			RIGHT
				PRINT #
				LEFT
				PRINT #
				GOTO E
```




Pasar de una cinta a otra UP (o DOWN), al ser la misma macro pero cambiando la llamada por MOVEUP por MOVEDOWN lo he simplificado, si estamos en la última cinta y hacemos DOWN, nos quedamos en la misma posición al no haber cinta debajo, y lo mismo si estamos en la primera cinta y hacemos UP:

```
[INI]			IF 0 GOTO CINTA0
				IF 1 GOTO CINTA1
				[MOVEUP, MOVEDOWN]

[CINTA0]	 	PRINT A
				GOTO INI

[CINTA1]	 	PRINT B
				GOTO INI
```


Macro MOVEDOWN (o MOVEUP, cambiando todos los RIGHT por LEFT):

```
				RIGHT TO NEXT C
				// si al movernos estamos en una X quiere decir que estábamos
				// en la última cinta, necesitamos volver donde lo dejamos
				IF X GOTO VOLVER
[MOVE]			IF A GOTO FIN_A
				IF B GOTO FIN_B
				RIGHT
				GOTO MOVE

[FIN_A]			PRINT 0
				GOTO E

[FIN_B]			PRINT 1
				GOTO E

[FIN_Z]			PRINT #
				GOTO E

				// si no hay cinta debajo
				// volvemos a la posición donde nos habíamos intentado mover
[VOLVER]			LEFT
				IF A GOTO FIN_A
				IF B GOTO FIN_B
				IF Z GOTO FIN_Z
				GOTO LEFT
```

Macro RIGHT TO NEXT C, también paramos si encontramos una X ya que eso quiere decir que no hay cinta debajo:

```
[A]				IF C GOTO E
				IF X GOTO E
				RIGHT
				GOTO A
```

Para hacer la macro MOVEDOWN sería igual, solo que cambiando los RIGHT por LEFT y viceversa.
