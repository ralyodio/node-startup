#node-startup#

Startup script for Linux-based systems for running a [Node.js](http://nodejs.org/) app when rebooting, using an **/etc/init.d** script.


## Looking for a maintainer

If you use node-startup and would like to be a maintainer, send me a message.

##Why node-startup?##

When my VPS was rebooted occassionally by the hosting provider, my Node.js app was not coming back online after boot. This script can be used in **/etc/init.d**, which will allow rc.d to restart your app when the machine reboots without your knowledge.

If you are using [MongoDB](http://www.mongodb.org/), [Redis](http://redis.io/), or [Nginx](http://nginx.org/), you want to add those to your default run-level as well.

##Installation##

Clone the repo:

    git clone https://github.com/chovy/node-startup.git
    cd node-startup/init.d

Edit the **node-app** script with your settings from the **Configuration** section, then follow instructions in the **Running** section.

##Configuration##

At the top of the **node-app** file, a few items are declared which are either passed to the Node.js app or used for general execution/management.

###Node.js Config###

The items declared and passed to the Node.js application are:

- **NODE_ENV** - the type of environment - **development**, **production**, etc. - can be read by the application to do things conditionally (defaults to **"production"**)
- **PORT** - the port that the Node.js application should listen on - should be read by the application and used when starting its server (defaults to **"3000"**)
- **CONFIG_DIR** - used for [node-config](https://github.com/lorenwest/node-config) (defaults to **"$APP_DIR"**); is required, but should be kept as the default if not needed

###Execution Config###

The items declared and used by the overall management of executing the application are:

- **NODE_EXEC** - location of the Node.js package executable - useful to set if the executable isn't on your PATH or isn't a service (defaults to `$(which node)`)
- **APP_DIR** - location of the Node.js application directory (defaults to **"/var/www/example.com"**)
- **NODE_APP** - filename of the Node.js application (defaults to **"app.js"**)
- **PID_DIR** - location of the PID directory (defaults to **"$APP_DIR/pid"**)
- **PID_FILE** - name of the PID file (defaults to **"$PID_DIR/app.pid"**)
- **LOG_DIR** - location of the log (Node.js application output) directory (defaults to **"$APP_DIR/log"**)
- **LOG_FILE** - name of the log file (defaults to **"$LOG_DIR/app.log"**)

##Running##
	
Copy the startup script **node-app** to your **/etc/init.d** directory:

    sudo bash -l
    cp ./init.d/node-app /etc/init.d/

###Available Actions###

The service exposes 4 actions:

- **start** - starts the Node.js application
- **stop** - stops the Node.js application
- **restart** - stops the Node.js application, then starts the Node.js application
- **status** - returns the current running status of the Node.js application (based on the PID file and running processes)

####Force Action####

In addition to the **start**, **stop**, and **restart** actions, a **--force** option can be added to the execution so that the following scenarios have the following outcomes:

- **start** - PID file exists but application is stopped -> removes PID file and starts the application
- **stop** - PID file exists but application is stopped -> removes PID file
- **restart** - either of the above scenarios occur

###Testing###

Test that it all works:

    /etc/init.d/node-app start
    /etc/init.d/node-app status
    /etc/init.d/node-app restart
    /etc/init.d/node-app stop

Add **node-app** to the default runlevels:

    update-rc.d node-app defaults

Finally, reboot to be sure the Node.js application starts automatically:

    sudo reboot

##Supported OS##

Tested with Debian 6.0, but it should work on other Linux systems that use startup scripts in **/etc/init.d** (Red Hat, CentOS, Gentoo, Ubuntu, etc.).

##LICENSE##

(The MIT License)


[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/chovy/node-startup/trend.png)](https://bitdeli.com/free "Bitdeli Badge")

