![[06_MinimosCuadrados_y_QR.tex.png]]
Considere el siguiente ejemplo numérico,
```run-python
np.random.seed(0)
m = 50
xi = np.linspace(-1,2,m)
yi = 2*xi-5.+0.4*np.random.normal(0,1,m)

plt.figure(figsize=(4,4))
# If you want to try what happen with a polynomial interpolation with this data, uncomment the following 3 lines and see it by yourself!
#p = BarycentricInterpolator(xi,yi)
#xx = np.linspace(-1,2,20*n)
#plt.plot(xx,p(xx),'k-', label='Inter. Pol.')
plt.plot(xi,yi,'b.', markersize=10, label='data')
plt.xlabel('$x$')
plt.ylabel('$y$')
plt.grid(True)
plt.legend()
plt.show()
```

El cual induce las siguientes preguntas:
1. **¿Es razonable interpolar la data?**
2. **¿Hay alguna tendencia que se detecte?**
3. **¿Qué se puede hacer en este caso?**

```run-python
np.random.seed(0)
m = 50
xi = np.linspace(-1,2,m)
yi = 2*xi-5.+0.4*np.random.normal(0,1,m)
plt.figure(figsize=(4,4))
plt.plot(xi,yi,'.', markersize=10, label='data')
plt.plot(xi,1.9*xi-4.5,'r-', label='Aproximación 1')
plt.plot(xi,2.3*xi-4.,'g-', label='Aproximación 2')
plt.plot(xi,1.95*xi-4.9,'-', color='orange', label='Aproximación 3')
plt.plot(xi,2*xi-5,'-', color='purple', label='Aproximación 4')
plt.xlabel('$x$')
plt.ylabel('$y$')
plt.grid(True)
plt.legend()
plt.show()
```

# ¿Cómo definimos la _mejor_ aproximación?
Considere que tenemos ahora el siguiente conjunto de datos $(x_{1},y_{1}),\dots,(x_m,y_m)$, de los cuales **sabemos** que se relacionan linealmente, es decir, $y \approx a+b\,x$. Con esta información podemos definir las siguientes ecuaciones,$$\begin{align*}
	a+b\,x_1 &=\widehat{y\hspace{1pt}}_1 \approx y_1,\\
	a+b\,x_2 &=\widehat{y\hspace{1pt}}_2 \approx y_2,\\
	\vdots \hspace{9pt} &= \hspace{2pt} \vdots \hspace{5pt} \approx \hspace{2pt} \vdots\\
	a+b\,x_m &=\widehat{y\hspace{1pt}}_m \approx y_m,\\
\end{align*}$$donde $\widehat{y}_i$ (_notar que se agrega un sombrero a la variable_) corresponde al valor de la evaluación de la función $a+b\,x$ en $x_i$, que no necesariamente es igual a $y_i$.
Lo cual desde el punto de vista gráfico obtenemos,
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
Para continuar, se necesitan algunas precisiones:
1. Tanto $x_i$ como $y_i$ son los **únicos** datos a los que se tiene acceso.
2. Sabemos que el dato $y_i$ puede contener un error dado que se considera que es una medición por algún instrumento. En particular lo denotaremos como $y_i=y_i^{(e)}+\varepsilon_i$, donde $y_i^{(e)}$ corresponde al valor _exacto_ si se hubiera medido _perfectamente_ y $\varepsilon_i$ es el error inducido por el instrumento. **Notar que tanto $y_i^{(e)}$ como $\varepsilon_i$ son desconocidos, solo conocemos su _suma_**.
3. En general, se busca construir un modelo capaz obtener $y_i^{(e)}$ a partir de $x_i$, digamos $f_{\text{exact}}(x_i)=y_i^{(e)}$, sin embargo, en la mayoría de los casos lo que se puede hacer es proponer un modelo aproximado, digamos por simplicidad $f(x)=a+b\,x$ , tal que $f(x_i)=\widehat{y\hspace{1pt}}_i$, donde se **ajustan** los parámetros libres $a$ y $b$ tal que se reduzca el error cuadrático (podrían definirse otro tipo de errores también).
4. La identidad $a+b\,x_i = \widehat{y\hspace{1pt}}_i$ indica que se obtiene $\widehat{y\hspace{1pt}}_i$ al evaluar la aproximación lineal en $x_i$.
5. La triple identidad $y_i = y_i^{(e)}+\varepsilon_i = \widehat{y\hspace{1pt}}_i +r_i$ muestra la relación entre la **data obtenida** $y_i$, **el dato exacto** $y_i^{(e)}$, y el **dato obtenido por la aproximación utilizada** $\widehat{y\hspace{1pt}}_i$.
6. $r_i=y_i-\widehat{y\hspace{1pt}}_i$, corresponde al **residuo** obtenido al aproximar la **data** $y_i$ con el modelo elegido (que puede o no ser lineal).
# En resumen, necesitamos:
1. **Datos**: $(x_i,y_i)$.
2. **Modelo**: $f(x)=a+b\,x$.
3. **Definición de error a reducir**: Error cuadrático.
# Definición del error cuadrático, varias representaciones equivalentes
$$
\begin{align*}
	E(a,b) &= \sum_{i=1}^m (y_i-f(x_i))^2\\
			&= \sum_{i=1}^m (y_i-a-b\,x_i)^2\\
			&= \sum_{i=1}^m (y_i-\widehat{y\hspace{1pt}}_i)^2\\
			&= \sum_{i=1}^m r_i^2\\
\end{align*}
$$
# ¿Cómo obtenemos el _mejor_ modelo entonces?
Minimizando $E(a,b)$, para lo cual tenemos 2 familias de algoritmos:
- [[Mínimos Cuadrados por Minimización]]
- [[Mínimos Cuadrados desde el Álgebra Lineal]] 

# Ejemplos de modelos
- Lineal: $y = c_1 + c_2 \, t$
- Cuadrático: $y=c_1 + c_2 \, t + c_3 \, t^2$
- Sinusoidal: $y=c_1 + c_2 \cos(2\pi \, t) + c_3 \, \sin(2\pi \, t) + c_4 \, \cos(4\pi \, t)$
- Exponencial: $y=c_1 \, \exp(c_2 \, t)$ + cambio de variables nos da $\log(y)=\log(c_1)+c_2\,t$
- Ley de potencia: $y=c_1 \, t^{c_2}$ + cambio de variables nos da $\log(y)=\log(c_1)+c_2\,\log(t)$

#OK
#Tema_7