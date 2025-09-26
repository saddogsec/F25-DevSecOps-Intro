Create new key for authentification:
```
F25-DevSecOps-Intro on  feature/lab3 [?] 
❯ ssh-keygen -t rsa -C "saddog.sec@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/saddog/.ssh/id_rsa): /home/saddog/.ssh/id_rsa_git
Enter passphrase for "/home/saddog/.ssh/id_rsa_git" (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/saddog/.ssh/id_rsa_git
Your public key has been saved in /home/saddog/.ssh/id_rsa_git.pub
The key fingerprint is:
```
Read the public part of the key and add it to github, then:
```
F25-DevSecOps-Intro on  feature/lab3 [?] 
❯ git config --global user.signingkey /home/saddog/.ssh/id_rsa_git

F25-DevSecOps-Intro on  feature/lab3 [?] 
❯ git config --global commit.gpgSign true

F25-DevSecOps-Intro on  feature/lab3 [?] 
❯ git config --global gpg.format ssh
```
