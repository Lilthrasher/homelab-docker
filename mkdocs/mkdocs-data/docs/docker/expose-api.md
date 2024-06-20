# Exposing Docker API

## Modifying `docker.service`

!!! note "Operating System"

    This guide is for Ubuntu Server. If you are using a different operating system, you should check out the [Docker Guide]{:target="_blank"} page for your operating system.
    
[Docker Guide]: https://docs.docker.com/engine/install/

!!! danger "Exposing Docker API Without TLS"
    Exposing the Docker API can allow unauthorized access to both Docker and the host if not secured properly. Follow the [Docker Guide]{:target="_blank"} to secure the connection to the Docker API.

[Docker Guide]: https://docs.docker.com/engine/security/protect-access/

1 - Open an override file for `docker.service`

```
sudo systemctl edit docker.service
```

2 - Add the following lines to the override file

``` bash
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://127.0.0.1:2375
```

The override file should look something like this

``` bash
### Editing /etc/systemd/system/docker.service.d/override.conf
### Anything between here and the comment below will become the new contents of the file

[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://127.0.0.1:2375

### Lines below this comment will be discarded
```

3 - Reload the systemctl configuration

```
sudo systemctl daemon-reload
```

4 - Restart the Docker service

```
sudo systemctl restart docker.service
```