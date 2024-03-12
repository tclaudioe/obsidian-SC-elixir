![[09b_IVP.tex.png]]
Considere el siguiente IVP:$$
\begin{align*}
	\dot{y}(t)&=y(t), \,\, \text{para } t\in]0,T],\\
	y(0)&=1.
\end{align*}
$$Donde lo que nos dice la ODE es lo siguiente: **¿Existe alguna función $y(t)$ tal que al derivarla se obtenga la misma función y que al evaluarla en $t=0$ su valor sea $1$?**
La respuesta es **sí** y en esta caso particular corresponde a $y(t)=\exp(t)$.

Ahora, si consideramos de forma general,$$
\begin{align*}
	\dot{y}(t)&=f(t,y(t)), \,\, \text{para } t\in]0,T],\\
	y(0)&=y_0,
\end{align*}$$y de forma gráfica,
```tikz
\begin{document}
	\begin{tikzpicture}
		\draw[line width=0.3mm,->] (-0.5,0) -- (5,0) node[below] {$t$};
		\draw[line width=0.3mm,->] (0,-0.5) -- (0,3.5) node[left,red] {$y(t)$};
		\draw[-] (-0.1,2.5) -- (0.1,2.5) node[left,pos=0,blue] {$y_0$};
		\draw[-] (4.5,-0.1) -- (4.5,0.1) node[below,pos=0,blue] {$T$};
		\draw[red] (0,2.5) .. controls (0.75,1.5) and (1.75,3) .. (2.25,2);
		\draw[red] (2.25,2) .. controls (2.75,0.5) and (3.5,2) .. (4.5,1);
	\end{tikzpicture}
\end{document}
```

Entonces, tanto en la representación algebraica como en la representación gráfica, lo que buscamos es la función $y(t)$, es decir conocemos lo que está en azul y buscamos lo que está en rojo. Notar sin embargo que:
1. La función $f(t,y)$ es conocida, es decir es una función de dos variables que se puede evaluar en cualquier par ordenado $(t,y)$.
2. Adicionalmente conocemos al ODE, es decir $\dot{y}(t)=f(t,y(t))$, la cual es válida para todo $t$.
3. Al conocer la ODE, también conocemos la **pendiente** de $y(t)$, en particular, para $t=0$ sabemos que $\dot{y}(0)=f(t,y_0)$.
Con esta información disponible, podemos construir los algoritmos numéricos para crear una aproximación numérica de la función $y(t)$. Esto significa que se construirá una [[versión numérica]] de la función $y(t)$.

Recuerde que si usted puede resolver la ODE de forma algebraica, se sugiere encarecidamente hacerlo así, y solo utilizar métodos numéricos cuando sea requerido.

# Integrando un IVP
Considere la ODE,$$\dot{y}(t)=f(t,y(t))$$ la cual es válida par $t\in]0,T]$. Integrémosla en el intervalo $[0,t_1]$, donde $0<t_1\leq T$,$$
	\int_0^{t_1} \dot{y}(s)\mathrm{d}s = \int_0^{t_1} f(s,y(s))\,\mathrm{d}s,
$$ utilizando el segundo Teorema Fundamental del Cálculo obtenemos,$$
y(t_1)-y(0) =\int_0^{t_1} f(s,y(s))\,\mathrm{d}s.
$$Reemplazando valores conocidos y despejando $y(t_1)$ se obtiene,$$
y(t_1) =y_0+\int_0^{t_1} f(s,y(s))\,\mathrm{d}s.$$Ahora, los distintos algoritmos que discutiremos se basan en _aproximar_ la integral anterior.
# Método de Euler
Se aproxima $\displaystyle\int_0^{t_1} f(s,y(s))\,\mathrm{d}s$ con la suma de Riemann por la **izquierda**, entonces,$$
	y(t_1) =y_0+\int_0^{t_1} f(s,y(s))\,\mathrm{d}s\\
	\approx y_0+f(0,y_0)\,t_1.$$Y de modo general,$$y_{i+1} = y_i+f(t_i,y_i)\,(t_{i+1}-t_i).$$ De forma gráfica se obtiene,
```tikz
\begin{document}
\begin{tikzpicture}[scale=3]
    \def\myfun#1{1.5+0.2*(#1^2-0.45*#1^3)-1}
    \def\myfunp#1{0.2*(2*#1-0.45*3*#1^2)}
    \def\dx{0.2}

    \draw[line width=0.3mm,->] (-0.5,0) -- (3,0) node[below,pos=0.9] {$t$};
    \draw[line width=0.3mm,->] (0,-0.5) -- (0,1.5) node[left,pos=0.9] {$y(t)$};
    \draw[scale=1, domain=0:2.5, smooth, variable=\x, black, dashed] plot ({\x}, {\myfun{\x}});

    % y_i
    \node[circle, fill=blue, inner sep=2pt] at (1, { \myfun{1} }){};
    \node[anchor=south,blue] at (1, { \myfun{1} }) {$y_i$};
    % t_i
    \draw[-] (1,-0.1) -- (1,0.1) node[below,pos=0] {$t_i$};
    % f(t_i,y_i)
    \draw[-,red,line width=0.5mm] ({1-\dx},{\myfun{1}-\myfunp{1}*\dx}) -- ({1+\dx},{\myfun{1}+\myfunp{1}*\dx}) node[below,pos=0.5] {};
    \node[red] at (1,{\myfun{1}-\dx*0.5}) {$f(t_i,y_i)$};

    % y(t_{i+1})
    \node[circle, fill=green!50!black, inner sep=2pt] at (2, { \myfun{2} }){};
    \node[anchor=north,green!50!black] at (2, { \myfun{2} }) {$y(t_{i+1})$};
    % t_{i+1}
    \draw[-] (2,-0.1) -- (2,0.1) node[below,pos=0] {$t_{i+1}$};

    % y_{i+1}
    \node[circle, fill=blue, inner sep=2pt] at (2, {\myfun{1}+\myfunp{1}*1}){};
    \node[anchor=south,blue] at (2, {\myfun{1}+\myfunp{1}*1}) {$y_{i+1}$};
    \node[anchor=south,blue] at (2.8, {\myfun{1}+\myfunp{1}*1}) {$=y_i+f(t_i,y_i)\,(t_{i+1}-t_i)$};
    % line
    \draw[-,dashed,red] (1,{\myfun{1}}) -- (2,{\myfun{1}+\myfunp{1}*1}) node[below,pos=0] {};

    %  Error
    \node[anchor=west,cyan] at (2.1, {(\myfun{1}+\myfunp{1}*1+\myfun{2})*0.5}) {Error};
    % line
    \draw[-,cyan,line width=0.5mm] (2,{\myfun{2}}) -- (2,{\myfun{1}+\myfunp{1}*1}) node[below,pos=0] {};
    \end{tikzpicture}
\end{document}
```
En código de traduce a,
```python
def eulerMethod(t0,T,N,y0,f):
    t = np.linspace(t0,T,N+1)
    h = (T-t0)/N
    y = np.zeros(N+1)
    y[0] = y0
    for i in np.arange(N):
        y[i+1] = y[i]+f(t[i],y[i])*h
    return t, y
```
# Backward Euler
Se aproxima $\displaystyle\int_0^{t_1} f(s,y(s))\,\mathrm{d}s$ con la suma de Riemann por la **derecha**, entonces,$$
	y(t_1) =y_0+\int_0^{t_1} f(s,y(s))\,\mathrm{d}s\\
	\approx y_0+f(t_1,y(t_1))\,t_1.$$Y utilizando $y_1\approx y(t_1)$ se obtiene la siguiente ecuación,$$y_1 = y_0+f(t_1,y_1)\,t_1.$$Notar que del integrando solo conocemos su evaluación para el valor $s=0$, por lo que no podemos evaluar $f(t_1,y(t_1))$ porque no conocemos $y(t_1)$, entonces, **¿Cómo obtenemos $y_1$?**



Se busca una raíz de$$\widehat{f}(y_1)=y_1-f(t_1,y_1)\,t_1-y_0=0.$$De modo general, se busca una raíz de$$\widehat{f}_i(y_{i+1})=y_{i+1}-y_i-f(t_{i+1},y_{i+1})\,(t_{i+1}-t_i)=0.$$
```python
def backwardEulerMethod(t0,T,N,y0,f):
    t = np.linspace(t0,T,N+1)
    h = (T-t0)/N
    y = np.zeros(N+1)
    y[0] = y0
    for i in np.arange(N):
        f_hat= lambda x:  x - y[i]-f(t[i+1],x)*h
        # You must select a solver, "find_root" is just a generic name. The input used are the anonymous function "f_hat" and the initial guess y_i.
        x = find_root(f_hat,y[i])
        y[i+1] = x
    return t, y
```
# Runge-Kutta 2 (RK2)
Se aproxima $\displaystyle\int_0^{t_1} f(s,y(s))\,\mathrm{d}s$ con la **Regla del Punto Medio**, entonces,$$y(t_1) = y_0+\int_0^{t_1} f(s,y(s))\,\mathrm{d}s
\approx y_0+f(t_1/2,y(t_1/2))\,(t_1-0).$$ Entonces, al igual que el caso anterior, como no conocemos $y(t_1/2)$ no podemos evaluar $f(t_1/2,y(t_1/2))$. Sin embargo la estrategia en este caso es distinta, la cual corresponde a la siguiente,$$
\begin{align}
	k_1 &= f(0,y_0),\\
	y_{1} &= y_0 + h\,f\left(h/2,y_0+\dfrac{h}{2}\,k_1\right).
\end{align}$$Es decir, se estima $y(t_1/2)$ con $y_0+\dfrac{h}{2}\,k_1=y_0+\dfrac{h}{2}\,f(0,y_0)$. En el caso general,$$
\begin{align}
	k_1 &= f(t_i,y_i),\\
	y_{i+1} &= y_i + h\,f\left(t_i+h/2,y_{i}+\dfrac{h}{2}\,k_1\right).
\end{align}$$De forma gráfica se obtiene,
```tikz
\begin{document}
\begin{tikzpicture}[scale=3]
    \def\myfun#1{1.5+0.2*(#1^2-0.45*#1^3)-1}
    \def\myfunp#1{0.2*(2*#1-0.45*3*#1^2)}
    \def\dx{0.2}
    \def\dy{0.05}

    \draw[line width=0.3mm,->] (-0.5,0) -- (3,0) node[below,pos=0.9] {$t$};
    \draw[line width=0.3mm,->] (0,-0.5) -- (0,1.5) node[left,pos=0.9] {$y(t)$};
    \draw[scale=1, domain=0:2.5, smooth, variable=\x, black, dashed] plot ({\x}, {\myfun{\x}});

    % y_i
    \node[circle, fill=blue, inner sep=2pt] at (1, { \myfun{1} }){};
    \node[anchor=south,blue] at (1, { \myfun{1} }) {$y_i$};
    % t_i
    \draw[-] (1,-0.1) -- (1,0.1) node[below,pos=0] {$t_i$};
    % f(t_i,y_i)
    \draw[-,red,line width=0.5mm] ({1-\dx},{\myfun{1}-\myfunp{1}*\dx}) -- ({1+\dx},{\myfun{1}+\myfunp{1}*\dx}) node[below,pos=0.5] {};
    \node[red] at (1,{\myfun{1}-\dx*0.5}) {$k_1=f(t_i,y_i)$};
    % y_{i+1/2}
    \node[circle, fill=violet, inner sep=2pt] at (1.5, {\myfun{1}+\myfunp{1}*0.5}){};
    \node[anchor=south,violet] at (1.5, {\myfun{1}+\myfunp{1}*0.5}) {$y_{i}+\frac{h}{2}\,k_1$};
    \draw[-] (1.5,-0.1) -- (1.5,0.1) node[below,pos=0] {$t_i+\frac{h}{2}$};
    % k2
    \def\kTwoFix{\myfun{1}+\myfunp{1}*0.5}
    \draw[-,red,line width=0.5mm] ({1.5-\dx},{\kTwoFix-(\myfunp{1.5}+0.1)*\dx}) -- ({1.5+\dx},{\kTwoFix+(\myfunp{1.5}+0.1)*\dx}) node[below,pos=0.5] {};
    \node[red] at (1.5,{\myfun{1.5}-\dx*0.5}) {$k_2$};

    % y_{i+1}
    \node[circle, fill=blue, inner sep=2pt] at (2, { \myfun{2}+\dy }){};
    \node[anchor=south,blue] at (2, { \myfun{2}+\dy }) {$y_{i+1}$};
    \node[anchor=south,blue] at (2.35, {\myfun{2}+\dy}) {$=y_i+h\,k_2$};

    % y(t_{i+1})
    \node[circle, fill=green!50!black, inner sep=2pt] at (2, { \myfun{2} }){};
    \node[anchor=north,green!50!black] at (2, { \myfun{2} }) {$y(t_{i+1})$};
    % t_{i+1}
    \draw[-] (2,-0.1) -- (2,0.1) node[below,pos=0] {$t_{i+1}$};

    %  Error
    \node[anchor=west,cyan] at (2.1, {(\myfun{1}+\myfunp{1}*1+\myfun{2})*0.5-0.05}) {Error};
    % line
    \draw[-,cyan,line width=0.5mm] (2,{\myfun{2}}) -- (2,{\myfun{2}+\dy}) node[below,pos=0] {};
    \end{tikzpicture}
\end{document}
```
**Se sugiere ver adicionalmente la derivación alternativa de RK2 en los apuntes.**
```python
def RK2(t0,T,N,y0,f):
    t = np.linspace(t0,T,N+1)
    h = (T-t0)/N
    y = np.zeros(N+1)
    y[0] = y0
    for i in np.arange(N):
        k1 = f(t[i],y[i])
        y[i+1] = y[i]+f(t[i]+h/2,y[i]+k1*h/2)*h
    return t, y
```

# Runge-Kutta de 4to orden (RK4)
RK4 se reduce a los siguientes 4 pasos:
$$
\begin{align*}
	k_1 &= f(t_i,y_i)\\
	k_2 &= f\left(t_i+\dfrac{h}{2},y_{i}+\dfrac{h}{2}\,k_1\right)\\
	k_3 &= f\left(t_i+\dfrac{h}{2},y_{i}+\dfrac{h}{2}\,k_2\right)\\
	k_4&= f\left(t_i+h,y_{i}+h\,k_3\right)\\
	y_{i+1} &= y_i + \dfrac{h}{6}\left(k_1+2\,k_2+2\,k_3+k_4\right).
\end{align*}
$$
Y su implementación,
```python
def RK4(t0,T,N,y0,f):
    t = np.linspace(t0,T,N+1)
    h = (T-t0)/N
    y = np.zeros(N+1)
    y[0] = y0
    for i in np.arange(N):
        k1 = f(t[i],y[i])
        k2 = f(t[i]+h/2,y[i]+k1*h/2)
        k3 = f(t[i]+h/2,y[i]+k2*h/2)
        k4 = f(t[i]+h,y[i]+k3*h)
        y[i+1] = y[i]+(k1+2*k2+2*k3+k4)*h/6
    return t, y
```
De forma gráfica,
```tikz
\begin{document}
	\begin{tikzpicture}[scale=3]
    \def\myfun#1{1.5+0.2*(#1^2-0.45*#1^3)-1}
    \def\myfunp#1{0.2*(2*#1-0.45*3*#1^2)}
    \def\dx{0.2}
    \def\dy{0.01}

    \draw[line width=0.3mm,->] (-0.5,0) -- (3,0) node[below,pos=0.9] {$t$};
    \draw[line width=0.3mm,->] (0,-0.5) -- (0,1.5) node[left,pos=0.9] {$y(t)$};
    \draw[scale=1, domain=0:2.5, smooth, variable=\x, black, dashed] plot ({\x}, {\myfun{\x}});

    % y_i
    \node[circle, fill=blue, inner sep=2pt] at (1, { \myfun{1} }){};
    \node[anchor=south,blue] at (1, { \myfun{1} }) {$y_i$};
    % t_i
    \draw[-] (1,-0.1) -- (1,0.1) node[below,pos=0] {$t_i$};
    % f(t_i,y_i)
    \draw[-,red,line width=0.5mm] ({1-\dx},{\myfun{1}-\myfunp{1}*\dx}) -- ({1+\dx},{\myfun{1}+\myfunp{1}*\dx}) node[below,pos=0.5] {};
    \node[red] at (1,{\myfun{1}-\dx*0.5}) {$k_1=f(t_i,y_i)$};
    
    % y_{i+1/2} - k2
    \def\kTwoFix{\myfun{1}+\myfunp{1}*0.5}
    \def\kTwoShift{0.2}
    \node[circle, fill=violet, inner sep=2pt] at (1.5, {\myfun{1}+\myfunp{1}*0.5+\kTwoShift}){};
    \node[anchor=south,violet] at (1.5, {\myfun{1}+\myfunp{1}*0.5+\kTwoShift}) {$y_{i}+\frac{h}{2}\,k_1$};
    % k2
    \draw[-,red,line width=0.5mm] ({1.5-\dx},{\kTwoFix-(\myfunp{1.5}+0.1)*\dx+\kTwoShift}) -- ({1.5+\dx},{\kTwoFix+(\myfunp{1.5}+0.1)*\dx+\kTwoShift}) node[below,pos=0.5] {};
    \node[red] at (1.5,{\myfun{1.5}-\dx*0.5+\kTwoShift}) {$k_2$};

    % y_{i+1/2} - k3
    \def\kTwoFix{\myfun{1}+\myfunp{1}*0.5}
    \def\kThreeShift{-0.4}
    \node[circle, fill=violet, inner sep=2pt] at (1.5, {\myfun{1}+\myfunp{1}*0.5+\kThreeShift}){};
    \node[anchor=south,violet] at (1.5, {\myfun{1}+\myfunp{1}*0.5+\kThreeShift}) {$y_{i}+\frac{h}{2}\,k_2$};
    % k3
    \draw[-,red,line width=0.5mm] ({1.5-\dx},{\kTwoFix-(\myfunp{1.5}-0.1)*\dx+\kThreeShift}) -- ({1.5+\dx},{\kTwoFix+(\myfunp{1.5}-0.1)*\dx+\kThreeShift}) node[below,pos=0.5] {};
    \node[red] at (1.5,{\myfun{1.5}-\dx*0.5+\kThreeShift}) {$k_3$};
    \draw[-] (1.5,-0.1) -- (1.5,0.1) node[below,pos=0] {$t_i+\frac{h}{2}$};

    % y_{i+1/2} - k4
    \def\kFourFix{\myfun{2}}
    \def\kFourShift{0.3}
    \node[circle, fill=violet, inner sep=2pt] at (2, {\myfun{2}+\kFourShift}){};
    \node[anchor=south,violet] at (2, {\myfun{2}+\kFourShift}) {$y_{i}+h\,k_3$};
    % k3
    \draw[-,red,line width=0.5mm] ({2-\dx},{\kFourFix-(\myfunp{2}+0.1)*\dx+\kFourShift}) -- ({2+\dx},{\kFourFix+(\myfunp{2}+0.1)*\dx+\kFourShift}) node[below,pos=0.5] {};
    \node[red] at (2,{\myfun{2}-\dx*0.5+\kFourShift}) {$k_4$};

    % y_{i+1}
    \node[circle, fill=blue, inner sep=2pt] at (2, { \myfun{2}+\dy }){};
    \node[anchor=west,blue] at (2.1, { \myfun{2}+\dy }) {$y_{i+1}=y_i+\frac{h}{6}\,\left(k_1+2\,k_2+2\,k_3+k_4\right)$};

    % y(t_{i+1})
    \node[circle, fill=green!50!black, inner sep=2pt] at (2, { \myfun{2} }){};
    \node[anchor=north,green!50!black] at (2, { \myfun{2} }) {$y(t_{i+1})$};
    % t_{i+1}
    \draw[-] (2,-0.1) -- (2,0.1) node[below,pos=0] {$t_{i+1}$};

    %  Error
    \draw[-,cyan,line width=0.5mm] (2,{\myfun{2}}) -- (2,{\myfun{2}+\dy}) node[below,pos=0] {};
    \end{tikzpicture}
\end{document}
```

#OK 
#Tema_9