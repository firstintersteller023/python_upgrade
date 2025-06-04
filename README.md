---

## Python 3.8.5 Manual Setup – Final Steps to be Followed

### 1. Download and Extract Python 3.8.5 Source

```bash
cd /tmp
wget https://www.python.org/ftp/python/3.8.5/Python-3.8.5.tgz
tar xzf Python-3.8.5.tgz
cd Python-3.8.5
```

---

### 2. Configure and Compile Python with Shared Library Support

```bash
./configure --prefix=/opt/python3.8.5 --enable-optimizations --enable-shared
make -j$(nproc)
make install
```

---

### 3. Fix Missing `libpython3.8.so.1.0` Error

Add the shared library path to the environment:

```bash
echo "export LD_LIBRARY_PATH=/opt/python3.8.5/lib:$LD_LIBRARY_PATH" >> ~/.bashrc
source ~/.bashrc
```

---

### 4. Set Python 3.8.5 as Default `python3`

```bash
sudo ln -s /opt/python3.8.5/bin/python3 /usr/bin/python3
```

> Note: If `/usr/bin/python3` already exists, back it up first using:

```bash
sudo mv /usr/bin/python3 /usr/bin/python3.bak
```

---

### 5. Run `ensurepip` to Install pip/setuptools (After `zlib` Fix)

If `zlib` was missing, install it first:

```bash
sudo yum install -y zlib-devel
```

Then run:

```bash
LD_LIBRARY_PATH=/tmp/Python-3.8.5 ./python -E -m ensurepip
```

---

### 6. Install Required Python Modules (If Not Already Installed)

```bash
python3 -m pip install psycopg2-binary
python3 -m pip install requests
pip install "urllib3<2"
```

---

### 7. Recreate Virtual Environment

```bash
cd /mmapps/scripts/cvm-sfs-job-refresh
mv env env.bak
python3 -m venv env
source env/bin/activate
```

Verify:

```bash
env/bin/python3 --version
# Should return: Python 3.8.5
```

---

### Note for Non-root Users

To avoid errors like:

```
libpython3.8.so.1.0: cannot open shared object file
```

Non-root users should run:

```bash
export LD_LIBRARY_PATH=/opt/python3.8.5/lib:$LD_LIBRARY_PATH
```

Or add it to `~/.bashrc`.

---

### Reference Notes Used During Setup

```bash
ls -l $(which python3)
sudo mv /usr/bin/python3 /usr/bin/python3.bak
sudo ln -s /usr/local/bin/python3.8.5 /usr/bin/python3

# If ModuleNotFoundError occurs
python3 -m pip install psycopg2-binary
python3 -m pip install requests
pip install "urllib3<2"

# Reconfigure (if needed)
wget https://www.python.org/ftp/python/3.8.5/Python-3.8.5.tgz
tar xzf Python-3.8.5.tgz
cd Python-3.8.5
./configure --prefix=/opt/python3.8.5 --enable-optimizations --enable-shared
make -j$(nproc)
make install
echo "export LD_LIBRARY_PATH=/opt/python3.8.5/lib:$LD_LIBRARY_PATH" >> ~/.bashrc

# Fix virtualenv
cd /mmapps/scripts/cvm-sfs-job-refresh
mv env env.bak
python3 --version           # Should return Python 3.8.5
python3 -m venv env
env/bin/python3 --version   # Should return Python 3.8.5
```

---

