Si se tuviera que definir cuales son los temas fundamentales en Computación Científica, sin duda que resolver **sistemas de ecuaciones lineales** estaría en la lista. La razón de esto es porque nos permite resolver la siguiente ecuación:
$$
A\,\mathbf{x}=\mathbf{b},
$$
donde conocemos la matriz $A\in\mathbb{R}^{n\times n}$ y el vector $\mathbf{b}\in\mathbb{R}^n$, y queremos encontrar el vector $\mathbf{x}\in\mathbb{R}^n$. La solución _matemática_ que uno puede sugerir es la siguiente:
$$
\mathbf{x}=A^{-1}\,\mathbf{b}.
$$
La cual en teoría está correcta, sin embargo tiene una desventaja, que es la computación de la matriz inversa de $A$, es decir $A^{-1}$. Lo cual en principio se podría hacer, sin embargo surge la duda, ¿Se podría obtener $\mathbf{x}$ sin necesariamente obtener la inversa de $A$? La respuesta es sí, y veremos varios algoritmos que permiten esto sin necesidad alguna de calcular la inversa.
- **Ventajas**
	- Reducción del tiempo de computación, entre otras.
- **Desventajas**
	- Elegir el algoritmo adecuado para cada problema. _¡En realidad esto es una oportunidad!_

# Familia de algoritmos
Los algoritmos existentes para la resolución de **sistemas de ecuaciones lineales** se puede organizar en dos categorías:
- **Algoritmos completos**: son aquellos que terminan después de una cantidad finita de #OperacionesElementales. La principal ventaja de estos algoritmos es que se sabe _a priori_ que se obtendrá la _solución numérica_ en un tiempo finito.
- **Algoritmos iterativos**: son aquellos que definen procesos iterativos (podrían llamarse iteraciones de punto fijo de alta dimensión) que reducen el error o residuo al iterar. Al igual que las iteraciones de punto fijo en una dimensión, pueden generar iteraciones convergentes y divergentes. La principal ventaja es que uno puede ir obteniendo aproximaciones de la solución en cada iteración.

# Algoritmos Completos
- [[Eliminación Gaussiana]]
- [[Backward Substitution]]
- [[Forward Substitution]]
- [[GMRes]]

# Algoritmos Iterativos
- [[Jacobi]]
- [[Gauss-Seidel]]
- [[GMRes]] 

# Consideraciones Adicionales
Es crucial conocer el número de condición de una matriz antes de resolver el sistema de ecuaciones lineales asociado dado que nos indica si es conveniente resolver directamente el problema o se debe _modificar_ previamente. Con modificar nos referimos a utilizar un pre-condicionador, esto está fuera del _scope_ del curso pero es importante conocer el problema. La esencia del problema se reduce en la siguiente ecuación:
$$
\cfrac{1}{\kappa (A)}  \cdot
    	\dfrac{\left\|\mathbf{b} - A \cdot \mathbf{x}_a\right\|}{\left\|\mathbf{b}\right\|}
    	\leq \dfrac{\left\|\mathbf{x}_a - \textbf{x}\right\|}{\left\|\textbf{x}\right\|}
    	\leq \kappa(A) \cdot \dfrac{\left\|\textbf{b} - A \cdot \textbf{x}_a\right\|}{\left\|\textbf{b}\right\|},
$$
donde el número de condición $\kappa_p(A)$ se define como $\|A\|_p \, \left\|A^{-1}\right\|_p\geq1$, para alguna norma matricial  $\left\|\cdot\right\|_p$, y $\mathbf{x}_a$ corresponde a la aproximación numérica obtenida por la ejecución de algún algoritmo, por ejemplo, utilizando aritmética de punto flotante. Lo cual significa que lo que uno obtiene es una aproximación de la solución $\mathbf{x}$. En general lo que uno puede asegurar que sea _pequeño_ en magnitud es $\dfrac{\left\|\textbf{b} - A \cdot \textbf{x}_a\right\|}{\left\|\textbf{b}\right\|}$, sin embargo lo que uno quiere que sea pequeño en magnitud es $\dfrac{\left\|\mathbf{x}_a - \textbf{x}\right\|}{\left\|\textbf{x}\right\|}$, es decir, la desigualdad nos da una cota superior del error en $\dfrac{\left\|\mathbf{x}_a - \textbf{x}\right\|}{\left\|\textbf{x}\right\|}$ basado en $\dfrac{\left\|\textbf{b} - A \cdot \textbf{x}_a\right\|}{\left\|\textbf{b}\right\|}$ pero escalado por $\kappa_p(A)$. Notar que en general lo mejor que uno puede asegurar el valor de $\dfrac{\left\|\textbf{b} - A \cdot \textbf{x}_a\right\|}{\left\|\textbf{b}\right\|}$ es $\approx10^{-16}$, por lo que si uno tiene un número de condición del orden de $\approx 10^{10}$ significa que perdió aproximadamente $10$ decimales en la aproximación. Incluso, si el número de condición es mayor a $10^{16}$, significa que la aproximación $\mathbf{x}_a$ obtenida podría no tener ningún dígito significativo correcto!

#OK
#Tema_4