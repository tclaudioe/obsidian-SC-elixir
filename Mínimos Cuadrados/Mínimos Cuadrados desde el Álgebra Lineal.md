En la introducción anterior, es decir [[Problemas de mínimos cuadrados]], se presentó la siguiente gráfica,
```tikz
\begin{document}
  \begin{tikzpicture}
            \draw[-,line width=1pt,violet]  (-1,1) -- (7,5);
            % Axis
            \draw[->,line width=1pt]  (-0.3,0) -- (7,0) coordinate[label = {below:$x$}] (xmax);
            \draw[->,line width=1pt]  (0,-0.3) -- (0,5) coordinate[label = {right:$y$}] (ymax); 
            % x's
            \draw[shift={(1,0)},color=black] (0pt,3pt) -- (0pt,-3pt) node [below]{$x_1$};
            \draw[shift={(2,0)},color=black] (0pt,3pt) -- (0pt,-3pt) node [below]{$x_2$};
            \draw[shift={(3,0)},color=black] (0pt,3pt) -- (0pt,-3pt) node [below]{$x_3$};
            \draw[shift={(3.5,0)},color=black] node [below]{$\ldots$};
            \draw[shift={(4,0)},color=black] (0pt,3pt) -- (0pt,-3pt) node [below]{$x_i$};
            \draw[shift={(4.5,0)},color=black] node [below]{$\ldots$};
            \draw[shift={(5,0)},color=black] (0pt,3pt) -- (0pt,-3pt) node [below]{$x_m$};
            % y's
            \draw [-,line width=1pt,blue]  (1,2) -- (1,2.7) node[left]{$y_1$};
            \draw [fill=red,red] (1,2) circle  (2pt) node[below,red]{$\widehat{y}_1$};
            \draw [fill=blue,blue] (1,2.7) circle  (2pt);
            
            \draw [-,line width=1pt,blue]  (2,2.5) -- (2,1.7) node[left]{$y_2$};
            \draw [fill=red,red] (2,2.5) circle  (2pt)node[above]{$\widehat{y}_2$};;
            \draw [fill=blue,blue] (2,1.7) circle  (2pt);
            
            \draw [-,line width=1pt,blue]   (3,3) -- (3,3.7) node[left]{$y_3$};
            \draw [fill=red,red] (3,3) circle  (2pt)node[below]{$\widehat{y}_3$};;
            \draw [fill=blue,blue] (3,3.7) circle  (2pt);
            
            \draw [-,line width=1pt,blue]   (4,3.5) -- (4,3) node[below]{$y_i$};
            \draw [fill=red,red] (4,3.5) circle  (2pt) node[above]{$\widehat{y}_i$};
            \draw [fill=blue,blue] (4,3) circle  (2pt);
            
            \draw [-,line width=1pt,blue]  (5,4) -- (5,3.2) node[left]{$y_m$};
            \draw [fill=red,red] (5,4) circle  (2pt)node[above]{$\widehat{y}_m$};
            \draw [fill=blue,blue] (5,3.2) circle  (2pt);
            
            \draw[->, black,line width=1pt]   (4,2.5) -- (4,1.5) node[below]{$a + b \cdot x_i=\widehat{y}_i \approx y_i$};
        \end{tikzpicture}
\end{document}
```
de la cual se deducen las siguientes ecuaciones,$$\begin{align*}
	a+b\,x_1 &=\widehat{y\hspace{1pt}}_1 \approx y_1,\\
	a+b\,x_2 &=\widehat{y\hspace{1pt}}_2 \approx y_2,\\
	\vdots \hspace{9pt} &= \hspace{2pt} \vdots \hspace{5pt} \approx \hspace{2pt} \vdots\\
	a+b\,x_m &=\widehat{y\hspace{1pt}}_m \approx y_m,\\
\end{align*}$$ las cuales se pueden representar de la siguiente forma,$$\begin{align*}
        a+b\,x_1 + r_1 &= y_1,\\
        a+b\,x_2 + r_2 &= y_2,\\
        a+b\,x_3 + r_3 &= y_3,\\
        \vdots & \\
        a+b\,x_m + r_m &= y_m.
    \end{align*}$$donde $r_i=y_i-\widehat{y\hspace{1pt}}_i$ representa el **residuo** e $\widehat{y\hspace{1pt}}_i=a+b\,x_i$ es la aproximación elegida para este problema. La cual puede escribirse vectorialmente de la siguiente forma:$$\underbrace{\begin{bmatrix}
            r_1 \\
			r_2 \\
			r_3 \\
			\vdots\\
			r_m 
        \end{bmatrix}}_{\displaystyle{\mathbf{r}}}
        =
        \underbrace{\begin{bmatrix}
            y_1 \\
			y_2 \\
			y_3 \\
			\vdots\\
			y_m 
        \end{bmatrix}}_{\displaystyle{\mathbf{b}}}
        -
        \underbrace{\begin{bmatrix}
            1 & x_1 \\
			1 & x_2 \\
			1 & x_3 \\
			\vdots & \vdots \\
			1 & x_m
        \end{bmatrix}}_{\displaystyle{A}}
        \begin{bmatrix}
            a\\
            b
        \end{bmatrix}.$$Ahora, escribiendo la matriz $A=[\mathbf{v}_1,\mathbf{v}_2]$ y
recordando que queremos minimizar el error cuadrático, notamos que podemos escribirlo directamente de la siguiente forma,$$E(a,b)=\|\mathbf{r}\|_2^2=\left\|\mathbf{b}-a\,\mathbf{v}_1 - b\,\mathbf{v}_2\right\|_2^2.$$Más aún, podemos _graficar_ esta relación de la siguiente manera,
```tikz
\begin{document}
\begin{tikzpicture}
            % v2
            \draw[->,line width=1.5pt]  (0,0) -- (3,3) node[above]{$\mathbf{v}_2$};
            \draw[-,dashed,line width=1pt,red]  (3,3) -- (5,5);
            % b
            \draw[->,line width=1pt,blue]  (0,0) -- (5,3);
            \draw [fill=blue,blue] (5.1,3) circle  (2pt) node[right]{$\mathbf{b}$};
            % A\,\overline{x}
            \draw[->,dashed,line width=1pt,violet]  (0,0) -- (5,2);
            \draw [fill=blue,violet] (5.1,2) circle  (2pt)node[below]{$A\,\overline{\mathbf{x}}$};
            % Blue message
            \draw[->,blue, line width=1pt]  (5.2,2.5) -- (5.5,2.5) node[right]{$\mathbf{r}$: Se quiere minimizar esta distancia, es decir $\|\mathbf{r}\|_2$};
            % r=b-A\,\overline{x}
            \draw[<-,dashed,line width=1pt,blue]  (5.1,2.9) -- (5.1,2);
            % sub-space
            \draw [red] (4,1) node[below]{$a \cdot \mathbf{v}_1 + b \cdot \mathbf{v}_2 = A \, \mathbf{x}$};
            % v1
            \draw[->,line width=1.5pt]  (0,0) -- (3,0) node[above]{$\mathbf{v}_1$};
            \draw[-,dashed,line width=1pt,red]  (5,5) -- (13,5);
            \draw[-,dashed,line width=1pt,red]  (9,0) -- (13,5);
            \draw[-,dashed,line width=1pt,red]  (3,0) -- (9,0);
            % Sub-space text
            \draw [red] (5,4.5) node[right, text width=6.4cm]{sub-espacio vectorial definido por $\mathbf{v}_1$ y $\mathbf{v}_2$, es decir corresponde a span$(\mathbf{v}_1,\mathbf{v}_2)$.};
        \end{tikzpicture}
\end{document}
```
lo cual se puede interpretar de la siguiente forma:
_La "mejor" combinación lineal de los $\mathbf{v}_1$ y $\mathbf{v}_2$ que minimiza la norma del residuo es ortogonal al vector residual._
Lo cual implica la siguiente ecuación:$$
\left\langle A\,\mathbf{x},\mathbf{b}-A\,\overline{\mathbf{x}}\right\rangle=\left(A\,\mathbf{x}\right)^T\,\left(\mathbf{b}-A\,\overline{\mathbf{x}}\right)=0,
$$de la cual se deducen las **ecuaciones normales** #EcuacionesNormales,$$A^*\,A\,\overline{\mathbf{x}}=A^*\,\mathbf{b}.$$En su versión general se presenta de la siguiente forma,
```tikz
\begin{document}
\begin{tikzpicture}[scale=0.5]
            % A x
		    \draw[-,red,line width=1pt]  (-2,-2) -- (8,8);
		    % x-axis
            \draw[->,line width=1pt]  (-3,0) -- (10,0);
			% y-axis
            \draw[->,line width=1pt]  (0,-3) -- (0,8);
			% A\,\overline{x} 
            \draw [fill=violet,violet] (5,5) circle  (4pt) node[below right]{$A\, \overline{\mathbf{x}}$};
            \draw[<-,violet, line width=1pt]  (6.5,4.5) -- (7,4.5) node[right]{donde $\overline{\mathbf{x}}$ es el minimizador.};
            \draw[->,dashed,line width=1.5pt,violet]  (0,0) -- (4.9,4.9);
            % b
            \draw [fill=blue,blue] (3.2,7) circle  (4pt) node[above]{$\mathbf{b}$};
            \draw[->,line width=1.5pt,blue]  (0,0) -- (3.1,6.8);
            % r
            \draw [blue] (4, 6.0) node[above right]{$\mathbf{r}$};%$=\mathbf{b}-A \, \bar{\mathbf{x}}$};
            \draw[<-,dashed,line width=1.5pt,blue]  (3.3,6.8) -- (4.9,5);
            % A\,x
            \draw[<-,red, line width=1pt]  (7.3,7) -- (8,7);
            \draw [red] (8.3, 7) node[right,text width=4cm]{$A \, \mathbf{x}$, sub-espacio vectorial generado por las columnas de $A$.};
            % right-angle
            \draw[-,line width=1pt] (4.5,4.5) -- (4,5);
            \draw[-,line width=1pt] (4,5) -- (4.5,5.5);
		\end{tikzpicture}
\end{document}
```

Retomando el caso inicial, escribir explícitamente las ecuaciones normales,$$\begin{bmatrix}
            1 & 1 & 1 & \dots &  1 \\
			x_1 & x_2 & x_3 & \dots &  x_m \\
        \end{bmatrix}
        \begin{bmatrix}
            1 & x_1 \\
			1 & x_2 \\
			1 & x_3 \\
			\vdots & \vdots \\
			1 & x_m
        \end{bmatrix}
        \begin{bmatrix}
            \overline{a}\\
            \overline{b}
        \end{bmatrix}
        =
        \begin{bmatrix}
            1 & 1 & 1 & \dots &  1 \\
			x_1 & x_2 & x_3 & \dots &  x_m \\
        \end{bmatrix}
        \begin{bmatrix}
            y_1 \\
			y_2 \\
			y_3 \\
			\vdots\\
			y_m 
        \end{bmatrix}.$$Al multiplicar las matrices se obtiene,$$\begin{bmatrix}
            m & \displaystyle{\sum_{i=1}^{m} x_i} \\
    		\displaystyle{\sum_{i=1}^{m} x_i} & \displaystyle{\sum_{i=1}^{m} x_{i}^2}
        \end{bmatrix}
        \begin{bmatrix}
            \overline{a}\\
            \overline{b}
        \end{bmatrix}
        =
        \begin{bmatrix}
            \displaystyle{\sum_{i=1}^{m} y_{i}} \\
    		\displaystyle{\sum_{i=1}^{m} y_{i} \, x_i}
        \end{bmatrix}.$$ El cual es el mismo sistema de ecuaciones lineales que se obtuvo antes en [[Mínimos cuadrados por minimización]]!! Lo que indica que ambos procedimientos son equivalentes!

En resumen, este procedimiento nos entregó las **ecuaciones normales**, es decir, $A^*\,A\,\overline{\mathbf{x}}=A^*\,\mathbf{b}$, las cuales efectivamente entregan el vector $\overline{\mathbf{x}}$ que minimiza el error cuadrático $\|\mathbf{b}-A\,\mathbf{x}\|_2^2$.

# Ventajas
1. Procedimiento alternativo a [[Mínimos cuadrados por minimización]] que no requiere la computación de ningún gradiente.
2. Permite conectar los problemas de mínimos cuadrados con Álgebra Lineal.
# Desventajas
1. Requiere la computación de las ecuaciones normales, la cual es un producto matriz-matriz.
2. Si la matriz $A$ es mal condicionada, el producto $A^*\,A$ elevará al cuadrado el número de condición, lo cual empeora significativamente el problema.

# Alternativa a las ecuaciones normales
La alternativa a la utilización **explícita** de las ecuaciones normales, es la utilización **implícita** de las ecuaciones normales por medio de la [[Factorización QR]] que puede construirse con la [[Ortonormalización de Gram-Schmidt]].

#OK
#Tema_7
