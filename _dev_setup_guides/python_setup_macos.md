---
layout: resource
title: Python, Pyenv, and Virtualenvwrapper - MacOS Dev Setup
slug: golang_setup_macos
description: "The worry-free Python development environment setup for MacOS"
order_number: 3
---

## Our choice of tools: Pyenv & VirtualenvWrapper

Installing Python 3 & managing different versions and virtual environments on a Mac has been the subject of [many](https://docs.python-guide.org/starting/install3/osx/), [many](https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-local-programming-environment-on-macos), online [guides](https://installpython3.com/mac/), and I have spent many, many, hours as a Python dev struggling to use these guides and the infinite combinations of Python management tools to create a local development environment that doesn't drive me insane.
Eventually, I found that the combination of tools that provide the control and simplicity I need:

 * Pyenv:  for installing & switching between different python versions
 * Virtualenvwrapper: for creating & managing virtual environments

Hopefully by writing up this guide I can save someone else the hours I have lost to the Python ecosystem!

## 1. Do Not Install Anything! (Yet)

Do not brew install python3, do not mess with Python 2 and Python 3 versions that ship with Mac OS, do not pass go, do not collect $200 - I promise you this way will be easier.

## 2. Install and Set Up Pyenv

### Install Pyenv
```
% brew install pyenv
```

[Pyenv](https://github.com/pyenv/pyenv) handles most of our Python version magic for us by install various versions of Python in the home directory, and placing the directory at the front of our path.

### Initialize Pyenv

```
% eval "$(pyenv init -)"
```

The `~/.pyenv/` directory has now been created and added to the beginning of your `PATH`:
```
% echo $PATH
/Users/franco/.pyenv/shims:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
```

As long as `~/.pyenv/shims` sits before `/usr/local/bin/` and `/usr/bin` in the `PATH`, invoking `python`, `pip`, or other related tools will all be managed by pyenv.

Add this to your `~/.zshrc` to ensure pyenv is initialized on each new shell session:
```
if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi
```
Check out this [Stack Overflow post](https://stackoverflow.com/questions/592620/how-can-i-check-if-a-program-exists-from-a-bash-script) if you want to get in the weeds of what those dense-looking shell commands mean.

### Install Your Python Versions

If we test Pyenv out as-as, we don't have much.
```
% pyenv versions
* system (set by /Users/franco/.pyenv/version)
% which python
/usr/bin/python
```
We only have access to the Mac OS-included python 2, which we generally want to avoid. Furthermore, the file Pyenv is looking for, `~/.pyenv/version`, doesn't even exist which is why Pyenv is sticking with default python install.

Here's where the magic starts. Let's install some versions of Python we want to work with:

```
% pyenv install 3.6.8
% pyenv install 3.8.0
% pyenv versions
* system (set by /Users/franco/.pyenv/version)
  3.6.8
  3.8.0
```

Now we have 3 versions available. Switch the version for this shell session and start it up to see the effect:

```
% pyenv shell 3.6.8
% python
Python 3.6.8 (default, Jan 21 2020, 21:10:14) 
...
>>>
```

### Set Your Global Default Python Version
The final Pyenv configuration step - to make sure you always start a shell session with your preferred default, add this line to your `~/.zshrc`:

```
export PYENV_VERSION=3.8.0  # Or whichever version you prefer.
```

And see the effects on your next shell session:
```
% pyenv versions
  system
  3.6.8
* 3.8.0 (set by PYENV_VERSION environment variable)
```