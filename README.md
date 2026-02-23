# Various container recipes

Container files for creating various services. Containers based on Fedora
and are primarily tested on Fedora IoT or similar Fedora deliverables.

A sample command to build one of the containers:
```
podman build --tag fedora:mosquitto -f mosquitto/Containerfile
```
