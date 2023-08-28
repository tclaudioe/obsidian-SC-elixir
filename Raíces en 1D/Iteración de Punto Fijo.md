#Definition #PuntoFijo El número real $r$ es un punto fijo de la función $g(x)$ si $g(r) = r$.

# Algoritmo
#IPF
```python
xi = "inital guess"
for i in np.arange(n):
	xi = g(xi)
	if "Criterio de detención logrado":
		break
```
ver [[Criterio de detención para búsqueda de raíces 1D]] 

# Objetivo
1. Encontrar el punto fijo de una función $g(x)$.

# Consideraciones
1. **Ventaja**: Muy simple de implementar y generalizar!
2. **Desventaja**: La construcción de funciones $g(x)$ puede generar iteraciones convergentes y divergentes.

# Ejemplo
Función $g(x)=\cos(x)$ con *initial guess* $x_0=1.0$.
```run-python
xi = 1.0
g = lambda x : np.cos(x)
for i in np.arange(30):
	xi = g(xi)
	if np.mod(i,3) == 0: # Just to show fewer output lines
		print(xi)
```

La cual genera un **punto fijo** cercano al punto $r\approx0.7390851332151607$.

# Aplicaciones
- Búsqueda de raíces #root. Considere la función $f(x)$, uno podría construir una #IPF que encuentre la raíz de la función,  recuerde que la raíz satisface la siguiente ecuación: $f(r)=0$, ahora, considere el siguiente desarrollo. Notar que no es necesario conocer la raíz $r$: $$
  \begin{align}
	  0 &= f(r), \text{ multipliquemos por $a$}\\
	  0 &= a\,f(r), \text{ sumemos $r$}\\
	  r &= r+a\,f(r)=g(r), \text{ definimos $g(r)$}\\
	  r &= g(r).
  \end{align}
$$
Entonces de esta forma hemos construido un punto fijo $r=g(r)$ tal que el valor de $r$ también es la raíz de $f(x)$!! 😀 La única duda que surge acá es la definición de $a$, en este caso diremos que es un parámetro ajustable. Ver [[Convergencia de una Iteración de Punto Fijo]] .
# Link sugeridos
- [Diagrama Cobweb](https://en.wikipedia.org/wiki/Cobweb_plot)
- [[Convergencia de una Iteración de Punto Fijo]]

#OK