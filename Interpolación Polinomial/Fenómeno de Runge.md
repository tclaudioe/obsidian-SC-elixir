El fenómeno de Runge se puede apreciar mejor en la siguiente gráfica,
```run-python
m = 5
n = 2*m+1
x_interpolation = np.linspace(-1,1,n)
y_interpolation = np.zeros(n)
y_interpolation[m] = 1
p = BarycentricInterpolator(x_interpolation, y_interpolation)
xx = np.linspace(-1,1,1000)

plt.figure(figsize=(4,4))
plt.plot(x_interpolation, y_interpolation, 'ro', label='data')
plt.plot(xx, p(xx), 'b-', label='$p(x)$')
plt.grid(True)
plt.xlabel(r'$x$')
plt.legend()

plt.show()
```

Es decir, aparecen oscilaciones en los extremos del intervalo de interpolación. Las cuales afectan negativamente la interpolación polinomial.

#OK 
#Tema_6