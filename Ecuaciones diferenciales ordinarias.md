Hasta este puntos hemos discutido diversos temas, por ejemplo:
1. [[Estándar de punto flotante IEEE 754]]
2. [[Pérdida de importancia y errores de cancelación]]
3. [[Raíces en 1D]]
4. [[Resolución numérica de sistemas de ecuaciones lineales]]
5. [[Resolución numérica de sistemas de ecuaciones no-lineales]]
6. [[Interpolación Polinomial]]
7. [[Problemas de Mínimos Cuadrados]]
8. [[Algoritmos de integración numérica]]
Los cuales se podrían haber entendido como temas **independientes**, sin embargo ya sabemos que en realidad todos los temas se van acoplando de una u otra forma.

En esta sección, se irán conectando naturalmente todos estos temas y es importante poder aplicar el conocimiento y competencias adquiridas de las secciones previas.

# Introducción
En las secciones anteriores, por ejemplo, se buscaba una escalar, un vector o una representación continua, es decir:
- En [[Raíces en 1D]] se busca un escalar, denominado $r$, tal que $f(r)=0$.
- En [[Resolución numérica de sistemas de ecuaciones lineales]] y [[Resolución numérica de sistemas de ecuaciones no-lineales]] se buscaba un vector, digamos $\mathbf{x}$, tal que $A\,\mathbf{x}=\mathbf{b}$ o, $\mathbf{F}(\mathbf{x})=\mathbf{0}$.
- En [[Interpolación Polinomial]] se busca una representación continua, es decir un polinomio $p(x)$, tal que interpole un conjunto de datos $p(x_i)=y_i$.
- En [[Problemas de Mínimos Cuadrados]] se busca un vector que minimice el error cuadrático, es decir $\overline{\mathbf{x}}=\underset{\mathbf{x}\in\mathbb{R}^n}{\text{argmin}}\|\mathbf{b}-A\,\mathbf{x}\|$, que también, en algunos casos, se conecta con una representación continua, como lo sería, por ejemplo, una recta que minimiza el error cuadrático.
- En [[Algoritmos de integración numérica]] se busca el valor de la integral, es decir $c$, tal que $c\approx \displaystyle\sum_{i=1}^n f(x_i)\,w_i$.
Ahora, en ODE (del _inglés_ Ordinary Differential Equations) lo que se tiene es una **ecuación diferencial** donde la incógnita es **una función** de **una variable**. En particular, se consideran dos posibles casos:
- $y(t)$, es decir una función que dependen de una variable _temporal_. A estos problemas se les denomina [[Problemas de Valor Inicial]] o **Initial Value Problems**.
- $y(x)$, es decir una función que depende de una variable _espacial_. A estos problemas se les denomina [[Problemas de Valor de Frontera]] o **Boundary Value Problems**.
En general, cada uno de estos casos requiere un trato levemente distinto, por lo cual se explicará en cada uno de sus archivos el detalle.

# ¿Qué sería más general que ODE?
La respuesta es simple, algo más general que ODE sería PDE, que corresponde a **Partial Differential Equations**. A modo de ejemplo considere el siguiente IBVP (_Initial Boundary Value Problem_),
$$
\begin{align*}
	u_t(x,t) &=\dfrac{\partial^2 u}{\partial x^2}(x,t),\quad x\in[0,L]\text{ y }t\in]0,T],\text{ PDE},\\
	u(x,0)&=f(x),\quad\text{Condición inicial},\\
	u(0,t)&=l(t),\quad\text{Condición de borde izquierdo},\\
	u(L,t)&=r(t),\quad\text{Condición de borde derecho}.	
\end{align*}
$$
Esta es la _famosa_ ecuación de calor!
```tikz
\begin{document}
\begin{tikzpicture}[scale=0.5]
        %points
        \node[draw,circle,fill=black,scale=0.4] at (0,4-0.5) {}; 
        \node[draw,circle,fill=black,scale=0.4] at (0+4,4+4) {}; 
        % u(x,t) axis
        \draw[->] (0,-0.2) -- (0,7) node[below,pos=0] {$t=0$} node[left,pos=1] {$u(x,t)$};
        \draw[-,dashed] (0+4,-0.2+4) -- (0+4,7+4);
        %t axis
        \draw[->] (-0.2,0) -- (14,0) node[left,pos=0] {$x=0$} node[below,pos=0.5] {$t$};
        \draw[->] (-0.2+4,0+4) -- (14+4,0+4) node[left,pos=0] {$x=L$};
        %x axis
        \draw[-] (0,0) -- (4,4) node[above left,pos=0.5] {$x$};
        %dashed lines
        \draw[dashed,gray!50] (1,0) -- (1+4,0+4);
        \draw[dashed,gray!50] (2,0) -- (2+4,0+4);
        \draw[dashed,gray!50] (3,0) -- (3+4,0+4);
        \draw[dashed,gray!50] (4,0) -- (4+4,0+4);
        \draw[dashed,gray!50] (5,0) -- (5+4,0+4);
        \draw[dashed,gray!50] (6,0) -- (6+4,0+4);
        \draw[dashed,gray!50] (7,0) -- (7+4,0+4);
        \draw[dashed,gray!50] (8,0) -- (8+4,0+4);
        \draw[dashed,gray!50] (9,0) -- (9+4,0+4);
        \draw[dashed,gray!50] (10,0) -- (10+4,0+4);
        \draw[dashed,gray!50] (11,0) -- (11+4,0+4);
        %curve in x=0
        \draw[line width = 0.3mm] plot [smooth, tension=0.5] coordinates {(0,4-0.5)
        					 (1,3.9-0.5)
        					 (2,3.7-0.5)
        					 (3,3.8-0.5)
        					 (4,3.5-0.5)
        					 (5,3.2-0.5)
        					 (6,3.0-0.5) 
        					 (7,2.8-0.5) 
        					 (8,2.9-0.5) 
        					 (9,3.1-0.5) 
        					 (10,3.4-0.5) 
        					 (11,3.2-0.5) 
        					 (12,3.1-0.5)};
        %curve in x=L
        \draw[line width = 0.3mm] plot [smooth, tension=0.5] coordinates {(0+4,4+4) 
        					 (1+4,3.9+4) 
        					 (2+4,3.7+4)
        					 (3+4,3.8+4) 
        					 (4+4,3.5+4) 
        					 (5+4,3.2+4) 
        					 (6+4,3.0+4) 
        					 (7+4,2.8+4) 
        					 (8+4,2.9+4) 
        					 (9+4,3.1+4) 
        					 (10+4,3.4+4) 
        					 (11+4,3.2+4) 
        					 (12+4,3.1+4)};
        %curve in t=0
        \draw[line width = 0.3mm] (0,4-0.5) .. controls (1,6) and (3,3) .. (0+4,4+4);
        %curves in other times
        \draw[gray] (1,3.9-0.5) .. controls (2,6) and (4,3) .. (1+4,3.9+4);
        \draw[gray] (2,3.7-0.5) .. controls (3,6) and (5,3) .. (2+4,3.7+4);
        \draw[gray] (3,3.8-0.5) .. controls (4,6) and (6,3) .. (3+4,3.8+4);
        \draw[gray] (4,3.5-0.5) .. controls (5,6) and (7,3) .. (4+4,3.5+4);
        \draw[gray] (5,3.2-0.5) .. controls (6,6) and (8,3) .. (5+4,3.2+4);
        \draw[gray] (6,3.0-0.5) .. controls (7,6) and (9,3) .. (6+4,3.0+4);
        \draw[gray] (7,2.8-0.5) .. controls (8,6) and (10,3) .. (7+4,2.8+4);
        \draw[gray] (8,2.9-0.5) .. controls (9,6) and (11,3) .. (8+4,2.9+4);
        \draw[gray] (9,3.1-0.5) .. controls (10,6) and (12,3) .. (9+4,3.1+4);
        \draw[gray] (10,3.4-0.5) .. controls (11,6) and (13,3) .. (10+4,3.4+4);
        \draw[gray] (11,3.2-0.5) .. controls (12,6) and (14,3) .. (11+4,3.2+4);
        %some labels
        \node at (1.65,6) {$u(x,t=0)$};
        \node[right] at (12,3.1-0.5) {$u(x=0,t)$};
        \node[right] at (12+4,3.1+4) {$u(x=L,t)$};
        %final arrow with label
        \draw[->] (8,5) -- (9,9) node[above,pos=1] {$u(x,t)$};
        \end{tikzpicture}
\end{document}
```
_Créditos imagen original: Profesor Cristopher Arenas. Updated in 2021._

Lo interesante, es que con los métodos que estudiemos **ODEs** se pueden resolver **PDEs**!

# Referencia Interesante
- [Exploring ODEs](https://people.maths.ox.ac.uk/trefethen/ExplODE/), ver PDF descargable.

# Secciones a estudiar
1.  [[Problemas de Valor Inicial]]
2.  [[Problemas de Valor de Frontera]] 

#OK 
#Tema_9
