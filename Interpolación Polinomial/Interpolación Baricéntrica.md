La interpolación Baricéntrica es otro algoritmo utilizado en interpolación polinomial, el cual tiene la ventaja que puede ser construido a partir de la [[Interpolación de Lagrange]].

# Previo a la derivación
- En la [[Interpolación de Lagrange]] se define las siguientes funciones:$$\begin{align*}
	L_i(x) &=\dfrac{l_i(x)}{l_i(x_i)},\\
	l_i(x) &=\prod_{k=1,i \neq k}^n (x-x_i) = (x-x_1)\,(x-x_2)\dots(x-x_{i-1})\,(x-x_{i+1})\dots(x-x_n)
\end{align*}$$
- Acá se definirán las siguientes generalizaciones:$$
\begin{align*}
	l(x) &=\prod_{k=1}^n (x-x_i) = (x-x_1)\,(x-x_2)\dots(x-x_n),\\
	l_i(x) &= \dfrac{l(x)}{x-x_i}.
\end{align*}$$ Notar que la **generalización** solo re-escribe $l_i(x)$ de una forma distinta, pero representa lo mismo. El único cuidado que hay que tener es cuando se evalúa en $x_i$, en ese caso se debe tomar el límite.
# Derivación - Caso General
- De la [[Interpolación de Lagrange]] sabemos que podemos escribir el polinomio interpolador de la siguiente forma,$$\begin{align*}
   p_{n-1}(x) &= \sum_{i=1}^n y_i\,L_i(x)\\
   &= \sum_{i=1}^n y_i\,\dfrac{l_i(x)}{l_i(x_i)}.
   \end{align*}$$
- Definiendo $w_i=\dfrac{1}{l_i(x_i)}$, reemplazando $l_i(x)=\dfrac{l(x)}{(x-x_i)}$ y factorizando el término común, obtenemos lo siguiente,$$\begin{align*}
   p_{n-1}(x) &= \sum_{i=1}^n y_i\,\dfrac{l_i(x)}{l_i(x_i)}\\
   &= \sum_{i=1}^n y_i\,l_i(x) \dfrac{1}{l_i(x_i)}\\
   &= \sum_{i=1}^n y_i\,\dfrac{l(x)}{(x-x_i)}\,w_i\\
   &= l(x)\,\sum_{i=1}^n y_i\,\dfrac{w_i}{(x-x_i)}
   \end{align*}$$ Lo cuál ya es un avance. A esta representación la llamaremos **pre-Baricéntrica**.
- Ahora, podemos utilizar la representación **pre-Baricéntrica** para interpolar la función constante igual a $1$, esto significa que para cualquier valor de $x$ y para cualquier cantidad de puntos, el interpolador debe entregar $1$. Entonces, utilizando la representación **pre-Baricéntrica** obtenemos la siguiente identidad,$$\begin{align*}
   p_{n-1}(x) & = l(x)\,\sum_{i=1}^n \underbrace{y_i}_{1}\,\dfrac{w_i}{(x-x_i)}\\
   &= l(x)\,\sum_{i=1}^n \dfrac{w_i}{(x-x_i)} = 1
   \end{align*}$$ Esto nos permite construir la siguiente identidad,$$l(x) = \dfrac{1}{\displaystyle{\sum_{i=1}^n \dfrac{w_i}{(x-x_i)}}}.
$$ La cual es válida para cualquier valor de $n$.
- Utilizando la identidad recién obtenida para $l(x)$ en la interpolación **pre-Baricéntrica**, obtenemos la siguiente representación,$$\begin{align*}
   p_{n-1}(x) & = l(x)\,\sum_{i=1}^n y_i\,\dfrac{w_i}{(x-x_i)}\\
   & = \dfrac{\displaystyle{\sum_{i=1}^n y_i\,\dfrac{w_i}{(x-x_i)}}}{\displaystyle{\sum_{i=1}^n \dfrac{w_i}{(x-x_i)}}}
   \end{align*}
  $$ La cual es la _gran_ **Interpolación Baricéntrica**!!
   
# Caso Base
Considere que tiene a su disposición los puntos $(x_1,y_1)$ y $(x_2,y_2)$.
- La interpolación **Baricéntrica** en esta caso es:$$\begin{align*}
  p_1(x) &= \displaystyle{\dfrac{y_1\,\dfrac{w_1}{x-x_1}+y_2\,\dfrac{w_2}{x-x_2}}{\dfrac{w_1}{x-x_1}+\dfrac{w_2}{x-x_2}}},\\
  w_1 &= \dfrac{1}{l_1(x_1)}=\dfrac{1}{(x_1-x_2)},\\
  w_2 &= \dfrac{1}{l_2(x_2)}=\dfrac{1}{(x_2-x_1)}.\\
  \end{align*}$$
# Ventajas
- Permite evaluar el polinomio interpolardor en $\mathcal{O}(n)$ operaciones elementales!
- Para la construcción del polinomio:
	- Requiere $\mathcal{O}(n^2)$ operaciones elementales
	- Pero si los nodos utilizados en la interpolación siguen un patrón regular, por ejemplo: son equiespaciados o se utilizan los [[Puntos de Chebyshev]], entonces se puede construir el polinomio en $\mathcal{O}(n)$ operaciones elementales.
- Es el algoritmo recomendado!
- En $\verb|SciPy|$ ver [scipy.interpolate.barycentric_interpolate](https://docs.scipy.org/doc/scipy/reference/generated/scipy.interpolate.barycentric_interpolate.html#scipy.interpolate.barycentric_interpolate) y [scipy.interpolate.BarycentricInterpolator](https://docs.scipy.org/doc/scipy/reference/generated/scipy.interpolate.BarycentricInterpolator.html#scipy.interpolate.BarycentricInterpolator).
# Desventajas
- La teoría es más _intensa_ que en los algoritmos anteriores, pero vale la pena!

#OK 
#Tema_6