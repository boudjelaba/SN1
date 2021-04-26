## Affichage des images dans Jupyter Notebook

```python
from IPython.display import Image
Image(filename = "img/circuit1.png", width=400)
```

# Diagramme de Bode (Module et phase)

```python
import numpy as np
from scipy import signal
from matplotlib import pyplot as plt
%matplotlib inline

# Valeurs des composants
R = 10e3
C = 10e-9
 
# Coefficients du numérateur
num = [-R*C, 1]
# Coefficients du dénominateur

den = [R*C,  1]

s1 = signal.lti(num, den)

plt.figure()
w, mag, phase = signal.bode(s1)
plt.semilogx (w, mag, color="blue", linewidth="1")
plt.ylim(-1,1)
plt.xlabel ("Freq")
plt.ylabel ("Amplitude")
plt.grid()

 
plt.figure()
w, mag, phase = signal.bode(s1, np.arange(100, 1000000, 10).tolist())
plt.semilogx (w, phase, color="red", linewidth="1.1")
plt.xlabel ("Freq")
plt.ylabel ("Amplitude")
plt.grid()

