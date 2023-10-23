Se conoce como **Backward Substitution** al algoritmo que permite resolver un sistema de ecuaciones lineales cuando la matriz es **triangular superior**. Por ejemplo, en la factorización LU debemos resolver el siguiente sistema de ecuaciones lineales:$$\begin{bmatrix}
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
        \end{bmatrix}.$$
De lo anterior, rápidamente notamos que podemos obtener $x_n$ de la **última ecuación** del sistema, es decir $u_{n,n}\,x_n=c_n$. Luego para la penúltima ecuación, $u_{n-1,n-1}\,x_{n-1}+u_{n-1,n}\,x_n=c_{n-1}$ podemos despejar $x_{n-1}$ y así sucesivamente. En resumen, el sistema de ecuaciones lineales anterior se puede transformar en las siguientes expresiones,$$
\begin{align*}
\sum_{k=1}^{n} u_{1,k}\,x_k &=c_1\\
\sum_{k=2}^{n} u_{1,k}\,x_k &=c_2\\
\sum_{k=3}^{n} u_{1,k}\,x_k &=c_3\\
\vdots &= \vdots\\
u_{n-1,n-1}\,x_{n-1}+u_{n-1,n}\,x_n &=c_{n-1}\\
u_{n,n}\,x_n&=c_n.
\end{align*}
$$ Al igual que el caso del algoritmo [[Forward Substitution]], podemos re-escribir las ecuaciones convenientemente para despejar las incógnitas $x_k$, sin embargo en este caso es útil escribir desde la última  ecuación a la primera al despejar,$$
\begin{align*}
x_n&=\dfrac{c_n}{u_{n,n}}\\
x_{n-1} &=\dfrac{c_{n-1}-u_{n-1,n}\,x_n}{u_{n-1,n-1}}\\
\vdots &= \vdots\\
x_n &=\dfrac{c_1-\sum_{k=1}^{n-1} u_{1,k}\,x_k}{u_{1,k}}\\
\end{align*}
$$ Es decir, para obtener $x_{n-1}$ se necesita el valor de $x_n$, y así sucesivamente.

## Ventajas
1. Tiene una complejidad computacional $\sim n^2$. ¡Lo cual es muy bueno!
## Desventajas
1. Solo sirve para matrices triangulares **superiores**.

# Complejidad Computacional
Para obtener la complejidad computacional de **Backward Substitution** es conveniente escribir el algoritmo anterior en pseudo-código,
```python
for i in range(n-1,0,-1):
	for j in range(i+1,n):
		c[i] = c[i] - U[i,j]*x[j]
	x[i] = c[i]/U[i,i]
```
Entonces,$$
\begin{align*}
	{\sum_{i=n}^{1}}\left[ \left( \sum_{j=i+1}^{n} 2 \right) + 1 \right]
		&= \sum_{i=1}^{n}\left[ 2 \, \left( \sum_{j=i+1}^{n} 1 \right) + 1 \right]\\
		&= \sum_{i=1}^{n} \left[ 2 \, (n - i + 1 - 1) + 1 \right]\\
		&= 2 \, n \left( \sum_{i=1}^{n} 1 \right) - 2 \, \left( \sum_{i=1}^{n} i \right) + \sum_{i=1}^{n} 1\\
		&= 2 \, n \, n - 2 \, \cfrac{n \, (n+1)}{2} + n\\
		&= 2 \, n^2 - n^2 - n + n\\
		&= n^2.
\end{align*}
$$ Notar que en la primera sumatoria se inicia en $n$ y termina en $1$ para indicar que el primer ciclo es decreciente.

#OK
#Tema_4