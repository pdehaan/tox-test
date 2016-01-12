# tox-test

Testing [tox](https://tox.readthedocs.org/en/latest/index.html) and [flake8](http://flake8.readthedocs.org/en/latest/index.html).


## Docker setup:
```sh
# Get a fresh Docker env and connect to the shell.
$ docker pull python
$ docker run -it python /bin/bash
```

## Tox/Flake8/Python setup:
```sh
# Once inside the Docker image's shell, you can optionally install vim and any other packages.
$ apt-get update
$ apt-get -q -y install vim

# TL;DR: You can clone this crappy repo for a quick test, instead creating a bunch of sample files manually.
$ git clone https://github.com/pdehaan/tox-test.git

# Install tox globally, and then you can add your setup.py and tox.ini files.
$ pip install tox

# Finally just type `tox` to run the linters and whatever. I have no clue what I'm doing.
$ tox
```

### Output:
```sh
$ tox
GLOB sdist-make: /home/tox-test/setup.py
py35 create: /home/tox-test/.tox/py35
py35 installdeps: flake8
py35 inst: /home/tox-test/.tox/dist/sample-1.0.0.zip
py35 installed: flake8==2.5.1,mccabe==0.3.1,pep8==1.5.7,pyflakes==1.0.0,sample==1.0.0,wheel==0.24.0
py35 runtests: PYTHONHASHSEED='2505737366'
py35 runtests: commands[0] | flake8 .
__________________________________________________________ summary ___________________________________________________________
  py35: commands succeeded
  congratulations :)


$ echo $?
0
```

If you modify one of your files to remove the parens around the Python `print()` command and then re-run `tox`, you should see some lovely output similar to the following:

```sh
$ cat test.py
print("hello man")

print "Allo"

name = 'Peter'


$ tox
GLOB sdist-make: /home/tox-test/setup.py
py35 inst-nodeps: /home/tox-test/.tox/dist/sample-1.0.0.zip
py35 installed: flake8==2.5.1,mccabe==0.3.1,pep8==1.5.7,pyflakes==1.0.0,sample==1.0.0,wheel==0.24.0
py35 runtests: PYTHONHASHSEED='1588081323'
py35 runtests: commands[0] | flake8 .
./test.py:3:13: E901 SyntaxError: invalid syntax
ERROR: InvocationError: '/home/tox-test/.tox/py35/bin/flake8 .'
__________________________________________________________ summary ___________________________________________________________
ERROR:   py35: commands failed


$ echo $?
1
```

If you have multiple tox environments specified in your tox.ini file, you can specify a specific environment using the `-e` flag, as seen in the following example:

```sh
$ cat tox.ini
[tox]
envlist = py35

[testenv]

[testenv:flake8]
deps =
  flake8
commands = flake8 .

[flake8]
ignore = E121
max-line-length = 160
```

```sh
$ tox -e flake8

GLOB sdist-make: /home/tox-test/setup.py
flake8 create: /home/tox-test/.tox/flake8
flake8 installdeps: flake8
flake8 inst: /home/tox-test/.tox/dist/sample-1.0.0.zip
flake8 installed: flake8==2.5.1,mccabe==0.3.1,pep8==1.5.7,pyflakes==1.0.0,sample==1.0.0,wheel==0.24.0
flake8 runtests: PYTHONHASHSEED='387675537'
flake8 runtests: commands[0] | flake8 .
./test.py:3:13: E901 SyntaxError: invalid syntax
ERROR: InvocationError: '/home/tox-test/.tox/flake8/bin/flake8 .'
__________________________________________________________ summary ___________________________________________________________
ERROR:   flake8: commands failed
```

## Further reading
If this wasn't poorly explained enough for you, I have a couple more [worse] examples in [The Wiki](https://github.com/pdehaan/tox-test/wiki) which cover how to install and run Node scripts via Tox.
