Recordando la fórmula del [[Error de Interpolación Polinomial]]:$$\begin{equation*}
	f(x) - p(x) = \frac{(x-x_{1})(x-x_{2})...(x-x_{n})}{n!} \, f^{(n)}(c).
\end{equation*}$$Entonces, de análisis realizado anteriormente, lo _único_ que podemos **elegir** adicionalmente en la fórmula anterior son los puntos de interpolación $x_i$. En particular, nos interesa buscar los puntos $x_i$ tales que minimice el peor caso de:
$$\min_{\displaystyle{x_i\in[-1,1]}} \max_{\displaystyle{-1 \leq x \leq 1}} \left|(x-x_1)\dots(x-x_n)\right|.$$ Lo cual se resume en el siguiente #Teorema :

#Teorema  #TeoremaDeChebyshev
    	La elección de los números reales $-1 \leq x_1,x_2,\dots,x_n \leq 1$ que hace el valor de   	$$\begin{align*}
    		\max_{-1 \leq x \leq 1} \left|(x-x_1)\dots(x-x_n)\right|
    	\end{align*}$$lo más pequeño posible es   	$$\begin{equation*}
    		 x_i = \cos \left( \frac{(2\,i-1)\,\pi}{2\,n}\right), \quad i\in\{1,2,\dots,n\}, 
    	\end{equation*}$$
    	y el valor mínimo es $\dfrac{1}{2^{n-1}}$. De hecho, el mínimo es alcanzado por:   	$$\begin{equation*}
    		(x-x_1)...(x - x_n) = \dfrac{1}{2^{n-1}} \, T_n(x),
    	\end{equation*}$$donde $T_n(x) = \cos \left(n \, \arccos(x)\right)$ es polinomio de Chebyshev de grado $n$

Para visualizar qué ocurre con la utilización de los puntos de Chebyshev en el mismo experimento realizado en el [[Fenómeno de Runge]], donde se observaba oscilaciones en el extremo del intervalos, se repetirá pero utilizando los puntos de Chebyshev,

```run-python
m = 3
n = 2*m+1
i = np.arange(1,n+1)
theta_i = (2*i-1)*np.pi/(2*n)
x_cheb = np.cos(theta_i)
y_cheb = np.zeros(n)
y_cheb[m] = 1
p = BarycentricInterpolator(x_cheb, y_cheb)
xx = np.linspace(-1,1,1000)

plt.figure(figsize=(4,4))
plt.plot(xx, p(xx), 'b-', label='$p(x)$')
plt.plot(x_cheb, y_cheb, 'mo', label='data')
plt.grid(True)
plt.xlabel(r'$x$')
plt.legend()

plt.show()
```

En resumen, ahora no hay oscilaciones!!

## Comparación entre puntos equiespaciados y puntos de Chebyshev para interpolación polinomial

```run-python
n = 7

f = lambda x: 1./(1.+12*np.power(x,2.))

x_interpolation = np.linspace(-1,1,n)
y_interpolation = f(x_interpolation)
p = BarycentricInterpolator(x_interpolation, y_interpolation)

i = np.arange(1,n+1)
theta = (2*i-1)*np.pi/(2*n)
x_cheb = np.cos(theta)
y_cheb = f(x_cheb)
p_cheb = BarycentricInterpolator(x_cheb, y_cheb)

xx = np.linspace(-1,1,1000)

plt.figure(figsize=(6,8))
plt.subplot(211)
plt.plot(xx, f(xx), 'k-', label='$f(x)$')

plt.plot(x_interpolation, y_interpolation, 'bo', label='data', alpha=0.5)
plt.plot(xx, p(xx), 'b-', label='equiespaciados')

plt.plot(x_cheb, y_cheb, 'go', label='data', alpha=0.5)
plt.plot(xx, p_cheb(xx), 'g-', label='Chebyshev')

plt.grid(True)
plt.xlabel(r'$x$')
plt.legend()

plt.subplot(212)
plt.semilogy(xx, abs(f(xx)-p(xx)),'b-', label='Abs. Error Equi.')
plt.semilogy(xx, abs(f(xx)-p_cheb(xx)),'g-', label='Abs. Error Cheb.')
plt.xlabel('$x$')
plt.grid(True)
plt.legend()
plt.xlabel(r'$x$')
plt.show()
```

```run-python
n = 21

f = lambda x: 1./(1.+12*np.power(x,2.))

x_interpolation = np.linspace(-1,1,n)
y_interpolation = f(x_interpolation)
p = BarycentricInterpolator(x_interpolation, y_interpolation)

i = np.arange(1,n+1)
theta = (2*i-1)*np.pi/(2*n)
x_cheb = np.cos(theta)
y_cheb = f(x_cheb)
p_cheb = BarycentricInterpolator(x_cheb, y_cheb)

xx = np.linspace(-1,1,1000)

plt.figure(figsize=(6,8))
plt.subplot(211)
plt.plot(xx, f(xx), 'k-', label='$f(x)$')

plt.plot(x_interpolation, y_interpolation, 'bo', label='data', alpha=0.5)
plt.plot(xx, p(xx), 'b-', label='equiespaciados')

plt.plot(x_cheb, y_cheb, 'go', label='data', alpha=0.5)
plt.plot(xx, p_cheb(xx), 'g-', label='Chebyshev')

plt.grid(True)
plt.xlabel(r'$x$')
plt.legend()

plt.subplot(212)
plt.semilogy(xx, abs(f(xx)-p(xx)),'b-', label='Abs. Error Equi.')
plt.semilogy(xx, abs(f(xx)-p_cheb(xx)),'g-', label='Abs. Error Cheb.')
plt.xlabel('$x$')
plt.grid(True)
plt.legend()
plt.xlabel(r'$x$')
plt.show()
```

#OK 
#Tema_6