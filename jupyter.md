# [Jupyter](https://jupyter.org/)

## links
* [list of the magic commands](https://ipython.readthedocs.io/en/stable/interactive/magics.html)
  > `%lsmagic`


## jupyter installation
```sh
sudo apt install python3-notebook
```

## jupyter start
```sh
jupyter notebook
```
with sandbox creation
```sh
python -m venv jupyter_env
source jupyter_env/bin/activate   
jupyter notebook
```


## jupyter commands
bash command execution
```
%%bash
ls -la
```
