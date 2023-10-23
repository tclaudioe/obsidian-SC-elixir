Se conoce como **Forward Substitution** al algoritmo que permite resolver un sistema de ecuaciones lineales cuando la matriz es **triangular inferior**. Por ejemplo, en la factorización LU debemos resolver el siguiente sistema de ecuaciones lineales:$$\begin{bmatrix}
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
        \end{bmatrix}.$$
De lo anterior, rápidamente notamos que podemos obtener $c_1$ de la **primera ecuación** del sistema, es decir $1\,c_1=b_1$. Luego para la segunda ecuación, $l_{2,1}\,c_1+1\,c_2=b_2$ podemos despejar $c_2$ y así sucesivamente. En resumen, el sistema de ecuaciones lineales anterior se puede transformar en las siguientes expresiones,$$
\begin{align*}
c_1 &= b_1,\\
l_{2,1}\,c_1+c_2 &= b_2,\\
l_{3,1}\,c_1+l_{3,2}\,c_2+c_3 &= b_3,\\
\vdots &= \vdots\\
\left(\sum_{k=1}^{n-1} l_{n,k}\right)+c_n &=b_n.
\end{align*}
$$ Las cuales se pueden despejar de la siguiente forma,$$
\begin{align*}
c_1 &= b_1,\\
c_2 &= b_2-l_{2,1}\,c_1,\\
c_3 &= b_3-\left(l_{3,1}\,c_1+l_{3,2}\,c_2\right),\\
\vdots &= \vdots\\
c_n &=b_n-\left(\sum_{k=1}^{n-1} l_{n,k}\right).
\end{align*}
$$ Es decir, para obtener $c_2$ se necesita el valor de $c_1$, y así sucesivamente.

## Ventajas
1. Tiene una complejidad computacional $\sim n^2$. ¡Lo cual es muy bueno!
## Desventajas
1. Solo sirve para matrices triangulares **inferiores**.

## Observaciones
En general se puede aplicar a matrices triangulares inferiores donde la diagonal no necesariamente tiene $1$'s, por ejemplo,$$\begin{bmatrix}
        	l_{1,1} & 0 & \cdots & \cdots & 0 \\
        	l_{2,1} & l_{2,2} & \ddots & \ddots & 0 \\
        	l_{3,1} & l_{3,2} & l_{3,3} & \ddots & 0 \\
        	\vdots & & \ddots & \ddots &\vdots  \\
        	l_{n,1} & \cdots & \cdots & l_{n,n-1} & l_{n,n}
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
        \end{bmatrix}.$$ El único cuidado que se requiere que los coeficientes de la diagonal sean distinto de $0$ y que al despejar las ecuaciones se divida por el coeficiente en la diagonal, dado que no es necesariamente 1.

#OK
#Tema_4