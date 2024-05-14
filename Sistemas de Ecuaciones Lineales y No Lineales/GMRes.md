![[07_GMRes.tex.png]]
**Generalized Minimal Residual Method**
# Introducción

#Teorema #TeoremaDeCayleyHamilton
    	El teorema de Cayley-Hamilton dice que una matriz de dimensiones $n\times n$ es aniquilada por su [polinomio característico](https://en.wikipedia.org/wiki/Characteristic_polynomial) $p(\lambda)=\text{det}(\lambda\,I-A)$, el cual es mónico de grado $n$.
Lo que implica lo siguiente:
1. $p(\lambda)=\text{det}(\lambda\,I-A)=\lambda^n+\check{c}_{n-1}\,\lambda^{n-1}+\dots+\check{c}_1\,\lambda+(-1)^n\,\text{det}(A)$.
2. $p(\lambda_i)=0$, para cualquier valor propio $\lambda_i$ de $A$.
3. $p(A)=A^n+\check{c}_{n-1}\,A^{n-1}+\dots+\check{c}_1\,A+(-1)^n\,\text{det}(A)\,I=\underline{\underline{0}}$.
4. Multiplicando por $A^{-1}$ se obtiene: $A^{n-1}+\check{c}_{n-1}\,A^{n-2}+\dots+\check{c}_1\,I+(-1)^n\,\text{det}(A)\,A^{-1}=\underline{\underline{0}}$.
5. $A^{-1}=\dfrac{(-1)^{n-1}}{\text{det}(A)}\left(A^{n-1}+\check{c}_{n-1}\,A^{n-2}+\dots+\check{c}_2\,A+\check{c}_1\,I\right)$.
Entonces, si queremos resolver un sistema de ecuaciones lineales de la forma $A\,\mathbf{x}=\mathbf{b}$, podríamos escribir la solución como **una combinación lineal de potencias de la matriz $A$**.$$\begin{align*}
	A\,\mathbf{x}&=\mathbf{b},\\
	\,\mathbf{x}&=A^{-1}\,\mathbf{b},\\
	&= \dfrac{(-1)^{n-1}}{\text{det}(A)}\left(A^{n-1}+\check{c}_{n-1}\,A^{n-2}+\dots+\check{c}_2\,A+\check{c}_1\,I\right) \,\mathbf{b},\\
	&= \sum_{i=1}^n \mathring{c}_i\,A^{i-1}\,\mathbf{b}.
\end{align*}$$
Por lo tanto, una **aproximación** de la solución $\mathbf{x}$ se puede construir encontrando la _mejor_ (en el sentido $l_2$) aproximación en el sub-espacio de **Krylov** $\mathcal{K}_k=\text{span}\left(\mathbf{b}, A\,\mathbf{b}, A^2\,\mathbf{b}, A^3\,\mathbf{b}, \dots, A^{k-1}\,\mathbf{b}\right)$. Esto significa,$$
\begin{align*}
\overline{\mathbf{x}}_k&=\underset{\displaystyle{\widehat{\mathbf{x}}_k\in\mathcal{K}_k}}{\text{argmin}}\left\|\mathbf{b}-A\,\widehat{\mathbf{x}}_k\right\|_2^2.
\end{align*}
$$Desde un punto de vista gráfico, se puede interpretar de la siguiente forma, pero notando que en esta minimización con restricciones no se preserva necesariamente la ortogonalidad.

```tikz
	\begin{document}
		\begin{tikzpicture}
		% Axis
		\draw [->, line width=1pt] (0,-2) -- (0,7);
		\draw [->, line width=1pt] (-2,0) -- (12,0);
		% A\,x
		\draw [-,line width=1.5pt,red]  (-1, -0.5) -- (10, 4.95);
		\draw [red] (3.2,1.3) node [below]{$A\,\widehat{\mathbf{x}}_k$};
		% r
		\draw [->,line width=1.5pt,orange,dashed]  (7.1,3.5) -- (6.1,5.8);
		\draw [fill=orange,color=orange] (7.1, 5) node[above right]{$\mathbf{r}_k = \mathbf{b}-A\,\overline{\mathbf{x}}_k$};
		% b
		\draw [fill=blue,blue] (6,6) circle  (2pt) node[above]{$\mathbf{b}$};
		\draw [->,line width=1.5pt,blue]  (0,0) -- (5.8,5.8);
		% A\,\overline{x} 
		\draw [fill=violet,violet] (7.1,3.5) circle  (2pt) node[below right]{$A\,\overline{\mathbf{x}}_k$};
		\draw[->,dashed,line width=2.5pt,violet]  (0,0) -- (6.95,3.45);
		\end{tikzpicture}
	\end{document}
```
Entonces lo que se minimiza en este caso es el error cuadrático. **Sin embargo, se debe considerar que en este caso la minimización cuadrática es con restricciones debido a que $\widehat{\mathbf{x}}_k\in\mathcal{K}_k$, lo cual es distinto a lo discutido anteriormente en [[Problemas de Mínimos Cuadrados]]. Afortunadamente esto se solucionará prontamente!**
# El desafío de encontrar una base adecuada
Si bien uno podría construir el sub espacio de Krylov $\mathcal{K}_k=\text{span}\left(\mathbf{b}, A\,\mathbf{b}, A^2\,\mathbf{b}, A^3\,\mathbf{b}, \dots, A^{k-1}\,\mathbf{b}\right)$ directamente, se genera un problema al utilizar el [[Estándar de punto flotante IEEE 754]] . Por ejemplo considere el siguiente gráfico,
```tikz
\begin{document}
	\begin{tikzpicture}[scale=2]
		% Axis
		\draw [->, line width=1pt] (0,-0.5) -- (0,3);
		\draw [->, line width=1pt] (-0.5,0) -- (4,0);
		% Vectors
		\draw[->,line width=1.5pt, violet] (0,0) -- ({4*cos(37.5)},{4*sin(37.5)}) node[below right] {$A^3\,\mathbf{b}$};
		\draw[->,line width=1.5pt,red] (0,0) -- ({3*cos(35)},{3*sin(35)}) node[below right] {$A^2\,\mathbf{b}$};
		\draw[->,line width=1.5pt,blue] (0,0) -- ({2*cos(30)},{2*sin(30)}) node[below right] {$A\,\mathbf{b}$};
		\draw[->,line width=1.5pt,black] (0,0) -- ({1*cos(20)},{1*sin(20)}) node[below right] {$\mathbf{b}$};
	\end{tikzpicture}
\end{document}
```
Los cual al **normalizarlos** muestran el siguiente comportamiento,
```tikz
\begin{document}
	\begin{tikzpicture}[scale=4]
		% Axis
		\draw [->, line width=1pt] (0,-0.5) -- (0,1);
		\draw [->, line width=1pt] (-0.5,0) -- (1.5,0);
		% Vectors
		\draw[->,line width=1.5pt, violet] (0,0) -- ({cos(37.5)},{sin(37.5)});
		\draw[->,line width=1.5pt,red] (0,0) -- ({cos(35)},{sin(35)});
		\draw[->,line width=1.5pt,blue] (0,0) -- ({cos(30)},{sin(30)});
		\draw[->,line width=1.5pt,black] (0,0) -- ({cos(20)},{sin(20)});
	\end{tikzpicture}
\end{document}
```
Es decir, se graficaron los vectores $\dfrac{\mathbf{b}}{\|\mathbf{b}\|}$, $\dfrac{A\,\mathbf{b}}{\|A\,\mathbf{b}\|}$, $\dfrac{A^2\,\mathbf{b}}{\|A^2\,\mathbf{b}\|}$, y $\dfrac{A^3\,\mathbf{b}}{\|A^3\,\mathbf{b}\|}$, respectivamente. El problema que genera esta base es que al obtener distintas potencias de la matriz $A$ y multiplicarlas por el vector $\mathbf{b}$, la secuencia de vectores que se obtiene irán aproximando al **vector propio dominante**, es decir, el vector propio asociado al valor propio de mayor magnitud. Esto significa que numéricamente se generará una secuencia de vectores _casi_, o prácticamente, linealmente dependiente. Por lo tanto, no será posible construir una base para el sub-espacio de Krylov $\mathcal{K}_k$.

# ¿Qué hacemos?
- Problema: ¿Cómo construimos una base del sub-espacio de Krylov $\mathcal{K}_k$?
- Solución: Utilizamos la ortonormalización de [[Ortonormalización de Gram-Schmidt]]!

En este contexto la [[Ortonormalización de Gram-Schmidt]] se conoce como la iteración de Arnoldi ( #IteraciónDeArnoldi ) .

La iteración de Arnoldi es un procedimiento que nos permite construir una base ortonormal para el sub-espacio de Krylov $\mathcal{K}_k$, es decir, determina la secuencia de vectores $\mathbf{q}_i$, tal que sean unitarios y ortonormales (**¡que es el caso ideal para una base!**). Por lo tanto, al ser una base, se debe satisfacer la siguiente identidad,$$
\begin{align*}
\mathcal{K}_k
&=\text{span}\left(\mathbf{b}, A\,\mathbf{b}, A^2\,\mathbf{b}, A^3\,\mathbf{b}, \dots, A^{k-1}\,\mathbf{b}\right),\\
&=\text{span}\left(\mathbf{q}_1, \mathbf{q}_2, \mathbf{q}_3, \mathbf{q}_4, \dots, \mathbf{q}_k\right).
\end{align*}$$**NOTA: Estos vectores $\mathbf{q}_i$ son ortonormales, al igual que los vectores que se obtienen en la [[Factorización QR]], pero acá no se utilizan para construir la factorización QR de una matriz.**
Notar que de la identidad anterior, uno puede deducir directamente que $\mathbf{q}_1=\dfrac{\mathbf{b}}{\|\mathbf{b}\|}$.
Por otro lado, se puede deducir que los vectores $\mathbf{q}_i$ deben satisfacer la siguiente identidad también,$$
\begin{align}
	\mathcal{K}_k
	&=\text{span}\left(\mathbf{q}_1, A\,\mathbf{q}_1, A\,\mathbf{q}_2, A\,\mathbf{q}_3, \dots, A\,\mathbf{q}_{k-1}\right).
\end{align}
$$Lo cual nos induce la siguiente secuencia de identidades,$$
\begin{align*}
	A\,\mathbf{q}_1 &= h_{11}\,\mathbf{q}_1+h_{21}\,\mathbf{q}_2,\\
	A\,\mathbf{q}_2 &= h_{12}\,\mathbf{q}_1+h_{22}\,\mathbf{q}_2+h_{32}\,\mathbf{q}_3,\\
	\vdots&\nonumber\\
	A\,\mathbf{q}_{k} &= \sum_{j=1}^{k+1} h_{j,k}\,\mathbf{q}_j,
\end{align*}
$$donde los coeficientes $h_{i,j}$ son coeficientes necesarios para representar la combinación lineal respectiva. Lo interesante de este relación es que nos permite escribir las identidades en su forma matricial. Por ejemplo, si consideramos que la primer identidad corresponde a la primera columna de la identidad, y así sucesivamente, obtenemos,$$\begin{equation*}
        \begin{bmatrix} 
            A\,\mathbf{q}_1, & A\,\mathbf{q}_2, & \dots, & A\,\mathbf{q}_k
        \end{bmatrix} 
        =
        \begin{bmatrix} 
            h_{11}\,\mathbf{q}_1+h_{21}\,\mathbf{q}_2, & 
            h_{12}\,\mathbf{q}_1+h_{22}\,\mathbf{q}_2+h_{32}\,\mathbf{q}_3, & 
            \dots, &
            \displaystyle\sum_{j=1}^{k+1} h_{j,k}\,\mathbf{q}_j
        \end{bmatrix}.
    \end{equation*}$$Factorizando la matriz $A$  y construyendo la matriz $Q_k$ al lado izquierdo, y al lado derecho las matrices $Q_{k+1}$ y $\widetilde{H}_k$, obtenemos,$$\begin{equation*}
        A\,
        \underbrace{\begin{bmatrix} 
            \mathbf{q}_1, & \mathbf{q}_2, & \dots, & \mathbf{q}_k
        \end{bmatrix}
        }_{\displaystyle{Q_k}}
        =
        \underbrace{\begin{bmatrix} 
            \mathbf{q}_1, & \mathbf{q}_2, & \dots, & \mathbf{q}_{k}, & \mathbf{q}_{k+1}
        \end{bmatrix}
        }_{\displaystyle{Q_{k+1}}}
        \underbrace{\begin{bmatrix}
    	    h_{11} & h_{12} & \dots  & h_{1,k-1}  & h_{1,k} \\
    	    h_{21} & h_{22} & \dots  & h_{2,k-1}  &  h_{2,k} \\
    	    0      & h_{32} & \ddots & h_{3,k-1}  &  h_{3,k} \\
    	    \vdots & \ddots & \ddots & \ddots     & \vdots \\
    	    0      & \ddots  & \ddots &  h_{k,k-1} &  h_{k,k}\\
    	    0      & \dots  & \dots &  0         &  h_{k+1,k}
    	\end{bmatrix}}_{\displaystyle{\widetilde{H}_k}}.
    \end{equation*}$$ Es decir, obtenemos la identidad matricial, $$A\,Q_k=Q_{k+1}\,\widetilde{H}_k,$$ que corresponde a reducción parcial de $A$ a una forma de Hessenberg.
# ¿Cómo se utiliza lo recién obtenido para minimizar el error cuadrático?
Primero debemos recordar que es lo que estamos minimizando,$$
\begin{align*}
\overline{\mathbf{x}}_k&=\underset{\displaystyle{\widehat{\mathbf{x}}_k\in\mathcal{K}_k}}{\text{argmin}}\left\|\mathbf{b}-A\,\widehat{\mathbf{x}}_k\right\|_2^2.
\end{align*}
$$Lo cual es un problema de minimización pero con la restricción de que $\widehat{\mathbf{x}}_k\in\mathcal{K}_k$, es decir, que $\widehat{\mathbf{x}}_k$ debe pertenecer al sub-espacio de Krylov $\mathcal{K}_k$. Una alternativa para satisfacer esta restricción, es **parametrizar** $\mathcal{K}_k$ considerando la siguiente matriz,$$
K_k=\begin{bmatrix} 
	\mathbf{b}, & A\,\mathbf{b}, & \dots, & A^{k-1}\,\mathbf{b}
\end{bmatrix}.$$Es decir, $K_k$ corresponde a los vectores que definen $\mathcal{K}_k$. Entonces el **problema de minimización con restricciones** se puede convertir en un **problema de minimización sin restricciones**, lo cual genera la siguiente expresión,$$
\begin{align*}
	\overline{\tilde{\mathbf{c}}}_k &= \underset{\displaystyle{\tilde{\mathbf{c}}_k\in\mathbb{R}^k}}{\text{argmin}}\left\|\mathbf{b}-A\,K_k\,\tilde{\mathbf{c}}_k\right\|_2^2,\\
	\overline{\mathbf{x}}_k &= K_k\,\overline{\tilde{\mathbf{c}}}_k.
\end{align*}$$Lo cual es un avance parcial, debido a que numéricamente $K_k$ generará columnas _casi_ linealmente dependiente. **Este es un problema de mínimos cuadrados donde la matriz asociada, es decir $A\,K_k$, es de dimensión $n\times k$**. Desde el punto de vista gráfico, el problema de minimización anterior se traduce a:

```tikz
	\begin{document}
		\begin{tikzpicture}
		% Axis
		\draw [->, line width=1pt] (0,-2) -- (0,7);
		\draw [->, line width=1pt] (-2,0) -- (12,0);
		% square
		\draw [-,line width=1.5pt,black]  (6.9, 4) -- (6.5, 3.8);
		\draw [-,line width=1.5pt,black]  (6.75, 3.3) -- (6.5, 3.85);
		% A\,x
		\draw [-,line width=1.5pt,red]  (-1, -0.5) -- (10, 4.95);
		\draw [red] (3.2,1.3) node [below]{$A\,K_k\,\tilde{\mathbf{c}}_k$};
		% r
		\draw [->,line width=1.5pt,orange,dashed]  (7.1,3.5) -- (6.1,5.8);
		\draw [orange] (7.1, 5) node[above right]{$\mathbf{r}_k = \mathbf{b}-A\,K_k\,\overline{\tilde{\mathbf{c}}}_k$};
		% b
		\draw [fill=blue,blue] (6,6) circle  (2pt) node[above]{$\mathbf{b}$};
		\draw [->,line width=1.5pt,blue]  (0,0) -- (5.8,5.8);
		% A\,\overline{x} 
		\draw [fill=violet,violet] (7.1,3.5) circle  (2pt) node[below right]{$A\,K_k\, \overline{\tilde{\mathbf{c}}}_k$};
		\draw[->,dashed,line width=2.5pt,violet]  (0,0) -- (6.95,3.45);
		\end{tikzpicture}
	\end{document}
```
donde la _matriz_ del problema a minimizar es $A\,K_k$, a la cual, en principio, podría obtener su factorización QR, sin embargo, existe una mejor opción.
Por otro lado, la observación que es necesaria hacer acá es dado que el $\text{Range}(K_k)=\text{Range}(Q_k)$, entonces se satisface la siguiente identidad,$$
\begin{equation*}
	\widehat{\mathbf{x}}_k=K_k\,\tilde{\mathbf{c}}_k = Q_k\,\mathbf{c}_k.
\end{equation*}$$ Por lo tanto, nuestro problema de minimización se transforma de la siguiente forma,$$
\begin{align*}
	\overline{\mathbf{c}}_k &= \underset{\displaystyle{\mathbf{c}_k\in\mathbb{R}^k}}{\text{argmin}}\left\|\mathbf{b}-A\,Q_k\,\mathbf{c}_k\right\|_2^2,\\
	\overline{\mathbf{x}}_k &= Q_k\,\overline{\mathbf{c}}_k.
\end{align*}$$Ahora, si bien es una mejora, aún no es la forma final. Aquí es cuando podemos utilizar dos cosas:
1. La reducción parcial de Hessenberg: $A\,Q_k=Q_{k+1}\,\widetilde{H}_k$.
2. La definición de $\mathbf{q}_1=\dfrac{\mathbf{b}}{\|\mathbf{b}\|}$, pero re-escrita como $\mathbf{b}=\|\mathbf{b}\|Q_{k+1}\,\mathbf{e}_1$, donde $\mathbf{e}_1$ es el primer vector canónico de dimensión $k+1$.
Entonces, la minimización se puede simplificar de la siguiente forma,$$
\begin{align*}
	\overline{\mathbf{c}}_k &= \underset{\displaystyle{\mathbf{c}_k\in\mathbb{R}^k}}{\text{argmin}}\left\|\mathbf{b}-A\,Q_k\,\mathbf{c}_k\right\|_2^2\\
	&= \underset{\displaystyle{\mathbf{c}_k\in\mathbb{R}^k}}{\text{argmin}}\left\|\mathbf{b}-Q_{k+1}\,\widetilde{H}_k\,\mathbf{c}_k\right\|_2^2\\
	&= \underset{\displaystyle{\mathbf{c}_k\in\mathbb{R}^k}}{\text{argmin}}\left\|\|\mathbf{b}\|Q_{k+1}\,\mathbf{e}_1-Q_{k+1}\,\widetilde{H}_k\,\mathbf{c}_k\right\|_2^2\\
	&= \underset{\displaystyle{\mathbf{c}_k\in\mathbb{R}^k}}{\text{argmin}}\left\|Q_{k+1}\left(\|\mathbf{b}\|\,\mathbf{e}_1-\widetilde{H}_k\,\mathbf{c}_k\right)\right\|_2^2\\
	&= \underset{\displaystyle{\mathbf{c}_k\in\mathbb{R}^k}}{\text{argmin}}\left\|\|\mathbf{b}\|\,\mathbf{e}_1-\widetilde{H}_k\,\mathbf{c}_k\right\|_2^2\\
	\overline{\mathbf{x}}_k &= Q_k\,\overline{\mathbf{c}}_k.
\end{align*}$$Entonces, el problema de minimización de dimensión queda de la siguiente forma,$$
\begin{align*}
	\overline{\mathbf{c}}_k
	&= \underset{\displaystyle{\mathbf{c}_k\in\mathbb{R}^k}}{\text{argmin}}\left\|\|\mathbf{b}\|\,\mathbf{e}_1-\widetilde{H}_k\,\mathbf{c}_k\right\|_2^2\\
	\overline{\mathbf{x}}_k &= Q_k\,\overline{\mathbf{c}}_k.
\end{align*}$$El cual reduce el problema de mínimos cuadrados de uno de dimensión $n\times k$ a uno de dimensión $(k+1)\times k$. **¡Lo cual es una mejora significativa!**. Explícitamente se debe resolver el siguiente problema de minimización,$$\begin{equation}
	\overline{\mathbf{c}}_k
	= \underset{\displaystyle{\mathbf{c}_k\in\mathbb{R}^k}}{\text{argmin}}\left\|\begin{bmatrix} 
        	\|\mathbf{b}\|\\
        	0\\
        	0\\
        	\vdots\\
        	\vdots\\
        	0\\
        	0
    	\end{bmatrix}-\begin{bmatrix}
    	    h_{11} & h_{12} & \dots  & h_{1,k-1}  & h_{1,k} \\
    	    h_{21} & h_{22} & \dots  & h_{2,k-1}  &  h_{2,k} \\
    	    0      & h_{32} & \ddots & h_{3,k-1}  &  h_{3,k} \\
    	    \vdots & \ddots & \ddots & \ddots     & \vdots \\
    	    0      & \ddots  & \ddots &  h_{k,k-1} &  h_{k,k}\\
    	    0      & \dots  & \dots &  0         &  h_{k+1,k}
    	\end{bmatrix}
    	\begin{bmatrix} 
        	c_1\\
        	c_2\\
        	c_3\\
        	\vdots\\
        	c_{k-1}\\
        	c_k
    	\end{bmatrix}\right\|_2^2.
    \end{equation}$$Notar que $\left\|\mathbf{b}-A\,Q_k\,\mathbf{c}_k\right\|_2^2=\left\|\|\mathbf{b}\|\,\mathbf{e}_1-\widetilde{H}_k\,\mathbf{c}_k\right\|_2^2$, por lo que si la norma del problema pequeño es $0$ entonces también lo es la otra! Esto significa que se encontró la solución exacta! Esto ocurre cuando el coeficiente $h_{k+1,k}=0$ y se le denomina _breakdown_.
# El algoritmo
```python
# Assuming we execute "m" iterations, where m<n
x0 = # "Initial Guess"
r0 = b - np.dot(A, x0) # Initial residual # y = afun(x0)
nr0=np.linalg.norm(r0) # Storing the initial residual norm
Q = np.zeros((n,m+1))
H = np.zeros((n,m))
Q[:,0] = r0 / nr0
for k in np.arange(m):
	y = np.dot(A, Q[:,k]) # y = afun(Q[:,k])
	for j in np.arange(k+1):
		H[j,k] = np.dot(Q[:,j], y)
		y = y - np.dot(H[j,k],Q[:,j])
	H[k+1,k] = np.linalg.norm(y)
	if (np.abs(H[k+1,k]) > 1e-16):
		Q[:,k+1] = y/H[k+1,k]
	else:
		print('flag_break has been activated')
		flag_break=True
	# Do you remember e_1? The canonical vector.
	e1 = np.zeros((k+1)+1)        
	e1[0]=1
	H_tilde=H[0:(k+1)+1,0:k+1]
	# Solving the 'SMALL' least square problem. 
	ck = np.linalg.lstsq(H_tilde, nr0*e1,rcond=None)[0] 
	xk = x0 + np.dot(Q[:,0:(k+1)], ck)
```

# Ventajas y Desventajas
## Ventajas
- Permite resolver sistemas de ecuaciones lineales sin necesidad de operar sobre los elementos de la matriz $A$ y solo utiliza el producto matriz-vector asociado.
- Utiliza memoria proporcional al número de iteraciones, las cuales se pueden _reiniciar_.
- Puede encontrar la solución _exacta_ en menos de $n$ iteraciones, a esto se le denomina _breakdown_.
- Considerando aritmética exacta, encuentra la solución _exacta_ en a lo más $n$ iteraciones.
## Desventajas
- Los requerimientos de memoria crecen cuadráticamente con la cantidad de iteraciones.
- Requiere resolver un problema de mínimos cuadrados por iteración.
	- **Aunque se pueden utilizar rotaciones de _Givens_ para reducir la cantidad de operaciones elementales y además, al resolver con la factorización QR, se pueden reutilizar las factorizaciones previas.**
- Es más desafiante de entender **pero ayuda a resolver problemas que de otra forma requerirían mucha memoria**.

#OK
#Tema_4