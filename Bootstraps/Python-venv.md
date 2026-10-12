```bash
myenv/bin/pip install --upgrade pip
myenv/bin/pip install \
  pwntools pycryptodome requests flask \
  angr z3-solver tqdm rich ipython jupyter \
  keystone-engine unicorn capstone
```

```bash
pip install python-libnmapsudo apt install -y python3-venv

sudo apt install -y python3-venv
python3-venv is already the newest version (3.13.9-3).
python3-venv set to manually installed.
Summary:
  Upgrading: 0, Installing: 0, Removing: 0, Not Upgrading: 1313

cd ~    

python3 -m venv .venv

source .venv/bin/activate

python -m pip install --upgrade pip
python -m pip install .



#for w4af
PIPENV_VENV_IN_PROJECT=1 pipenv --python 3.11 install
```
