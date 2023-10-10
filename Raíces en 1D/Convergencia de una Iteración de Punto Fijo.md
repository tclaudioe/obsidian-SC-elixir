#Teorema #ConvergenciaDeUnaIteraciónDePuntoFijo
		Asuma que una función $g(x)$ es continua y diferenciable, que tiene un punto fijo $g(r) = r$, y que $S = |g'(r)| < 1$. 
		Entonces la iteración de Punto Fijo converge linealmente con tasa $S$ al punto fijo $r$ para *initial guesses* lo suficientemente cerca de $r$.

# Resumen
Si el valor absoluto de la derivada de $g(x)$ en el punto fijo es menor a $1$ entonces hay convergencia!

En la práctica uno no conoce *a priori* el valor de $S = |g'(r)|$, sin embargo se puede aproximar por medio de ejecutar la iteración de punto fijo respectiva.

# Casos para S
- $S<1$ y $S\neq 0$.
	- En este caso se observa #ConvergenciaLineal , ver [[Convergencia Numérica]], es decir el cuociente de errores consecutivos satisface la siguiente relación:$$
	  \lim_{i\rightarrow \infty} \dfrac{e_{i+1}}{e_i}=S<1.
	$$donde el error $e_i=|x_i-r|$. La cual se puede interpretar como si el error de la iteración $i+1$ es aproximadamente el producto entre $S$ y $e_i$, es decir $e_{i+1}\approx S\,e_i$. Lo que implica que el error se reduce dado que $S<1$.
- $S=0$
	- En este caso se observa #ConvergenciaCuadrática , ver [[Convergencia Numérica]].  En particular el [[Método de Newton]] exhibe este tipo de convergencia, la cual en muy pocas iteraciones llega a la raíz. Se dice que en cada iteración duplica la cantidad de decimales correctos!

# Explicación de ejemplo utilizado en la Aplicación de Iteración de Punto Fijo
En [[Iteración de Punto Fijo]] se propone construir la función $g(x)=x+a\,f(x)$ para encontrar la raíz de $f(x)$, sin embargo no se explicó como encontrar, ahora sí podemos entenderlo dado que conocemos el teorema #ConvergenciaDeUnaIteraciónDePuntoFijo . El teorema indica que  si $S<1$ entonces hay convergencia, entonces con esta información podemos determinar el mejor caso posible, es decir que $S=0$, lo cual nos da lo siguiente:$$
\begin{align}
	g(x) &= x+a\,f(x), \text{ apliquemos la derivada}\\
	g'(x) &= 1+a\,f'(x), \text{ evaluemos en $x=r$}\\
	g'(r) &= 1+a\,f'(r), \text{ considerando el mejor paso posible}\\
	0 &= 1+a\,f'(r), \text{ despejamos $a$ obtenemos}\\
	a &= \dfrac{-1}{f'(r)}.
\end{align}
$$ Lo cual nos indica que el mejor valor posible para $a$ es $-\dfrac{1}{f'(r)}$. 
Nuevamente caemos en la aparente contradicción de que necesitamos la raíz $r$, que es lo que estamos buscando, y adicionalmente la derivada de $f(x)$, para obtener una buena aproximación de $a$. En realidad lo que necesitamos simplemente es una estimación de $f'(r)$ y podríamos lograr que $S<1$, lo que es suficiente para asegurar convergencia!

*Se sugiere en este punto ver la relación entre la explicación anterior y [[Método de Newton]]*.

# Desafío
#Challenge
Siguiendo la misma idea anterior, considere que usted tiene a su disposición la función $g(x)$, que tiene el punto fijo $r=g(r)$. Sin embargo no genera una iteración de punto fijo convergente a $r$, lo que implica que $S=|g'(r)|>1$. ¿Puede usted construir una nueva iteración de punto fijo que sea convergente a $r$ basada en $g(x$)? Es decir, se debe definir una nueva función $G(x)$ tal que $r=G(r)$.
**Hint**: Podría ser interesante analizar el problema haciendo la relación inversa entre búsqueda de raíces e iteración de punto fijo, es decir, pasar de $r=g(r)$ a $f(r)=0$ y conectarlo con el desarrollo previo. 


#OK
#Tema_3