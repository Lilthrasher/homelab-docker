# Post Installation

## Adding current user to Docker group

!!! note "Operating System"

    This guide is for Ubuntu Server. If you are using a different operating system, you should check out the [Docker Guide]{:target="_blank"} page for your operating system.
    
[Docker Guide]: https://docs.docker.com/engine/install/

Adding the current user allows the user to run the docker command with out `sudo`

1 - Add the current user to the Docker group created during installation.

```
sudo usermod -aG docker $USER
```

2 - Log out and then log in for the change to take effect.