# Users Management

| Command  | Description|
|---|---| 
| ```whoami```   | My current username (the user I am currently logged in)  | 
|`ls -l /home`| print currently logged in users' home directories |  
|`users`| list of all users  currently logged in according to system (actual users not system users) |  
|```exit```| Leave user you logged in (or if you are admin exit from server)|  
|```su - [USERNAME]``` OR ```sudo -i [USERNAME]]```| change user to the user with name: [USERNAME] |



### `/etc/passwd` File:
An Important File for System User Management contains **All Users Information** (Actual users & System* Users)
- `cat /etc/passwd`: prints All Users info 
    - `cat /etc/passwd | wc -l`: prints the number of all users
    - `cat /etc/passwd | grep [USERNAME]`: print the line of the particular [USERNAME]

### `/etc/passwd` Fields: 
Contains one entry per line for each user. Fields are separated by a colon (:) symbol. Total of seven fields as follows: 

<img src="images/passwd-file.jpg" alt="passwd" width="500px">

1. **Username**: It is used when user logs in. 
    - It should be between 1 and 32 characters in length.
1. **Password**: An x character indicates that encrypted password is stored in `/etc/shadow` file. 
1. **User ID (UID)**: Each user must be assigned a user ID (UID). 
    - UID 0 (zero) is reserved for root 
    - UIDs 1-99 are reserved for other predefined accounts-
    - UIDs 100-999 are reserved by system for administrative and system accounts/groups.
    - UIDs 1000+ are reserved for user accounts (admin user has usually UID: 1000)
1. **Group ID (GID)**: The primary group ID (stored in /etc/group file)
1. **User ID Info (GECOS)**: The comment field. It allow you to add extra information about the users.
    - such as user’s full name, phone number etc. This field use by finger command.
1. **Home directory**: The absolute path to the directory the user will be in when they log in. 
    - If this directory does not exists then users directory becomes `/`
1. **Command/shell**: The absolute path of a command or shell 
    - For **User Accounts** the (`/bin/bash`) is a typical shell
    - For **Systme Accounts** (sysadmin) nologin shell are used (`/sbin/nologin`), which acts as a replacement shell for the user accounts. If shell set to /sbin/nologin and the user tries to log in to the Linux system directly, the /sbin/nologin shell closes the connection.

**Notes:**

- System Users: are going to run in the background, schedule tasks processes etc.
- In the `/etc/shadow` file (hashed password is not the actual password it the hashed version).


### Add Users (Actual Users & System Users):

 | Command  | Description|
 |---|---| 
 |` sudo useradd [USERNAME]`| add a new user with name: [USERNAME] and no home directory (if you are admin no sudo needed) |  
 |` sudo useradd -m [USERNAME]`| add a new User with [USERNAME] AND a home directory (if you are admin no sudo needed)|  
 |` sudo useradd -r [USERNAME]`| add a new SYSTEM User with [USERNAME] (this type of user has ID under 1000 ** |  

**Notes**:
- You may change the default way that you add users via `cat /etc/default/useradd` which sets the defaults for user's add.


### Remove Users (Actual Users & System Users):

 | Command  | Description|
 |---|---| 
 |`sudo userdel [USERNAME]`| deletes the user with name: [USERNAME] |  
 |`sudo userdel -f [USERNAME]`| deletes the user with name: [USERNAME] AND their home directory |  

 
 
### Set Password for Users 

When you create a user as admin you didn't set a password for this user so you need an additional command

| Command  | Description|
|---|---| 
|```sudo passwd <user_name>```| Change userpassword |  
 
 
 
### `/etc/shadow` File:

Stores the hashed passphrase (or “hash”) format for Linux user account with additional properties related to the user password

- `cat /etc/shadow`: prints All Users hash passwords with additional properties 
    - `cat /etc/shadow | wc -l`: prints the number of all users hash passwords
    - `cat /etc/shadow | grep [USERNAME]`: print the line of the particular [USERNAME]

### `/etc/passwd` Fields: 
Contains one entry per line for each user. Fields are separated by a colon (:) symbol. Total of seven fields as follows: 

<img src="images/shadow-file.jpg" alt="passwd" width="500px">
  
1. **Username**: A valid account name, which exist on the system.
2. **Password**: Your encrypted password is in hash format. 
- The password should be minimum 15-20 characters long including special characters, digits, lower case alphabetic and more. 
3. **Last password change (lastchanged)**: The date of the last password change, expressed as the number of days since Jan 1, 1970 (Unix time). 
- The value 0 has a special meaning, which is that the user should change her password the next time she will log in the system. An empty field means that password aging features are disabled.
4. **Minimum**: The minimum number of days required between password changes i.e. the number of days left before the user is allowed to change her password again. An empty field and value 0 mean that there are no minimum password age.
5. **Maximum**: The maximum number of days the password is valid, after that user is forced to change her password again.
6. **Warn**: The number of days before password is to expire that user is warned that his/her password must be changed
7. **Inactive**: The number of days after password expires that account is disabled.
8. **Expire**: The date of expiration of the account, expressed as the number of days since Jan 1, 1970.



 | Command  | Description|
 |---|---| 
 |```sudo getent group sudo```| list of accounts that belong to the sudo Permissions|  
 |` `|  |  
 |` `|  |  
 



**References**: 
- [Linux Crash Course - Managing Users](https://www.youtube.com/watch?v=19WOD84JFxA)
- [Understanding /etc/passwd File Format](https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/)
- [Understanding /etc/shadow file format on Linux](https://www.cyberciti.biz/faq/understanding-etcshadow-file/)
- [Text](https:///.com)
