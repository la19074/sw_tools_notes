# Unix Systems

## Permissions

* files in unix operating systems belong to a **user** and a **group**.
* `root` is the super user with UID 0 and has all permissions
* `/etc/passwd` records information on all users
* `/etc/group ` records information on all groups

File permissions are stored as follows:  
![File Permissions][1]

## Useful Commands

`passwd` changes your password  
`chown` changes who owns a file  
`chmod` changes the file's permissions  

### Setuid Programs

`su` switch to user (by default root) with their password  
`sudo` switch to user if the sysadmin says you’re allowed to with your password  
`doas` modern rewrite of sudo with fewer bugs and Spiderman references  

[1]: data/permissions_img1.png