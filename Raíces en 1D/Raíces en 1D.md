#Definition La función $f(x)$ tiene una raíz en $x = r$ si $f(r) = 0$. #root

```tikz
\begin{document}
  \begin{tikzpicture}[domain=0:4]
    \draw[very thin,color=gray] (-0.1,-1.1) grid (3.9,3.9);
    \draw[->] (-0.2,0) -- (4.2,0) node[right] {$x$};
    \draw[->] (0,-1.2) -- (0,4.2) node[above] {$y$};
    \draw[color=orange]    plot (\x,\x^2/2-\x/2-1) node[right] {$f(x)$};
  \end{tikzpicture}
\end{document}
```

La idea en este tema es que dada una función  $f(x)$ se quiere encontrar el número real $r$ (en realidad un número #doublePrecision) que al evaluarla en la función da $0$.

# Restricciones:
### **Disponible**
- Se puede considerar por ahora es que uno tiene a su disposición la posibilidad de evaluar la función a la cual se le busca la raíz
- Adicionalmente se puede considerar que se tiene a disposición solo las operaciones elementales: Suma, Resta, Multiplicación y División.
### **No Disponible**
- Evaluar la función inversa. 
- Esto significa que si uno quiere encontrar la raíz de la función $f(x)$ significa resolver la ecuación $f(r)=0$. En este caso de puede **despejar** la incógnita $r$ utilizando la función inversa de $f(x)$, es decir $r=f^{-1}(0)$. Notar que nos referimos a la función inversa de $f(x)$ a la función $f^{-1}(x)$ que satisface la siguiente identidad $f^{-1}(f(x))=x$ y **no** a la función  $\dfrac{1}{f(x)}$.

# Variantes:
1. Problema: ¿Cómo encontramos cuando una función $\tilde{f}(x)$ sea igual a 2? O a cualquier constante en realidad.
	1. Solución: Definir una nueva función $f(x)=\tilde{f}(x)-2$ y buscar la raíz de $f(x)$.
2. Problema: ¿Buscar un punto crítico de $f(x)$?
	1. Solución: Buscar la/s raíces de $f'(x)$.
3. Problema: Encontrar la intersección de las funciones $f_1(x)$ y $f_2(x)$.
	1. Solución: Buscar la raíz de $f(x)=f_1(x)-f_2(x)$.
4. En resumen, en general uno debe #transformar un posible problema que esté enfrentando a la forma búsqueda de raíces correspondiente, es decir, se debe construir $f(x)$ primero antes de decidir que algoritmo se debe aplicar.

# Algoritmos
- [[Método de la Bisección]]
- [[Iteración de Punto Fijo]]
- [[Método de Newton]]
- [[Método de la Secante]]

# Videos online
1. [Animation of numerical methods](https://computacioncientifica.page.link/M3bd) - Playlist - Bisección, Iteración de punto fijo y Newton.
2. [Numerical root finding](https://www.youtube.com/watch?embed=no&v=qBAMl94gMjc) - Video - Bisección, Iteración de punto fijo y Newton-Raphson, y su condición de convergencia.


#OK
#Tema_3