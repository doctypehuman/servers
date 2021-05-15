# Apache Web HTTP Web Server


A web server is a network service that serves content to a client over the web. This typically means web pages, but any other documents can be served as well. 
Web servers are also known as HTTP servers, as they use the hypertext transport protocol (HTTP).

The Apache server is an open source web server that is developed by the [Apache Software Foundation](http://www.apache.org)


The name of the service is known at **httpd** and the current version that is being used in RHEL is 2.4.37.



### Installing the server


Apache is part of the packages available in the default repositories and can be installed using yum like so


		# yum install -y httpd


Alternatively you can also install it from the modules and as a container image from redhat's registry. That is currently out of scope of this document.



### Configuration Files

The service configuration files are located in the below locations. 

`/etc/httpd/conf/httpd.conf` This is the main configuration file

`/etc/httpd/conf.d/` This is an auxillary directory for configuration files that are included in the main configuration file

`/etc/httpd/conf.modules.d/` Again an auxillary directory which contains configuration files which loads modules

To check if the syntax of the configuration file is OK run the below command

		apachectl configtest

Before making any changes to the configuration files it is wise to make a copy of them in case an error occurs and we need to revert to the original form.

Also please keep in mind that to make the changes made in the configuration file effective the server needs to be restarted.

### Managing HTTP service

To start the server

		# systemctl start httpd.service

To stop the server

		# systemctl stop httpd.service

To restart the server

		# systemctl restart httpd.service

Add http services in firewall

		# firewall-cmd --add-service=http --permanent

		# firewall-cmd --reload


### Set up Single Instance 

Apache by defualt generates a the directory `/var/www/html/` where html files can be stored which are later served by the server.

The default port that is used is port 80. Follow the steps below to start serving static htmls files on your server.

1. Add the port permanently using `firewall-cmd`

		# firewall-cmd --add-port=80/tcp --permanent 

2. Reload the daemon for the changes to be implemented

		# firewall-cmd --reload

3. Add HTML files with content in them and place the files in `/var/www/html/`


4. Ensure that the SELinux context is the right one on those HTML files. You can do that by running the command `ls -Z /var/www/html` 


5. If you were to create the files in the folder directly the context should be fine, but if you were to paste these files from another location you would need to change the context. You can do that by running the below commands.

		# semanage fcontext -m -t httpd_sys_content_t "/var/www/html(/.*)?"
		# restorecon -R -v /var/www/html

6. Once that is done restart the server

		# systemctl restart httpd.service

7. Verify that the HTML file is being served

		# curl http://localhost:80

If you have multiple HTML files in the folder run the below command

		# curl http://localhost/example.html

 


### Configuring Name Based Virtual Hosts


### Configuring TLS encryption 

#### Adding TLS encryption

#### Setting supported TLS protocol version

#### Setting supported ciphers 


### Loading  Modules

### Writing Modules



