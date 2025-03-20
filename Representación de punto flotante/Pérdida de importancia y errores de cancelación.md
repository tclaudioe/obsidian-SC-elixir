![[02_AritmeticaPuntoFlotante.tex.png]]
Para entender la pérdida de importancia nos podemos basar en la representación de los siguientes número reales: $1$, $x$ y $1+\epsilon_{\text{mach}}$, en la recta numérica.

```tikz
\begin{document}
  \begin{tikzpicture}[scale=1]        		
        	  	\draw[->,line width=1pt]  (-0.3,0) -- (3.5,0) coordinate[label = {below:}] (xmax);
          		\draw[shift={(1,0)},color=black] (0pt,3pt) -- (0pt,-3pt);
          		\draw[shift={(1,0)},color=black] (0pt,0pt) -- (0pt,-3pt) node[below] {$1$};
          		\draw[shift={(2,0)},color=black] (0pt,3pt) -- (0pt,-3pt);
          		\draw[shift={(2,0)},color=black] (0pt,0pt) -- (0pt,-3pt) node[below] {$x$};
          		\draw[shift={(3,0)},color=black] (0pt,3pt) -- (0pt,-3pt);
          		\draw[shift={(3,0)},color=black] (0pt,0pt) -- (0pt,-3pt) node[below] {$1 + \epsilon_{mach}$};
        	\end{tikzpicture}
\end{document}
```
donde $\epsilon_{\text{mach}}$ representa [Machine Epsilon](Machine%20Epsilon.md). Del [Estándar de punto flotante IEEE 754](Estándar%20de%20punto%20flotante%20IEEE%20754.md) recordamos que en #doublePrecision (y otros formatos) solo se puede representar un conjunto de números reales, en este caso corresponde a los número $1$ y $1+\epsilon_{\text{mach}}$. También, de la [Regla de redondeo](Regla%20de%20redondeo.md), sabemos que debemos tomar una decisión si queremos almacenar el número real $x$, es decir, debemos almacenar $1$ o $1+\epsilon_{\text{mach}}$, en este caso. La situación se extiende a toda la recta numérica al utilizar #doublePrecision u otro formato de máquina ([Representación de máquina](Representación%20de%20máquina.md)) definido con un cantidad finita de *bits*.
Ahora, ¿Cómo afecta esto la computación y/o evaluación de expresiones matemáticas considerando ?
Para entender esto, se presentan 2 ejemplos:
# Ejemplo 1: Pérdida de importancia
Problema: sumar 3 números, digamos $n_1=1$, $n_2=\epsilon_{\text{mach}}/4$ y $n_3=\epsilon_{\text{mach}}/4+\epsilon_{\text{mach}}/8$. Notar que los 3 números se pueden almacenar en #doublePrecision .
**Algoritmo 1**: $o_1=(n_1+n_2)+n_3$.
**Algoritmo 2**: $o_2=n_1+(n_2+n_3)$.
Es decir lo que se quiere estudiar es la siguiente pregunta: ¿Se preserva la #asociatividad en aritmética de punto flotante?
Los resultados en #doublePrecision son:
$o_1=1$
$o_2=1+\epsilon_{\text{mach}}$
Entonces, no se preserva la #asociatividad en aritmética de punto flotante. Esto implica que el orden en que se hagan las operaciones es importante! 
Aunque la diferencia es pequeña, se observa claramente el problema.
*En el siguiente código se puede ejecutar el experimento:*
```run-python
epsMach = np.power(2.,-52)
n1 = 1.0 # Notice we defined "1" as "1.0" to make sure it is a float.
n2 = epsMach/4.
n3 = epsMach/4.+epsMach/8.
o1 = (n1+n2)+n3
o2 = n1+(n2+n3)
print(o1)
to_fps_double(o1)
print(o2)
to_fps_double(o2)
```
# Ejemplo 2: Error de cancelación
Problema: evaluar $f(x)=1-\sqrt{1-x}$ para $x=\epsilon_{\text{mach}}/8$.
**Algoritmo 1**: evaluación directa, es decir,
$$\begin{align*}
o_1 &=f(\epsilon_{\text{mach}}/8)\\
&= 1-\sqrt{\text{fl}(1-\epsilon_{\text{mach}}/8)}\\
&= 1-\sqrt{1}\\
&= 0.
\end{align*}
$$
Notar que en este caso se utilizó la [Regla de redondeo](Regla%20de%20redondeo.md) de la [Estándar de punto flotante IEEE 754](Estándar%20de%20punto%20flotante%20IEEE%20754.md) en el lado derecho de la evaluación, por esto $1-\epsilon_{\text{mach}}/8$ se convierte en $1$. En este caso se utilizó la notación $\text{fl}(x)$ #notacionFL, que recibe un número $x\in\mathbb{R}$ y retorna el número que se almacenaría, en este caso, en #doublePrecision . 
**Algoritmo 2**: Modificación algebraica de la expresión matemática antes de evaluar.
- Paso 1: Multiplicar por un 1 conveniente, en este caso $\left(1+\sqrt{1-x}\right)/\left(1+\sqrt{1-x}\right)$,$$
  \begin{align}
	  f(x) &= 1-\sqrt{1-x}\\
		&= \left(1-\sqrt{1-x}\right)\,\dfrac{\left(1+\sqrt{1-x}\right)}{\left(1+\sqrt{1-x}\right)}\\
		&= \dfrac{x}{1+\sqrt{1-x}}.
  \end{align}$$
  - Paso 2: Evaluar la expresión, $$
  \begin{align}
  o_2 &= f(\epsilon_{\text{mach}}/8)\\
	  &= \dfrac{\epsilon_{\text{mach}}/8}{1+\sqrt{\textbf{fl}(1-\epsilon_{\text{mach}}/8)}}\\
	  &= \dfrac{\epsilon_{\text{mach}}/8}{1+\sqrt{1}}\\
	  &= \epsilon_{\text{mach}}/16.
  \end{align}$$
```run-python
epsMach = np.power(2.,-52)
f1 = lambda x: 1-np.sqrt(1-x)
f2 = lambda x: x/(1+np.sqrt(1-x))

print(f1(epsMach/8))
print(f2(epsMach/8))
print(epsMach/16)
```

Ahora, considere la versión gráfica del experimento.

```run-python
f1 = lambda x: 1-np.sqrt(1-x)
f2 = lambda x: x/(1+np.sqrt(1-x))

x = np.logspace(-50,0,50)
plt.figure(figsize=(5,5))
plt.loglog(x,f1(x),'rd',label=r'$f_1(x)=1-\sqrt{1-x}$')
plt.loglog(x,f2(x),'b.', label=r'$f_2(x)=\dfrac{x}{1+\sqrt{1-x}}$')
plt.grid(True)
plt.xlabel('$x$')
plt.legend(loc='best')
plt.show()
```
En este caso la gráfica de $f_1(x)=1-\sqrt{1-x}$ se muestra con $\text{{\color{red}rombos rojos}}$ y la gráfica de $f_2(x)=\dfrac{x}{1+\sqrt{1-x}}$ con $\text{{\color{blue}puntos azules}}$. El resultado del experimento numérico es claro,$f_1(x)$ falla para valores de $x$ pequeños. En este caso para valores aproximadamente $x\approx10^{-16}$.
# Conclusiones:
  - Tanto el Algoritmo 1 como el Algoritmo 2 deben entregar la misma salida en aritmética exacta, sin embargo en aritmética de punto flotante, en este caso utilizando #doublePrecision , entregan un resultado distinto.
  - Si bien uno puede considerar que $o_1=0$ es cercano a $o_2=\epsilon_{\text{mach}}/16$, en realidad son bien distintos. Por ejemplo, es muy distinto multiplicar por $0$ que por un número pequeño pero mayor que $0$, como lo es $\epsilon_{\text{mach}}/16$.
# Link sugerido
- [Ecuación cuadrática](https://github.com/tclaudioe/Scientific-Computing-V3/blob/main/Bonus%20-%20current/Bonus%20-%2002%20-%20Quadratic%20formula.ipynb)

#OK
#Tema_2
