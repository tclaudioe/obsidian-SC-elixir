![[09c_BVP.tex.png]]
Considere el siguiente BVP:$$
\begin{align*}
	y''(x)&=0, \,\, \text{para } x\in]0,L[,\\
	y(0)&=c_1,\\
	y(L)&=c_2.\\
\end{align*}
$$Donde lo que nos dice la ODE es lo siguiente: **¿Existe alguna función $y(x)$ tal que al derivarla dos veces se obtenga $0$ y que al evaluarla en $x=0$ su valor sea $c_1$ y al evaluarla en $x=L$ su valor sea $c_2$?**
La respuesta es **sí** y en esta caso particular corresponde a $$
\begin{align*}
	y(x)&=c_1\,\dfrac{x-L}{0-L}+c_2\,\dfrac{x-0}{L-0}\\
		&=c_1+\dfrac{c_2-c_1}{L}\,x.
\end{align*}$$
Ahora, si consideramos un caso un poco más general,$$
\begin{align*}
	a(x)\,y''(x)+b(x)\,y'(x)+c(x)\,y(x)&=f(x), \,\, \text{para } x\in]0,L[,\\
	y(0)&=u_0,\\
	y(L)&=u_L,
\end{align*}$$donde $a(x)\neq 0$ para $x\in]0,L[$. De forma gráfica,
```tikz
\begin{document}
	\begin{tikzpicture}
        \draw[line width=0.3mm,->] (-0.5,0) -- (5,0) node[below,pos=0.5] {$x$};
        \draw[line width=0.3mm,->] (0,-0.5) -- (0,2.5) node[below,pos=0] {$x=0$};
        \draw[line width=0.3mm,dashed] (0+4.5,-0.5) -- (0+4.5,2.5) node[below,pos=0] {$x=L$};
        \draw[-] (-0.1,1.5) -- (0.1,1.5) node[left,pos=0,blue] {$u_0$};
        \draw[-] (-0.1+4.5,1) -- (0.1+4.5,1) node[right,pos=1,blue] {$u_L$};
        \draw[-,red] (0,1.5) .. controls (1,0) and (3,0) .. (4.5,1);
        \end{tikzpicture}
\end{document}
```
Entonces, lo que buscamos acá es:
1. Una función $y(x)$ que al reemplazarla en la ODE obtenga la igualdad indicada. 
2. Que cumpla **ambas** condiciones de borde.
_En principio, al igual que en casos anteriores, si fuera posible resolver la ecuación de forma algebraica, se recomienda hacerlo. Pero, si el problema no se puede resolver de forma algebraica, podemos recurrir a métodos numéricos._
La diferencia principal acá radica en que la dependencia es espacial y no temporal, y como, en la mayoría de los casos, nos veremos enfrentados a BVP donde está involucrada la segunda derivada de la **función incógnita**, es decir $y''(x)$, necesitamos 2 condiciones de borde para obtener las 2 constantes de integración respectivas.

**¿Podemos integrar la BVP tal cual lo hicimos para la BVP?**

No en general, acá el enfoque será en trabajar con todo el **dominio** al mismo tiempo. Aunque hay una excepción, que es el uso de [[Método del Disparo]].

En este capítulo estudiaremos 2 algoritmos:
1. [[Método del Disparo]]
2. [[Diferencias Finitas]]

#OK 
#Tema_9  