Se denomina *versión numérica* de una función cuando uno no tiene acceso a la definición algebraica de una función. Por ejemplo:
- **Versión algebraica**: $f(x)=x^2$.
- **Versión numérica**: $$\{(0,0),(1,1),(2,4),(3,9)\},$$ donde el conjunto de pares ordenados representan las coordenadas $x$ y $y$ respectivamente. Notar que la versión numérica solo representa una muestra de puntos de la versión algebraica, sin embargo ambas representan la función en cuestión.

La gráfica inferior muestra la comparación entre la versión continua (algebraica) en azul y la versión discreta (numérica) con puntos negros.

```run-python
f = lambda x: np.power(x,2)
# This is more or less continue.
x_continuo = np.linspace(-1,4,100)

x_discreto = np.array([0,1,2,3])
y_discreto = np.array([0,1,4,9])

plt.figure()
plt.plot(x_continuo,f(x_continuo),'b-',label='$f(x)$')
plt.plot(x_discreto,y_discreto,'k.', markersize=10, label='$f(x_i)$')

plt.legend(loc='best')
plt.xlabel('$x$')
plt.ylabel('$y$')
plt.grid(True)
plt.show()
```

#OK
#Tema_3