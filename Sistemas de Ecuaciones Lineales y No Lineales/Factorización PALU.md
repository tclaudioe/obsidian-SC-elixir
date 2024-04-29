La factorización PALU, al igual que la [[Factorización LU]] de una matriz, consiste en encontrar la siguiente factorización de la matriz $A$, es decir, $$P\,A=L\,U.$$Esto significa ya no solo encontrar las matrices $L$ y $U$, que siguen siendo **triangular inferior** y **triangular superior**, sino que además una matriz $P$. La buena noticia es que la matriz $P$ es una matriz de permutación, es decir, simplemente permuta el orden de las filas de la matriz $A$ y luego se obtiene la [[Factorización LU]] del producto de las $P$ por $A$. En principio es simple de explicar, y luego parece complejo de ejecutar, sin embargo, para nuestra ventaja, la matriz $P$ se puede obtener durante la ejecución del algoritmo de la [[Factorización LU]]. Es decir, no se agregan operaciones elementales y solo se requiere hacer algunas permutaciones de filas antes de aplicar la #EG en la [[Factorización LU]].

## ¿Cuándo es importante la factorización PALU de una matriz?
Considere el siguiente sistema de ecuaciones lineales,$$
\begin{bmatrix}
	10^{-20} & 1\\
	1 & 2
\end{bmatrix}
\begin{bmatrix}
	x_1\\
	x_2
\end{bmatrix}
=
\begin{bmatrix}
	1\\
	4
\end{bmatrix}.$$ Entonces si obtenemos la [[Factorización LU]] de la matriz asociada utilizando #doublePrecision, nos entrega lo siguiente, $$\begin{bmatrix}
			1 & 0\\
			10^{20} & 1
		\end{bmatrix}
		\begin{bmatrix}
			10^{-20} & 1\\
			0 & -10^{20}
		\end{bmatrix}.$$ Para verificar que lo obtenido es _razonable_, uno puedo obtener el producto anterior y el resultado debiera ser la matriz original, sin embargo obtenemos, $$
\begin{bmatrix}
	10^{-20} & 1\\
	1 & 0
\end{bmatrix}.$$ Lo cual claramente no es la matriz inicial, dado que el coeficiente de la segunda fila de la segunda columna ahora es $0$ pero debería ser $2$. Ahora, si hubiéramos resuelto el sistema de ecuaciones anterior con la [[Factorización LU]] obtenida nos hubiera entregado la siguiente aproximación de la solución,$$
\mathbf{x}_a 
=
\begin{bmatrix}
	0\\
	1
\end{bmatrix}.$$ La cual no resuelve el sistema de ecuaciones lineales original.

Por otro lado, si alternamos las filas del sistema de ecuaciones lineales asociado, por medio de la multiplicación por la izquierda de la matriz de permutación $$
P=
\begin{bmatrix}
	0 & 1\\
	1 & 0
\end{bmatrix},$$ es decir,$$
\begin{align}
P \, A \, \mathbf{x} &= P\,\mathbf{b}\\
\begin{bmatrix}
	0 & 1\\
	1 & 0
\end{bmatrix}
\begin{bmatrix}
	10^{-20} & 1\\
	1 & 2
\end{bmatrix}
\begin{bmatrix}
	x_1\\
	x_2
\end{bmatrix}
&=
\begin{bmatrix}
	0 & 1\\
	1 & 0
\end{bmatrix}
\begin{bmatrix}
	1\\
	4
\end{bmatrix}\\
\begin{bmatrix}
	1 & 2\\
	10^{-20} & 1
\end{bmatrix}
\begin{bmatrix}
	x_1\\
	x_2
\end{bmatrix}
&=
\begin{bmatrix}
	4\\
	1
\end{bmatrix}
\end{align}$$
Notar que en este caso la permutación de filas tiene que ser aplicada tanto al lado izquierdo de la ecuación como al lado derecho del sistema de ecuaciones lineales. Entonces, ahora, si aplicamos la [[Factorización LU]] a la matriz de este nuevo sistema de ecuaciones lineales, obtenemos la siguiente factorización, $$\begin{bmatrix}
    		1 & 0\\
    		&\\
    		10^{-20} & 1
		\end{bmatrix}
		\,
		\begin{bmatrix}
			1 & 2\\
			&\\
			0 & 1
		\end{bmatrix}.$$ Con la cual obtenemos la siguiente solución numérica del sistema de ecuaciones lineales por medio de utilizar [[Forward Substitution]] y [[Backward Substitution]], $$
\mathbf{x}_a 
=
\begin{bmatrix}
	2\\
	1
\end{bmatrix}.$$
## ¿Qué es el pivote?
En la [[Factorización LU]] se utilizan los coeficiente que van quedando en la diagonal como pivote para hacer cero los coeficiente bajo la diagonal, por ejemplo considere la siguiente matriz,$$
\begin{bmatrix}
	a_{1,1} & a_{1,2} & a_{1,3} & \dots & a_{1,n} \\
	a_{2,1} & a_{2,2} & a_{2,3} & \ddots & a_{2,n} \\
	a_{3,1} & a_{3,2} & a_{3,3} & \ddots & a_{3,n} \\
	\vdots & & \ddots  & \ddots & \vdots \\
	a_{n,1} & a_{n,2} & \dots & \dots & a_{n,n} 
\end{bmatrix}$$En el caso de la [[Factorización LU]], según lo indicado, se utilizaría el coeficiente $a_{1,1}$ para hacer $0$ desde el coeficiente $a_{2,1}$ hasta el coeficiente $a_{n,1}$. Sin embargo en la factorización PALU se elige el coeficiente de mayor magnitud (es decir en valor absoluto) de la columna en cuestión  desde la diagonal hasta la última fila. Por ejemplo, si en el caso anterior se sabe que $|a_{3,1}|>|a_{k,1}|$ para $k\in\{1,2,4,\dots,n\}$, entonces PALU permutaría las fila y quedaría la siguiente matriz,$$
\begin{bmatrix}
	a_{3,1} & a_{3,2} & a_{3,3} & \dots & a_{3,n} \\
	a_{2,1} & a_{2,2} & a_{2,3} & \ddots & a_{2,n} \\
	a_{1,1} & a_{1,2} & a_{1,3} & \ddots & a_{1,n} \\
	\vdots & & \ddots  & \ddots & \vdots \\
	a_{n,1} & a_{n,2} & \dots & \dots & a_{n,n} 
\end{bmatrix}$$ Y ahora se puede ejecutar la #EG sobre la primera columna de la matriz. La cual generaría lo siguiente,$$
\begin{bmatrix}
	a_{3,1} & a_{3,2} & a_{3,3} & \dots & a_{3,n} \\
	0 & \widetilde{a}_{2,2} & \widetilde{a}_{2,3} & \ddots & \widetilde{a}_{2,n} \\
	0 & \widetilde{a}_{1,2} & \widetilde{a}_{1,3} & \ddots & \widetilde{a}_{1,n} \\
	\vdots & & \ddots  & \ddots & \vdots \\
	0 & \widetilde{a}_{n,2} & \dots & \dots & \widetilde{a}_{n,n} 
\end{bmatrix}$$donde los coeficientes con tilde ($\widetilde{\phantom{a}}$) indica que se modificaron los valores originales por medio de las operaciones filas requeridas para hacer $0$ los coeficientes de la primera columna. Ahora, se necesita elegir el nuevo pivote, notar que en este caso solo se debe buscar en la **segunda** columna desde la **segunda** fila, ya que si permutamos la primera fila, perderemos el patrón que estamos construyendo. Y así sucesivamente!
## Ventajas
1. Al igual que la [[Factorización LU]], 
	1. La cantidad de operaciones elementales requeridas para construir la factorización PALU es $\sim \dfrac{2}{3}n^3$.
	2. Permite reutiliza la factorización PALU para resolver numéricamente secuencia de sistema de ecuaciones lineales de la forma $A\,\mathbf{x}_k=\mathbf{b}_k$.
2. Adicionalmente a las mismas ventajas de la [[Factorización LU]], la factorización PALU permite manejar los casos cuando uno encuentra $0$ o coeficientes muy pequeños en le _pivote_. Esto lo realiza por medio de la permutación de filas antes de ejecutar la #EG.
## Desventajas
1. Se requiere esperar la ejecución completa de las $\sim \dfrac{2}{3}n^3$ operaciones elementales para obtener la solución $\mathbf{x}$.
2. Si bien almacenar las permutaciones requiere memoria adicional, es solo orden $n$, y las matrices $L$ y $U$ es orden $n^2$, por lo que es mínimo.

#OK
#Tema_4