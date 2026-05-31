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
