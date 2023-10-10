En este caso se obtiene la representación sub-normal #repSubNormal definida de la siguiente forma:
$\pm 0\centerdot b_{1}\,\,b_{2}\,\,b_{3}\,\,\ldots\,\,b_{52}\,\,\cdot 2^{-1022}$
Es decir, en esta caso se representan número aún más pequeños en magnitud que utilizando la #repNormal.

- Ejemplo 1: Menor número representable.
```run-python
x = np.power(2.,-52)*np.power(2.,-1022)
print(x)
to_fps_double(x)
```
- Ejemplo 2: ¿Qué ocurre si se quiere un número menor que el anterior?
```run-python
x = np.power(2.,-52)*np.power(2.,-1022)*(np.power(2.,-1))
print(x)
to_fps_double(x)
```
- Ejemplo 3: ¿Cómo aplica la [[Regla de redondeo]]?
```run-python
x = np.power(2.,-52)*np.power(2.,-1022)*(np.power(2.,-1)+np.power(2.,-2))
print(x)
to_fps_double(x)
```
- Ejemplo 4: ¿Cómo aplica la [[Regla de redondeo]]?
```run-python
x = np.power(2.,-52)*np.power(2.,-1022)*(np.power(2.,-2)+np.power(2.,-3))
print(x)
to_fps_double(x)
```

#OK
#Tema_1