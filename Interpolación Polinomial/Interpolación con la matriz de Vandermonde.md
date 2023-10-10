# Caso Base
Considere que tiene a su disposición los puntos $(x_1,y_1)$ y $(x_2,y_2)$. **¿Cómo construiría un polinomio que interpole ambos puntos?**
1. Se propone una _estructura_ polinomial: $p_1(x)=a_0+a_1\,x$. Notar que el sub-índice en $p_1(x)$ corresponde al grado del polinomio a construir. En este caso es 1 ya que hay 2 puntos, y 2 puntos se pueden interpolar con una recta. También notar que $a_0$ y $a_1$ son desconocidos.
2. Se conecta la estructura polinomial con la **data**, lo genera las siguientes ecuaciones:$$\begin{align*}
   p_1(x_1) &= a_0+a_1\,x_1 = y_1,\\
   p_1(x_2) &= a_0+a_1\,x_2 = y_2.\\
   \end{align*}$$
3. Se **transforma** lo obtenido en algo conocido, es decir, en un sistema de ecuaciones lineales! El cual se puede resolver con cualquiera de los algoritmos vistos anteriormente en [[Resolución numérica de sistemas de ecuaciones lineales]] : $$\begin{bmatrix}
	        1 & x_1\\
	        1 & x_2
	    \end{bmatrix}
	    \begin{bmatrix}
	        a_0\\
	        a_1
	    \end{bmatrix}
	    =
	    \begin{bmatrix}
	        y_1\\
	        y_2
	    \end{bmatrix}$$
4. Al resolver el sistema de ecuaciones lineales obtendremos los valores de $a_0$ y $a_1$ que no conocíamos, por lo cual ahora podemos evaluar el polinomio interpolador en _cualquier_ valor de $x$ que necesitemos.
	1. Hay que considerar que es recomendado en este caso evaluar el interpolador $p_1(x)$ para valores de $x\in[\min(x_1,x_2),\max(x_1,x_2)]$. Notar que se usa $\min$ y $\max$ en este caso porque no sabemos si $x_1<x_2$ o $x_2<x_1$. Solo sabemos que no pueden ser iguales, de otra forma la matriz del sistema de ecuaciones lineales sería singular.
# Caso General
La idea es la misma, pero ahora tendremos $n$ pares ordenados, digamos: $(x_1,y_1),(x_2,y_2),\dots,(x_n,y_n)$.
1. La _estructura_ polinomial en este caso sería:$$
   \begin{align*}
	   p_{n-1}(x) &= \sum_{i=0}^{n-1} a_{i}\,x^i\\
	   &= a_0+a_1\,x+a_2\,x^2+\dots+a_{n-1}\,x^{n-1}.
   \end{align*}
   $$
   Notar que esta estructura es compatible con el caso base cuando $n=2$.
2. La conexión de la estructura polinomial y la data queda de la siguiente forma,$$
   \begin{align*}
   p_{n-1}(x_1) & = \sum_{i=0}^{n-1} a_{i}\,x_1^i = a_0+a_1\,x_1+a_2\,x_1^2+\dots+a_{n-1}\,x_1^{n-1}= y_1,\\
   p_{n-1}(x_2) & = \sum_{i=0}^{n-1} a_{i}\,x_2^i = a_0+a_1\,x_2+a_2\,x_2^2+\dots+a_{n-1}\,x_2^{n-1}= y_2,\\
   \vdots & \vdots\\
   p_{n-1}(x_k) & = \sum_{i=0}^{n-1} a_{i}\,x_k^i = a_0+a_1\,x_k+a_2\,x_k^2+\dots+a_{n-1}\,x_k^{n-1}= y_k,\\
   \vdots & \vdots\\
   p_{n-1}(x_n) & = \sum_{i=0}^{n-1} a_{i}\,x_n^i = a_0+a_1\,x_n+a_2\,x_n^2+\dots+a_{n-1}\,x_n^{n-1}= y_n.\\
   \end{align*}
   $$
3. **Transformando** lo obtenido, se construye la matrix de Vandermonde asociada al siguiente sistema de ecuaciones lineales:$$
   \begin{align*}
   \begin{bmatrix}
	1 & x_1 & x_1^2 & \dots & x_1^{n-2} & x_1^{n-1}\\
	1 & x_2 & x_2^2 & \dots & x_2^{n-2} & x_2^{n-1}\\
	\vdots & \vdots & \ddots & \vdots & \vdots & \vdots\\
	1 & x_k & x_k^2 & \dots & x_k^{n-2} & x_k^{n-1}\\
	\vdots & \vdots & \ddots & \vdots & \vdots & \vdots\\
	1 & x_n & x_n^2 & \dots & x_n^{n-2} & x_n^{n-1}
	\end{bmatrix}
	\begin{bmatrix}
	a_0\\
	a_1\\
	\vdots\\
	\vdots\\
	\vdots\\
	a_{n-1}\\
	\end{bmatrix}
	=
	\begin{bmatrix}
	y_1\\
	y_2\\
	\vdots\\
	y_k\\
	\vdots\\
	y_n\\
	\end{bmatrix}
   \end{align*}
   $$
4. Al igual que en el caso anterior, se puede resolver el sistema de ecuaciones lineales anterior con los algoritmos discutidos en [[Resolución numérica de sistemas de ecuaciones lineales]]. Luego de obtener los coeficientes $a_{i}$, podemos evaluar la expresión anterior en "cualquier" valor de $x$ que se requiera.

# Ventajas
- Reutiliza conocimiento previo, es decir, se conecta con sistemas de ecuaciones lineales.
- La estructura de la matriz de Vandermonde sigue un patrón muy regular, por lo cual se pueden construir de forma vectorizada. Por ejemplo en $\verb|NumPy|$ se puede construir con [numpy.vander](https://numpy.org/doc/stable/reference/generated/numpy.vander.html).
- Es relativamente directo de entender.
- Para casos **pequeños** es una alternativa útil.
# Desventajas
- Construir la matriz tiene un costo computacional $\sim n^2$ .
- Resolver el sistema de ecuaciones lineales tiene un costo computacional $\sim \dfrac{2}{3} n^3$ si se utiliza [[Factorización PALU]].
- El número de condición $\kappa(V_n)$, donde $V_n$ es la matriz de Vandermonde, es en general muy alto, por lo que se considera que es un sistema de ecuaciones lineales mal condicionado, lo que implica que no es recomendado trabajar con esa matriz de forma directa. Ver la sección de _Consideraciones Adicionales_ en [[Resolución numérica de sistemas de ecuaciones lineales]].
- Evaluar el polinomio interpolador $p_{n-1}(x) = \sum_{i=0}^{n-1} a_{i}\,x^i$ sin ninguna optimización tiene un costo computacional de $\sim \dfrac{n^2}{2}$.

#OK 
#Tema_6