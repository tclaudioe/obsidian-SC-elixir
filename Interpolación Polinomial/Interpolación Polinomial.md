# Introducción
La idea principal detrás de **Interpolación Polinomial** consiste en construir un polinomio a partir de observaciones o datos puntuales de una función. Por ejemplo considere la siguiente gráfica:
```run-python
xi = np.linspace(-1,1,5)
yi = np.array([-0.90929743, -0.84147098, 0., 0.84147098, 0.90929743])
plt.figure(figsize=(4,4))
plt.plot(xi,yi,'.', markersize=10)
plt.xlabel('$x$')
plt.ylabel('$y$')
plt.grid(True)
plt.show()
```
Las preguntas/comentarios que surgen son los siguientes:
- En este caso se tiene la [[versión numérica]] de una función en 5 puntos equiespaciados.
	- Los puntos en este caso están en orden pero podrían no estarlos.
	- Los puntos en este caso son equiespaciados pero podrían no serlos.
	- En general uno puede tener $n$ puntos.
- ¿Tiene que ser un polinomio?
	- Es lo primero que se estudia, pero hay otras opciones.
- ¿Qué se puede hacer si quiero obtener alguna estimación de la función que representan estos **datos** en un punto que no está en la grilla?
	- Podría conectar puntos consecutivos con una línea recta y listo. Pero surge la inquietud, ¿Es lo mejor que se puede hacer? Así quedaría la **interpolación** con lineas rectas.
```run-python
xi = np.linspace(-1,1,5)
yi = np.array([-0.90929743, -0.84147098, 0., 0.84147098, 0.90929743])
plt.figure(figsize=(4,4))
plt.plot(xi,yi,'.', markersize=10)
plt.plot(xi,yi,'-k')
plt.xlabel('$x$')
plt.ylabel('$y$')
plt.grid(True)
plt.show()
```
- En este caso particular la **data** corresponde a la función $\sin(2\,x)$, pero en general: 
	- No tiene acceso a esa información, es decir, no tiene como saber cómo se generó la **data**.
	- o incluso si se tuviera la función, podría ser muy costosa evaluarla y sería necesario tener una **versión** menos costosa de evaluar. Por ejemplo, una función que podría considerarse costosa de evaluar es la función Gamma, definida de la siguiente forma, $$\Gamma(x)=\displaystyle\int_0^\infty y^{x-1}\,\exp(-x)\,\mathrm{d}y, \quad \mathrm{Re}(x)>0.$$ En este caso es importante notar que para evaluar $\Gamma(x)$ en un $x$ determinado, se debe evaluar la integral de $0$ a $\infty$.
- Dado que sabemos cómo se generó la data, podemos comparar la **data**, la aproximación por líneas rectas entre puntos y la función **exacta**,
```run-python
xi = np.linspace(-1,1,5)
yi = np.array([-0.90929743, -0.84147098, 0., 0.84147098, 0.90929743])
plt.figure(figsize=(4,4))
plt.plot(xi,yi,'.', markersize=10, label='data')
plt.plot(xi,yi,'-k', label='Piecewise linear')
xx = np.linspace(-1,1,100)
plt.plot(xx,np.sin(2*xx),'-', label='Exact function')
plt.xlabel('$x$')
plt.grid(True)
plt.show()
```
- Entonces, **¿se podrá hacer algo mejor?**
	- La respuesta en general es depende!
	- En nuestro caso utilizaremos **interpolación polinomial**.

# Teoría
- #InterpolaciónEn1D Una función $y=p(x)$ _interpola_ los datos $(x_{1},y_{1}),\dots,(x_{n},y_{n})$ si $p(x_{i})=y_{i}$ para cada $i\in\{1,...,n\}$.
- #UnicidadDeLaInterpolaciónPolinomial Sea $(x_{1},y_{1}),...,(x_n,y_n)$ $n$ puntos en el plano $\mathbb{R}^2$ con distintos $x_i$, **entonces** existe uno y sólo un polinomio $p(x)$ de grado $(n-1)$ o menor que satisface la siguiente ecuación: $p(x_{i}) = y_{i}$ para $i\in\{1,2,\dots,n\}$.
- **Importante**: 
	- En esta sección es crítico considerar que los datos $x_i$ son todos distintos, pueden estar cerca pero deben ser distintos.
	- El interpolador polinomial es único, así que da lo mismo como se construya, considerando aritmética exacta.
	- El polinomio interpolador de $n$ puntos puede ser a lo más de grado $n-1$ o menor.
# Algoritmos de interpolación a estudiar

- [[Interpolación con la matriz de Vandermonde]]
- [[Interpolación de Lagrange]]
- [[Interpolación Baricéntrica]]

# Más teoría y aplicaciones de interpolación polinomial

Una de las principales aplicaciones de interpolación polinomial es para aproximar funciones, digamos $f(x)$. Y uno de las razones para utilizar alguno de los algoritmos estudiados anteriormente, es que solo requiere la utilización de operaciones elementales, que son lo que mejor hace un procesador!

- [[Error de Interpolación Polinomial]]
- [[Fenómeno de Runge]]
- [[Puntos de Chebyshev]]

#OK 