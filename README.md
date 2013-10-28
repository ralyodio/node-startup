node-startup
============

Startup script for Linux-based systems for running node app when rebooting from /etc/init.d script.

Why node-startup?
----

When my vps was rebooted occassionally by the hosting provider, my node.js app was not coming back online. This script can be used in /etc/init.d/node-app which will allow rc.d to restart your app when the machine reboots without your knowledge.

If you are using mongodb, redis, or nginx, you want to add those to your default run-level as well.

Installation
----

Clone the repo

	git clone https://github.com/chovy/node-startup.git
	cd node-startup/init.d

Edit the node-app script with your settings for node path, node environment (ie: production or development), path to application directory (where your app.js is - this is also NODE_APP variable), and a path to a pid file.

	vi node-app

	NODE_EXEC=/usr/local/bin/node
	NODE_ENV="production"
	NODE_APP="app.js"
	APP_DIR='/var/www/example.com';
	PID_FILE=$APP_DIR/pid/app.pid
	LOG_FILE=$APP_DIR/log/app.log
	CONFIG_DIR=$APP_DIR/config

CONFIG_DIR is required to be defined, but can be ignored. It is required for node-config: https://github.com/lorenwest/node-config -- if you do not need it, you can simply set it to $APP_DIR.

	CONFIG_DIR=$APP_DIR

If your are using node-config:

	CONFIG_DIR=$APP_DIR/config

By default, it expects the pid file to be in /var/www/example.com/pid/app.pid and your log file to be in /var/www/example.com/log/app.log
	
Copy the startup script node-app to your /etc/init.d directory

	sudo bash -l
	cp ./init.d/node-app /etc/init.d/


Test that it all works:

	/etc/init.d/node-app start
	/etc/init.d/node-app status
	/etc/init.d/node-app restart
	/etc/init.d/node-app stop

Add node-app to the default runlevels

	update-rc.d node-app defaults

Finally, reboot to be sure app starts automatically

	reboot

Supported OS
----

Tested with Debian 6.0, but it should work on other Linux systems that use startup scripts in /etc/init.d (Redhat, CentOS, Gentoo, Ubuntu, etc).

Gotchas
----

If there is a app.pid file already, but node is not running, and you try to start it will not start. You will have to manually remove the .pid file and run it again.

I will add a --force in the near future.

LICENSE
----

(The MIT License)

