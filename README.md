# OpenSSH-Username-Enumeration





This is the original script, you have only modified to add an exception and omit the warnings generated by paramiko. It can be run both locally and with Docker.

It is important to specify that paramiko version == 2.0.8 is necessary


CVE: 2018-15473

https://www.exploit-db.com/exploits/45233


```python
import warnings
warnings.filterwarnings(action='ignore',module='.*paramiko.*')
```





# Docker


This script can be launched with a docker, although it has the problem that when generating text file, a root user is required to read the volume where the output files are saved.

In the future I will have to investigate how to mount the volume within the directory itself to be able to access it more efficiently, although in the tests I have had problems.


```bash
# Create volumen
docker volume create --name data-45233
# Generate image docker
docker build -t app-45233 .
# Execute image
docker run -it --rm -v data-45233:/usr/src/app:rw app-45233
```

The volume directory where the dictionary is located should be _/var/lib/docker/volumes/data-45233/_data/_ although it can be checked with the following command


```bash
docker volume inspect data-45233
```


# Virtualenv

```bash
virtualenv -p /usr/bin/python2.7 venv
source venv/bin/activate
python -V
pip install -r requirements.txt
python ./45233.py 10.10.10.181 --outputFile file.txt --userList dict.txt
```




