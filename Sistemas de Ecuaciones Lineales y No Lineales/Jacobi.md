El método de Jacobi, bajo ciertas condiciones, genera una secuencia de vectores convergentes a la solución de un sistema de ecuaciones **lineales**. Nuevamente puede considerarse como una [[Iteración de Punto Fijo]] de alta dimensión, al igual que el **método de Newton en alta dimensión** desarrollado en [[Resolución numérica de sistemas de ecuaciones no-lineales]].

En este caso se propone una **descomposición** matricial, notar que no es una **factorización** matricial. Es decir, se representa una matriz $A$ como la suma de 3 matrices, es decir,$$
\begin{align*}
	A &= L+D+U\\
	&= \begin{bmatrix}
		0 & 0 & \dots & 0 & 0\\
		a_{2,1} & 0 & \ddots & 0 & 0 \\
		\vdots  & \ddots & \ddots & \ddots & \vdots \\
		a_{n-1,1}& a_{n-1,2} &\dots & 0 & 0 \\
		a_{n,1}  & a_{n,2} & \dots & a_{n,n-1} & 0
	\end{bmatrix}\\
	&\phantom{=}+
	\begin{bmatrix}
		a_{1,1} & 0 & \dots & 0 & 0\\
		0 & a_{2,2} & \ddots & 0 & 0 \\
		\vdots  & \ddots & \ddots & \ddots & \vdots \\
		0 & 0 &\dots & a_{n-1,n-1} & 0 \\
		0  & 0 & \dots & 0 & a_{n,n}
	\end{bmatrix}\\
	&\phantom{=}+
	\begin{bmatrix}
		0 & a_{1,2} & \dots & a_{1,n-1} & a_{1,n}\\
		0 & 0 & \ddots & a_{2,n-1} & a_{2,n} \\
		\vdots  & \ddots & \ddots & \ddots & \vdots \\
		0 & 0 &\dots & 0 & a_{n-1,n} \\
		0  & 0 & \dots & 0 & 0
	\end{bmatrix}
\end{align*}
$$Es **crucial** destacar que las matrices denominadas $L$ y $U$ acá son distintas a las matrices $L$ y $U$ definidas en la [[Factorización LU]] o [[Factorización PALU]]. Acá la **descomposición** es _instantánea_ dado que no se requiere ejecutar ningún algoritmo para obtener las matrices $L$, $D$ y $U$.

# ¿Cómo se construye el método de Jacobi?
Primero, re-destacar que es una iteración de punto fijo lineal en alta dimensión. Por lo que debemos derivar una iteración de punto fijo, de la siguiente forma,$$
\begin{align*}
	A\,\mathbf{x} &= \mathbf{b},\\
	(L+D+U)\,\mathbf{x} &= \mathbf{b},\\
	L\,\mathbf{x}+D\,\mathbf{x}+U\,\mathbf{x} &= \mathbf{b}.
\end{align*}$$Ahora solo dejamos el término $D\,\mathbf{x}$ al lado izquierdo y lo demás se muevo al lado derecho,$$
\begin{align*}
	D\,\mathbf{x}&= \mathbf{b}-L\,\mathbf{x}-U\,\mathbf{x},\\
	D\,\mathbf{x}&= \mathbf{b}-(L+U)\,\mathbf{x}.
\end{align*}$$ _Despejando_ $\mathbf{x}$,$$
\begin{align*}
	D^{-1}\,D\,\mathbf{x}&= D^{-1}\,(\mathbf{b}-(L+U)\,\mathbf{x}),\\
	\mathbf{x}&= D^{-1}\,(\mathbf{b}-(L+U)\,\mathbf{x}).
\end{align*}$$ De lo cual se obtiene la siguiente iteración de punto fijo,$$
\begin{align*}
	\mathbf{x}_0 &= \text{``dato inicial''},\\
	\mathbf{x}_{n+1} &= D^{-1}
	\left( \mathbf{b} - \left( L + U \right) \mathbf{x}_n \right),
\end{align*}$$
## Ventajas
1. Es un algoritmo iterativo que permite entregar soluciones intermedias a medida que avanza.
2. Puede demorarse menos que la [[Factorización LU]] o [[Factorización PALU]] si la matriz es muy _diagonal dominante_.
## Desventajas
1. No necesariamente termina luego de una cantidad finita de pasos, como sí lo hace [[Factorización LU]] o [[Factorización PALU]].

#OK
#Tema_4