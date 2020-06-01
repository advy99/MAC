
# Ejercicio 1: Contruir un programa con variables numéricas que calcule $f(x_1, x_2) = x_1 + x_2$ y otro que calcule $f(x_1, x_2) = x_1*x_2$:


## Suma dos entradas X1 y X2

```
[INICIOX1]		IF X1 != 0 GOTO SUMAX1
[INICIOX2]		IF X2 != 0 GOTO SUMAX2
				HALT

[SUMAX1]		Y <- Y+1
				X1 <- X1-1
				GOTO INICIOX1

[SUMAX2]		Y <- Y+1
				X2 <- X2-1
				GOTO INICIOX2
```



## Multiplicar $x_1$ por $x_2$: sumar $x_1$ tantas veces como nos diga la variable $x_2$

```
				IF X1 != 0 GOTO INICIO
				HALT
[INICIO]		Z <- X2
				X2 <- X1
[COMPROBAR]		IF Z != 0 GOTO SUMA1MAS
				HALT

[SUMA1MAS]		X1 <- X1+X2
				Z <- Z-1
				GOTO COMPROBAR
```
