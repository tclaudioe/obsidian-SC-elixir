Un sistema de ecuaciones **no-lineales** puede representarse de la siguiente forma, $$
\mathbf{F}(\mathbf{x})=\mathbf{0},$$donde $\mathbf{F}:\mathbb{R}^n\rightarrow\mathbb{R}^n$ y $\mathbf{0}\in\mathbb{R}^n$. Es decir, buscamos el vector $\mathbf{x}$ que permita que la función $\mathbf{F}$ sea $\mathbf{0}$ al evaluarla en $\mathbf{x}$. Otra forma de verlo, es encontrar una raíz de una función vectorial.

El algoritmo que estudiaremos para resolver sistemas de ecuaciones **no-lineales** es el método de Newton en $\mathbb{R}^n$. Pero para poder presentarlo, es conveniente recordar el [[Método de Newton]] en 1D, el cual se reduce a,$$
x_{i+1} = x_{i} - \frac{f(x_{i})}{f'(x_{i})},$$En este caso, sabemos que el método converge cuadráticamente si la multiplicidad de la raíz es $1$. En el caso de alta dimensión, la derivación es similar a lo realizado en 1D, sin embargo el álgebra es distinta porque trabajamos con matrices y vectores, no solo escalares. Entonces, la linealización de $\mathbf{F}$ entorno al punto $\mathbf{x}_i$ _cercano_ a la raíz $\mathbf{x}$ genera la siguiente expansión, $$
\begin{align*}
	\mathbf{F}(\mathbf{x}) &= \mathbf{F}(\mathbf{x}_{i}) + J(\mathbf{x}_{i}) (\mathbf{x} - \mathbf{x}_{i} ) + O(||\mathbf{x} - \mathbf{x}_{i}||^2).
\end{align*}
$$Ahora, considerando que la raíz es $\mathbf{x}$, entonces sabemos que $\mathbf{F}(x)=\mathbf{0}$, lo cual reduce la expresión a,$$
\begin{align*}
	\mathbf{0} &= \mathbf{F}(\mathbf{x}_{i}) + J(\mathbf{x}_{i}) (\mathbf{x} - \mathbf{x}_{i} ) + O(||\mathbf{x} - \mathbf{x}_{i}||^2).
\end{align*}
$$Descartando los términos de orden superior (lo que implica que ya no obtendremos $\mathbf{x}$ si no una aproximación la cual llamaremos $\mathbf{x}_{i+1}$ ) y despejando obtenemos,$$
\begin{align*}
	\mathbf{x}_{i+1} &= \mathbf{x}_{i} - J^{-1}(\mathbf{x}_{i}) \, \mathbf{F}(\mathbf{x}_{i}).
\end{align*}
$$Es decir, es análogo al [[Método de Newton]] en 1D pero _traducido_ a alta dimensión.

## Ventajas
1. Permite aproximar numéricamente soluciones a sistemas de ecuaciones no-lineales.
2. Puede ser útil utilizar métodos iterativos, como el [[Jacobi]], dado que podría no ser necesario resolver _exactamente_ cada sistema de ecuaciones lineales en cada paso del Método de Newton.
## Desventajas
1. Requiere la resolución de un nuevo sistema de ecuaciones lineales en cada iteración.

#OK