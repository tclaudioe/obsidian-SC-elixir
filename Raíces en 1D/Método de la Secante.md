Primero debemos recordar la definición de la derivada,
$$f'(x)=\lim_{\Delta x \to 0} \dfrac{f(x+\Delta x)-f(x)}{\Delta x},$$
la cual adaptaremos de la siguiente forma,
$$f'(x)=\lim_{\Delta x \to 0} \dfrac{f(x)-f(x-\Delta x)}{\Delta x}.$$
Ahora, para presentar el método de la Secante ahora debemos construir una aproximación de [[Diferencias Finitas]] de la derivada de una función. Considere que usted tiene un [[versión numérica]] de una función $f(x)$, es decir:
$$
\{(x_1,f(x_1)),\dots,(x_{i-1},f(x_{i-1})),(x_i,f(x_i)),(x_{i+1},f(x_{i+1})),\dots,(x_n,f(x_n))\}
$$
Siguiendo la misma idea de la segunda aproximación presentada anteriormente obtenemos,$$f'(x_i)=\lim_{\Delta x \to 0} \dfrac{f(x_i)-f(x_i-\Delta x)}{\Delta x}\approx \dfrac{f(x_i)-f(x_{i-1})}{x_i-x_{i-1}}$$
Es decir, se aproxima $f'(x_i)$ por $\dfrac{f(x_i)-f(x_{i-1})}{x_i-x_{i-1}}$. Con esta aproximación podemos construir el método de la Secante a partir de método de Newton de la siguiente forma: $$\begin{align}
x_{i+1} &= x_i-\dfrac{f(x_i)}{f'(x_i)},\\
	&\approx x_i-\dfrac{f(x_i)}{\dfrac{f(x_i)-f(x_{i-1})}{x_i-x_{i-1}}}\\
	&\approx x_i-\dfrac{f(x_i)\,(x_i-x_{i-1})}{f(x_i)-f(x_{i-1})}
\end{align}$$
Entonces podemos definir el método de la Secante,
$$
\begin{align}
x_0 &= \text{''initial guess 1''}\\
x_1 &= \text{''initial guess 2''}\\
x_{i+1} &= x_i-\dfrac{f(x_i)\,(x_i-x_{i-1})}{f(x_i)-f(x_{i-1})}
\end{align}
$$
# Consideraciones
- **Ventajas**:
	- Exhibe #ConvergenciaSuperLineal , lo cual es mejor que el [[Método de la Bisección]].
	- No require la derivada como el [[Método de Newton]]
- **Desventajas**:
	- Requiere dos *initial guesses*. Lo cual se puede resolver perturbando un poco el *initial guess* tradicional, pero ejemplo si $x_0=\alpha$  entonces $x_1=\alpha+\delta$, donde $\delta$ es un número pequeño. Uno podría relacionarlo con [[Machine Epsilon]].

# Ejemplo numérico
Se utilizará el mismo ejemplo presentado en el [[Método de Newton]], pero adaptado al método de la secante,
```run-python
f = lambda x: x-np.cos(x)
x0 = 1
x1 = 1.1
for i in np.arange(10):
	if np.abs(x1-x0)>0:  
		x2 = x0-f(x0)*(x1-x0)/(f(x1)-f(x0))
		x0 = x1
		x1 = x2 
		print(i, ',', x2)
	else:
		print('Root found!')
		break
```

**Nota**: En este caso se requirió incluir un segundo _initial guess_ y además tomó más iteraciones que en el [[Método de Newton]], sin embargo no se necesitó la derivada de la función $f(x)$.

#OK