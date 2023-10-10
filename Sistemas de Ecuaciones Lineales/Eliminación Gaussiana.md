Los algoritmos LU y PALU son obtenidos de la clásica **Eliminación Gaussiana** ( #EG). Para entender rápidamente los algoritmos LU y PALU debemos recordar brevemente qué hace la #EG, la cual podemos reducir a los siguientes pasos:
1. Definición inicial del problema: $A\,\mathbf{x}=\mathbf{b}$, donde $A\in\mathbb{R}^{n\times n}$, $\mathbf{x}\in\mathbb{R}^n$ y $\mathbf{b}\in\mathbb{R}^n$.
2. Construcción del _tableau_: $[A | \mathbf{b}]$, o en su forma explícita,$$\begin{equation*}
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
    \end{equation*}$$
3. Aplicar operaciones fila para dejar el _tableau_ con una estructura del tipo triangular superior, es decir: $[U | \mathbf{c}]$, o en forma explícita,$$\begin{equation*}
        \left[
			\begin{array}{ c c c c : c }
				u_{1,1} & u_{1,2} & \cdots & u_{1,n} & c_1 \\
				0 & u_{2,2} &  & u_{2,n} & c_2 \\
				\vdots & & \ddots & \vdots & \vdots \\
				0 & 0 & \cdots & u_{n,n} & c_n
			\end{array}
		\right]
		=
		\left[
		\begin{array}{ c : c }
		\phantom{.} & \\
	    & \\
	    U & \mathbf{c} \\
	    & \\
		& \phantom{.}
		\end{array}
		\right]
    \end{equation*}$$
4. Construir el sistema de ecuaciones lineales **equivalente**:$$U\,\mathbf{x}=\mathbf{c}$$
5. Resolver con [[Backward Substitution]] para obtener $\mathbf{x}$.

## Ventajas
1. Permite resolver un sistema de ecuaciones lineales.
2. Es bastante _algoritmizable_ y se puede implementar de forma conveniente con #vectorizacion .
## Desventajas
1. Si se resuelve el sistema de ecuaciones lineales $A\,\mathbf{x}_1=\mathbf{b}_1$, y uno quisiera luego resolver $A\,\mathbf{x}_2=\mathbf{b}_2$, es decir 2 sistemas de ecuaciones lineales con la misma matriz $A$ pero distinto lado derecho, debe ejecutar el algoritmo completo nuevamente, aunque uno sepa que ambas ejecuciones generarán el _tableau_ con la misma matriz $U$, es decir, $[U | \mathbf{c}_1]$ y $[U | \mathbf{c}_2]$, respectivamente. Es decir, no toma ventaja de conocer $U$. Esto se resuelve con la [[Factorización LU]].
2. Si se encontrara un $0$ en la diagonal (o incluso un número muy pequeño), el algoritmo no puede continuar. Esto se resuelve con la [[Factorización PALU]].

#OK
#Tema_4