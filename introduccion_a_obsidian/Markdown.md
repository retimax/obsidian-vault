# Reglas de Markdown
Es texto plano y la extensión del fichero es .md

## Formateo del texto
**negrita**
*cursiva*
***negrita y cursiva***
___negrita y cursiva___
Markdown no permite el subrayado de texto para no confundirlo con enlaces.
Ni tampoco mas de dos líneas en blanco, si las ves las unificara en una, el espacio.

**TACHADO**
Este ~~texto~~ esta tachado 

**Marcado de color**
El texto puede estar ==resaltado==

## Divisor y presentaciones
Divisores con tres guiones

---

Presentación 1

---

Presentación 2 

---

## Citas
> Como dijo Newton, en 4 todas se ven cúlonas

 - [ ] Checkbox without marked  
 - [x] Checbox marked

## Listas
LISTAS SIN NUMERAR
Creamos un guión 
- asfdasdf
- asdfas
- asfd

LISTAS NUMERADAS
Escribimos un numero con un punto, y para crear sub-listas lo hacemos con tab
1. ASFASDF
2. 123423
	1. asdfasdf
	2. asdfasdf
3. asdf (Con shift movemos la tabulación a su origen)
4. asdfa

## Lista de tareas
GUION CON CORCHETES ESPACIADOS
- [ ] ASDF
- [ ] ASDFASDF
- [ ] ASDFASDF
- [ ] ASDFASDF

## Imágenes
![[Pasted image 20240913223040.png]]

## Bloques de código o lo que no quieras que se compile en Markdown
```
nmap -sCV 10.10.10.10 -oN targeted
```

## Comentarios para que no compile Markdown
%%
Así se hace un comentario
%%

### Caracteres Markdown que no quieras compilar
Añadir barra invertida delante del caracter
\*\*No compila
\[\[No compila

## Tablas
| Nombre  | Valor |     |
| ------- | ----- | --- |
| Docena  | 12    |     |
| Centena | 100   |     |

## Enlaces externos
### Enlaces externos con markdown
- https://github.com/retimax
- Este es mi [perfil de GitHub](https://github.com/retimax)
- CTRL + k sale el formato de markdown para enlaces
- [Enlace](https://github.com/retimax)

## Enlaces entre notas
### Enlaces entre notas con Wikilinks
[[Enlace markdown]]