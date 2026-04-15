# webos-dvr-key-extract
Extract dvr key from webos requires root

# INSTRUCTIONS
1) Root your tv using the public avaliable methods
2) Put frida-server inside /home/root via webos dev manager or scp to the tv (the one in the repo targets webos 4 (arm32 non hard)
3) Launch frida-server via ssh with /home/root/frida-server -v -l 0.0.0.0
4) Install frida in your host (if you use the frida-server from this repo needs to be the version 12.5.7 (use egg to install pip will fail install) - required python 3.7) can use docker.



# DOCKER INSTRUCTIONS for the client v12.5.7 required for frida-server from this repo
docker run --rm -it -v "$HOME/frida-client:/work" python:3.7-bullseye bash
python -m venv /work/venv
source /work/venv/bin/activate
python -m pip install --upgrade pip setuptools wheel
wget https://files.pythonhosted.org/packages/a4/f7/0863766252ea6f44788a34601d03219c06aece71fb2e52e03b5860bb1439/frida-12.5.7-py3.6-linux-x86_64.egg
```python - <<'PY'
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

