# python_template
Generic python template for PNSN projects

# Virtualenv
System python should be avoided when possible and a project virtualenv should be used. By creating a virtualenv you can ensure the proper python version and depenencies will be scoped only to yoru project. 

# Python 2.7 is deprecated
You should never start a new project in python 2 unless a dependency absolutely requires it.  Python 2 was deprecated on 1/1/2020
# python3
You can see a list of python3 versions in

`/usr/bin/`

or the rh repo versions at

`/opt/rh`

You should always use the lastest release possible

# Vitualenvwrapper
To make virtualenvs easier to manage, the PNSN uses virtualenv wrapper, which provives many simple calls. Note, if virtualenvwrapper is used to create an environment, you can still use the virtualenv calls.

## Installation
This requires sudo priveledges. Virtualenvwrapper should always be installed using the system pip3
Example:

`$: sudo /opt/rh/rh-python36/root/usr/bin/pip3  install virtualenvwrapper`
## Configuration
Add to profile

`export WORKON_HOME=~/var/.virtualenvs`

`source /usr/local/bin/virtualenvwrapper.sh`
### Issues
If you get a error regarding shared libraries you may need to point to manually set the LD_LIBRARY_PATH

`export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/rh/rh-python36/root/lib64`

If System has conda installed you may need to specifiy the 

VIRTUALENV_WRAPPER_PYTHON to the same release you built it with.
Add this to either the users profile or in a file /etc/profile.d/

`export VIRTUALENVWRAPPER_PYTHON=/opt/rh/rh-python36/root/usr/bin/python3`

## Use
To create a virtualenv with virtualenvwrapper
for these examples we will name the environment pnsn_env

`$: mkvirtualenv pnsn_env -p /opt/rh/rh-python38/root/bin/python3.8`

This will create an environment at

`/var/.virtualenvs/pnsn_env`

To use this environment

`$: workon pnsn_env`

You should see the prompt change. verify by 

`$: python --version`

If you see 2.7.x you messed up

If you see 3.x your shell now will operate in this environment. All python and pip commands will source the respective binaries in 

`/var/.virtualenvs/pnsn_env/bin`

Meaning you do not need to specify python3 or python3.x

To exit environment 

`$: deactivate`

### Other commands
to remove env

`rmvirtualenv pnsn_env`

to list all envs

`lsvirtualenv`
### Shell Scripts
To envoke a python script automatically such as by cron, you must call it from a shell script:
<pre><code>
#!/usr/bin/env bash
source `which virtualenvwrapper.sh`
workon pnsn_env
python example.py
deactivate
</code></pre>

# Dependencies
All pip installed dependencies must be added to the requirements.txt file
The cleanest way to do this is to create a virtualenv locally, pip install your needed files then

`$: pip freeze > requirements.txt`

Only do this if you are not using the system python, otherwise it will add all system packages to the file. 

Otherwise one package per line

`numpy==1.9.2`

or if you want to allow patches but not minor releases

`numpy=1.9.2,<1.10`

Once the file is complete, you can check it in to git. 

To build or update your dependencies

`$: pip install -r requirements.txt`

# WSGI webserver
Running a WSGI webserver for a Flask or Django app is a bit challanging since you make Apache or Nginx aware of which python to use, and there are lots of SELinux rules. See PNSN wiki for more details. 









