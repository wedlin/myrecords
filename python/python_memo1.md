## Update python, Cause Ubuntu 20.04 is Python 2.7.18
```				
Add deadsnakes PPA					
sudo apt update					
sudo apt install software-properties-common -y					
sudo add-apt-repository ppa:deadsnakes/ppa					
sudo apt update
```					
					
**Setup specific Python version (e.g. 3.11)**
```					
sudo apt install python3.11 python3.11-venv python3.11-dev -y
```					
					
**Create python virtual environment for run devtool / bitbake**
```					
python3.11 -m venv /home/callan/data2/ventura/python3.11   <== only in first time					
source /home/callan/data2/ventura/python3.11/bin/activate  <== frequently used
```
```					
python --version  #check version	
```
---