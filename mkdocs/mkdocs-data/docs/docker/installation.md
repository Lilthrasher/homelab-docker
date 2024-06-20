# Installation

## Installing using the apt repository

!!! note "Operating System"

    This guide is for Ubuntu Server. If you are using a different operating system, you should check out the [Docker Guide]{:target="_blank"} page for your operating system.
    
[Docker Guide]: https://docs.docker.com/engine/install/

Installing Docker with the apt repository allows you to update docker with `sudo apt update && sudo apt upgrade`.

1 - Update list of available packages.

```
sudo apt-get update
```

2 - Install ca-certificates and curl if not installed.

```
sudo apt-get install ca-certificates curl
```

3 - Download GPG key for checking authenticity of docker packages.

```
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

4 - Set read permission for GPG key to all users.

```
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

5 - Add Docker repository entry, including system information and GPG key path for verifying packages.

```
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

6 - Update list of available packages, including packages from the newly added Docker repository.

```
sudo apt-get update
```

7 - Install Docker packages.

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```