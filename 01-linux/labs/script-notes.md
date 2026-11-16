Steps on how to edit file permissions for a bash script

1. Created script below to manage permissions

 !#/bin/bash
 
 echo "Hello from CoderCo!"

 2. Made it executable by running the command below.

 chmod +x fileperms.sh

 3. Ran the script with the command below.

 ./fileperms.sh

 4. Changed ownership of .sh file with command below.

sudo chmod 644 

r+w=6
r=4

5. Checked file owner permissions with the command below.

ls -l fileperms.sh

Result: -rwxrwxr-x 1 root root 41 Nov 16 22:09 fileperms.sh
