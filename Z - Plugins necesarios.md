- Se debe instalar en los **community plugin** y activar posteriormente. Se incluye el link al repositorio de GitHub del proyecto respectivo de forma referencial.
	- **TikZJax**: https://github.com/artisticat1/obsidian-tikzjax
	- **Execute Code**: https://github.com/twibiral/obsidian-execute-code
		Nota: Se debe incluir el PATH a la carpeta de instalación de Python.
- Se debe agregar el siguiente código en la sección **Inject Python Code**:
```python
import numpy as np
import matplotlib.pyplot as plt
import bitstring as bs

##############################################
##############################################
##############################################

# This function shows the bits used for the sign, exponent and mantissa for a 64-bit double presision number.
# fps: Floating Point Standard
# Double: Double precision IEEE 754
def to_fps_double(f):
    b = bs.pack('>d', f)
    b = b.bin
    #show sign + exponent + mantisa
    print(b[0]+' '+b[1:12]+ ' '+b[12:])

##############################################
##############################################
##############################################

def bisect(f, a, b, tol=1e-5, maxNumberIterations=100, usePandas=False):
    # Evaluating the extreme points of the interval provided
    fa = f(a)
    fb = f(b)
    # Iteration counter.
    i = 0
    # Just checking if the sign is not negative => not root  necessarily 
    if np.sign(f(a)*f(b)) >= 0:
        print('f(a)f(b)<0 not satisfied!')
        return None
  
    # Output table to store the numerical evolution of the algorithm
    output_table = []
    if not usePandas:
        #Printing the evolution of the computation of the root
        print(' i |    a    |    c    |    b    |   fa  |   fc   |   fb   | b-a')
        print('------------------------------------------------------------------')
    
    # Main loop: it will iterate until it satisfies one of the two criterias:
    # The tolerance 'tol' is achived or the max number of iterations is reached.
    while ((b-a)/2 > tol) and i<=maxNumberIterations:
        # Obtaining the midpoint of the interval. Quick question: What could happen if a different point is used?
        c = (a+b)/2.
        # Evaluating the mid point
        fc = f(c)
        # Saving the output data
        output_table.append([i, a, c, b, fa, fc, fb, b-a])
        if not usePandas:
            print('%2d | %.5f | %.5f | %.5f | %.3f | %.3f | %.3f | %.3f' % (i+1, a, c, b, fa, fc, fb, b-a))

        # Did we find the root?
        if fc == 0:
            print('f(c)==0')
            break
        elif np.sign(fa*fc) < 0:
            # This first case consider that the new inetrval is defined by [a,c]
            b = c
            fb = fc
        else:
            # This second case consider that the new interval is defined by [c,b]
            a = c
            fa = fc
        # Increasing the iteration counter
        i += 1
    
    if usePandas:
        # Showing final output table
        columns    = ['$i$', '$a_i$', '$c_i$', '$b_i$', '$f(a_i)$', '$f(c_i)$', '$f(b_i)$', '$b_i-a_i$']
        df = pd.DataFrame(data=output_table, columns=columns)
        display(df)
    
    # Computing the best approximation obtaind for the root, which is the midpoint of the final interval.
    xc = (a+b)/2.
    return xc

##############################################
##############################################
##############################################

def fpi_reduced(g, x0, k=10):
    x = np.zeros(k+1)
    x[0] = x0
    for i in range(k):
        x[i+1] = g(x[i])
    return x

def cobweb2(x,g=None):
    min_x = np.amin(x)
    max_x = np.amax(x)
    
    f = plt.figure()
    ax = plt.axes()
    plt.plot(np.array([min_x,max_x]),np.array([min_x,max_x]),'b-')
    for i in np.arange(x.size-1):
        delta_x = x[i+1]-x[i]
        head_length =  np.abs(delta_x)*0.04
        arrow_length = delta_x-np.sign(delta_x)*head_length
        ax.arrow(x[i], x[i], 0, arrow_length, head_width=1.5*head_length, head_length=head_length, fc='k', ec='k')
        ax.arrow(x[i], x[i+1], arrow_length, 0, head_width=1.5*head_length, head_length=head_length, fc='k', ec='k')
    
    if g!=None:
        y = np.linspace(min_x,max_x,1000)
        plt.plot(y,g(y),'r')
    
    plt.title('Cobweb diagram')
    plt.grid(True)
    #plt.show()
	return f

##############################################
##############################################
##############################################


``` 