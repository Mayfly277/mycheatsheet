# LFI

## LFI To RCE

- Using file upload forms/functions
- Using the PHP wrapper expect://command
- Using the PHP wrapper php://file
- Using the PHP wrapper php://filter
- Using PHP input:// stream
- Using data://text/plain;base64,command
- Using /proc/self/environ
- Using /proc/self/fd
- Using log files with controllable input like:
  - /var/log/apache/access.log
  - /var/log/apache/error.log
  - /var/log/vsftpd.log
  - /var/log/sshd.log
  - /var/log/mail
- Using php session
