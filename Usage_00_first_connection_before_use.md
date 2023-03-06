# Before use
## Connection setting for the use of 'jiang@azur'

### Step 1: Create ssh keys on your local termial
Japanese version [sshキー(秘密鍵・公開鍵)の作成と認証　流れ](https://qiita.com/soma_sekimoto/items/35845495bc565c38ae9d)
```
  # move to ssh directory
  $ cd .ssh
  
  # create ssh key
  $ ssh-keygen  # by default
  Generating public/private rsa key pair.
  Enter file in which to save the key (/Users/ユーザー名/.ssh/id_rsa): <-- # (save place) input nothing just enter by default
  Enter passphrase (empty for no passphrase): <-- # enter your passphrase (password)
  Enter same passphrase again:  <-- # confirm your passphrase (password)
  Your identification has been saved in id_rsa.
  Your public key has been saved in id_rsa.pub.
  
  # check (this are default names without input in the first step)
  $ ls 
  id_rsa  id_rsa.pub
```
**private key: id_rsa (should not be shown to others)**  
****public key: id_rsa.pub (can be shown to others)***

### Step 2: Send public key to jiang
```
  # copy public key to desktop 
  $ cp id_rsa.pub ~/Desktop/
  
  # send it to jiang
  
```

### Step 3: Connection
```
  # connect after setting by jiang
  $ ssh -p *** jiang@**** (provided after setting)
  
  # successful connection will be like this:
            Enter passphrase for key '/Users/username/.ssh/id_rsa': 
            Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.15.0-67-generic x86_64)

             * Documentation:  https://help.ubuntu.com
             * Management:     https://landscape.canonical.com
             * Support:        https://ubuntu.com/advantage

            70 updates can be applied immediately.
            To see these additional updates run: apt list --upgradable

            New release '22.04.2 LTS' available.
            Run 'do-release-upgrade' to upgrade to it.

            Your Hardware Enablement Stack (HWE) is supported until April 2025.
            Last login: Sat Mar  4 20:41:57 2023 from 209.17.80.224
            (base) jiang@azur:~$ 

```

### Step 4: Start your work
```
  # check your workplace
  (base) jiang@azur:~$ ls
  mambaforge-pypy3  user_name1  user_name2  user_name3 ...
  
  # move to your workplace
  (base) jiang@azur:~$ cd user_name2
  (base) jiang@azur:~/user_name2$
  
  # Now, you are ready to start any work!
```

Finish!
