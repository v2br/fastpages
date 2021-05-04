---
layout: post
title: "Notes on installing and configuring miniconda on WSL2"
description: "Proper conda installation without docker"
toc: true
comments: false
hide: false
search_exclude: true
categories: [conda, python]
metadata_key1: metadata_value1
---

# Notes on installing and configuring miniconda on WSL2

**Make sure that you have atleast 2GB of free space for the this project**


## Install miniconda

As a first step we need uptodate clean system
```
sudo apt-get update
sudo apt-get upgrade
sudo apt autoremove
```

1  Download the latest miniconda from https://docs.conda.io/projects/conda/en/latest/user-guide/install/linux.html


2 You need to be on user, not a superuser account (no sudo)
```
bash Miniconda3-latest-Linux-x86_64.sh
```

Conda may have some bug fixes after the release, so let's update it.
```
conda update conda
```

### Configure environment & install packages

Next step is to create your environmet and download requred pckages.
You have a choice of named environment in default location  ($HOME/miniconda3/envs)
or location environment. Named environment is more convenient and easy on integrations with 3 party tools.
To check current conda  configuration 
```
conda info
```
With custom location there is no environment name!
It can be located inside your project directory. As result you may keep all project files in one place.

#### Create **named** environmet
```
conda create --name my-conda-env
```
System will tell you where new environment will be created.
Once created you need to activate it and change  env variables
```
conda activate my-conda-env
source ~/.bashrc
conda env list 
```
You should see star against your new env
and to list all info use  "conda info"


#### Create **location** based environment 
```
conda create --prefix $HOME/projects/fun-project/
conda activate $HOME/projects/fun-project/
source ~/.bashrc
conda env list
conda info
```
in this case conda will create directory fun-project/env

To save named envitonment in  custom location 
you need to set it explicitly
```
conda config --append envs_dirs "$HOME"/projects/my-fun-project/conda-env
conda create --name my-conda-env
conda activate  my-conda-env
source ~/.bashrc
conda env list 
```
Conda will create environment in given directory. The only problem is that now you have define environment   location directory every for every new project. Otherwise it will be created as child of the last used "envs_dir" parameter.



### Create new env from project requrements

```
conda create --name my-conda-env -f "full path to env_requrements.yml"
```
Example  of file 
name: env_requrements
channels:
- conda-forge 
- some_fancy_channel
dependencies:
  - python=3.8
  - numpy=1.2
  - matplotlib
  - pandas=0.23
-pip:
- some_pip_package==0.1


If you want to run bashrc automatically after login
Open your .profile and add the last line 
source ~/.profile

And finally: 
### Install Jupyter 
```
conda install -c conda-forge nodejs=14.14
conda install -c conda-forge jupyter_contrib_nbextensions
jupyter contrib nbextension install --user
```
### Set jupiter to use new environment and run Jupyter
```
ipython kernel install --user --name 
conda install -n fun-project ipykernel
conda update --all
#-- RUN!!
jupyter lab --no-browser
```
And copy and paste the full URL including the token! 
You up and running!!! 

#------------------------------------------------

### Delete kernel and environment 
#### Delete just kernel 
List jupiter data directory 
```
jupyter --data-dir
```
Find subfolder kernel and delete it. 
##### Remove conda env AND  all  packages assosiated with it
'''
conda remove --name prj1 --all
conda info
'''
if your custom directory still shown in env directories
open $HOME/miniconda/env
and delete dir  


### Optional 

if you want a short name for your prompt 
```
conda config --set env_prompt "(fun)"
```


conda install -n metagenomics_env --override-channels -c conda-forge -c bioconda -c defaults kraken
It is recommended to install all required packages at once to avoid dependecy conflicts. 

### Poetry 
```
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
```

alias jupyter-notebook="~/.local/bin/jupyter-notebook --no-browser"


### Save  environment
conda env export --file environment.yml 
#---------------------------


### Initialize new project 

Steps:
* create a new directory and initialize a git repository
* create an environment.yml file
* * create a conda environment given the environment specification

<script src="https://gist.github.com/v2br/0b4e39cf1429a70d08e6214241329d70.js"></script>


### Update packages 
```
conda update -n conda-env pandas 
conda update =n conda-env  --channel conda-forge opencv 
```

### Clean after install 
```
conda clean --all 
```

#-------------------------------------------------------------
## Optional: graphical interface for ubuntu  (xfce)

<script src="https://gist.github.com/v2br/4b1780c99053c955be64fd1ae9a708ef.js"></script>


Use Remote Desktop Connection with your <IP Address>:3388
#-------------------------------------------------------------


## Optional Visual Studio  setup

### Resources: 

* 
* [ Microsoft WSL2] https://devblogs.microsoft.com/commandline/author/crloewenmicrosoft-com/* [WSL2 docs] https://docs.microsoft.com/en-us/windows/wsl/
* [Jupyter & Visual Studio ] https://code.visualstudio.com/docs/python/jupyter-support
* 


#---------------------------------------------

https://pybit.es/guest-anaconda-workflow.html
