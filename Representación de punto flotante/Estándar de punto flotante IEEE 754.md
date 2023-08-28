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
# Links sugeridos
- [Jupyter Notebook de Representación de Punto Flotante](https://github.com/tclaudioe/Scientific-Computing/blob/master/SC1v2/02_floating_point_arithmetic.ipynb)

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