- #Definition El número #machineEpsilon, denotado por $\epsilon_{\text{mach}}$, para alguna representación de punto flotante finita se define como la distancia entre $1$ y el menor número representable mayor a $1$.
- En el modelo IEEE de aritmética de punto flotante, el error relativo de redondeo de fl($x$) no es más que la mitad de $\epsilon_{mach}$: $$\dfrac{\left| \text{fl}(x)-x\right|}{\left| x \right|} \leq \frac{1}{2} \cdot \epsilon_{mach}$$O mejor, $$\left|\text{fl}(x)-x\right| \leq \frac{1}{2} \cdot \epsilon_{mach}\left| x \right|$$Es decir, el error de representar un número en #doublePrecision  (y otros formatos) es proporcional al tamaño del número original.
- Computacionalmente #machineEpsilon está relacionado con el 52-ésimo *bit* de la *mantisa* en [Representación de máquina](Representación%20de%20máquina.md) y la [Regla de redondeo](Regla%20de%20redondeo.md).
- El siguiente código muestra la representación de máquina el número $1$.
```run-python
x = 1.0
to_fps_double(x)
```
- El siguiente código muestra la representación de máquina el número $1+\epsilon_{mach}$, donde se observa que $1+\epsilon_{mach}$ es efectivamente el siguiente número representable mayor a $1$.
```run-python
x = 1.0+np.power(2.,-52)
to_fps_double(x)
x = x+np.power(2.,-53)
to_fps_double(x)

```

#OK
#Tema_1