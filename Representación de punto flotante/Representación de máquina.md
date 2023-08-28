En #doublePrecision los 64 *bits* se utilizan de la siguiente forma y se denomina *representación normal* #repNormal:

|  $s$ | $e_1\,e_2\,\dots\,e_{11}$| $b_1\,b_2\,\dots\,b_{51}\,b_{52}$|
|---|------------|------|
| 1 bit | 11 bits | 52 bits |
donde
- $s$ corresponde al signo: 0 si es positivo y 1 si es negativo.
- $e_1\,e_2\,\dots\,e_{11}$ corresponde al exponente: $p=e_{1} \, 2^{10} + e_{2} \, 2^{9}+ e_{3} \, 2^{8} + \dots + e_{11} \, 2^{0} - 1023$. Notar que se excluyen en este caso cuando los *bits* del exponente son todos $0$ o cuando son todos $1$.
- $m=b_1\,2^{-1}+b_2\,2^{-2}+\dots+b_{52}\,2^{-52}$ corresponde a la mantisa.
- El número representado corresponde a $\pm (1+m)\,2^p$.
- El coeficiente $1023$ del exponente se denomina *shift* y nos ayuda a definir número tanto positivos como negativos en el exponte.
- Ejemplo 1:
```run-python
x = np.exp(1)
print(x)
to_fps_double(x)
print(int('0b01111111111', 2))
print(int('0b10000000000', 2))
```
- Ejemplo 2:
```run-python
x = 1.0
to_fps_double(x)
for i in np.arange(1,52+1):
	x+=np.power(2.,-i)
to_fps_double(x)
to_fps_double(x+np.power(2.,-52))
```

# Casos especiales:
- [[Bits del exponente solo de 1s]]: $e_1\,e_2\,\dots\,e_{11}=11111111111$. 
- [[Bits del exponente solo de 0s]]: $e_1\,e_2\,\dots\,e_{11}=00000000000$. #repSubNormal 

#OK
