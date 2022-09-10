# Install and Configure SquirrelMail in CentOS 7

**To configure a mail server we need to install**
1. Postfix
2. Dovecot
3. SquirrelMail 


**First of all disable SELINUX**

**# vim /etc/selinux/config**

**# reboot**

or 

**# setenforce 0**

# 1. Installing postfix 

**# yum install epel-release -y**

** # yum install postfix -y**

We need to make change to postfix file: 

**# vim/etc/postfix/main.cf**

![img](mailserver/img/mailserver11.png)

**# systemctl restart postfix**

**# systemctl enable postfix**

![img](mailserver/img/mailserver9.png)


# 2. Installing and configure dovecot 

**# yum install dovecot**

**# systemctl start dovecot**

**# systemctl enable dovecot**

![img](mailserver/img/mailserver1.png)

![img](mailserver/img/mailserver12.png)


**# vim /etc/dovecot/dovecot.conf**

protocols = imap pop3 lmtp

**# vim /etc/dovecot/conf.d/10-mail.conf**

mail_location = maildir:~/Maildir

**# vim /etc/dovecot/conf.d/10-auth.conf**

disable_plaintext_auth = yes

auth_mechanisms = plain login

**# vim /etc/dovecot/conf.d/10-master.conf**

unix_listener auth-userdb {

//mode = 0600

user = postfix ## uncomment line 91 and enter postfix

group = postfix ## uncomment line 92 and enter postfix

**# systemctl restart dovecot**

**# systemctl enable dovecot**

# 3. Install and Configure Squirrelmail

**# yum install httpd -y**

**# systemctl start httpd**

**# systemctl enable --now httpd**

**# yum install squirrelmail -y**

![img](mailserver/img/mailserver6.png)

**# cd /usr/share/squirrelmail/config/**

**# ./conf.pl**

![img](mailserver/img/mailserver13.png)

**# vim /etc/httpd/conf/httpd.conf**

Alias /webmail /usr/share/squirrelmail

<Directory /usr/share/squirrelmail>

Options Indexes FollowSymLinks

RewriteEngine On

AllowOverride All

DirectoryIndex index.php

Order allow,deny

Allow from all

</Directory> 



![img](mailserver/img/mailserver14.png)

**# systemctl restart httpd**

**# systemctl restart dovecot**

**# adduser mailtask**

**# passwd mailtask**

(Ok , now open mariadb.conf and copy-paste commands to your shell) 

![img](mailserver/img/mailserver2.png)

![img](mailserver/img/mailserver3.png)

![img](mailserver/img/mailserver4.png)

![img](mailserver/img/mailserver7.png)


Ok, we installed and configure mariadb,open browser and type http://yourdomainname/webmail

![img](mailserver/img/mailserver8.png)

![img](mailserver/img/mailserver10.png)



