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

# Ejemplo 1
Función $g(x)=\cos(x)$ con *initial guess* $x_0=1.0$.
```run-python
xi = 1.0
g = lambda x : np.cos(x)
for i in np.arange(91):
	xi = g(xi)
	# if np.mod(i,3) == 0: # Just to show fewer output lines
	print(xi)
```

La cual genera un **punto fijo** cercano al punto $r\approx0.7390851332151607$.

```run-python
# Visual version
x0 = 1.0
g = lambda x : np.cos(x)
# G = lambda x : x+(-1/(1+2*np.sin(1)))*(x-g(x))
x = fpi_reduced(g,x0,k=10)
print('x:', x)
f = cobweb2(x,g)
plt.show()
```

# Ejemplo 2
Considere la siguiente iteración de punto $g(x)=2\,\cos(x)$, con _initial guess_ $x_0=1.0$. ¿Se puede corregir la iteración de punto fijo tal que se puede efectivamente llegar al punto fijo $r=2\,\cos(r)$? La respuesta es sí. Ver más abajo la nueva iteración de punto fijo convergente.
```run-python
# Visual version

x0 = 1.0
g = lambda x : 2*np.cos(x)
x = fpi_reduced(g,x0,k=10)
print('x:', x)

f = cobweb2(x,g)
plt.show()

```

```run-python
# Visual version

x0 = 1.0
g = lambda x : 2*np.cos(x)
G = lambda x : x+(-1/(1+2*np.sin(1)))*(x-g(x))
x = fpi_reduced(G,x0,k=10)
print('x:', x)

f = cobweb2(x,G)
plt.show()

```
# Aplicaciones
- Búsqueda de raíces #root. Considere la función $f(x)$, uno podría construir una #IPF que encuentre la raíz de la función,  recuerde que la raíz satisface la siguiente ecuación: $f(r)=0$, ahora, considere el siguiente desarrollo. Notar que no es necesario conocer la raíz $r$: $$
  \begin{align}
	  0 &= f(r), \text{ multipliquemos por $a$}\\
	  0 &= a\,f(r), \text{ sumemos $r$}\\
	  r &= r+a\,f(r)=g(r), \text{ definimos $g(r)$}\\
	  r &= g(r).
  \end{align}$$
Entonces de esta forma hemos construido un punto fijo $r=g(r)$ tal que el valor de $r$ también es la raíz de $f(x)$!! 😀 La única duda que surge acá es la definición de $a$, en este caso diremos que es un parámetro ajustable. Ver [[Convergencia de una Iteración de Punto Fijo]] .
# Link sugeridos
- [Diagrama Cobweb](https://en.wikipedia.org/wiki/Cobweb_plot)
- [[Convergencia de una Iteración de Punto Fijo]]

#OK
#Tema_3