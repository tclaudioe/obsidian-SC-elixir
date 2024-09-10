Supuestos:
1. Se tiene acceso a una función $f(x)$ continua que tiene solo $1$ raíz en el intervalo $[a,b]$, para $a<b$.
2. Se sabe que $f(a)\,f(b)<0$. Esto asegura que hay una raíz en el intervalo $[a,b]$. Si el producto $f(a)\,f(b)=0$ entonces la raíz es $a$ o $b$ y el problema se resuelve de inmediato.
3. Si el producto $f(a)\,f(b)>0$ entonces no podemos asegurar que exista una raíz en el intervalo $[a,b]$.
# Algoritmo:
En el caso de que $f(a)\,f(b)<0$, entonces podemos acotar el intervalo de donde se encuentra la raíz de la siguiente forma:
1. Definir $c=\dfrac{a+b}{2}$, esta es la estimación de la raíz.
2. Evaluar $f(c)$: Si es $0$, problema resuelto.
3. Determinar si $f(a)\,f(c)<0$:
	1. Si el producto es menor que $0$, entonces la raíz está en el intervalo $[a,c]$. Se asigna el valor de $c$ a $b$. Volver al paso 1.
	2. En caso contrario, entonces la raíz está en el intervalo $[c,b]$. Se asigna el valor de $c$ a $a$. Volver al paso 1.
El algoritmo se ejecuta hasta que se cumpla algún [[Criterio de detención para búsqueda de raíces 1D]].

Una característica importante del método de la bisección es que en cada iteración es que en cada iteración el error disminuye a la mitad, lo que permite definir el error de la siguiente forma: $$|x_k-r|\leq \dfrac{b-a}{2^{k+1}}$$ La cual requiere $k+2$ evaluaciones de $f(x)$.

- Ejemplo: Búsqueda de la raíz de $f(x)=x-\cos(x)$
```run-python
f = lambda x: x-np.cos(x)
bisect(f,0,3)

# f = lambda x: np.power(x,2.)-2
# bisect(f,1,2)
```

#OK
#Tema_3