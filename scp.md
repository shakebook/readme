# scp#
SCP examples

**Copy file from a remote host to local host SCP example:**


`scp username@from_host:file.txt /local/directory/`
 

**Copy file from local host to a remote host SCP example:**


`scp file.txt username@to_host:/remote/directory/`
 

**Copy directory from a remote host to local host SCP example:**


`scp -r username@from_host:/remote/directory/  /local/directory/`
 

**Copy directory from local host to a remote hos SCP example:**


`scp -r /local/directory/ username@to_host:/remote/directory/`
 

**Copy file from remote host to remote host SCP example:**


`scp username@from_host:/remote/directory/file.txt username@to_host:/remote/directory/`
 

Notes:

— SCP example:  scp -r  root@123.123.123.123:/var/www/html/ /home/hydn/backups/test/ Also see: Backup solutions.

— Host can be IP or domain name. Once you click return, you will be prompted for SSH password.

— Although this page covers SCP Linux, the instructions will also work for Mac using “Terminal”. You can also use WinSCP to accomplish this on a Windows PC/server.

— When copying a source file to a target file which already exists, scp will replace the contents of the target file. So be careful.