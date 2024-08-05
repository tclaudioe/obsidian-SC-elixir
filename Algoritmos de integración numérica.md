![[08_IntegracionNumerica.tex.png]]
Los algoritmos de integración numérica tienen como objetivo base poder estimar el valor de una integral definida que, en la mayoría de los casos, no se puede integrar de forma exacta. La forma general se define a continuación:$$c=\int_a^b f(x)\,\mathrm{d}x,$$donde $a$, $b$ y $c$ son número reales y $f:\mathbb{R}\to\mathbb{R}$. Se denota como $c$ al valor numérico que representa la integral definida. $a$ corresponde al límite inferior de la integral y $b$ corresponde al límite superior de la integral.
# Selección de Teoremas y propiedades a tener presente
- $\dfrac{\mathrm{d}}{\mathrm{d}x} F(x)= \dfrac{\mathrm{d}}{\mathrm{d}x} \displaystyle\int_a^x f(x) \,\mathrm{d}x=f(x)$, lo cual corresponde a el _primer Teorema fundamental del cálculo_ (versión corta).
- $\displaystyle\int_a^b F'(x) \,\mathrm{d}x=F(b)-F(a)$, lo cual corresponde a el _segundo Teorema fundamental del cálculo_ (versión corta).
- $\displaystyle\int_a^b \alpha\,f(x)+\beta\,g(x)\,\mathrm{d}x=\alpha \,\int_a^b f(x) \,\mathrm{d}x+\beta\,\int_a^b g(x) \,\mathrm{d}x$, donde $\alpha$ y $\beta$ son constantes, es decir el operador de integración es _lineal_.
-  $\displaystyle\int_a^b f(x) \,\mathrm{d}x=-\int_b^a f(x) \,\mathrm{d}x$.
- $\displaystyle\int_a^a f(x) \,\mathrm{d}x=0$.
- $\displaystyle\int_a^b f(x)\,\mathrm{d}x= \int_a^c f(x) \,\mathrm{d}x+\int_c^b f(x) \,\mathrm{d}x$, para $a\leq c \leq b$.
- $\displaystyle\int_a^b f(g(x))\,g'(x)\,\mathrm{d}x=\int_{g(a)}^{g(b)} f(u)\,\mathrm{d}u$, donde $g:[a,b]\to \Omega$ es continua y diferenciable, y $f:\Omega \to \mathbb{R}$. Lo cual corresponde al cambio de variable $u=g(x)$.
- $\displaystyle\int_a^b u(x)\,v'(x)\,\mathrm{d}x=\left.\left(u(x)\,v(x)\right)\right|_a^b-\int_a^b u'(x)\,v(x)\,\mathrm{d}x$, la cual corresponde a la _integración por partes_.
- Ver la siguiente lista de integrales [link](https://en.wikipedia.org/wiki/Lists_of_integrals)
# Breve lista de ejemplos
## Donde SÍ podríamos integrar de forma exacta con seguridad
Considere $\alpha\in\mathbb{R}$, $n>1$.
- $\displaystyle\int_a^b \alpha \,\mathrm{d}x=\alpha \int_a^b 1 \,\mathrm{d}x=\alpha\,(b-a)$
- $\displaystyle\int_a^b \alpha\,x \,\mathrm{d}x=\alpha\,\left(\dfrac{b^2-a^2}{2}\right)$
- $\displaystyle\int_a^b \alpha\,x^n \,\mathrm{d}x=\alpha\,\left.\dfrac{x^{n+1}}{n+1}\right|_a^b=\alpha\,\left(\dfrac{b^{n+1}}{n+1}-\dfrac{a^{n+1}}{n+1}\right)$
- $\displaystyle\int_a^b \exp(x) \,\mathrm{d}x=\alpha\,(\exp(b)-\exp(a))$
## Donde NO podríamos integrar de forma exacta con seguridad
- $\displaystyle\int \dfrac{\sin(x)}{x} \,\mathrm{d}x$
- $\displaystyle\int \exp\left(-x^2\right) \,\mathrm{d}x$
# ¿Qué hacemos con los casos donde NO se puede integrar _exactamente_?
¡Integrar numéricamente!

# (algunos) Algoritmos de integración numérica
**Idea** para _casi_ todos: Dividir y conquistar.
## Suma de Riemann por la izquierda
Considere la siguiente partición $P=\{x_0,x_1,\dots,x_{m}\}$,$$
\begin{align*}
	c = \int_a^b f(x) dx = \sum_{k=0}^{m-1} f(x_k) (x_{k+1} - x_{k})+E_L,
\end{align*}$$
## Suma de Riemann por la derecha
Considere la siguiente partición $P=\{x_0,x_1,\dots,x_{m}\}$,$$
\begin{align*}
	c = \int_a^b f(x) dx = \sum_{k=0}^{m-1} f(x_{k+1}) (x_{k+1} - x_{k})+E_R,
\end{align*}$$
## Regla del Punto Medio
- Considerando el primer intervalo de la partición $P=\{x_0,x_1,\dots,x_{m}\}$,$$\begin{equation*}
	\int_{x_0}^{x_1} f(x)\,dx = f(x_*)\,h+\dfrac{h^3}{24}\,f^{''}(\widehat{c}),
\end{equation*}$$donde $h=x_1-x_0$, $x_*=\dfrac{x_0+x_1}{2}$, y $\widehat{c}\in[x_0,x_1]$.
- Ahora, **sumando** la aproximación para **todos** los intervalos,$$
  \begin{align*}
    \int_{a}^{b} f(x)\, dx &= 
		\sum_{i=1}^{m}      \int_{x_{i-1}}^{x_{i}} f(x)\, dx  \\
		 &= \sum_{i=1}^{m} h\, f(x_{*,i}) + \frac{(b-a)}{24}\, h^2 \, f''(\widehat{c}),
\end{align*}$$donde $h=(b-a)/m$, $x_{*,i}=\frac{1}{2}(x_{i-1}+x_{i})$ y $\widehat{c} \in [a, b]$.
```python
def midpoint(myfun, m, a, b)
    # Vectorization of function 'myfun' so it can be apply it 
    # to arrays element-wise.
    f = np.vectorize(myfun)
    # We want m bins, so we need m+1 points.
    x = np.linspace(a, b, m+1)
    h = (b-a)/m
    xhat = x[:-1] + h/2
    w = np.full(m,h)
    return np.dot(f(xhat),w)
```
## Regla del Trapecio
- Considerando el primer intervalo de la partición $P=\{x_0,x_1,\dots,x_{m}\}$,$$
\begin{equation*}
	\int_{x_0}^{x_1} f(x) dx = \frac{h}{2}\,(f(x_0) + f(x_1)) - \frac{h^3}{12}\,f''(\widehat{c}),
\end{equation*}$$donde $h=x_1-x_0$ y $\widehat{c}\in[x_0,x_1]$.
- Ahora, **sumando** la aproximación para **todos** los intervalos,$$
  \begin{align*}
    \int_{a}^{b} f(x)\, dx &= \sum_{i=1}^{m} \int_{x_{i-1}}^{x_{i}} f(x)\, dx  \\
		 &= \frac{h}{2}\,\left[f(a) + f(b) + 2\,\sum_{i=1}^{m-1} f(x_i) \right] - (b-a) \frac{h^2}{12}\, f''(\widehat{c}),
    \end{align*}$$donde $h=(b-a)/m$ y $\widehat{c} \in [a, b]$.
```python
def trapezoid(myfun, m, a, b):
    f = np.vectorize(myfun)
    x = np.linspace(a, b, m+1)
    h = (b-a)/m
    w = np.full(m+1,h)
    # Why do we need the next two lines?
    w[0]/=2
    w[-1]/=2
    return np.dot(f(x), w)
```
## Regla de Simpson
- Motivación:$$
\begin{equation*}
	\int_{x_0}^{x_2} a\,x^2 + b\,x + c\,dx = (x_1-x_0)\,\frac{1}{3}\,\left[(a\,x_0^2 + b\,x_0 + c) + 4\,(a\,x_1^2 + b\,x_1 + c) + (a x_2^2 + b\,x_2 + c)\right],
\end{equation*}$$
- Considerando el primer intervalo de la partición $P=\{x_0,x_1,\dots,x_{m}\}$,$$
\begin{equation*}
	\int_{x_0}^{x_2} f(x)\,dx = \frac{h}{3}\,(f(x_0) + 4\,f(x_1) +  f(x_2)) - \frac{h^5}{90}\,f^{(4)}(\widehat{c}),
\end{equation*}$$donde $h=x_1-x_0=x_2-x_1$ y $\widehat{c}\in[x_0,x_2]$.
- Ahora, **sumando** la aproximación para **todos** los intervalos,$$
\begin{align*}
    \int_{a}^{b} f(x)\,dx & = \frac{h}{3}\,\left(f(x_0) + 4\,\sum_{i=1}^{n} f(x_{2\,i-1}) + 2\,\sum_{i=1}^{n-1} f(x_{2\,i}) +  f(x_m)\right) \\
     &\phantom{ = } - (b-a)\frac{h^4}{180}\,f^{(4)}(\widehat{c})
    \end{align*}$$donde $h=(b-a)/m$, $\widehat{c} \in [a, b]$ y recordar que $m$ **debe ser par**.
```python
def simpsons(myfun, m, a, b):
    f = np.vectorize(myfun)
    x = np.linspace(a, b, m+1)
    h = (b-a)/m
    w = np.full(m+1,h/3)
    w[1:-1:2]*=4
    w[2:-1:2]*=2
    return np.dot(f(x), w)
```
## Cuadratura Gaussiana
Trabajaremos por simplicidad en el intervalo $[-1,1]$, similar a lo que hicimos con los [[Puntos de Chebyshev]].$$
\begin{equation*}
	\int_{-1}^1f(x)\,\mathrm{d}x\approx\sum_{i=1}^{n} w_i\,f(x_i)=\mathbf{w}\cdot f(\mathbf{x}).
\end{equation*}$$La pregunta natural que surge:
**¿Cuál sería la mejor forma de elegir los pesos $w_i$ y nodos $x_i$ para obtener $\displaystyle\int_{-1}^1 f(x)\,dx$?**
O una variante,
**¿Podríamos elegir nodos $x_i$ y pesos $w_i$ para $i\in\{1,2,\dots,n\}$ tal que integren exactamente polinomios de grado $2\,n-1$?**
### Una forma de responder la pregunta anterior
Considere el siguiente polinomio de grado $2\,n-1$,$$
\begin{equation}
	p(x)=\sum_{j=0}^{2\,n-1} a_j\,x^j,
\end{equation}$$donde $a_j\in\mathbb{R}$ son valores conocidos. Entonces, si integramos $p(x)$ obtenemos,$$
\begin{align*}
	\int_{-1}^1p(x)\,\mathrm{d}x&=\int_{-1}^1\sum_{j=0}^{2\,n-1} a_j\,x^j\,\mathrm{d}x\\
	&=\sum_{j=0}^{2\,n-1} a_j\,\int_{-1}^1x^j\,\mathrm{d}x.
\end{align*}$$Por lo tanto, si integramos **exactamente** los términos $x^j$ seríamos capaces de conocer el valor exacto de $\displaystyle\int_{-1}^1p(x)\,\mathrm{d}x$. Recordando que sí podemos integrar exactamente monomios, se obtiene,$$
\begin{align*}
	\int_{-1}^1 x^j\,\mathrm{d}x &=
	\begin{cases}
	2,&\text{si $j=0$},\\
	\dfrac{1+(-1)^j}{1+j}, &\text{e.t.o.c.}
	\end{cases}.
\end{align*}$$Por otro lado, recordando que andamos buscando los nodos $x_i$ y los pesos $w_i$ podemos integrar **también** los monomios de la siguiente forma,$$
\begin{align*}
	\int_{-1}^1 x^j\,\mathrm{d}x&=\sum_{i=1}^{n} w_i\,x_i^j.
\end{align*}$$Lo que implica las siguientes ecuaciones,$$
\begin{align*}
	\sum_{i=1}^n w_i&=2,\\
	\sum_{i=1}^n w_i\,x_i &= 0,\\
	\sum_{i=1}^n w_i\,x_i^2 &= \dfrac{2}{3},\\
	& \vdots\\
	\sum_{i=1}^n w_i\,x_i^{2\,n-1} &= 0.
\end{align*}$$Por ejemplo,
- $n=1$: $x_1=0$ y $w_1=2$.
- $n=2$: $x_1=-\sqrt{\frac{1}{3}} \approx -0.577$, $x_2=+\sqrt{\frac{1}{3}} \approx 0.577$, $w_1=1$, y $w_2=1$.
### Pero, ¿Cómo se obtienen los nodos y pesos en general?
- De una **Tabla**, o,
```python
def gaussianquad(f, m, a, b):
    x, w = gaussian_nodes_and_weights(m)
    return np.dot(w,f(x))
def gaussian_nodes_and_weights(m):
    if m==1: 
        return np.array([1]), np.array([2])
    # Why an eigenvalue problems is related to the roots of the Legendre polynomial mentioned before? (This is a tricky question!)
    beta = .5 / np.sqrt( 1. - (2.*np.arange(1.,m))**(-2) )
    T = np.diag(beta,1) + np.diag(beta,-1)
    D, V = np.linalg.eigh(T)
    x = D
    w = 2*V[0,:]**2
    return x, w
```

# En resumen
Integración numérica puede entenderse como una aplicación de un producto interno (punto) conveniente!

#OK
#Tema_8