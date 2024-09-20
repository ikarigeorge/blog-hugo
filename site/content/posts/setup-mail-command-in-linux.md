---
title: Setup mail command in linux
date: 2024-09-13T14:52:25.073Z
tags:
  - linux
---
Supposing we have sendmail in the linux server, the easiest way to setup any server without a valid FDQN hostname is using msmtp:

```
apt-get install msmtp msmtp-mta mailutils
```

If used by root, create the file
```
nano /etc/msmtprc
```

Otherwise, create a local file
```
nano ~/.msmtprc
```

Inside, paste the configuration you need, specifically, the original email address used by the "from" field
```
# Set default values for all following accounts.
defaults

# Set the mail server port.
port 587

# Use TLS.
tls on

# Use system certificates
tls_trust_file /etc/ssl/certs/ca-certificates.crt

# Mail account
account xxx@vvvvvv.com

# SMTP server
host smtp.mail.com

# Envelope-from address
from xxx@vvvvvv.com

# Authentication. The password is given using one of five methods, see below.
auth on

# user name for mail server
user xxx@vvvvvv.com

password superpassword

# Set a default account
account default: xxx@vvvvvv.com

# Map local users to mail addresses (for crontab)
aliases /etc/aliases
```

For services like sendgrid, the above configuration worked, for using 1&1 / Ionos, following line is needed:

```
set_from_header on
```


After editing, set the correct permissions on the file:

```
chmod 600 /etc/msmtprc
```
or 
```
chmod 600 ~/.msmtprc
```


For fallback addresses, it's needed to update the /etc/aliases file

```
root: xxx@vvvvvv.com
default: xxx@vvvvvv.com
```

At the end, we need to tell sendmail to use msmtp, edit the /etc/mail.rc file:

```
set sendmail="/usr/bin/msmtp -t"
```



To test, you can send an email like this:

```
echo "hello world" | mail -s "test email" foo@bar.com
```


Extra: if your application is throwing an error, saying "sendmail: account default not found" or something like that, verify that the user belongs to the "msmtp" group and check that the binary is correct:
```
chown root:msmtp /etc/msmtprc
chmod 640 /etc/msmtprc
```