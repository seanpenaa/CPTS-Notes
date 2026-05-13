TCP/1521
supports TCP/IP, UDP, IPX/SPX, and AppleTalk.

The configuration files for Oracle TNS are called tnsnames.ora and listener.ora and are typically located in the $ORACLE_HOME/network/admin directory.

Oracle 9 has a default password, CHANGE_ON_INSTALL, whereas Oracle 10 has no default password set.

The Oracle DBSNMP service also uses a default password, dbsnmp

listener.ora file is a server-side configuration file that defines the listener process's properties and parameter

$ORACLE_HOME/sqldeveloper - PL/SQL Exclusion List (PlsqlExclusionList)

| Setting | Description |
|---------|-------------|
| `DESCRIPTION` | A descriptor that provides a name for the database and its connection type. |
| `ADDRESS` | The network address of the database, which includes the hostname and port number. |
| `PROTOCOL` | The network protocol used for communication with the server. |
| `PORT` | The port number used for communication with the server. |
| `CONNECT_DATA` | Specifies the attributes of the connection, such as the service name or SID, protocol, and database instance identifier. |
| `INSTANCE_NAME` | The name of the database instance the client wants to connect. |
| `SERVICE_NAME` | The name of the service that the client wants to connect to. |
| `SERVER` | The type of server used for the database connection, such as dedicated or shared. |
| `USER` | The username used to authenticate with the database server. |
| `PASSWORD` | The password used to authenticate with the database server. |
| `SECURITY` | The type of security for the connection. |
| `VALIDATE_CERT` | Whether to validate the certificate using SSL/TLS. |
| `SSL_VERSION` | The version of SSL/TLS to use for the connection. |
| `CONNECT_TIMEOUT` | The time limit in seconds for the client to establish a connection to the database. |
| `RECEIVE_TIMEOUT` | The time limit in seconds for the client to receive a response from the database. |
| `SEND_TIMEOUT` | The time limit in seconds for the client to send a request to the database. |
| `SQLNET.EXPIRE_TIME` | The time limit in seconds for the client to detect a connection has failed. |
| `TRACE_LEVEL` | The level of tracing for the database connection. |
| `TRACE_DIRECTORY` | The directory where the trace files are stored. |
| `TRACE_FILE_NAME` | The name of the trace file. |
| `LOG_FILE` | The file where the log information is stored. |

odat setup
```
#!/usr/bin/env bash
set -euo pipefail

# Safe ODAT setup for Kali / Parrot-style Debian systems
# - Uses apt for system packages
# - Uses a Python virtual environment for Python packages
# - Avoids sudo pip
# - Avoids --break-system-packages

ODAT_DIR="$HOME/odat"
VENV_DIR="$HOME/.venv"
CX_ORACLE_VERSION="8.3.0"
CX_ORACLE_TARBALL="cx_Oracle-${CX_ORACLE_VERSION}.tar.gz"
CX_ORACLE_DIR="$HOME/cx_Oracle-${CX_ORACLE_VERSION}"

echo "[+] Updating apt repositories..."
sudo apt-get update

echo "[+] Installing base build dependencies..."
sudo apt-get install -y \
  git \
  wget \
  build-essential \
  python3-dev \
  python3-venv \
  libgmp-dev

echo "[+] Installing Oracle async I/O dependency..."
if sudo apt-get install -y libaio1; then
  echo "[+] Installed libaio1"
else
  echo "[*] libaio1 unavailable; trying newer package names..."
  sudo apt-get install -y libaio1t64 libaio-dev || true
fi

echo "[+] Cloning ODAT..."
if [ ! -d "$ODAT_DIR" ]; then
  git clone https://github.com/quentinhardy/odat.git "$ODAT_DIR"
else
  echo "[*] $ODAT_DIR already exists; skipping clone."
fi

echo "[+] Initializing ODAT submodules..."
cd "$ODAT_DIR"
git submodule init || true
git submodule update || true

echo "[+] Creating Python virtual environment at $VENV_DIR..."
python3 -m venv "$VENV_DIR"

echo "[+] Activating virtual environment..."
# shellcheck disable=SC1091
source "$VENV_DIR/bin/activate"

echo "[+] Upgrading pip..."
python -m pip install --upgrade pip

echo "[+] Pinning setuptools below 81 for cx_Oracle/pkg_resources compatibility..."
python -m pip install --force-reinstall "setuptools<81" wheel packaging

echo "[+] Installing ODAT Python dependencies into venv..."
python -m pip install \
  colorlog \
  termcolor \
  passlib \
  python-libnmap \
  pycryptodome \
  openpyxl \
  scapy \
  pyasyncore

echo "[+] Downloading cx_Oracle ${CX_ORACLE_VERSION} source..."
cd "$HOME"
if [ ! -f "$CX_ORACLE_TARBALL" ]; then
  wget "https://files.pythonhosted.org/packages/source/c/cx_Oracle/${CX_ORACLE_TARBALL}"
else
  echo "[*] $CX_ORACLE_TARBALL already exists; skipping download."
fi

echo "[+] Extracting cx_Oracle..."
if [ ! -d "$CX_ORACLE_DIR" ]; then
  tar xzf "$CX_ORACLE_TARBALL"
else
  echo "[*] $CX_ORACLE_DIR already exists; skipping extraction."
fi

echo "[+] Building cx_Oracle..."
cd "$CX_ORACLE_DIR"
python setup.py build

echo "[+] Installing cx_Oracle inside the venv..."
python setup.py install

echo "[+] Running dependency checks..."
python -c "import cx_Oracle; print('cx_Oracle OK')"
python -c "from Crypto.Cipher import AES; print('Crypto OK')"
python -c "import asyncore; print('asyncore OK')"
python -c "from scapy.all import *; print('scapy OK')"
python -c "import colorlog, termcolor, passlib, libnmap, openpyxl; print('Other deps OK')"

echo "[+] Testing ODAT..."
cd "$ODAT_DIR"
python ./odat.py -h

echo
echo "[+] ODAT setup complete."
echo
echo "Use it like this:"
echo "  source $VENV_DIR/bin/activate"
echo "  cd $ODAT_DIR"
echo "  python ./odat.py -h"
echo
echo "Avoid:"
echo "  sudo pip install ..."
echo "  sudo python3 setup.py install"
echo "  pip install --break-system-packages"
```
