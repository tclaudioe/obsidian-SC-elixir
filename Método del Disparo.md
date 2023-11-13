El método del disparo para BVP consiste en considerar el BVP como un IVP acoplado con un _solver_ para búsqueda de raíces.
Para entender la idea, se presentará una secuencia de preguntas para guiar el entendimiento.
# ¿Qué significa ver un BVP como un IVP?
Tanto los IVP como los BVP discutidos acá son ODE, es decir son una _Ordinary Differential Equations_. La diferencia que los hace _parecer_ distintos es que los IVP tiene como variable independiente el _tiempo_ $t$ y los BVP tienen como variable independiente la variable _espacial_ $x$. Sin embargo, el **nombre** de la variable, ya sea $t$ o $x$, no cambia la esencia, es decir, que sea una ecuación diferencial.
# ¿Cómo se relaciona la variable espacial $x$ con la variable temporal $t$?
- **Versión corta**: $x \rightarrow t$.
- **Versión un poco más extendida**: Se hace un cambio de variable tal que ahora la variable independiente ya no sea $x$ sino $t$, por ejemplo, si consideramos el ejemplo anterior denominado _un poco más general_, entonces quedaría:
	1. ODE original: $a(x)\,y''(x)+b(x)\,y'(x)+c(x)\,y(x)=f(x)$.
	2. ODE modificada: $a(t)\,\ddot{y}(t)+b(t)\,\dot{y}(t)+c(t)\,y(t)=f(t)$.
	3. Se despeja $\ddot{y}(t)$, es decir $$\ddot{y}(t)=\dfrac{f(t)-b(t)\,\dot{y}(t)-c(t)\,y(t)}{a(t)}=:\widehat{f}(t,y(t),\dot{y}(t)).$$Notar que se usó notación $\widehat{f}(\cdot)$ porque $f(\cdot)$ ya estaba siendo usada.
# ¿Cómo quedaría definido el IVP?
Para definir un IVP necesitamos la ODE respectiva y las condiciones iniciales para cada _función_ involucrada. En este caso, como tenemos derivadas temporales de orden superior (es decir mayor a 1), se procede a construir un **sistema dinámico** considerando el siguiente cambio de variables:$$
\begin{align*}
	y_1(t) &= y(t),\\
	y_2(t) &= \dot{y}(t).
\end{align*}$$Luego al derivar cada ecuación con respecto a $t$ obtenemos,$$
\begin{align*}
	\dot{y}_1(t) &= \dot{y}(t)=y_2(t),\\
	\dot{y}_2(t) &= \ddot{y}(t) =\widehat{f}(t,y_1(t),y_2(t)).
\end{align*}$$Lo cual ya se puede escribir como un sistema dinámico,$$\dot{\mathbf{y}}=\mathbf{F}(t,\mathbf{y}),$$donde $\mathbf{y}=\mathbf{y}(t)=\begin{bmatrix}y_1(t) & y_2(t)\end{bmatrix}^\top$. Lo cual permite utilizar cualquier _solver_ de [[Problemas de Valor Inicial]].
# Pero, ¿Cuáles serían las condiciones iniciales?
Recordemos que la información que tenemos es la siguiente,$$
\begin{align*}
	y(0)&=u_0,\\
	y(L)&=u_L.
\end{align*}$$Lo que significa, luego del cambio de variables anterior, $y_1(0)=u_0$ y $y_1(L)=u_L$. De lo cual deducimos que la **primera** expresión, es decir  $y_1(0)=u_0$, es una condición inicial. Sin embargo, la **segunda** expresión, es decir $y_1(L)=u_L$, **no** es una _condición inicial_, más aún es una **condición _final_**. Entonces concluimos que **no conocemos** la condición inicial para $y_2(0)$.
**¿Qué hacemos?**
Definamos un valor _arbitrario_ y veamos que pasa, denotémoslo por $\alpha_0$. Entonces nuestro sistema dinámico quedaría,$$\begin{align*}
\dot{\mathbf{y}}&=\mathbf{F}(t,\mathbf{y}),\\
y_1(0) &= u_0, \quad \text{es decir un valor conocido},\\
y_2(0) &= \alpha_0, \quad \text{es decir una \textbf{estimación}}.
\end{align*}$$
# ¿Cómo sabemos si el valor seleccionado de $\alpha_0$ era el correcto?
En este ejemplo, considere que resolvimos el problema de valor inicial para $t\in[0,L]$ y graficamos la aproximación numérica obtenida, la cual nos da,
```tikz
\begin{document}
	\begin{tikzpicture}
        \draw[line width=0.3mm,->] (-0.5,0) -- (5,0) node[below,pos=0.5] {$x$};
        \draw[line width=0.3mm,->] (0,-0.5) -- (0,2.5) node[below,pos=0] {$x=0$};
        \draw[line width=0.3mm,dashed] (0+4.5,-0.5) -- (0+4.5,2.5) node[below,pos=0] {$x=L$};
        \draw[-] (-0.1,1.5) -- (0.1,1.5) node[left,pos=0,blue] {$u_0$};
        \draw[-] (-0.1+4.5,1) -- (0.1+4.5,1) node[right,pos=1,blue] {$u_L$};
        \draw[-,red] (0,1.5) .. controls (1,0) and (3,0) .. (4.5,1);
        \draw[-,gray] (0,1.5) .. controls (1,0.2) and (3,-0.4) .. (4.5,2);
        \draw[-] (-0.1+4.5,2) -- (0.1+4.5,2) node[right,pos=1,gray] {$y_{1,n}^{[0]} \approx y_1(L)$};
        \end{tikzpicture}
\end{document}
```
Notar que la solución numérica discreta de $y_1(t)$ obtenida con $n+1$ puntos corresponde a, $$\begin{bmatrix}y_{1,0}^{[0]} & y_{1,1}^{[0]} & \dots & y_{1,n}^{[0]} \end{bmatrix},$$donde el súper-índice en $y_{1,i}^{[0]}$ denota que estamos trabajando con $\alpha_0$ como _initial guess_ para $y_2(0)$. Entonces, dado que sabemos que se debe cumplir que $y_1(L)=u_L$, observamos que hay una _clara_ diferencia entre el valor que $y_1(L)$ debe obtener y el que obtiene, la diferencia la podemos denotar como,$$
E(\alpha_0)=y_{1,n}^{[0]}-u_L.
$$**Entonces se concluye que en este caso se eligió erróneamente el _initial guess_.**
Ahora que sabemos que se eligió erróneamente, ¿que podemos hacer?
# ¿Qué alternativas tenemos?
1. Buscar "aleatoriamente" el _initial guess_ adecuado.
2. ¿Qué sugiere usted?

**Efectivamente**, es una búsqueda de raíces pero la raíz de un _algoritmo_!!

Desde el punto de vista de código, debemos encontrar la raíz de $E(\alpha)$, en Python quedaría así,
```python
def E(alpha,n=1000):
    y0 = np.zeros(2)
    y0[0] = u1
    y0[1] = alpha
    # Following notation introduced in previous chapter.
    def f_IVP(t, y):
        y1, y2 = y
        dydt = [y2, widehat_f(t,y1,y2)]
        return dydt
    # Following notation introduced in previous chapter.
    # We consider the output variable has dimensions (N+1)x2
    y = SolverIVP(0,1,N,y0,f_IVP)
    return y[-1,0]-uL
```
Lo que nos indica que cada vez que ejecutemos la función anterior, estaremos resolviendo un IVP. En este caso se podría utilizar el [[Método de la Bisección]], alguna [[Iteración de Punto Fijo]] o el [[Método de la Secante]]. _¿Se puede utilizar el [[Método de Newton]]?_
# Ventajas
- Utilizamos algo conocido para resolver un nuevo tipo de problema.
# Desventaja
- Puede ser bastante lento determinar el valor adecuado de $\alpha$, es decir, esto significa que se podrían requerir mucha computación para llegar a obtener el valor de $\alpha$ correcto.

#OK 
#Tema_9 
