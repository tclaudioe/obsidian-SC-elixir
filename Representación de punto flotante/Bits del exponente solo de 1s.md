La siguiente tabla resume las opciones de este caso:

| $s$ | $e_1$ $e_2$ $\dots$ $e_{11}$ | $b_1$ $b_2$ $\dots$ $b_{52}$ | lo que representa |
|--|------------|-----------|-------------------|
| $0$ | $1$ $1$ $\dots$ $1$ | $0$ $0$ $\dots$ $0$ | $+\infty$ |
| $1$ | $1$ $1$ $\dots$ $1$ | $0$ $0$ $\dots$ $0$ | $-\infty$ |
| $1$ | $1$ $1$ $\dots$ $1$ | - - $\dots$ - | NaN |

donde *-* en la 3ra columna representa por lo menos 1 valor del *string* distinto de $0$. **En los siguientes ejemplos se observa como se pueden obtener tales casos**.

- Ejemplo 1: $+\infty$
```run-python
x = 1.0/np.power(2.,-1024)
to_fps_double(x)
print(np.inf)
to_fps_double(np.inf)
```
- Ejemplo 2: $-\infty$
```run-python
x = -1.0/np.power(2.,-1024)
to_fps_double(x)
x = -np.inf
to_fps_double(x)
```
- Ejemplo 3: NaN
```run-python
x = np.power(2.,-1075)/np.power(2.,-1075)
to_fps_double(x)
x = np.nan
to_fps_double(x)
```

#OK
#Tema_1