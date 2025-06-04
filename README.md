

---

##  Python 3.8.5 Manual Setup – Final Steps to be followed 

###  1. Downloaded and Extracted Python 3.8.5 Source

```bash
cd /tmp
wget https://www.python.org/ftp/python/3.8.5/Python-3.8.5.tgz
tar xzf Python-3.8.5.tgz
cd Python-3.8.5
```

---

###  2. Configured and Compiled Python with Shared Lib Support

```bash
./configure --prefix=/opt/python3.8.5 --enable-optimizations --enable-shared
make -j$(nproc)
make install
```

---

###  3. Fixed Missing `libpython3.8.so.1.0` Error

Added the shared library path to environment:

```bash
echo "export LD_LIBRARY_PATH=/opt/python3.8.5/lib:$LD_LIBRARY_PATH" >> ~/.bashrc
source ~/.bashrc
```

---

###  4. Symlinked Python 3.8.5 as Default `python3`

```bash
sudo ln -s /opt/python3.8.5/bin/python3 /usr/bin/python3
```

> Note: `mv /usr/bin/python3 /usr/bin/python3.bak` failed earlier because the file didn't exist.

---

###  5. Ran Ensurepip to Install pip/setuptools (After `zlib` Fix)

After facing the `zlib` error, we installed:

```bash
sudo yum install -y zlib-devel
```

Then re-ran ensurepip automatically post install:

```bash
LD_LIBRARY_PATH=/tmp/Python-3.8.5 ./python -E -m ensurepip
```

---

###  6. Installed Required Python Modules

```bash
python3 -m pip install psycopg2-binary
python3 -m pip install requests
pip install "urllib3<2"
```

---

###  7. Fixed and Re-created Virtual Environment

```bash
cd /mmapps/scripts/cvm-sfs-job-refresh
mv env env.bak
python3 -m venv env
source env/bin/activate
```

Checked:

```bash
env/bin/python3 --version  # returned Python 3.8.5
```

---

###  Note for Non-root Users

To avoid `libpython3.8.so.1.0` not found errors:

* Non-root users must manually export the library path:

```bash
export LD_LIBRARY_PATH=/opt/python3.8.5/lib:$LD_LIBRARY_PATH
```

or add it to their `~/.bashrc`.

---


#I have also refered this 

```
ls -l $(which python3)
sudo mv /usr/bin/python3 /usr/bin/python3.bak
sudo ln -s /usr/local/bin/python3.8.5 /usr/bin/python3

# -- Additionally Installed
ModuleNotFoundError: No module named 'psycopg2'
	$ python3 -m pip install psycopg2-binary

ModuleNotFoundError: No module named 'requests'
	$ python3 -m pip install requests

ImportError: urllib3 v2 only supports OpenSSL 1.1.1+, currently the 'ssl' module is compiled with 'OpenSSL 1.0.2k-fips
	$ pip install "urllib3<2"

# Re configure python
wget https://www.python.org/ftp/python/3.8.5/Python-3.8.5.tgz
tar xzf Python-3.8.5.tgz
cd Python-3.8.5
./configure --prefix=/opt/python3.8.5 --enable-optimizations --enable-shared
make -j$(nproc)
make install
echo "export LD_LIBRARY_PATH=/opt/python3.8.5/lib:$LD_LIBRARY_PATH" >> ~/.bashrc

# -- TO FIX virtualenv
cd /mmapps/scripts/cvm-sfs-job-refresh
mv env env.bak
python3 --version           # return Python 3.8.5
python3 -m venv env
env/bin/python3 --version   # return Python 3.8.5
```
