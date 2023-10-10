#Teorema #ErrorDeInterpolación
	    Asuma que $p(x)$ es el polinomio interpolador (de grado $n-1$ o menor) que ajusta $n$ puntos $(x_{1}, y_{1}), \dots, (x_{n}, y_{n})$.  El error de interpolación es,
        $$\begin{equation*}
            f(x) - p(x) = \frac{(x-x_{1})(x-x_{2})...(x-x_{n})}{n!} \, f^{(n)}(c),
        \end{equation*}$$donde $c$ está entre el menor y el mayor de los números $x, x_{1}, ... , x_{n}$.

Para entender mejor lo que significa el error que se genera al interpolar una función $f(x)$ por medio de un polinomio con $n$ puntos se presenta el siguiente ejemplo donde se interpola la función $f(x)=\dfrac{1}{1+12\,x^2}$, para este caso particular se utiliza $n=5$.

```run-python
f = lambda x: 1./(1.+12*np.power(x,2.))
x_interpolation = np.linspace(-1,1,5)
y_interpolation = f(x_interpolation)
p = BarycentricInterpolator(x_interpolation, y_interpolation)
xx = np.linspace(-1,1,1000)

plt.figure(figsize=(4,8))
plt.subplot(211)
plt.plot(xx, f(xx), 'k-', label='$f(x)$')
plt.plot(x_interpolation, y_interpolation, 'ro', label='data')
plt.plot(xx, p(xx), 'b-', label='$p(x)$')
plt.grid(True)
plt.xlabel(r'$x$')
plt.legend()

plt.subplot(212)
plt.semilogy(xx, abs(f(xx)-p(xx)),'c-', label='Absolute Error')
plt.xlabel('$x$')
plt.grid(True)
plt.legend()
plt.xlabel(r'$x$')
plt.show()
```

# Análisis

Una posible interpretación de lo que _nos dice_ la fórmula de error de interpolación, que se repite acá por completitud, $$\begin{equation*}
	f(x) - p(x) = \frac{(x-x_{1})(x-x_{2})...(x-x_{n})}{n!} \, f^{(n)}(c),
\end{equation*}$$ es que la diferencia (o error) generado por el polinomio interpolador $p(x)$ al interpolar la función $f(x)$ con $n$ puntos, **depende** de:
1. **De la cantidad de puntos $n$**, que hace que _disminuya_ el lado derecho al aumentar $n$, pero lamentablemente no es suficiente.
2. **De la $n$-ésima derivada de $f(x)$, es decir $f^{(n)}(c)$**, que no depende de _nosotros_.
3. **Y de $(x-x_{1})(x-x_{2})...(x-x_{n})$**, de lo cual no controlamos $x$ pero sí podríamos elegir los puntos de interpolación $x_i$. Por ejemplo, si se utilizan puntos **equiespaciados** entonces aparece el [[Fenómeno de Runge]], pero si elegimos otros puntos, tales como los [[Puntos de Chebyshev]], esto puede mejorar mucho!

#OK 
#Tema_6