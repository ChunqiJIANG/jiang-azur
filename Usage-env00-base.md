The **'base'** environment is the **default** environment of conda, usually not be used, **ALWAYS** activate your own environment.

---
FYI, this page introduces the **common/basic command usages** (suitable for any *MacOS* or *LinuxOS*, including any installed conda environments).

<p align="right"> Updated on 2023-06-09 </p>

#### Usage description
Usage text 
```
  # annotation line
  (environment) ~/location$ command line
  
  # for any environments and any locations
  (*) $ command line
  
  # '$' in Linux = '%' in Mac, means the current location
```


### General Usage
For a detailed explanation, please see here or google it.
```
## Change directory/location
# change to a specific folder
(*) $ cd path/to/your/folder/ 
# back to the upper directory
(*) $ cd ..
# back to home directory (~)
(*) $ cd

## Check the current directory/location
(*) $ pwd

## List 
# list contents for the current directory
(*) $ ls
# list contents for a specific directory
(*) $ ls path/to/directory/
# list contents with all details
(*) $ ls -l

## Create a new directory named 'NAME'
(*) $ mkdir NAME

## Copy
# 
(*) $ cp

## Move
# 
(*) $ mv

## Remove
# 
(*) $ rm

## Clean the screen 
(*) $ clear
``` 


### Remote upload/download
```
## for file
# upload to remote
(*) $ scp /path/to/local/file user_name@IP_address:/path/to/directory/
# dowload to local
(*) $ scp user_name@IP_address:/path/to/file/ /path/to/local/directory/ 

## for folder
# upload to remote
(*) $ scp -r /path/to/local/folder/ user_name@IP_address:/path/to/directory/
# dowload to local
(*) $ scp -r user_name@IP_address:/path/to/folder/ /path/to/local/directory/ 

```
*'user_name@IP_address' : same as your connect/login info*


### Conda Related
```
# list all environments 
(*) $ conda-env list
# enter a sepcific environment (ex: 'NAME')
(*) $ conda activate NAME
(NAME) $ 
# quit current environment
(NAME) $ conda deactivate
(base) $
# list all installed tools in the located environment ('NAME')
(NAME) $ conda list
``` 
