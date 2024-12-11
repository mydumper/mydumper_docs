Build
-----
I'm currently using Ubuntu Jammy
```
git clone https://github.com/mydumper/mydumper_docs.git
cd mydumper_docs/
apt-get update
sudo apt-get install pip sphinx-common
pip install furo sphinx_copybutton
cmake .
make
```
