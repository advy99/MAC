% Entrega Relación 3 y 4
% Antonio David Villegas Yeguas

# Ejercicio 1

Diseñas un programa con variables que dadas dos cadenas u y w compuestas por ceros y unos, calcule $u^{|w|}$. Diseña un programa Post-Turing que calcule la misma función, suponed que la entrada es ucv donde c es un símbolo de 0 y 1.


Dada la entrada u y w en las variables X1 y X2 respectivamente:

```
				// si le quedan simbolos a X2 (en total lo ejecutaremos |X2| veces)
[INI]			IF X2 ENDS 1 GOTO COPIAR		
				IF X2 ENDS 0 GOTO COPIAR
				// si no le quedan simbolos a X2 paro de copiar
				HALT									

				// inicio del bucle, copiamos X1 en Z
[COPIAR]	 	Z <- X1
				// si quedan simbolos en Z, los copiamos en Y
[BUCLE]			IF Z STARTS 0 GOTO C0
				IF Z STARTS 1 GOTO C1
				// si hemos acabado de copiar X1, le quitamos un simbolo a X2
				X2 <- X2-
				// y volvemos al inicio			
				GOTO INI								

				// Eliminamos el primer símbolo de Z
[C0]			Z <- Z- 						
				// Lo insertamos en Y al final		
				Y <- 0Y
				// volvemos al bucle para copiar								
				GOTO BUCLE							

				// Eliminamos el primer símbolo de Z
[C1]			Z <- Z-		
				// Lo insertamos en Y al final
				Y <- 1Y 								
				GOTO BUCLE
```

Como resumen de este ejercicio, lo que haremos será mirar si X2 (w) tiene algún símbolo, si tiene algún símbolo, pasamos a concatenar X1 (u) en la salida y quitar un símbolo de X2 (w) y volvemos a empezar, de forma que si X2 (w) tiene $|w|$ símbolos, la salida contendrá $u^{|w|}$.


Para hacer la segunda parte del ejercicio simplemente adaptamos las macros como vimos en teoría. Al adaptar el programa con variables a máquina Post-Turing tenemos que m  = 2 ya que tenemos dos entradas, k = 1 ya que usaremos una única variable auxiliar, por lo que l = 2 + 1 + 1 = 4. Para que funcione como hemos visto en teoría la entrada tiene la forma X1#X2, sin embargo en este caso tiene la forma $ucw$ ya que el símbolo en blanco no forma parte del alfabeto de entrada, luego lo primero que haremos será cambiar esa c por un espacio en blanco, volver al inicio y trabajar como hemos visto en teoría:

```
				// buscamos la c
[A]				IF c GOTO B
				RIGHT
				GOTO A

				// ponemos un # y volvemos al principio, continuando por INI
[B]				PRINT #
				LEFT TO NEXT BLANK
				RIGHT

				// es X2, luego tenemos que movernos dos veces
[INI]			RIGHT TO NEXT BLANK 2			
				LEFT
				// como no nos importa si es 0 o 1, añadimos el segundo IF
				IF 1 GOTO C							
				IF 0 GOTO C
				// básicamente solo va a ir a D si es símbolo en blanco						
				GOTO D
[C]				LEFT TO NEXT BLANK 2
				GOTO COPIAR
				// nos vamos al principio y paramos
[D]				RIGHT
				LEFT TO NEXT BLANK 2

				HALT


				// vamos a copiar la primera variable, no tenemos que movernos
[COPIAR]		RIGHT									
				IF 0 GOTO COPIAZ0
				IF 1 GOTO COPIAZ1
				// si es blanco seguimos adelante para copiar u en Y una vez más										
				// como es el principio de Z, no el final, nos movemos dos, no tres
[BUCLE]			RIGHT TO NEXT BLANK 2 			
				RIGHT
				// copiamos el caracter que encontremos
				IF 0 GOTO C0
				IF 1 GOTO C1
				// si no quedan caracteres acabamos
				GOTO D1

				// nos vamos a la variable de la que queremos copiar
[C0]			LEFT TO NEXT BLANK 2
				GOTO COPIA0
[C1]			LEFT TO NEXT BLANK 2
				GOTO COPIA1

				// cuando ya esta todo copiado, volvemos al inicio
[D1]			RIGHT
				LEFT TO NEXT BLANK 2
				GOTO INI

				// vamos al final de la variable auxiliar, borramos al final de esta
[COPIA0]		RIGHT TO NEXT BLANK 3 			
				LEFT
				IF # GOTO C
				MOVE BLOCK RIGHT 3
				RIGHT
				GOTO BUCLE
				LEFT TO NEXT BLANK 2

				// Ponemos un 0 al principio, tal como se traduce en teoría
				RIGHT TO NEXT BLANK 4			
				MOVE BLOCK RIGHT 4-3+1
				RIGHT
				PRINT 0
				LEFT TO NEXT BLANK 3
				GOTO BUCLE

				// vamos al final de la variable auxiliar, borramos al final de esta
[COPIA1]		RIGHT TO NEXT BLANK 3 			
				LEFT
				IF # GOTO C
				MOVE BLOCK RIGHT 3
				RIGHT
				GOTO BUCLE
				LEFT TO NEXT BLANK 2

				// Ponemos un 1 al principio, tal como se traduce en teoría
				RIGHT TO NEXT BLANK 4			
				MOVE BLOCK RIGHT 4-3+1
				RIGHT
				PRINT 1
				LEFT TO NEXT BLANK 3
				GOTO BUCLE





[COPIAZ0]		PRINT A
				// nos vamos al blanco donde acaba la Z
				RIGHT TO NEXT BLANK 3
				// hacemos hueco para un símbolo más
				MOVE BLOCK RIGHT					
				PRINT 0
				// volvemos a donde estabamos copiando
				LEFT TO NEXT A						
				// hemos vuelto, cambiamos la A por 0
				PRINT 0							
				// seguimos copiando
				GOTO COPIAR							

[COPIAZ1]		PRINT B
				// nos vamos al blanco donde acaba la Z
				RIGHT TO NEXT BLANK 3		
				// hacemos hueco para un símbolo más
				MOVE BLOCK RIGHT					
				PRINT 1
				// volvemos a donde estabamos copiando
				LEFT TO NEXT A						
				// hemos vuelto, cambiamos la B por 1
				PRINT 1							
				// seguimos copiando
				GOTO COPIAR							
```

Esta máquina Post-Turing se trata de una traducción literal de como vemos en teoría a excepción de la traducción de la macro `Z <- X1`, que no la he encontrado en las diapositivas ni en los vídeos de teoría, por lo que la he implementado yo.




\newpage

# Ejercicio 2

Diseñar un programa con variables numéricas que calcule el máximo común divisor de dos enteros.

Implementaremos una variable que calcule el resto dado dos números. El programa final quedaría de la siguiente forma:

Dos enteros como entrada en X1 y X2
```
					// + 1 para anular el -1 que haremos en la siguiente instrucción
					Y <- X1 + 1						

					// mientras algún resto sea distinto de 0
					// para alguna de las dos variables, seguimos restando
[BUCLE]				Y <- Y - 1						
					Z <- X1 % Y
					IF Z != 0 GOTO BUCLE
					Z <- X2 % Y
					IF Z != 0 GOTO BUCLE
					HALT

```

Operador % usado, entrada en dos variables X1 y X2, salida en Y (X1 % X2 = Y):

```
[INI]				IF X1 != X2 GOTO DISTINTOS
					// si son iguales el resto es 0
					HALT								

					// si son distintos, metemos X1 en Y y X2 en Z
[DISTINTOS]			Y <- X1
					Z <- X2

					// A la variable Y le restamos Z
[BUCLE]				Y <- Y - 1
					Z <- Z - 1
					IF Z != 0 GOTO BUBLE			//En Y (X1) restamos Z (X2)

					// cuando acabemos de restar, si Y vale 0 es que  X1 < X2,
					// sabemos que no son iguales porque si no habría parado en INI
					// en caso contrario, (Y != 0), quiere decir que X1 era mayor que X2
					IF Y != 0 GOTO ESMAYOR	// Si  Y != 0, X1 > X2, tenemos que seguir
					Y <- X1					// Si Y = 0, X1 < X2, luego X1 % X2 = X1
					HALT

					// si era mayor, nos quedamos en X1 con el nuevo valor de Y
[ESMAYOR]			X1 <- Y
					GOTO INI		 // volvemos al inicio
```


Algunos ejemplo de como funcionaría esta macro:

- Entrada 4 y 4: Es trivial, en INI comprobamos que son iguales y devolvemos resto 0

- Entrada 7 y 3:

```
	X1: 7
	X2: 3
```

Como son distintos, introducimos en Y el valor 7 y en Z el valor 3

```
	X1: 7
	X2: 3
	Y: 7
	Z: 3
```
A la variable Y le restamos Z
```
	X1: 7
	X2: 3
	Y: 4
	Z: 0
```
Como Y es distinto de 0, hacemos que X1 valga Y y volvemos al principio
```
	X1: 4
	X3: 3
	Y: 4
	Z: 0
```
Repetimos el proceso y al volver al inicio tenemos lo siguiente:
```
	X1: 1
	X3: 3
	Y: 1
	Z: 0
```
Al repetir el proceso, antes de ir a INI, llegamos al siguiente estado (da igual que a 1 le restemos tres, como vimos en teoría se sigue quedando en 0 si seguimos restando):
```
	X1: 1
	X3: 3
	Y: 0
	Z: 0
```
Como Y ha llegado a 0, el resto es lo que queda en X1, así que hacemos que almacenamos X1 en Y y paramos, el resto es 1.

- Entrada 6 y 3: Tras la primera iteración, almacenaríamos en X1 el valor de Y, que sería $6 - 3 = 3$, y al volver al inicio X1 y X2 tendrían el mismo valor por lo que devolveríamos 0.


Macro IF X1 != X2 GOTO L usada, E es la instrucción justo debajo desde donde se llama a la macro. Las entradas son copias, es decir, que si llamo a la macro con X1 y X2, al final mantienen su valor:

```
					Z1 <- X1
					Z2 <- X2

					// comprobamos Z1
[INI]				IF Z1 != 0 GOTO A
					// comprobamos Z2:
					// si llegamos aqui, sabemos que Z1 = 0, luego si Z2 != 0
					// son distintos, saltamos a L
					IF Z2 != 0 GOTO L
					// Z1 y Z2 valen 0, son iguales por lo que no saltamos
					// sigue la ejecución por donde iba
					GOTO E

[A]					IF Z2 != 0 GOTO RESTA
					// si Z2 vale 0 y Z1 no, son distintos saltamos a L
					GOTO L

					// si ambas son distintas de 0, restamos 1 a ambas
[RESTA]				Z1 <- Z1 - 1
					Z2 <- Z2 - 1
					GOTO INI

```
