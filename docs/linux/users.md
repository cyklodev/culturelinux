# Users

## regular 

### get id 
    $ id

### change password
    $ passwd
    Changing password for user cyklodev.
    Current password:
    New password:
    Retype new password:
    passwd: all authentication tokens updated successfully.

    
## root 

### add user to sudoers
    # usermod –aG wheel $USER
    