# server-startup

Startup script for [PortPi server](https://github.com/portpi/server) when system booting, using an **/etc/init.d** script.

## Configuration

At the top of the **server-startup** file, a few items are declared which are either passed to the Node.js app or used for general execution/management.

### Node.js Config

The items declared and passed to the Node.js application are:

- **NODE_ENV** - the type of environment - **development**, **production**, etc. - can be read by the application to do things conditionally (defaults to **"production"**)
- **PORT** - the port that the Node.js application should listen on - should be read by the application and used when starting its server (defaults to **"15926"**)
- **CONFIG_DIR** - used for [node-config](https://github.com/lorenwest/node-config) (defaults to **"$APP_DIR"**); is required, but should be kept as the default if not needed

### Execution Config

The items declared and used by the overall management of executing the application are:

- **NODE_EXEC** - location of the Node.js package executable - useful to set if the executable isn't on your PATH or isn't a service (defaults to `$(which node)`)
- **APP_DIR** - location of the Node.js application directory (defaults to **"/var/www/example.com"**)
- **NODE_APP** - filename of the Node.js application (defaults to **"app.js"**)
- **PID_DIR** - location of the PID directory (defaults to **"$APP_DIR/pid"**)
- **PID_FILE** - name of the PID file (defaults to **"$PID_DIR/app.pid"**)
- **LOG_DIR** - location of the log (Node.js application output) directory (defaults to **"$APP_DIR/log"**)
- **LOG_FILE** - name of the log file (defaults to **"$LOG_DIR/app.log"**)
- **APP_NAME** - name of the app for display and messaging purposes (defaults to **"Node app"**)

## Running
	
Copy the startup script **server-startup** to your **/etc/init.d** directory:

    sudo bash -l
    cp ./init.d/portpi-server /etc/init.d/

### Available Actions

The service exposes 4 actions:

- **start** - starts the Node.js application
- **stop** - stops the Node.js application
- **restart** - stops the Node.js application, then starts the Node.js application
- **status** - returns the current running status of the Node.js application (based on the PID file and running processes)

#### Force Action

In addition to the **start**, **stop**, and **restart** actions, a **--force** option can be added to the execution so that the following scenarios have the following outcomes:

- **start** - PID file exists but application is stopped -> removes PID file and starts the application
- **stop** - PID file exists but application is stopped -> removes PID file
- **restart** - either of the above scenarios occur

### Testing

Test that it all works:

    /etc/init.d/portpi-server start
    /etc/init.d/portpi-server status
    /etc/init.d/portpi-server restart
    /etc/init.d/portpi-server stop

Add **server-startup** to the default runlevels:

    # Debian
    update-rc.d portpi-server defaults

Finally, reboot to be sure the Node.js application starts automatically:

    sudo reboot

## LICENSE

(The MIT License)
