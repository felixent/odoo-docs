# Python Packages/Dependencies Setup

## Import Python PPA
The latest Python versions are available in the deadsnakes PPA.

Stable PPA: 
```
sudo add-apt-repository ppa:deadsnakes/ppa -y
```
Latest upstream version of Python, which is still under active development, import the Python Nightly PPA from the same team.
```
sudo add-apt-repository ppa:deadsnakes/nightly -y
```

Refresh the APT Cache After Python PPA Import
```
sudo apt update
```
## Installation of Python

[For Python 3.12] 
```
sudo apt install python3.12 -y
```
[For Python 3.13] 
```
sudo apt install python3.13 -y
```
```
python3.13 --version
```

## Package installer for Python libraries manage

### Install PIP for Python:

[Method -1] 
```
curl -sS https://bootstrap.pypa.io/get-pip.py | python3.13
```
[Method -2] 
```
sudo apt-get install -y python3-pip
```
## Change Python Version to Alternative

```
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.10

sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.10
    
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.13
```
```
sudo update-alternatives --config python
```

## Development Libraries and Dependencies

[For Python 3.12] 
```
sudo apt-get install python3.12-dev build-essential libjpeg-dev libpq-dev libjpeg8-dev libxml2-dev libssl-dev libffi-dev libmysqlclient-dev libxslt1-dev zlib1g-dev libsasl2-dev libldap2-dev liblcms2-dev
```
[For Python 3.13] 
```
sudo apt-get install python3.13-dev build-essential libjpeg-dev libpq-dev libjpeg8-dev libxml2-dev libssl-dev libffi-dev libmysqlclient-dev libxslt1-dev zlib1g-dev libsasl2-dev libldap2-dev liblcms2-dev
```

## Install additional modules for Python

[For Python 3.12] 
```
sudo apt install python3.12-{tk,dev,dbg,venv,gdbm}
```
[For Python 3.13] 
```
sudo apt install python3.13-{tk,dev,dbg,venv,gdbm}
```

## Pip Commands
Pip is a package management system used to install and manage software packages written in Python
| Commands | Descriptions |
|--|--|
| pip3 install --upgrade | Upgrades a software package to the latest version. |
| pip list | Lists installed packages |
| pip list --outdated | Lists outdated packages and shows the latest versions available. |
|pip3 search [Search_Term]|  Searches for a particular package.|
|pip3 install [Package_Name]  |   Installs the latest version of a software package.|
|pip3 uninstall [Package_Name]  |  Removes a Python package.|
|pip3 show [Package_Name]  |  Prints additional package details.|
|pip download [Package_Name]  |  Downloads a package without installing it.|

## Source Distributions (sdist) or _Wheels
Wheel files with the extension .whl are built distributions of Python packages designed to facilitate quick and easy installation.

In the virtual environment, install the necessary python modules by the command:
```
pip3 install wheel
```


```
pip3 install -r odoo/requirements.txt
```
