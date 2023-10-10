Los criterios de detención de algoritmos de búsqueda de $0$ se basan en las siguientes ideas:
1. La aproximación $x_k$ obtenida en la iteración $k$ de la raíz $r$ cumple con la siguiente desigualdad de **error absoluto**: $|x_k-r|<\delta$. Es decir, que la diferencia entre la aproximación y la raíz es menor que un umbral predefinido $\delta$, por ejemplo $\delta=10^{-10}$.
	1. **Ventaja**: Se asegura que la aproximación $x_k$ es cercana a la raíz en una distancia no mayor a $\delta$.
	2. **Desventaja**: Si la raíz $r$ que se está buscando es muy pequeña, es decir mucho menor que $\delta$, entonces no será buena la aproximación.
	3. **Aplicación**: En la práctica si se está buscando la raíz $r$ no tiene sentido utilizar la raíz $r$ para determinar el error, ya que si se tiene, el problema está resuelto. Lo que se hace en este caso es aproximar la raíz $r$ por lo mejor que se tiene en ese momento, entonces se podría aproximar $|x_k-r|\approx |x_k-x_{k+1}|$. Esto nos dice que para determinar el error en la iteración $k$ debemos obtener la aproximación de la siguiente iteración.
	4. **Nota**: La diferencia $|x_k-r|$ se conoce como #forwardError.
2. Una alternativa al **error absoluto** es utilizar el **error relativo**, el cual se define como $\dfrac{|x_k-r|}{|r|}<\delta$.
	1. **Ventaja**: El error se adapta a la magnitud de la raíz.
	2. **Desventaja**: Si la raíz fuera $0$ o muy pequeña podría generar resultados muy grandes en etapas iniciales.
	3. **Aplicación**: Al igual que el caso anterior se requiere aproximar la raíz de la siguiente forma: $\dfrac{|x_k-r|}{|r|}\approx \dfrac{|x_k-x_{k+1}|}{|x_{k+1}|}<\delta$.
3.  Otra alternativa es utilizar el #backwardError que en este caso se define como $|f(x_k)-f(r)|=|f(x_a)|$ dado que $f(r)=0$. Es decir, al igual en el caso anterior, uno define un umbral a cumplirse $|f(x_k)|<\varepsilon$.
	1. **Ventaja**: No se requiere aproximar $r$.
	2. **Desventaja**: Si uno considera la expansión de Taylor de $f(x)$ de la siguiente forma, $$f(r)=f(x_k)+f'(c)\,(x_k-r)$$ nos permite obtener la siguiente identidad $$|f(r)-f(x_k)|=|f'(c)|\,|x_k-r|$$ la cual provee la siguiente relación $$\dfrac{|f(r)-f(x_k)|}{|f'(c)|}=|x_k-r|$$ la cual indica que si bien el numerador $|f(r)-f(x_k)|$ puede que sea pequeño, no necesariamente implica que $|x_k-r|$ sea pequeño dado que es afectado por $|f'(c)|$. Es decir, si la función es tiene una pendiente cercana a $0$ cercana a la raíz, implica que tener un valor de $|f(r)-f(x_k)|=|f(x_k)|$ es pequeña, no necesariamente implica que $|x_k-r|$ sea pequeño, y esto último es lo que realmente se quiere. 
	3. **Aplicación**: Solamente se requiere definir adecuadamente $\varepsilon$.
4. Una última alternativa es definir una cantidad máxima de iteraciones, esto ayuda a no dejar un loop infinito en ejecución, por lo que se sugiere utilizar un número no tan pequeño. Si el número máximo de iteraciones es muy pequeño, podría implicar una detención prematura del algoritmo y la aproximación de la raíz podría contener un error significativo.

#OK
#Tema_3