Supuestos:
1. Se tiene acceso a una función $f(x)$ continua que tiene solo $1$ raíz en el intervalo $[a,b]$, para $a<b$.
2. Se sabe que $f(a)\,f(b)<0$. Esto asegura que hay una raíz en el intervalo $[a,b]$. Si el producto $f(a)\,f(b)=0$ entonces la raíz es $a$ o $b$ y el problema se resuelve de inmediato.
3. Si el producto $f(a)\,f(b)>0$ entonces no podemos asegurar que exista una raíz en el intervalo $[a,b]$.
```tikz
\begin{document}
\begin{tikzpicture}[domain=-3:5]
	\node[anchor=east] at (0.0,0.0) (origenreal) {};
	\node at (-0.2,0.1) (origen) {};
	\draw[very thin,color=gray] (-2,-2) grid (5,3);	
	% ejes
	\draw [->] (-3,0) -> (5,0);
	\draw [->] (0,-3) -> (0,3);
	% parábola
	\node[anchor=east] at (0,-1.5) (iii) {$f(x)$};
	\node[anchor=east] at (5,2) (ddd) {};
	\draw (iii) edge[-,out=0,in=240] (ddd);
	
	\node[anchor=east] at (5.5,0) (x) {$x$};
	\node[anchor=north] at (0,3.5) (y) {$y$};
	\draw[shift={(1,0)},color=black] (0pt,3pt) -- (0pt,-3pt) node [below]{$a$};
	\draw[shift={(3.45,0)},color=black] (0pt,3pt) -- (0pt,-3pt) node [below]{$r$};
	\draw[shift={(4.5,0)},color=black] (0pt,3pt) -- (0pt,-3pt) node [below]{$b$};
	\node at (1, -1.42) (oy) {$\cdot$};
	\node at (1, -1.67) (oy) {$f(a)$};
	\node at (4.5, 1.39) (oy) {$\cdot$};
	\node at (4.3, 1.8) (oy) {$f(b)$};
\end{tikzpicture}
\end{document}
```
# Algoritmo:
En el caso de que $f(a)\,f(b)<0$, entonces podemos acotar el intervalo de donde se encuentra la raíz de la siguiente forma:
1. Definir $c=\dfrac{a+b}{2}$, esta es la estimación de la raíz.
2. Evaluar $f(c)$: Si es $0$, problema resuelto.
3. Determinar si $f(a)\,f(c)<0$:
	1. Si el producto es menor que $0$, entonces la raíz está en el intervalo $[a,c]$. Se asigna el valor de $c$ a $b$. Volver al paso 1.
	2. En caso contrario, entonces la raíz está en el intervalo $[c,b]$. Se asigna el valor de $c$ a $a$. Volver al paso 1.
El algoritmo se ejecuta hasta que se cumpla algún [[Criterio de detención para búsqueda de raíces 1D]].

```tikz
\begin{document}
\begin{tikzpicture}
	\node[anchor=east] at (0.0,0.0) (origenreal) {};
	\node at (-0.2,0.1) (origen) {};
	\draw[very thin,color=gray] (-2,-2) grid (5,3);	
	% ejes
	\draw [->] (-3,0) -> (5,0);
	\draw [->] (0,-3) -> (0,3);
	% parábola
	\node[anchor=east] at (0,-1.5) (iii) {$f(x)$};
	\node[anchor=east] at (5,2) (ddd) {};
	\draw (iii) edge[-,out=0,in=240] (ddd);
	
	\node[anchor=east] at (5.5,0) (x) {$x$};
	\node[anchor=north] at (0,3.5) (y) {$y$};
	\draw[shift={(1,0)},color=black] (0pt,3pt) -- (0pt,-3pt) node [below]{$a$};
	\draw[shift={(3.45,0)},color=black] (0pt,3pt) -- (0pt,-3pt) node [below]{$r$};
	\draw[shift={(4.5,0)},color=black] (0pt,3pt) -- (0pt,-3pt) node [below]{$b$};
	\draw[shift={(2.75,0)},color=black] (0pt,3pt) -- (0pt,-3pt) node [below]{$c$};
	\node at (1, -1.42) (oy) {$\cdot$};
	\node at (1, -1.67) (oy) {$f(a)$};
	\node at (4.5, 1.39) (oy) {$\cdot$};
	\node at (4.3, 1.8) (oy) {$f(b)$};
	\node at (2.75, -0.65) (oy) {$\cdot$};
	\node at (2.75, -1) (oy) {$f(c)$};
\end{tikzpicture}
\end{document}
```

Una característica importante del método de la bisección es que en cada iteración es que en cada iteración el error disminuye a la mitad, lo que permite definir el error de la siguiente forma: $$|x_k-r|\leq \dfrac{b-a}{2^{k+1}}$$ La cual requiere $k+2$ evaluaciones de $f(x)$.

- Ejemplo: Búsqueda de la raíz de $f(x)=x-\cos(x)$
```run-python
f = lambda x: x-np.cos(x)
r=bisect(f,0,3,tol=1e-10)
print(f(r))

# f = lambda x: np.power(x,2.)-2
# r=bisect(f,0,2)
# print(f(r))
```

#OK
#Tema_3