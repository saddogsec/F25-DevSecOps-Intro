## SSH Commit Signature Verification 
### How to verify the commit
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


F25-DevSecOps-Intro on  feature/lab3 [?] 
❯ vim labs/submission3.md

F25-DevSecOps-Intro on  feature/lab3 [?] 
❯ git add labs/submission3.md 

F25-DevSecOps-Intro on  feature/lab3 [+?] 
❯ git commit -S -m "docs: add commit signing summary"
[feature/lab3 8b55b7e] docs: add commit signing summary
 1 file changed, 23 insertions(+)
 create mode 100644 labs/submission3.md

F25-DevSecOps-Intro on  feature/lab3 [?⇡] 
❯ git push origin feature/lab3
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 8 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 1.71 KiB | 1.71 MiB/s, done.
Total 4 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:saddogsec/F25-DevSecOps-Intro.git
   25a969e..8b55b7e  feature/lab3 -> feature/lab3
```

<img width="1891" height="669" alt="image" src="https://github.com/user-attachments/assets/0a6e2828-9872-4b7a-92e1-143752478579" />

### Why commits must be verified
The main reasons, why signing commits is important is authencity and accountability. The signed commit cannot be tampered with. And assuming the owner of key didn't leaked it(In such case different mechanisms used), there's no good way for malicious actor to do something. Also signing ensures that every commit can be traced down to each person, reducing the risk of insider threat. DevSecOps frameworks usually forbids unsigned commits entirely.

## Pre-commit Secret Scanning
### Verify hook functionality (failed run)
First add some fake token and try to commit it:
```
F25-DevSecOps-Intro on  feature/lab3 [?] took 9s 
❯ echo "Some definitely not fake token: xoxp-1234567890-abcdefghijklmno-0987654321-pqrstuvwxyzabcdef
  " >> labs/submission3.md 

F25-DevSecOps-Intro on  feature/lab3 [!?] 
❯ git add labs/submission3.md 

TEMPORARY PLACEHOLDER

```
### Verify hook functionality (successful run)
```
TEMPORARY PLACEHOLDER
```


##
