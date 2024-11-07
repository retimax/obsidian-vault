### Karol René Rivas Díaz

# 1. Código
**No puse los input de ejemplo porque era innecesario, solo definí los dos números complejos propios**
- 2 + 2i
- 4 + 2i 

```python
import numpy as np
import matplotlib.pyplot as plt 

# Function to convert numbers from rectangular to polar form 
def rectangularToPolar(a, b):
    r = np.sqrt(a ** 2 + b ** 2) # Module
    theta = np.arctan2(b, a) # Angle (radiants)
    return r, theta


# Function to convert numbers from rectangular to exponential
def rectangularToExponential(a, b):
    r, theta = rectangularToPolar(a, b)
    return r * np.exp(1j * theta)


# Function to plot complex numbers in a cartesian plane
def plotComplexNumber(a, b):
    plt.figure(figsize=(6, 6))
    plt.quiver(0, 0, a, b, angles='xy' , scale_units='xy' , scale=1, color='r')
    plt.xlim(-5, 5)
    plt.ylim(-5, 5)
    plt.axhline(0, color='black', lw=0.5)
    plt.axvline(0, color='black', lw=0.5)
    plt.grid()
    plt.title(f'Complex number: {a} + {b}i')
    plt.xlabel('Real part')
    plt.ylabel('Imaginary part')
    plt.show()


# First Example
print('First example -> 2 + 2i\n')

# Input
a = 2 # Real part
b = 2 # Imaginary part

# Polar
r, theta = rectangularToPolar(a, b)
print(f'Forma polar: r = {r:.2f}, 0 = {theta:.2f} rad')

# Exponential
expForm = rectangularToExponential(a, b)
print(f'Forma algebraica: z = {expForm:.2f}')

# Euler
euler = np.exp(1j + theta)
print(f'Formula de euler: {euler}\n')

# Graph complex number
plotComplexNumber(a, b)

print('----------------------------\n')

# Second Example
print('Second example -> 4 + 2i\n')

# Input
a = 4 # Real part
b = 2 # Imaginary part

# Polar
r, theta = rectangularToPolar(a, b)
print(f'Forma polar: r = {r:.2f}, 0 = {theta:.2f} rad')

# Exponential
expForm = rectangularToExponential(a, b)
print(f'Forma algebraica: z = {expForm:.2f}')

# Euler
euler = np.exp(1j + theta)
print(f'Formula de euler: {euler}')

# Graph complex number
plotComplexNumber(a, b)
```

# 1.1 Output del código
![[Pasted image 20241006214113.png]]

![[Pasted image 20241006214131.png]]

# 2. En papel
![[Pasted image 20241006220001.png]]

![[Pasted image 20241006215938.png]]