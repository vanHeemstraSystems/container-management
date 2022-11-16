# 100 - Installing Scope with Docker on a Single-node

To install Scope on a single node, run the following commands:

```
sudo curl -L git.io/scope -o /usr/local/bin/scope
sudo chmod a+x /usr/local/bin/scope
scope launch
```

This script downloads and runs a recent Scope image from Docker Hub. Scope needs to be installed onto every machine that you want to monitor.

After Scope is installed, open your browser to ```http://localhost:4040```.

If you are using docker-machine, you can find the IP by running, ```docker-machine ip <VM name>```.

Where,

```<VM name>``` is the name you gave to your virtual machine with docker-machine.
  
**Note**: Scope allows anyone with access to the user interface, control over your containers. As such, the Scope app endpoint (port 4040) should not be made accessible on the Internet. Also traffic between the app and the probe is insecure and should not traverse the Internet. This means that you should either use the private / internal IP addresses of your nodes when setting it up, or route this traffic through Weave Net. Put Scope behind a password, by using an application like [Caddy](https://github.com/mholt/caddy) to protect the endpoint and by making port 4040 available to localhost with Caddy proxying it. Or you can skip these steps, and just use Weave Cloud to manage the security for you.  
