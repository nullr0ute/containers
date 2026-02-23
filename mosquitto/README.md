# Fedora Mosquitto Container Image
Containers built with this Containerfile build from Fedora latest stable release
packages.

## Mount Points
The container has mount point options to enable persisting of data, logs and
passing in configuration files.

For configuration the following mount point is used:
```
/mosquitto/config
```

For persistent storage and logs the following container volumes have been created:
```
/mosquitto/data
/mosquitto/log
```

## User/Group
The image runs mosquitto under the mosquitto user and group.

## Running without a configuration file
Mosquitto 2.0 requires you to configure listeners and authentication before it
will allow connections from anything other than the loopback interface. In the
context of a container, this means you would normally need to provide a
configuration file with your settings.

By default the example runs mosquitto without any authentication, and without
any other config options except listening on the MQTT port (1883) and websockets
(8080) provided in the container for this purpose:
```
podman run --name mosquitto -it -p 1883:1883 -p 8080:8080 fedora-mosquitto
```

## Configuration
To use a custom configuration file, mount a **local** configuration file to `/mosquitto/config/mosquitto.conf`

```
podman run -it -p 1883:1883 -v <absolute-path-to-configuration-file>:/mosquitto/config/mosquitto.conf fedora-mosquitto
```

Your configuration file must include a `listener`, and you must configure some
form of authentication or allow unauthenticated access. If you do not do this,
clients will be unable to connect.

Unauthenticated access:
```
listener 1883
allow_anonymous true
```

File based authentication and authorisation:
```
listener 1883
password_file /mosquitto/data/mosquitto.password_file
acl_file /mosquitto/data/mosquitto.aclfile
```

If you are running mosquitto with non default ports in the mosquitto.conf
config file you also need to update the Containerfile EXPOSE line as well
as the podman command line.

To persist data and logs add the following to the mosquitto.conf:
```
persistence true
persistence_location /mosquitto/data/

log_dest file /mosquitto/log/mosquitto.log
```
