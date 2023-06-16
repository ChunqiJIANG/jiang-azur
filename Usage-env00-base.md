The **'base'** environment is the **default** environment of conda, usually not be used, **ALWAYS** activate your own environment.

---
FYI, this page introduces the **common/basic command usages** for beginners (suitable for any *MacOS* or *LinuxOS*, including any installed conda environments).

<p align="right"> Updated on 2023-06-16 </p>

#### Usage description
Usage text 
```
  # annotation line
  (environment) ~/location$ command line
  
  # for any environments and any locations
  (*) $ command line
  
  # '$' in Linux = '%' in Mac, means the current location
```
---

### Basic Usage
For a detailed explanation, please refer here
([English](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview) 
or 
[Japanese](https://lecture.ecc.u-tokyo.ac.jp/~hideo-t/howto/cheetsheet.html) 
or 
[Chinese](https://www.runoob.com/linux/linux-command-manual.html)) 
or 
[Google](google.com) it.
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
# copy one or more files
(*) $ cp path/to/file1 (file2 file3) path/to/save/directory/
# copy folder
(*) $ cp -r /path/to/folder path/to/save/directory/

## Move/rename
# move file/folder to other location (give path if needed)
(*) $ mv file1 path/to/save/directory/
# rename the file/folder (give path if needed)
(*) $ mv old_name new_name

# change extensions (.fa => .fna)
(*) $ for f in *fa; do mv -- "$f" "${f%.fa}.fna"; done 


## Remove/Delete
# delete files (give path if needed)
(*) $ rm file1 file2 file3
# delete folders (give path if needed)
(*) $ rm -r folder1 folder2 folder3

## Clean
# clean the screen (delete all contents on the current screen)
(*) $ clear
``` 

### zip and unzip
```
# unzip the .gz file
(*) $ gzip file.gz
# unzip current all .gz files ('./' means current directory )
(*) $ gzip ./*.gz
```

### Conda Related [(Homepage)](https://docs.conda.io/projects/conda/en/stable/glossary.html#miniconda-glossary)
```
# check conda version
(*) $ conda --version

# list all environments 
(*) $ conda-env list
or
(*) $ conda info --envs 

# enter a sepcific environment (ex: 'NAME')
(*) $ conda activate NAME
(NAME) $ 

# quit current environment
(NAME) $ conda deactivate
(base) $

# list all installed tools in the located environment ('NAME')
(NAME) $ conda list

# install/uninstall packages in the current environmnet
(NAME) $ conda install xxx
(NAME) $ conda uninstall xxx
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

---

### More General Usages

#### grant permission to the file
```
# 755 permission (777: all permission)
(*) $ sudo chmod 755 file
```

#### check running processes :['top'](https://phoenixnap.com/kb/top-command-in-linux)
```
# Top command
(*) $ top
```

#### cut the files: ['cut']()
```
# 
(*) $ 
```

#### split the files: ['split']()
```
# 
(*) $ 
```

#### sort the files: ['sort']()
```
# 
(*) $ 
```

#### text filter1 simple: ['grep']()
```
# 
(*) $ 
```

#### text filter2 stronger: ['sed']()
```
# 
(*) $ 
```

