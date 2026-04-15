# webos-dvr-key-extract
Extract dvr key from webos requires root

LG GO F*** YOURSELF ! - encripting our FTA recordings doesn't make sense !

Thanks https://github.com/anoane for the prebuilt binary of frida-server
# INSTRUCTIONS
1) Root your tv using the public avaliable methods
2) Put frida-server inside /home/root via webos dev manager or scp to the tv (the one in the repo targets webos 4 (arm32 non hard)
3) Launch frida-server via ssh with /home/root/frida-server -v -l 0.0.0.0
4) Install frida in your host (if you use the frida-server from this repo needs to be the version 12.5.7 (use egg to install pip will fail install) - required python 3.7) can use docker.
4) ensure you cloned this repo so you have the webos-dvr.js on your host machine
5) frida-ps -> check the pid of tvservice
6) frida -H <tv_ip_address>:27042 -p <tvservice_pid> -l <path_of_webos-dvr.js>
7) You should see something like this in frida terminal of your host machine
```bash
    ____
    / _  |   Frida 12.5.7 - A world-class dynamic instrumentation toolkit
   | (_| |
    > _  |   Commands:
   /_/ |_|       help      -> Displays the help system
   . . . .       object?   -> Display information about 'object'
   . . . .       exit/quit -> Exit
   . . . .
   . . . .   More info at http://www.frida.re/docs/home/
Attaching...
[+] PVR_DEBUG_RetrieveDvrKey @ 0xdea8a5
[*] before call: bits = 128
[*] calling PVR_DEBUG_RetrieveDvrKey(out, bits)
[+] rc = 0
[+] bits after = 128
[*] out:
               0  1  2  3  4  5  6  7  8  9  A  B  C  D  E  F  0123456789ABCDEF
the address    the fucking of the key
```

# DOCKER INSTRUCTIONS for the client v12.5.7 required for frida-server from this repo
```bash
docker run --rm -it -v "$HOME/frida-client:/work" python:3.7-bullseye bash
python -m venv /work/venv
source /work/venv/bin/activate
python -m pip install --upgrade pip setuptools wheel nano
wget https://files.pythonhosted.org/packages/a4/f7/0863766252ea6f44788a34601d03219c06aece71fb2e52e03b5860bb1439/frida-12.5.7-py3.6-linux-x86_64.egg
```
```bash
python - <<'PY'
ipfile, > import site, zipfile, os, sys
> egg = "/frida-12.5.7-py3.6-linux-x86_64.egg"
> dst = site.getsitepackages()[0]
> print("egg:", egg)
> print("dst:", dst)
> with zipfile.ZipFile(egg, "r") as z:
>     z.extractall(dst)
> print("done")
> PY
```
```bash
pip install frida-tools==2.0.0 --no-deps
pip install prompt_toolkit==2.0.10
pip install pygments==2.0.2
pip install colorama
nano webos-dvr.js <- paste the content from this repo or wget the file
frida-ps -> check the pid of tvservice
frida -H <tv_ip_address>:27042 -p <tvservice_pid> -l webos-dvr.js
```
