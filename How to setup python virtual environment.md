## How to setup python virtual environment 
### Setup python
```sudo apt-get update```  
```sudo apt-get -y upgrade```  
```python3 --version```  
* to manage software packages for python, install pip:  
```sudo apt-get install -y python3-pip```  
  
* There are a few more packages and development tools to install to ensure that we have a robust set-up for our programming environment   
```sudo apt-get install build-essential libssl-dev libffi-dev python-dev```  
  
### Setting up a virtual environment  
* We need to first install the venv module, part of the standard python3 library, so that we can create virtual environments, let's install venv:  
```sudo apt-get install -y python3-venv```  
  
```mkdir environments```  
```cd environments```  
```python3 -m venv project_env``` This will create a project_env folder in environment : ./environment/project_env  
```source project_env/bin/activate```  
