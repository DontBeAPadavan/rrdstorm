## RRDStorm 

This script will helps  to collect various statistics about your router and visualize it with some [cool graphs](https://github.com/ryzhovau/rrdstorm/wiki).


### Installation

* [Install](https://bitbucket.org/padavan/rt-n56u/wiki/EN/HowToConfigureEntware) Entware,
* Install necesary packages:
```
opkg install bash cron nginx rrdtool
```
* Download rrdstorm.sh, make it executable and initialize round-robin database:
```
wget --no-check-certificate -O /opt/bin/rrdstorm.sh  https://raw.githubusercontent.com/DontBeAPadavan/rrdstorm/master/opt/bin/rrdstorm.sh
chmod +x /opt/bin/rrdstorm.sh
rrdstorm.sh create 0 1 2 3 4 5
```
* Configure cron to update stats (once per minute) and graphs (once per hour):
```
wget --no-check-certificate -O /opt/etc/cron.1min/rrdstorm_update.sh https://raw.githubusercontent.com/DontBeAPadavan/rrdstorm/master/opt/etc/cron.1min/rrdstorm_update.sh
chmod +x /opt/etc/cron.1min/rrdstorm_update.sh
wget --no-check-certificate -O /opt/etc/cron.hourly/rrdstorm_graph.sh https://raw.githubusercontent.com/DontBeAPadavan/rrdstorm/master/opt/etc/cron.hourly/rrdstorm_graph.sh
chmod +x /opt/etc/cron.hourly/rrdstorm_graph.sh
sed -i 's|root|admin|g' /opt/etc/crontab
/opt/etc/init.d/S10cron start
```
* Configure web server to display statistics. Edit `/opt/etc/nginx/nginx.conf`. 
Change first line:
```
user  nobody nogroup;
```
Find `server` section and edit this line:
```
listen       81 default_server;
```
then find `location` section and change following line:
```
root   /opt/share/nginx/html/stat;
```
Now start web server and check `http://192.168.1.1:81` page from web browser:
```
/opt/etc/init.d/S80nginx start
```

