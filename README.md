# supervisor-labs

#useful links
https://stackoverflow.com/questions/33117068/use-of-supervisor-in-docker

#https://linuxconfig.org/configuring-gmail-as-sendmail-email-relay

ways to configure gmail as a sendmail email relay
apt-get install sendmail mailutils sendmail-bin 
mkdir -m 700 /etc/mail/info/
sudo su
cd /etc/mail/info/
sudo nano gmail-auth
write following contents into gmail-auth
AuthInfo: "U:root" "I:xyz@gmail.com" "P:*************"
create hash map for above file
makemap hash gmail-auth < gmail-auth
there you can see gmail-auth.db file beign created 
rm gmail-auth

COnfigure your sendmail
put below lines into your sendmail.mc configuration file right above 1st "MAILER" definition line:
   define(`SMART_HOST',`[smtp.gmail.com]')dnl
   define(`RELAY_MAILER_ARGS', `TCP $h 587')dnl
   define(`ESMTP_MAILER_ARGS', `TCP $h 587')dnl
   define(`confAUTH_OPTIONS', `A p')dnl
   TRUST_AUTH_MECH(`EXTERNAL DIGEST-MD5 CRAM-MD5 LOGIN PLAIN')dnl
   define(`confAUTH_MECHANISMS', `EXTERNAL GSSAPI DIGEST-MD5 CRAM-MD5 LOGIN PLAIN')dnl
   FEATURE(`authinfo',`hash -o /etc/mail/auth/gmail-auth.db')dnl

Do not put the above lines on the top of your sendmail.mc configuration file !

rebuild sendmail configuration
 make -C /etc/mail
 
reload sendmail service 
/etc/init.d/sendmail reload

you can send mail using mail command as follow:
echo "Just testing my sendmail gmail relay by prashanna" | /usr/sbin/sendmail -s "Sendmail test 1" ****@*****.com


