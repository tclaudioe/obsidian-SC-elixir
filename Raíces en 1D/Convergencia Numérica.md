# Convergencia Lineal
#Definition #ConvergenciaLineal 
		Sea $\displaystyle e_i = |x_i - r|$ el error absoluto del paso $i$ de un método iterativo. Si se cumple que:$$
		\begin{equation*}
			\lim_{i \to \infty} \frac{e_{i+1}}{e_i} = S < 1.
		\end{equation*}$$
		Se dice entonces que el método obedece **convergencia lineal** con tasa $S$.
# Convergencia Súper-Lineal
#Definition #ConvergenciaSuperLineal
	Sea $\displaystyle e_i = |x_i - r|$ el error absoluto del paso $i$ de un método iterativo. La iteración **converge super-lineal** si:$$\begin{equation*}
			\lim_{i \to \infty} \frac{e_{i+1}}{e_i^{\alpha}} = L<\infty,
		\end{equation*}$$donde $\alpha = \frac{1 + \sqrt{5}}{2} \approx 1.62$.
# Convergencia Cuadrática
#Definition #ConvergenciaCuadrática
        Sea $\displaystyle e_i = |x_i - r|$ el error absoluto del paso $i$ de un método iterativo. 
        La iteración **converge cuadráticamente** si:$$
        \begin{equation*}
            \lim_{i \to \infty} \frac{e_{i+1}}{e_{i}^2} = M < \infty.
        \end{equation*}$$
    #OK