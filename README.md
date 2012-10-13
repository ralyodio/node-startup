node-startup
============

Startup script for debian-based linux for running node app when rebooting from /etc/init.d

Why node-startup?
----

When my vps was rebooted occassionally by the hosting provider, my node.js app was not coming back online. This script can be used in /etc/init.d/node-app which will allow rc.d to restart your app when the machine reboots without your knowledge.

If you are using mongodb, redis, or nginx, you want to add those to your default run-level as well.

Installation
----

Clone the repo

	git clone https://github.com/chovy/node-startup.git
	cd node-startup/init.d

Edit the node-app script with your settings for node path, node environment (ie: production or development), path to application directory (where your app.js is), and a path to a pid file.

	vi node-app

	EXEC=/usr/local/bin/node
	NODE_ENV="production"
	APP_DIR='/var/www/example.com';
	PIDFILE=$APP_DIR/pid/app.pid

By default, it expects the pid file to be in /var/www/example.com/pid/app.pid
	
Copy the startup script node-app to your /etc/init.d directory

	sudo bash -l
	cp ./init.d/node-app /etc/init.d/


Test that it all works:

	/etc/init.d/node-app start
	/etc/init.d/node-app restart
	/etc/init.d/node-app stop

Add node-app to the default runlevels

	update-rd.c node-app defaults

Finally, reboot to be sure app starts automatically

	reboot

Gotchas
----

If there is a app.pid file already, but node is not running, and you try to start it will not start. You will have to manually remove the .pid file and run it again.
