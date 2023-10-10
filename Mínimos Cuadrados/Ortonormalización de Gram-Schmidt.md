La computación de la [[Factorización QR]] se puede realizar por el **algoritmo de la ortonormalización de Gram-Schmidt**. Sin embargo es importante notar que existen otros algoritmos que están fuera del alcance de estos apuntes. 
Las versiones de la ortonormalización de Gram-Schmidt que se estudiarán son las siguientes:
1. **Ortonormalización de Gram-Schmidt clásica**
2. **Ortonormalización de Gram-Schmidt modificada**
Por simplicidad de explicación, se presentarán ambas variantes juntas y se explicará donde difieren.

Para iniciar la explicación, retomemos la siguiente representación,$$
\begin{bmatrix} 
	\mathbf{a}_{1}, & \mathbf{a}_{2}, & \dots, & \mathbf{a}_{n}
\end{bmatrix}
=
\begin{bmatrix} 
	\mathbf{q}_{1}, & \mathbf{q}_{2}, & \dots, & \mathbf{q}_{n}
\end{bmatrix}
\begin{bmatrix}
	r_{11} & r_{12} & r_{13} & \dots  &  r_{1n} \\
	0      & r_{22} & r_{23} & \dots  &  r_{2n} \\
	0      & 0      & r_{33} & \dots  &  r_{3n} \\
	\vdots & \vdots & \ddots & \ddots &  \vdots \\
	0      & \dots  & \dots  &  0     &  r_{nn}
\end{bmatrix}.$$
Al hacer el producto del lado derecho obtenemos las siguientes ecuaciones,
 $\mathbf{a}_{1}=r_{11}\,\mathbf{q}_{1}$
- $\mathbf{a}_{2}=r_{12}\,\mathbf{q}_{1}+r_{22}\,\mathbf{q}_{2}$
- $\mathbf{a}_{3}=r_{13}\,\mathbf{q}_{1}+r_{23}\,\mathbf{q}_{2}+r_{33}\,\mathbf{q}_{3}$
- ...
- $\mathbf{a}_k=\displaystyle\sum_{i=1}^{k} r_{ik}\,\mathbf{q}_{i}$
- ...
- $\mathbf{a}_{n}=\displaystyle\sum_{i=1}^{n} r_{in}\,\mathbf{q}_{i}$
# Primera ecuación - clásica y modificada hacen lo mismo
$$\textcolor{blue}{\mathbf{a}_{1}}=\textcolor{red}{r_{11}\,\mathbf{q}_{1}}$$ Lo que está en $\textcolor{blue}{\text{azul}}$ es conocido y lo que está en $\textcolor{red}{\text{rojo}}$ se debe determinar.
1. Se puede obtener la norma del lado izquierdo y debe ser igual al lado derecho. 
2. Esto implica que $\|\mathbf{a}_1\|=|r_{11}|$ dado que sabemos que $\|\mathbf{q}_1\|=1$.
3. Por **convención** se determina que $r_{ii}>0$, por lo tanto $$r_{11}=\|\mathbf{a}_1\|.$$
4. Ahora que conocemos $r_{11}$, solo despejamos $\mathbf{q}_1$, entonces,$$\mathbf{q}_1=\dfrac{\mathbf{a}_1}{r_{11}}.$$
# Segunda ecuación - clásica y modificada hacen lo mismo
$$\textcolor{blue}{\mathbf{a}_{2}}=\textcolor{red}{r_{12}}\,\textcolor{blue}{\mathbf{q}_{1}}+\textcolor{red}{r_{22}\,\mathbf{q}_{2}}$$
1. Multiplicar por la izquierda por $\mathbf{q}_1^T$, _que es equivalente a decir que se hace el producto interno con $\mathbf{q}_1^T$_. Esto no entrega $r_{12}$ por la siguiente razón,$$\begin{align*}
	    \mathbf{a}_{2} &= r_{12}\,\mathbf{q}_{1}+r_{22}\,\mathbf{q}_{2},\\
	    \mathbf{q}_{2}^T\,\mathbf{a}_{2} &= r_{12}\,\underbrace{\mathbf{q}_{2}^T\mathbf{q}_{1}}_{0}+
	    r_{22}\,\underbrace{\mathbf{q}_{2}^T\mathbf{q}_{2}}_{1},\\
	    \mathbf{q}_{2}^T\,\mathbf{a}_{2} &= r_{22}.
	\end{align*}$$
2. Ahora movemos lo conocido al lado izquierdo y obtenemos,$$\mathbf{a}_{2} - r_{12}\,\mathbf{q}_{1}=r_{22}\,\mathbf{q}_{2}.$$Esto no deja una ecuación con la misma _estructura_ que la "Primera ecuación" analizada anteriormente, por lo tanto al aplicar la norma obtenemos,$$\|\mathbf{a}_{2} - r_{12}\,\mathbf{q}_{1}\|=r_{22}.$$
3. Finalmente _despejamos_ $\mathbf{q}_2$ y obtenemos,$$\mathbf{q}_{2} = \dfrac{\mathbf{a}_{2}-r_{12}\,\mathbf{q}_{1}}{r_{22}}.$$
# Tercera ecuación - AHORA si empieza la diferencia entre la ortonormalización de Gram-Schmidt clásica y modificada!
$$\textcolor{blue}{\mathbf{a}_{3}}
	    =
	    \textcolor{red}{r_{13}}
	    \,
	    \textcolor{blue}{\mathbf{q}_{1}}
	    +
	    \textcolor{red}{r_{23}}
	    \,
	    \textcolor{blue}{\mathbf{q}_{2}}
	    +
	    \textcolor{red}{r_{33}}
	    \,
	    \textcolor{red}{\mathbf{q}_{3}}.$$
## Primer paso - iguales
Multiplicamos por $\mathbf{q}_1^T$ por la izquierda,$$\begin{align*}
	    \mathbf{a}_{3} &=
	    r_{13}\,\mathbf{q}_{1}+r_{23}\,\mathbf{q}_{2}+r_{33}\,\mathbf{q}_{3},\\
	    \mathbf{q}_1^T\,\mathbf{a}_{3} &=
	    r_{13}\,\mathbf{q}_1^T\,\mathbf{q}_{1}+r_{23}\,\mathbf{q}_1^T\,\mathbf{q}_{2}+r_{33}\,\mathbf{q}_1^T\,\mathbf{q}_{3},\\
	     &=
	    r_{13}\,\underbrace{\mathbf{q}_1^T\,\mathbf{q}_{1}}_{1}+r_{23}\,\underbrace{\mathbf{q}_1^T\,\mathbf{q}_{2}}_{0}+r_{33}\,\underbrace{\mathbf{q}_1^T\,\mathbf{q}_{3}}_{0},\\
	     &=
	    r_{13}.
	\end{align*}$$ Por lo tanto,
$$r_{13}=\mathbf{q}_1^T\,\mathbf{a}_3$$
## Segundo paso - AQUÍ empieza la diferencia
### clásica
Multiplicamos por $\mathbf{q}_2^T$ por la izquierda,$$\begin{align*}
	    \mathbf{a}_{3} &=
	    r_{13}\,\mathbf{q}_{1}+r_{23}\,\mathbf{q}_{2}+r_{33}\,\mathbf{q}_{3},\nonumber\\
	    \mathbf{q}_2^T\,\mathbf{a}_{3} &=
	    r_{13}\,\mathbf{q}_2^T\,\mathbf{q}_{1}+r_{23}\,\mathbf{q}_2^T\,\mathbf{q}_{2}+r_{33}\,\mathbf{q}_2^T\,\mathbf{q}_{3},\\
	     &=
	    r_{13}\,\underbrace{\mathbf{q}_2^T\,\mathbf{q}_{1}}_{0}+r_{23}\,\underbrace{\mathbf{q}_2^T\,\mathbf{q}_{2}}_{1}+r_{33}\,\underbrace{\mathbf{q}_2^T\,\mathbf{q}_{3}}_{0},\nonumber\\
	     &=
	    r_{23}.\nonumber
	\end{align*}$$Por lo tanto,$$r_{23}=\mathbf{q}_2^T\,\mathbf{a}_3$$
### modificada
En este caso primero se realiza una manipulación algebraica considerando que ya se conoce $r_{13}$ obtenida en el "Primer paso",$$\begin{align*}
        \mathbf{a}_{3} &=
	    r_{13}\,\mathbf{q}_{1}+r_{23}\,\mathbf{q}_{2}+r_{33}\,\mathbf{q}_{3},\\
	    \mathbf{a}_{3} - r_{13}\,\mathbf{q}_{1}&=
	    r_{23}\,\mathbf{q}_{2}+r_{33}\,\mathbf{q}_{3},\\
	    \mathbf{q}_2^T\,\left(\mathbf{a}_{3} - r_{13}\,\mathbf{q}_{1}\right)&=
	    r_{23}\,\mathbf{q}_2^T\,\mathbf{q}_{2}+r_{33}\,\mathbf{q}_2^T\,\mathbf{q}_{3}\\
	     &=
	    r_{23}\,\underbrace{\mathbf{q}_2^T\,\mathbf{q}_{2}}_{1}+r_{33}\,\underbrace{\mathbf{q}_2^T\,\mathbf{q}_{3}}_{0},\nonumber\\
	     &=
	    r_{23}.\nonumber
	\end{align*}$$Por lo tanto,$$r_{23}=\mathbf{q}_2^T\,\left(\mathbf{a}_{3} - r_{13}\,\mathbf{q}_{1}\right)$$Una posible interpretación es considerar que al trabajar con [[Estándar de punto flotante IEEE 754]] uno está realizando aproximaciones que se ven afectadas por [[Pérdida de importancia y errores de cancelación]], lo que indica que lo que ocurre en **aritmética exacta** no necesariamente se replica en **aritmética de punto flotante**, por lo tanto al evitar hacer cancelaciones que vienen de la **aritmética exacta** en la construcción de algoritmos para **aritmética de punto flotante**, se puede mejorar el comportamiento numérico de los algoritmos bajo análisis.

## Paso general - clásica y modificada

```python
type_gram_schmidt = 'classic' # or 'modified'
for k in range(n):
        y = A[:,k]
        for i in range(k):
            if type_gram_schmidt == 'classic':
                R[i,k] = np.dot(Q[:,i],A[:,k])
            elif type_gram_schmidt == 'modified':
                R[i,k] = np.dot(Q[:,i],y)
            y=y-R[i,k]*Q[:,i]
        R[k,k] = np.linalg.norm(y)
        Q[:,k] = y/np.linalg.norm(R[k,k])
```

#OK
#Tema_7 

