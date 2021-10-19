# DON'T PANIC

## A completely unsupported guide to getting sane local versions of the packages we like to use.

Having a local installation of our dev dependincies can make things a little easier, sometimes, think linting and code inspection. Having the ability to quickly setup a python environment, for example to test some model you found on github, is also very usefull.

This isn't officially sopporded, but I find it usefull so here are my notes:

Tested on ubuntu 20.04

Steps for installing pyenv as desctibed at : https://github.com/pyenv/pyenv

1. Install python build deps:  
``` 
sudo apt-get update; sudo apt-get install --no-install-recommends make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

2. Clone pyenv:
```
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```
3. Optionally also do this:
```
cd ~/.pyenv && src/configure && make -C src
```
4. Update environment and path and initialize:
```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc

echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi
eval "$(pyenv init --path)"' >> ~/.bash_profile

echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi
eval "$(pyenv init --path)"' >> ~/.bashrc
```

4.1. Basically the following should be added to your bashrc (and or bash_profile?)
```
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi
eval "$(pyenv init --path)"
```

5. This adds some lines to the end of your `.bash_profile` and `.bashrc` file.  
Restart your shell:
```
exec "$SHELL"
```

5. Cool, now pyenv is installed. You should be able to check your version by running `pyenv -v`  
Now we need to install virtualenv as a pyenv plugin. To start off clone into the plugins folder of wherever you cloned pyenv:
```
git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
```
6. Add more stuff to your .bash_profile:
```
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash_profile
```
7. Restart your shell again:
```
exec "$SHELL"
```

8. Now you should be able to install a python version of your choice as follows:
```
pyenv install 3.9.1
```
9. Now you are ready to create an isolted virtual environment with the installed version of python as the python interpereter for that environment.
```
pyenv virtualenv 3.9.1 pigenv391
```
10. This environment can now be activated by running:
```
pyenv activate pigenv391
```
11. To deactivate do:
```
pyenv deactivate
```
12. For extra layzieness marks you can add an alias to your `.bashrc` file to save you a few key strokes the next time you need to activate or deactivate your envirnment:
```
echo 'alias pig="pyenv activate pigenv391"'>> ~/.bashrc
echo 'alias deac="pyenv deactivate"'>> ~/.bashrc
```
13. Finally, if you don't have a ponytail and would like your ide to know which python you are using you should tell it where to find the python executable. In the above example we created a environment with an interpereter located at:
```
~/.pyenv/versions/pigenv391/bin/python
```
I'm using VS Code with the python extension installed. If I now type `ctl+shift+p` it opens the `Command Palette` if I then type `Python: Select Interpreter` into the `Command Palette` it shows a list of available interpereters. From here I can select the one I would like to use and voila! My IDE now does all sorts of neat things.
* ctl+click now takes you to the class definitions of imported packages.
* Linting works-ish
