El método de Diferencias Finitas es un método que consiste en aproximar los operadores continuos de derivadas por operadores discretos.
Por ejemplo, recordemos la _definición_ conocida de la primera derivada,$$
y'(x) = \lim_{h\rightarrow 0} \dfrac{y(x+h)-y(x)}{h}.$$Lo que esta definición nos indica, _si es que existe la derivada_, es que al obtener la diferencia entre la evaluación de una función en 2 puntos que se acercan dividiéndola por la diferencia entre tales puntos se encuentra un valor finito. Esta diferencia entre puntos se puede hacer muy pequeña porque estamos considerando que la función está definida en $\mathbb{R}$. 
Para entender este comportamiento, consideremos el siguiente experimento numérico, el cual consiste en aproximar numéricamente la derivada de $f(x)=\sin(x)$, la cual sabemos que es $f'(x)=\cos(x)$. Esto nos sirve para cuantificar el _error_ inducido por la aproximación.
```run-python
n = 10
f = lambda x: np.sin(x)
fp = lambda x: np.cos(x)
h = np.logspace(-7,-1,n)
x0 = 1.0

# Forward Difference
fpFD = (f(x0+h)-f(x0))/h

for i in np.arange(n-1,-1,-1):
	print('h={:10.9f}, fpFD={:2.8f}, f\'(x0)={:2.8f}, log10(error)={:2.3f}'.format(h[i],fpFD[i],fp(x0),np.log10(np.abs(fp(x0)-fpFD[i]))))
```
y de forma grafica,
```run-python
n = 10
f = lambda x: np.sin(x)
fp = lambda x: np.cos(x)
h = np.logspace(-7,-1,n)
x0 = 1.0

# Forward Difference
fpFD = (f(x0+h)-f(x0))/h

plt.figure(figsize=(5,5))
plt.loglog(h,np.abs(fp(x0)-fpFD),'.')
plt.xlabel('$h$')
plt.ylabel('$|f\'(x_0)-fpFD(x0)|$')
plt.grid(True)
plt.show()
```
Pero, ¿qué ocurre si **seguimos** reduciendo el valor de $h$ y usamos una aproximación **alternativa** de la derivada?
```run-python
n = 20
f = lambda x: np.sin(x)
fp = lambda x: np.cos(x)
h = np.logspace(-14,-1,n)
x0 = 1.0

# Forward Difference
fpFD = (f(x0+h)-f(x0))/h
# Central Difference
fpCD = (f(x0+h)-f(x0-h))/(2*h)

plt.figure(figsize=(5,5))
plt.loglog(h,np.abs(fp(x0)-fpFD),'.',label='Forward Difference')
plt.loglog(h,np.abs(fp(x0)-fpCD),'.',label='Central Difference')
plt.xlabel('$h$')
plt.ylabel('$|f\'(x_0)-fpFD(x0)|$')
plt.grid(True)
plt.legend(loc='best')
plt.show()
```
Se observa que el error se reduce más rápido pero luego de todos modos crece. Por simplicidad, **por ahora se considera que no se usarán valores de $h$ tan pequeños**.
# ¿Qué aproximaciones de Diferencias Finitas tenemos a nuestra disposición?
En principio utilizaremos las siguientes aproximaciones,$$
\begin{align*}
        y'(x_i) &= \dfrac{y(x_i+h)-y(x_i)}{h} + \mathcal{O}(h), &\textbf{ Forward Difference}. \\
        y'(x_i) &= \dfrac{y(x_i)-y(x_i-h)}{h} + \mathcal{O}(h), &\textbf{ Backward Difference}. \\
        y'(x_i) &= \dfrac{y(x_i+h)-y(x_i-h)}{2\,h} +\mathcal{O}(h^2), &\textbf{ Central Difference}.
    \end{align*}$$Y, adicionalmente, una aproximación para la segunda derivada, es decir,$$\begin{align*}
        y''(x_i) &=
            \dfrac{y(x_i+h)-2\,y(x_i)+y(x_i-h)}{h^2}+\mathcal{O}(h^2).
    \end{align*}$$
# ¿Por qué queremos aproximar la derivada si podríamos obtenerla directamente de la función en estudio?
La idea de construir una aproximación de la derivada es cuando uno no tiene a su disposición la función _algebraica_, sino una [[versión numérica]] de la función. Por ejemplo, considere que tiene a su disposición el siguiente conjunto de pares ordenados,$$\begin{equation*}
\left\{ (x_0,y_0), (x_1,y_1), (x_2,y_2),\dots,(x_{i-1},y_{i-1}),(x_i,y_i),(x_{i+1},y_{i+1}),\dots, (x_n,y_n)\right\},
\end{equation*}$$Los cuales representan a una aproximación de la función $y(x)$ en los nodos $\{x_0,x_1,\dots,x_{i-1},x_i,x_{i+1},\dots,x_n\}$.
Entonces, dado que tenemos una versión **discreta** de la función $y(x)$, no podemos obtener su derivada de la _forma tradicional_. 
**¿Qué podemos hacer para aproximar la derivada en una _representación_ discreta de una función?**

¡Podemos usar _Diferencias Finitas_!

# ¿Cómo usamos _Diferencias Finitas_ para resolver BVP?
Para responde la pregunta, consideremos una variante del ejemplo inicial,$$
\begin{align*}
	y''(x)&=f(x), \,\, \text{para } x\in]0,L[,\\
	y(0)&=c_1,\\
	y(L)&=c_2.\\
\end{align*}$$Por simplicidad, consideremos que _discretizamos_ el dominio en 5 intervalos, lo que implica $n=5$ y la definición de los $6$ puntos, es decir,$$\{x_0,x_1,x_2,x_3,x_4,x_5\},$$ en particular se definen de la siguiente forma $x_i=i\,h$, donde $h=\dfrac{L}{n}=\dfrac{L}{5}$, para $i\in\{0,1,2,3,4,5\}$. Esta discretización espacial de la variable **independiente** $x$ implica que tenemos, inicialmente, $6$ valores desconocidos de la variable **dependiente**, es decir,$$\{y_0,y_1,y_2,y_3,y_4,y_5\}.$$De modo completo, tenemos,$$\begin{equation*}
\left\{ (x_0,y_0), (x_1,y_1), (x_2,y_2),(x_3,y_3),(x_4,y_4),(x_5,y_5)\right\}.
\end{equation*}$$De lo cual solo conocemos $x_i$. **El problema que debemos resolver ahora es construir un algoritmo para determinar los valores de $y_i$.**
## Condiciones de Borde
Las condiciones de borde nos indican lo siguiente,$$
\begin{align*}
	y(0)&=c_1,\\
	y(L)&=c_2,\\
\end{align*}$$donde $x_0=0$ y $x_5=L$, por lo tanto conocemos **explícitamente** $y=0$ y $y_5$ porque,$$
\begin{align*}
	y(0)=y_0&=c_1,\\
	y(L)=y_5&=c_2.\\
\end{align*}$$Entonces, por completitud, se destacará en ${\color{blue} \text{azul}}$ las variables que conocemos y en ${\color{red} \text{rojo}}$ las que **falta por determinar**.$$\begin{equation*}
\left\{
({\color{blue} x_0},{\color{blue} y_0}), 
({\color{blue} x_1},{\color{red} y_1}),
({\color{blue} x_2},{\color{red} y_2}),
({\color{blue} x_3},{\color{red} y_3}),
({\color{blue} x_4},{\color{red} y_4}),
({\color{blue} x_5},{\color{blue} y_5})
\right\}.
\end{equation*}$$Para determinar las **incógnitas** faltantes debemos utilizar la ODE.
## ¿Cómo se utiliza la ODE para determinar las incógnitas faltantes?
Hay que recordar la ODE primero,$$
\begin{align*}
	y''(x)&=f(x), \,\, \text{para } x\in]0,L[.\\
\end{align*}$$Lo que que sabemos de la ODE es que es válida para cualquier valor de $x$ en el intervalo $]0,L[$. En particular, de la lista de variables independientes $x_i$ que tenemos, podemos concluir que la ODE es válida en,$$\{x_1,x_2,x_3,x_4\}.$$Es decir, se excluyen $x_0$ y $x_5$ porque son lo valores en el borde donde la ODE no es válida.
Ahora, teniendo claridad donde es válida la ODE, podemos evaluarla, es decir,$$
\begin{align*}
	y''(x_1) &=f(x_1),\\
	y''(x_2) &=f(x_2),\\
	y''(x_3) &=f(x_3),\\
	y''(x_4) &=f(x_4).
\end{align*}$$Lo que nos indica esto es que la segunda derivada de $y(x_i)$ es igual a $f(x_i)$. Sin embargo, no tenemos a $y(x)$, pero sí tenemos acceso a una versión discreta, es decir, $\{y_0,y_1,y_2,y_3,y_4,y_5\}$, entonces debemos **aproximar** la segunda derivada, es decir,$$
\begin{align*}
	y''(x_1) &=f(x_1) &&\rightarrow && \dfrac{y_2-2\,y_1+y_0}{h^2}=f_1,\\
	y''(x_2) &=f(x_2) &&\rightarrow && \dfrac{y_3-2\,y_2+y_1}{h^2}=f_2,\\
	y''(x_3) &=f(x_3) &&\rightarrow && \dfrac{y_4-2\,y_3+y_2}{h^2}=f_3,\\
	y''(x_4) &=f(x_4) &&\rightarrow && \dfrac{y_5-2\,y_4+y_3}{h^2}=f_4.\\
\end{align*}$$Pero, como conocemos $y_0=c_1$ y $y_5=c_2$, dejamos lo conocido al lado derecho y los desconocido al lado izquierdo, entonces,$$
\begin{align*}
	y_2-2\,y_1&=h^2\,f_1-c_1,\\
	y_3-2\,y_2+y_1&=h^2\,f_2,\\
	y_4-2\,y_3+y_2&=h^2\,f_3,\\
	-2\,y_4+y_3&=h^2\,f_4-c_2.\\
\end{align*}$$ O matricialmente,$$
\begin{pmatrix}
  -2 & 1 & 0 & 0\\
  1 & -2 & 1 & 0\\
  0 & 1 & -2 & 1\\
  0 & 0 & 1 & -2
  \end{pmatrix}
  \begin{pmatrix}
  y_1\\
  y_2\\
  y_3\\
  y_4
  \end{pmatrix}
  =
  \begin{pmatrix}
  h^2\,f_1-c_1\\
  h^2\,f_2\\
  h^2\,f_3\\
  h^2\,f_4-c_2
\end{pmatrix}$$Lo cual lo resolvemos con alguno de los algoritmos discutidos anteriormente, tal como: [[Eliminación Gaussiana]], [[Factorización LU]], [[Factorización PALU]], [[Jacobi]], o [[GMRes]]!
## ¿Cómo se generaliza?
### Considere el caso _más_ general indicado anteriormente
$$\begin{align*}
	a(x)\,y''(x)+b(x)\,y'(x)+c(x)\,y(x)&=f(x), \,\, \text{para } x\in]0,L[,\\
	y(0)&=u_0,\\
	y(L)&=u_L,
\end{align*}$$
La idea es la misma, es decir,
1. Definir una grilla discreta, por simplicidad, esquiespaciada.
2. Utilizar las condiciones de borde.
3. Utilizar la ODE, en este caso se aproxima al evaluar en, por ejemplo, $x_i$,$$a(x_i)\,y''(x_i)+b(x_i)\,y'(x_i)+c(x_i)\,y(x_i)=f(x_i).$$Lo cual es su versión discreta es,$$
a_i\,\dfrac{y_{i+1}-2\,y_i+y_{i-1}}{h^2}+b_i\,\dfrac{y_{i+1}-y_{i-1}}{2\,h}+c_i\,y_i=f_i.$$Lo cual se convierte en un sistema de ecuaciones lineales y se resuelve!
### ¿Qué hacemos con un caso no lineal?
Por ejemplo,$$
y''(x)+\left(y'(x)\right)^2+\sin(y(x))=f(x).$$Entonces la discretización queda,$$\dfrac{y_{i+1}-2\,y_i+y_{i-1}}{h^2}+\left(\dfrac{y_{i+1}-y_{i-1}}{2\,h}\right)^2+\sin(y_i)=f_i,$$para $i\in\{1,2,\dots,n-1\}$.

**¿Qué algoritmo utilizaría?**

#OK 
#Tema_9 
