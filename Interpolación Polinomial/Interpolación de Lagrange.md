# Caso Base
Considere que tiene a su disposición los puntos $(x_1,y_1)$ y $(x_2,y_2)$.
1. En el caso de la Interpolación de Lagrange utilizaremos unas funciones auxiliares convenientes, las cuales llamaremos $L_i(x)$. Para este primer caso, solo necesitamos $L_1(x)$ y $L_2(x)$. Las cuales tienen las siguientes propiedades:
	- $L_1(x_1)=1$ y $L_1(x_2)=0$.
	- $L_2(x_1)=0$ y $L_2(x_2)=1$.
	Con estas definiciones, podemos construir nuestro interpolador polinomial.
2.  El polinomio interpolador es el siguiente:$$
p_1(x) = y_1\,L_1(x)+y_2\,L_2(x).
$$Notar que al evaluar tanto en $x_1$ y $x_2$ se obtiene lo que se necesita, es decir $y_1$ y $y_2$, respectivamente. $$\begin{align*}
   p_1(x_1) &= y_1\,L_1(x_1)+y_2\,L_2(x_1) = y_1\,1+y_2\,0 = y_1,\\
   p_1(x_2) &= y_1\,L_1(x_2)+y_2\,L_2(x_2) = y_1\,0+y_2\,1 = y_2,
\end{align*}
$$
## ¿Cómo se construyen $L_1(x)$ y $L_2(x)$?
- Para construir $L_1(x)$ y $L_2(x)$ tenemos que basarnos en las restricciones que se definen para cada función.
	- $L_1(x_1)=1$ y $L_1(x_2)=0$.
	- $L_2(x_1)=0$ y $L_2(x_2)=1$.
- Para lograr que $L_1(x)$ sea $0$ en $x_2$ considere la siguiente función: $l_1(x)=x-x_2$. Entonces $l_1(x)$ asegura que $l_1(x_2)=0$. De la misma forma podemos definir $l_2(x)=x-x_1$.
- Ahora, para lograr la segunda restricción podemos simplemente definir $L_1(x)=\dfrac{l_1(x)}{l_1(x_1)}$.
- En resumen.
	- $L_1(x) = \dfrac{l_1(x)}{l_1(x_1)} =\dfrac{x-x_2}{x_1-x_2}$
	- $L_2(x) = \dfrac{l_2(x)}{l_2(x_2)} = \dfrac{x-x_1}{x_2-x_1}$

# Caso General
La idea es la misma, pero ahora tendremos $n$ pares ordenados, digamos: $(x_1,y_1),(x_2,y_2),\dots,(x_n,y_n)$.
1. Se define ahora la versión general de las funciones auxiliares $L_i(x)$ de la siguiente forma,$$\begin{align*}
	L_i(x) &=\dfrac{l_i(x)}{l_i(x_i)},\\
	l_i(x) &=\prod_{k=1,i \neq k}^n (x-x_i) = (x-x_1)\,(x-x_2)...(x-x_{i-1})\,(x-x_{i+1})\dots(x-x_n)
\end{align*}$$
2. Se construye el interpolador polinomial,$$\begin{align*}
   p_{n-1}(x) &= \sum_{i=1}^n y_i\,L_i(x).
   \end{align*}$$
# Ventajas
- Se construye directamente el interpolador, sin necesidad de resolver un sistema de ecuaciones lineales como es el caso de [[Interpolación con la matriz de Vandermonde]].
- Al igual que la [[Interpolación con la matriz de Vandermonde]], sigue un patrón regular y relativamente directo implementar. En $\verb|SciPy|$ se implementa en [scipy.interpolate.lagrange](https://docs.scipy.org/doc/scipy/reference/generated/scipy.interpolate.lagrange.html).
# Desventajas
- La construcción del polinomio interpolador requiere $\sim 2\,n^2$ operaciones elementales.
- Tampoco es recomendable para valores de $n$ grandes.
- La evaluación tiene un costo computacional  $\sim 2\,n^2$ operaciones elementales.
