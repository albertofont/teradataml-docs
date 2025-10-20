# Installing, Uninstalling, and Upgrading Teradata Package for Python

## Installing Teradata Package for Python
### Teradata Package for Python System Requirements
### Required Software on Teradata Vantage
Based on the connection to Vantage, the  teradataml  package requires the following minimum software
**versions be installed on Vantage:**
Teradata Vantage with database release 16.20.16.01 or later or VantageCloud Lake.
### Required Software on Client
**The teradataml package requires a minimum version of the following software be installed on the client:**
* Operating Systems:
    * Windows OS: Windows 7
    * macOS: os x 10.9
    * Linux: Ubuntu 16.04, CentOS 7.0, RHEL 7.1 with gcc 5.5, or SLES 12
* Note:
    * The teradataml package only supports 64-bit version of these operating systems.
* Dependent packages:
    * teradatasqlalchemy 20.0.0.02
    * teradatasql 17.10.00.11
    * pandas 0.22.00
    * psutil
    * requests >=2.25.1
    * scikit-learn >= 0.24.2
    * IPython >= 8.10.0
    * imbalanced-learn >= 0.8.0
    * pyjwt >= 2.8.0
    * cryptography >= 42.0.5
2

### Compatible Development Environment
The following third-party Integrated Development Environment (IDE) and data science user interface (UI)
**tool is compatible with Teradata Package for Python:**
* Jupyter
* Spyder
### Installing Open Source Python on the Client
Python must be installed on the client system before you install the teradataml package. The minimum
version supported is Python 3.8.0.
Python installer can be downloaded from  https://www.python.org/downloads/  on the Python Software
Foundation home page or directly from  https://www.python.org/ftp/python/ .
Once python is installed, make sure python and pip's install locations are added/updated in the appropriate
environment variables to access python and pip.
### Installing Teradata Package for Python on a Client Through PyPI
* Install Teradata Package for Python on Windows
* Install the Teradata Package for Python on macOS or Linux OS
Install Teradata Package for Python on Windows
The teradataml, Teradata Package for Python, can directly be installed through PyPI on Windows client
using the following steps.
1.
* Upgrade  setuptools  using the command:
```python
py -3 -m pip install --upgrade setuptools
```
2.
* Install  teradataml  package along with its dependencies using the command:
```python
py -3 -m pip install teradataml
```
* Note:
```python
teradataml  has a dependency on  teradatasqlalchemy  and  teradatasql .
```
* Executing this command will also download and install  sqlalchemy ,  teradatasqlalchemy ,
* and  teradatasql , if they are not yet installed.
```python
teradatasqlalchemy , and  teradatasql  can also be separately installed from  https://pypi.org/
```
* user/teradata/ .
2: Installing, Uninstalling, and Upgrading Teradata Package for Python

3.
* Verify the installation by  Test teradataml Connection to Vantage .
Install the Teradata Package for Python on macOS or Linux OS
The teradataml, Teradata Package for Python, can directly be installed through PyPI on macOS or Linux
OS using the following steps.
1.
* Upgrade  setuptools  using the command:
```python
pip install --upgrade setuptools
```
2.
* Install  teradataml  package along with its dependencies using the command:
```python
pip install teradataml
```
* Note:
```python
teradataml  has a dependency on  teradatasqlalchemy  and  teradatasql .
```
* Executing this command will also download and install  sqlalchemy ,  teradatasqlalchemy ,
* and  teradatasql , if they are not yet installed.
```python
teradatasqlalchemy , and  teradatasql  can also be separately installed from  https://pypi.org/
```
* user/teradata/ .
3.
* Verify the installation by  Test teradataml Connection to Vantage .
### Installing Teradata Package for Python from Teradata Downloads
* Download Teradata Package for Python from Teradata Downloads
* Install Teradata Python Package on Windows from the Wheel File
* Install Teradata Python Package on macOS or Linux OS from the Wheel File
Download Teradata Package for Python from Teradata Downloads
1.
* Log in to  https://downloads.teradata.com/ .
2.
* Search for  teradataml  and select  Teradata Package for Python - teradataml .
3.
* Download the wheel file to a location you can access when installing Teradata Package for Python on
* Windows, macOS, or Linux OS.
Install Teradata Python Package on Windows from the Wheel File
1.
* Upgrade  setuptools  using the command:
2: Installing, Uninstalling, and Upgrading Teradata Package for Python

```python
py -3 -m pip install --upgrade setuptools
```
2.
* Install  teradataml  package along with its dependencies using the command:
```python
py -3 -m pip install < location of wheel file >
```
* Note:
```python
teradataml  has a dependency on  teradatasqlalchemy  and  teradatasql .
```
* Executing this command will also download and install  sqlalchemy ,  teradatasqlalchemy ,
* and  teradatasql , if they are not yet installed.
```python
teradatasqlalchemy , and  teradatasql  can also be separately installed from  https://pypi.org/
```
* user/teradata/ .
3.
* Verify the installation by  Test teradataml Connection to Vantage .
Install Teradata Python Package on macOS or Linux OS from the
Wheel File
1.
* Upgrade  setuptools  using the command:
```python
pip install --upgrade setuptools
```
2.
* Install  teradataml  package along with its dependencies using the command:
```python
pip install < location of wheel file >
```
* Note:
```python
teradataml  has a dependency on  teradatasqlalchemy  and  teradatasql .
```
* Executing this command will also download and install  sqlalchemy ,  teradatasqlalchemy ,
* and  teradatasql , if they are not yet installed.
```python
teradatasqlalchemy , and  teradatasql  can also be separately installed from  https://pypi.org/
```
* user/teradata/ .
3.
* Verify the installation by  Test teradataml Connection to Vantage .
### Test teradataml Connection to Vantage
After installing the  teradataml  package on the client, you can import the  teradataml  module, and
establish a connection to Vantage from the Python interpreter on the client.
2: Installing, Uninstalling, and Upgrading Teradata Package for Python

1.
* Open Python interpreter.
2.
* Import the  teradataml  module:
```python
from teradataml import *
```
3.
* Create the connection context using the  create_context  command:
* connection_name  = create_context(host = " tdhost_fqdn ", username=" username ",
```python
password = " password ")
```
4.
* Run the following command to check the connection context:
```python
print ( connection_name )
```
## Uninstall the Teradata Package for Python
### Uninstall the teradataml package on Windows
**Use the following command to uninstall the teradataml package:**
```python
py -3 -m pip uninstall -y teradataml
```
### Uninstall the teradataml package on macOS or Linux OS
**Use the following command to uninstall the teradataml package:**
```python
pip uninstall -y teradataml
```
## Upgrade the Teradata Package for Python
### Upgrade the teradataml package on Windows
**To upgrade the  teradataml  package, you can use the following command:**
```python
py -3 -m pip install --no-cache-dir -U teradataml
```
### Upgrade the teradataml package on macOS or Linux OS
**Use the following command to upgrade the  teradataml  package:**
```python
pip install --no-cache-dir -U teradataml
```
2: Installing, Uninstalling, and Upgrading Teradata Package for Python