### Astra Linux

# Устанавливаем Apache
apt install apache2

wget https://sourceforge.net/projects/squid-report/files/squid-report/6.6/squidanalyzer-6.6.tar.gz

tar -zxvf squidanalyzer-6.6.tar.gz

apt install make

cd squidanalyzer-6.6

perl Makefile.PL

make && make install

-----------------------------------------------------------------------------
1. Modify your httpd.conf to allow access to HTML output like follow:
        Alias /squidreport /var/www/squidanalyzer
        <Directory /var/www/squidanalyzer>
            Options -Indexes FollowSymLinks MultiViews
            AllowOverride None
            Order deny,allow
            Deny from all
            Allow from 127.0.0.1
        </Directory>
2. If necessary, give additional host access to SquidAnalyzer in httpd.conf.
   Restart and ensure that httpd is running.
3. Browse to http://my.host.dom/squidreport/ to ensure that things are working
   properly.
4. Setup a cronjob to run squid-analyzer daily:

     # SquidAnalyzer log reporting daily
     0 2 * * * /usr/local/bin/squid-analyzer > /dev/null 2>&1

or run it manually. For more information, see /README file.
-------------------------------------------------------------------------------

nano /etc/squidanalyzer/squidanalyzer.conf
меняем
LogFile /var/log/squid/access.log
Lang   /etc/squidanalyzer/lang/en_US.txt


nano /etc/apache2/sites-available/000-default.conf

        DocumentRoot /var/www/squidanalyzer
        Alias /squidreport /var/www/squidanalyzer
        <Directory /var/www/squidanalyzer>
        Options -Indexes +FollowSymLinks +MultiViews
        </Directory>
		

/usr/local/bin/squid-analyzer > /dev/null 2>&1
