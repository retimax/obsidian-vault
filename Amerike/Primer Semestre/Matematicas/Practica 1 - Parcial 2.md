# 1. Cifrado de una frase mediante la sucesión de Fibonacci
Decidí utilizar esta sucesión debido a su simplicidad, esto me haría acabar mas rápido, en esta ocasión cifraremos mi apellido paterno, el cual es ***RIVAS*** con el uso de esta sucesión y implementando el código ascii.

## 1.1 Qué es el código ascii?
Este es un código de caracteres el cual sirve para representar caracteres mediante unidades, se compone por 128 unidades y cada una representa un carácter ***humanamente legible***, a continuación dejo una guía de este código:

![[Pasted image 20241020223549.png]]

## 1.2 Que es la sucesión de Fibonacci?
Esta es una sucesión infinita de números reales la cual se basa en la suma de un numero por el resultado de una suma anterior repetidamente, de manera gráfica seria algo así:

```
0 + 1 = 1
1 + 1 = 2
2 + 1 = 3 
3 + 2 = 5
5 + 3 = 8
8 + 5 = 13
...
```

Y en una sola linea seria esto -> `0,1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,1597 `
# 2. Concepción del cifrado
Ya explicado que es cada concepto aplicado en este continuo con la explicación. Para realizar este cifrado lo primero es identificar que valor tiene cada letra de nuestra palabra a cifrar, en mi caso sera la palabra ***"RIVAS"***, a la cual le corresponden los siguientes valores:

| Carácter | Valor |
| -------- | ----- |
| R        | 82    |
| I        | 73    |
| V        | 86    |
| A        | 65    |
| S        | 83    |

Ya que tenemos estos valores solo nos queda sumar la unidad correspondiente de la sucesión de Fibonacci de esta forma:

| Carácter | Valor encriptado | Nuevo valor | Carácter correspondiente |
| -------- | ---------------- | ----------- | ------------------------ |
| R        | 82 + 1           | 83          | S                        |
| I        | 73 + 1           | 74          | J                        |
| V        | 86 + 2           | 88          | X                        |
| A        | 65 + 3           | 68          | D                        |
| S        | 83 + 5           | 88          | X                        |

El resultado de nuestro apellido encriptado mediante la sucesión de Fibonacci es **SJXDX**. Decidí no utilizar el 0 debido a que considero que seria otra pista de que se emplea esta sucesión. 

# 3. Como desencriptarlo? 
Para desencriptarlo es tan fácil como averiguar el valor de cada carácter en el código ascii y en ves de sumar restarle la unidad correspondiente:

| Carácter | Valor antiguo | Nuevo valor | Carácter correspondiente |
| -------- | ------------- | ----------- | ------------------------ |
| S        | 83 - 1        | 82          | R                        |
| J        | 74 - 1        | 73          | I                        |
| X        | 88 - 2        | 86          | V                        |
| D        | 68 - 3        | 65          | A                        |
| X        | 88 - 5        | 83          | S                        |

Y así funciona nuestro método, es bastante simple e inseguro, pero eso lo explicaremos a continuación.

# 4. Reflexión critica
Este método es bastante inseguro una vez entendido por un atacante debido a que con conocer la sucesión de fibonacci solo tendría que restar los valores correspondientes. Sin embargo sin el conocimiento previo de esta sucesión dudo que lo puedan descifrar, pero como todo en este ámbito, es cosa de observación ademas de prueba y error, no considero que sea difícil de encontrar su patron. Una manera para aumentar su dificultad por ejemplo podria ser volver a cifrar los números mediante ascii, de esta forma:

| Valor antiguo | Valor Nuevo |
| ------------- | ----------- |
| 82            | 56 50       |
| 73            | 55 51       |
| 86            | 56 54       |
| 65            | 54 53       |
| 83            | 56 51       |
Y ahora tenemos mi apellido con una segunda fase de encriptación la cual nos da valores en numeros ***56505551565454535651***,pero solo es una posible forma de aumentar la seguridad. 

***Karol René Rivas Díaz***