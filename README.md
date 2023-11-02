# Asterisk_21_Installation_Guide
Asterisk_21_Installation_Guide
<pre>
docker run -u root --privileged -it -d -p 5060:5060/tcp -p 5060:5060/udp --name astrisk ubuntu:latest
docker container exec -it astrisk bash


apt-get update -y
apt install sudo -y
sudo apt-get upgrade -y


sudo apt-get install -y build-essential git-core subversion wget libjansson-dev sqlite autoconf automake libxml2-dev libncurses5-dev libtool

cd /usr/src/

sudo wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-21-current.tar.gz
sudo tar zxf asterisk-21-current.tar.gz

cd asterisk-*/

sudo contrib/scripts/get_mp3_source.sh

sudo contrib/scripts/install_prereq install

sudo ./configure


sudo make menuselect
	selecting format_mp3:

sudo make -j2

sudo make install

sudo make samples

sudo make basic-pbx

sudo make config

sudo ldconfig

sudo adduser --system --group --home /var/lib/asterisk --no-create-home --gecos "Asterisk PBX" asterisk

vim /etc/default/asterisk

uncomment 
AST_USER="asterisk"
AST_GROUP="asterisk"

ESC : WQ enter

sudo usermod -a -G dialout,audio asterisk

sudo chown -R asterisk: /var/{lib,log,run,spool}/asterisk /usr/lib/asterisk /etc/asterisk
sudo chmod -R 750 /var/{lib,log,run,spool}/asterisk /usr/lib/asterisk /etc/asterisk

sudo systemctl enable asterisk

sudo systemctl start asterisk
or
service asterisk start


cat /etc/asterisk
vim pjsip.conf
Add 2 sip client
;================================
;Aaron Courtney
;Accounting and Records

[1106](endpoint-internal-d70)
auth = 1106
aors = 1106
callerid = 1106

[1106](auth-userpass)
password = 1106
username = 1106

[1106](aor-single-reg)
mailboxes = 1106@example
max_contacts=5 ; Adjust the maximum contacts to a higher numbe

;================================ SALES STAFF ==

;================================
;Garnet Claude
;Sales Associate

[1105](endpoint-internal-d70)
auth = 1105
aors = 1105
callerid = 1105

[1105](auth-userpass)
password = 1105
username = 1105

[1105](aor-single-reg)
mailboxes = 1105@example
max_contacts=5 ; Adjust the maximum contacts to a higher numbe
;================================



sudo asterisk -vvvr
or
asterisk -vrrr

pjsip set logger on


exit



vim extensions.conf
[from-internal]
exten => 1105,1,Answer()
same => n,Playback(welcome)
same => n,Hangup()

exten => 1106,1,Answer()
same => n,Playback(welcome)
same => n,Hangup()


service asterisk restart
systemctl status asterisk

asterisk -rx 'dialplan reload'





sudo ufw allow 5060/udp

sudo ufw allow 10000:20000/udp



</pre>
