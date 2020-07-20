# PDBTOOL installation on macOS

Steps to set up your environment for running older versions of python and python packages without messing up your current installations:

1. Install homebrew if you don’t already have it installed.
```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```
Source: 
	https://brew.sh/

2. Using homebrew install pyenv and the pyenv-virtualenv plugin
```
	$ brew update
	$ brew install pyenv pyenv-virtualenv
```
	Note: verify that you have virtualenv installed by running $ virtualenv —version
		If not, you’ll need to instal using pip: $ python3 -m pip instal virtualenv

Source: 
	https://github.com/pyenv/pyenv
	https://github.com/pyenv/pyenv-virtualenv

3. Add two lines to the end of your shell’s profile. 

```
	eval “$(pyenv init - )”
	eval “$(pyenv virtualenv-init -)”
```

	Notes: Mac used bash by default until MacOS Catalina where it has started using zsh.
		To figure out what your default shell is, use this command: `$ echo $SHELL`
		If your shell is ‘bash’ then use `$ ls -a ~` to find either “~/.profile” or “~/.bash_profile”.
		If you are using zsh, your profile file is either “~/.zshrc” or “~/.zprofile”. 
		whatever be the case, append the above two lines.

   Activate your new profile:
```	
	$ source ~/<profile filename>
```
4. Use pyenv to install the newest version of python 3.5 (3.5.9).
	Note: this will not affect your default python installation. This makes the version of python specified available 
	to you when you want to use it.
```
	$ pyenv install 3.5.9
```
5. Navigate to where you want the project directory, create the pdbtool virtualenv, and set the python version.
```	
	$ cd <your installation directory>
	$ pyenv local 3.5.9
	$ pyenv virtualenv pdbtool
```
	Note: after you run this command, whenever you are in this directory, your python version will be python3.5,
		outside of this dir it will be the default system python.

6. Finish setting up pdbtool environment:
```
	$ . ./bin/activate
	$ git clone https://github.com/pozharski/pdbtool.git
	$ pip install --upgrade pip
	$ pip install scipy==1.4.1 matplotlib==3.0.3
```
7. Append the path to the pdbtool source folder (not the environment folder) to PYTHONPATH
```
	$ export PYTHONPATH=<path-to-pdbtools>:$PYTHONPATH
```

    Note: you'll want to include this in your shell's profile as well.

7. You can now use pdbtool using the following command and appending the path to the script you would like to call seperated by dots.

e.g. to call the script sstin.py located at pdbtools/scripts/sstin.py

```
    $ python -m pdbtool.scripts.sstin
````

### A few notes to help you start on your way:

Most scripts in pdbtools/scripts are meant to be run directly with the exception of sstin_sqlite.py which is a helper class. 
You can run these scripts from the command line like any other script and include -h to get information about what the script does and what arguments you need/can include.
There are also a few runnable scripts from the pdbtools root directory as well. They are:
	*	bbcomp.py
	*	hbcomp.py
	*	tincomp.py

Usage:

When you want to run pdbtool, you’ll need to activate the pdbtool virtualenv we created with the command `. ./bin/activate` from the env root folder.

To use pdbtool, there are a environment variables that you can set to keep downloaded and generated files organized:

*	PISA_PATH		- the path to the download folder for PISA files
*	PDBDOWNLOAD	- the path to the downloads folder for PDB files
*	PDBMIRROR		- I can’t figure out what this one is for, seems like you don’t need it as long as you have PDBDOWNLOAD defined.


