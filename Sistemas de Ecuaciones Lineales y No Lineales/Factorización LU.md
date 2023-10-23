La #factorización LU de la matriz $A$ resuelve la primera desventaja presentada de la #EG, es decir, permite en principio resolver sistemas de ecuaciones lineales que comparten la matriz de forma más eficiente.
## ¿Qué es la factorización LU?
La #factorización LU de una matriz $A$ consiste en escribir la matriz $A$ como el producto de las matriz triangular inferior $L$ y la matriz triangular superior $U$ (esta es la misma $U$ ya obtenida por la #EG). Se omite presentar nuevamente la matriz $U$ porque ya se presentó antes, pero sí es importante presentar la matriz $L$, $$L=\begin{bmatrix}
        	1 & 0 & \cdots & \cdots & 0 \\
        	l_{2,1} & 1 & \ddots & \ddots & 0 \\
        	l_{3,1} & l_{3,2} & 1 & \ddots & 0 \\
        	\vdots & & \ddots & \ddots &\vdots  \\
        	l_{n,1} & \cdots & \cdots & l_{n,n-1} & 1
        \end{bmatrix},$$ la cual en conjunto con la matriz $U$ producen la matriz $A$ de la siguiente forma, $$A=L\,U,$$o en su forma explícita,$$
        \begin{bmatrix}
			a_{1,1} & a_{1,2} & a_{1,3} & \cdots & a_{1,n} \\
			a_{2,1} & a_{2,2} & a_{2,3} & \ddots & a_{2,n} \\
			a_{3,1} & a_{3,2} & a_{3,3} & \ddots & a_{3,n} \\
			\vdots & & \ddots  & \ddots & \vdots \\
			a_{n,1} & a_{n,2} & \cdots & \cdots & a_{n,n} 
		\end{bmatrix}
		=
		\begin{bmatrix}
        	1 & 0 & \cdots & \cdots & 0 \\
        	l_{2,1} & 1 & \ddots & \ddots & 0 \\
        	l_{3,1} & l_{3,2} & 1 & \ddots & 0 \\
        	\vdots & & \ddots & \ddots &\vdots  \\
        	l_{n,1} & \cdots & \cdots & l_{n,n-1} & 1
        \end{bmatrix}
        \begin{bmatrix}
			u_{1,1} & u_{1,2} & u_{1,3} & \cdots & u_{1,n} \\
			0 & u_{2,2} & u_{2,3} & \ddots & u_{2,n} \\
			0 & 0 & u_{3,3} & \ddots & u_{3,n} \\
			\vdots & & \ddots  & \ddots & \vdots \\
			0 & 0 & \cdots & 0 & u_{n,n} 
		\end{bmatrix}$$
## ¿Cómo se usa la factorización LU para resolver un sistema de ecuaciones lineales?
1. Problema inicial: $A\,\mathbf{x}=\mathbf{b}$.
2. Construir factorización LU de $A$: $A=L\,U$.
3. Reemplazar la factorización LU en el problema inicial dado que $A=L\,U$ es una identidad:$$L\,U\,\mathbf{x}=\mathbf{b}.$$
4. Resolver el sistema de ecuaciones lineales $L\,\mathbf{c}=\mathbf{b}$. Esto se obtiene considerando que dado que $\mathbf{x}$ es la incógnita, entonces $U\,\mathbf{x}$ también es desconocido y lo denotamos por $\mathbf{c}$. El sistema de ecuaciones lineales $L\,\mathbf{c}=\mathbf{b}$ donde $L$ es una matriz **triangular inferior**, en su forma explícita, representa lo siguiente,$$\begin{bmatrix}
        	1 & 0 & \cdots & \cdots & 0 \\
        	l_{2,1} & 1 & \ddots & \ddots & 0 \\
        	l_{3,1} & l_{3,2} & 1 & \ddots & 0 \\
        	\vdots & & \ddots & \ddots &\vdots  \\
        	l_{n,1} & \cdots & \cdots & l_{n,n-1} & 1
        \end{bmatrix}
        \begin{bmatrix}
        	c_1 \\
        	c_2 \\
        	c_3 \\
        	\vdots \\
        	c_{n-1} \\
        	c_n
        \end{bmatrix}
        =
        \begin{bmatrix}
        	b_1 \\
        	b_2 \\
        	b_3 \\
        	\vdots \\
        	b_{n-1} \\
        	b_n
        \end{bmatrix}.$$Rápidamente notamos que podemos obtener $c_1$ de la primera ecuación del sistema, es decir $1\,c_1=b_1$. Luego, para la segunda ecuación, $l_{2,1}\,c_1+1\,c_2=b_2$, podemos despejar $c_2$, y así sucesivamente. En realidad este procedimiento se conoce como [[Forward Substitution]]. El resultado genera los coeficientes del vector $\mathbf{c}$, es decir, genera el vector $\mathbf{c}$.
5. Ahora, como conocemos el vector $\mathbf{c}$, el cambio de variables anterior, es decir $U\,\mathbf{x}=\mathbf{c}$, se convierte en un nuevo sistema de ecuaciones lineales, pero ahora la matriz $U$ es **triangular superior**. Es decir tenemos el siguiente sistema de ecuaciones lineales,$$\begin{bmatrix}
			u_{1,1} & u_{1,2} & u_{1,3} & \cdots & u_{1,n} \\
			0 & u_{2,2} & u_{2,3} & \ddots & u_{2,n} \\
			0 & 0 & u_{3,3} & \ddots & u_{3,n} \\
			\vdots & & \ddots  & \ddots & \vdots \\
			0 & 0 & \cdots & 0 & u_{n,n} 
		\end{bmatrix}
        \begin{bmatrix}
        	x_1\\
        	x_2\\
        	x_3\\
        	\vdots\\
        	x_{n-1}\\
        	x_n
        \end{bmatrix}
        =
        \begin{bmatrix}
        	c_1\\
        	c_2\\
        	c_3\\
        	\vdots\\
        	c_{n-1}\\
        	c_n
        \end{bmatrix}.$$El cual, similar al caso anterior, se resuelve incógnita por incógnita, pero desde la última ecuación a la primera ecuación. Este procedimiento se conoce como [[Backward Substitution]].
## ¿Cómo se construye?
Recordando el _tableau_ anterior, $$B=\begin{equation*}
        \left[
			\begin{array}{ c c c c : c }
				a_{1,1} & a_{1,2} & \cdots & a_{1,n} & b_1 \\
				a_{2,1} & a_{2,2} &  & a_{2,n} & b_2 \\
				\vdots & & \ddots & \vdots & \vdots \\
				a_{n,1} & a_{n,2} & \cdots & a_{n,n} & b_n
			\end{array}
		\right]
		=
		\left[
		\begin{array}{ c : c }
		\phantom{.} & \\
	    & \\
	    A & \mathbf{b} \\
	    & \\
		& \phantom{.}
		\end{array}
		\right]
    \end{equation*}$$ Obtenemos los coeficientes de $U$ por medio de la #GE, es decir, aplicando operaciones fila.
```python
for j in range(n-1):
	for i in range(j+1,n):
		mult = B[i,j]/B[j,j]
		B[i,j+1:] = B[i,j+1:] - mult*B[j,j+1:]
```
Por otro lado, los coeficientes de $L$ son los coeficientes $\verb|mult = B[i,j]/B[j,j]|$ obtenidos en cada paso!!

## Ventajas
1. Permite resolver un sistema de ecuaciones lineales en $\sim \dfrac{2}{3}n^3$ operaciones elementales.
2. Permite reutiliza la factorización LU para resolver numéricamente secuencia de sistema de ecuaciones lineales de la forma $A\,\mathbf{x}_k=\mathbf{b}_k$.
## Desventajas
1. Se requiere esperar la ejecución completa de las $\sim \dfrac{2}{3}n^3$ operaciones elementales para obtener la solución $\mathbf{x}$.

#OK
#Tema_4