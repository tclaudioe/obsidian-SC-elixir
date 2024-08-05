#Definition **Regla de redondeo IEEE**: _Para #doublePrecision , si el 53vo bit a la derecha del punto binario es 0, redondear hacia abajo (truncar después del bit 52). Si el 53vo bit es 1, entonces redondear hacia arriba (agregar 1 al bit 52), a menos que todos los bits **conocidos** a la derecha de 1 son 0, en este caso 1 es agregado al bit 52 si y solo si el bit 52 es 1._
- Ejemplo 1: ¿Qué ocurre acá?
```run-python
x=1.+np.power(2.,-53)
to_fps_double(x)
```
- Ejemplo 2: ¿Cómo se está sumando?
```run-python
x=1.+np.power(2.,-53)+np.power(2.,-54)
to_fps_double(x)
```
- Ejemplo 3: ¿Es distinto al resultado anterior?
```run-python
x=(1.+np.power(2.,-53))+np.power(2.,-54)
to_fps_double(x)
```
- Ejemplo 4: ¿Qué resultado se obtiene?
```run-python
x=1.+(np.power(2.,-53)+np.power(2.,-54))
to_fps_double(x)
```
- Ejemplo 5: ¿Es distinto al Ejemplo 1? ¿Por qué?
```run-python
x=1.+np.power(2.,-52)
to_fps_double(x)
x=1.+np.power(2.,-52)+np.power(2.,-53)
to_fps_double(x)
```

#OK
#Tema_1