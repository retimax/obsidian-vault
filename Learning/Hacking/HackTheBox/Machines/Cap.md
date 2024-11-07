#HackTheBox #CTF
# Enumeración
Al realizar un escaneo de puertos mediante nmap podemos detectar los puertos ` 21,22,80 ` abiertos, corriendo estos servicios y versiones.

![[Pasted image 20241001150325.png]]

![[Pasted image 20241001150340.png]]

Ninguno de estos es realmente vulnerable a nivel de inyección de comandos asi que procedemos a entrar a la pagina web mediante la ip, al hacerlo nos encontramos con esta web.

# Explotación

![[Pasted image 20241001150444.png]]

Al navegar en ella podemos encontrar varias cosas como el output de un ` ip a `, los logs desde network status y una pagina llamada security snapshot, al entrar en ella nos aparece una interfaz que al parecer recopila paquetes, pero nos aparece en 0.

![[Pasted image 20241001150707.png]]

Si seguimos manipulando la maquina podemos ver que cambiando el numero del data nos aparecen mas paquetes, en mi caso utilice el numero 0 para despues descargar esta información. La cual viene en un archivo .pcap, **Pcap** es una interfaz de una aplicación de programación para capturar paquetes, como wireshark o aplicaciones del estilo, por lo que podemos abrir extensiones de este tipo con esta herramienta.

```bash
wireshark 0.pcap
```

Al analizar este archivo mediante wireshark podemos encontrar una serie de paquetes via tcp los cuales nos dan un usuario y contraseña.

![[Pasted image 20241001151430.png]]

Al intentar realizar una conexion ssh con este usuario y contraseña vemos que es correcta.

```bash
ssh nathan@10.10.10.245
```

![[Pasted image 20241001151628.png]]

# Escalada de privilegios
Es una de las escaladas de privilegios mas ridiculamente facil de htb, si listamos el directorio actual podemos encontrar entre tantas cosas un archivo .py el cual se encarga de darnos una shell root solo ejecutandola.

```bash
python3 file.py
```

