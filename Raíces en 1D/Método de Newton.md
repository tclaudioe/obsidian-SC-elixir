El método de Newton no es más que una iteración de punto fijo para la búsqueda de raíces de una función, la cual se define de la siguiente forma:$$
x_{i+1}=x_i-\dfrac{f(x_i)}{f'(x_i)}$$donde se puede denotar el lado derecho como $g_N(x)=x-\dfrac{f(x)}{f'(x)}$.

# Ejemplo
Buscar la raíz de $f(x)=x-\cos(x)$. Lo primero que necesitamos hacer es obtener la derivada de $f(x)$, es decir $f'(x)=1+\sin(x)$, entonces,
```run-python
f  = lambda x: x-2*np.cos(x)
fp = lambda x: 1+2*np.sin(x)
xi = 1
for i in np.arange(5):
	xi = xi-f(xi)/fp(xi)
	print(i, ',', xi)
```
*Notar que converge rapidísimo!* #ConvergenciaCuadrática
En este caso al [[Criterio de detención para búsqueda de raíces 1D]] utilizado fue simplemente la cantidad de iteraciones.

# Consideraciones
- **Ventaja**:
	1. #ConvergenciaCuadrática . [[Convergencia Numérica]]
	2.  Puede exhibir #ConvergenciaLineal  si justo la raíz está en un punto crítico, es decir, si tenemos que $f(r)=0$ y además $f'(r)=0$. Sin embargo, igual hay convergencia!
- **Desventajas**: Requiere la derivada de la función.
- #Teorema #ConvergenciaLinealDelMétodoDeNewton:
    	Asuma que $f$ es una función $(m + 1)$ - veces continua y diferenciable en $[a,b]$ y tiene una multiplicidad $m$ en la raíz $r$. Entonces el método de Newton es linealmente convergente a $r$ con tasa $S$,$$
	\lim_{i\to \infty} \frac{e_{i+1}}{e_i} = \frac{m-1}{m} = S \neq 0.$$
# Método de Newton modificado

En el caso de que el método de Newton exhiba #ConvergenciaLineal , esto significa que la multiplicidad de la raíz es mayor a $1$. Digamos que sea $m$, entonces el método de Newton se debe modificar para que siga exhibiendo #ConvergenciaCuadrática . La modificación es la siguiente:$$x_{i+1}=x_i-m\,\dfrac{f(x_i)}{f'(x_i)}$$
**Nota**: Una forma numérica de determinar la multiplicidad de la raíz es ejecutar el método de Newton original y determinar la tasa $S$ respectiva, entonces a partir de $S$ y del teorema #ConvergenciaLinealDelMétodoDeNewton se puede estimar $m$.

#OK
#Tema_3