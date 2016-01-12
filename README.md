# tox-test

Testing [tox](https://tox.readthedocs.org/en/latest/index.html) and [flake8](http://flake8.readthedocs.org/en/latest/index.html).


```sh
# Get a fresh Docker env and connect to the shell.
$ docker pull python
$ docker run -it python /bin/bash
```

```sh
# Once inside the Docker image's shell, you can optionally install vim and any other packages.
$ apt-get update
$ apt-get install vim

# Install tox globally, and then you can add your setup.py and tox.ini files.
$ pip install tox

# Finally just type `tox` to run the linters and whatever. I have no clue what I'm doing.
$ tox
```
