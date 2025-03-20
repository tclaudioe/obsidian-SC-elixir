![[02_AritmeticaPuntoFlotante.tex.png]]
- Utilizando el estándar 754 de la IEEE solo podemos representar un sub-conjunto de números reales cuando fijamos la cantidad de memoria disponible, en particular se usará 64 *bits* o 8 *Bytes* en la mayoría de los casos.
- La representación de punto flotante se puede entender como la notación científica para representar números pero en base *2*. Es decir, cualquier número se puede representar en formato de punto flotante.
- La representación de punto flotante se debe restringir para poder utilizarse en el almacenamiento de números *reales* en un computador.
- La representación de máquina de #doublePrecision considera el uso de:
	- 1 *bit* para el signo: 0 para positivo y 1 para negativo
	- 52 *bits* para la mantisa
	- 11  *bits* para el exponente
- Los 64 *bits* para representar un número real $x$ en #doublePrecision se utilizan de la siguiente forma:
	- $x\approx \pm (1+m)\,2^p$,
	- donde la mantisa es: $m=b_1\,2^{-1}+b_2\,2^{-2}+\dots+b_{52}\,2^{-52}$,
	- y el exponente $p=e_{1} \, 2^{10} + e_{2} \, 2^{9}+ e_{3} \, 2^{8} + \dots + e_{11} \, 2^{0} - 1023$.
- El coeficiente $1023$ se corresponde al *shift* utilizado para representar número positivos y negativos del exponente.
- Los coeficientes binarios $b_i$ y $e_j$ de la mantisa y exponente se definen en función del número a representar.
- En general si se quiere almacenar el número real $x$ en memoria, solo se puede asegurar que se almacenará  $\text{fl}(x)$ #notacionFL , que corresponde a la aproximación de punto flotante de $x$, por ejemplo si se considera #doublePrecision significa que se usarán los 64 *bits* para almacenar el número más cercano a $x$, es decir $\text{fl}(x)$. #repNormal #repSubNormal.
# Un ejemplo simple de por qué necesitamos estar pendiente de la computación que se haga con _double precision_

```run-python
# Se almacena un número de ejemplo en _double precision_
a = 1./9
# Se muestra el número almacenado
print(a)
# Se muestra los bits asociados al _signo_, _exponente_, y _mantisa_
to_fps_double(a)
```
Ejecutamos la siguiente computación que en **aritmética exacta** debería dar $0$.
```run-python
b = (a*10)/10 - a
print(b)
```
El problema es que no es $0$ pero debe ser $0$. Miremos los pasos intermedio.
```run-python
print('(a*10):')
to_fps_double((a*10))
print('(a*10)/10:')
to_fps_double((a*10)/10)
print('b=(a*10)/10 - a')
to_fps_double(b)
print('np.log2(b):',np.log2(b))
```

En la práctica multiplicar por $10$ y luego dividir por $10$ en _double precision_ no se cancela exactamente, solo aproximadamente debido a que tenemos una cantidad finita de bits para ser representados computacionalmente.
# Links sugeridos
- [Jupyter Notebook de Representación de Punto Flotante](https://github.com/tclaudioe/Scientific-Computing-V3/blob/main/02_floating_point_arithmetic.ipynb)

# Temas adicionales relacionados
- [[Representación de máquina]]
- [[Machine Epsilon]]
- [[Regla de redondeo]]

# Preguntas
#Challenge 
- ¿Cuál es el menor número positivo representable?
- ¿Cuál es la diferencia entre [[Machine Epsilon]] y el menor número positivo representable?
- Considerando #doublePrecision :
	- ¿Cuántos números se pueden representar entre $1$ y $2$?
	- ¿Cuántos números se pueden representar entre $2$ y $4$?
	- ¿Cuántos números se pueden representar entre $2$ y $3$?
	- ¿Cuántos números se pueden representar entre $1/2$ y $1$?

#OK
#Tema_1