La factorización QR **reducida** de una matriz $A\in\mathbb{R}^{m\times n}$ se representa de la siguiente forma,$$A=\widehat{Q}\,\widehat{R},$$donde $\widehat{Q}\in\mathbb{R}^{m\times n}$ es una **matriz unitaria** y $\widehat{R}\in\mathbb{R}^{n\times n}$ es una matriz triangular superior. De forma explícita podemos escribir la factorización QR de la siguiente forma,$$
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
\end{bmatrix}.$$ Adicionalmente sabemos lo siguiente,
1. $\|\mathbf{q}_i\|_2=1$.
2. $\langle \mathbf{q}_i,\mathbf{q}_j \rangle=\mathbf{q}_i^*\mathbf{q}_j=0$, $\forall\,i\neq j$.
# Resolución de las ecuaciones normales por medio de la factorización QR
La resolución de las ecuaciones normales por medio de la factorización QR _reducida_ (notar que también existe la factorización QR completa para lo cual recomienda estudiar los apuntes) se reduce a la siguiente expresión,$$\widehat{R}
        \,\overline{\mathbf{x}} = \widehat{Q}^T\,\mathbf{b},$$ donde se puede obtener $\overline{\mathbf{x}}$ por medio el algoritmo [[Backward Substitution]] dado que $\widehat{R}$ es triangular superior. Una forma de obtener la factorización QR reducida es por medio del la [[Ortonormalización de Gram-Schmidt]].

#OK
#Tema_7