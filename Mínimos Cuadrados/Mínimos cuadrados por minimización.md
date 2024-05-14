Para analizar esta familia de algoritmos, recordemos la siguiente definición del error cuadrático:
$$
\begin{align*}
	E(a,b) &= \sum_{i=1}^m (y_i-a-b\,x_i)^2.
\end{align*}
$$
## ¿Cómo encontrámos el mínimo de $E(a,b)$?
1. Obtenemos el gradiente de $E(a,b)$: $\nabla E(a,b)$, $$\begin{align*}
	\dfrac{\partial}{\partial a} E(a,b) &= -2\,\sum_{i=1}^m (y_i-a-b\,x_i),\\
	\dfrac{\partial}{\partial b} E(a,b) &= -2\,\sum_{i=1}^m x_i\,(y_i-a-b\,x_i).
   \end{align*}$$
2. Igualamos a $\textbf{0}$ el gradiente y simplificamos,$$\begin{align*}
	\sum_{i=1}^m (y_i-\overline{a}-\overline{b}\,x_i) &=0,\\
	\sum_{i=1}^m x_i\,(y_i-\overline{a}-\overline{b}\,x_i) &=0.
   \end{align*}$$Notar que en las ecuaciones aparecen $\overline{a}$ y $\overline{b}$ porque corresponden a los valores de $a$ y $b$ que resuelven las ecuaciones anteriores, son valores particulares, por eso es necesarios definirlos de forma distinta. En este caso serían los valores que minimizan el error.
3. Re-escribimos como un sistema de ecuaciones lineales:$$\begin{equation}
        \begin{bmatrix}
            m & \displaystyle{\sum_{i=1}^{m} x_i} \\
    		\displaystyle{\sum_{i=1}^{m} x_i} & \displaystyle{\sum_{i=1}^{m} x_{i}^2}
        \end{bmatrix}
        \begin{bmatrix}
            \overline{a}\\
            \overline{b}
        \end{bmatrix}
        =
        \begin{bmatrix}
            \displaystyle{\sum_{i=1}^{m} y_{i}} \\
    		\displaystyle{\sum_{i=1}^{m} y_{i} \, x_i}
        \end{bmatrix}.
    \end{equation}$$
4. Se resuelve: $$\begin{align*}
        \overline{a} &= \dfrac{ \left( \sum_{i=1}^{m} x_{i}^2 \right) \, \left( \sum_{i=1}^{m} y_{i} \right) - \left( \sum_{i=1}^{m} x_{i} \right) \, \left( \sum_{i=1}^{m} y_{i} \, x_i \right)}{m \, \left( \sum_{i=1}^{m} x_{i}^2 \right)- \left( \sum_{i=1}^{m} x_{i} \right)^2}. \\
        \overline{b} &= \dfrac{m \, \left( \sum_{i=1}^{m} y_{i} \, x_i \right) - \left( \sum_{i=1}^{m} x_{i} \right) \, \left( \sum_{i=1}^{m} y_{i} \right)}{m \, \left( \sum_{i=1}^{m} x_{i}^2 \right)- \left( \sum_{i=1}^{m} x_{i} \right)^2}.
    \end{align*}$$
5. Se utiliza $f(x)=\overline{a}+\overline{b}\,x$.

#OK
#Tema_7