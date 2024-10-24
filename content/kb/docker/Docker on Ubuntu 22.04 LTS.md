sudo:
```bash
#!/bin/bash
apt-get update
apt-get install -yq git wget
# Install Docker

apt-get install -yq \
apt-transport-https \
ca-certificates \
curl \
software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key
add -
apt-key fingerprint 0EBFCD88
add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) \
stable"
install -m 0755 -d /etc/apt/keyrings
chmod a+r /etc/apt/keyrings/docker.gpg

apt-get update
apt-get install -yq docker-ce
curl -L "https://github.com/docker/compose/releases/download/`curl -fsSLI -o /dev/null -w %{url_effective} https://github.com/docker/compose/releases/latest | sed 's#.*tag/##g' && echo`/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

rootless:
```shell
sudo apt install -y dbus-user-session
sudo spt install -y uidmap
sudo apt install -y systemd-container
sudo machinectl shell jason@ /bin/sh
dockerd-rootless-setuptool.sh install
ctrl-] three times in 1 second
dockerd-rootless-setuptool.sh install
export DOCKER_HOST=unix:///run/user/1000/docker.sock
docker version
systemctl --user start docker

```




[INFO] IJun 15 20:28:34 cleansurface dockerd-rootless.sh[4861]: time="2024-06-15T20:28:34.145054246-05:00" level=info msg="containerd successfully booted in 0.087750s"                                                                                               Jun 15 20:28:35 cleansurface dockerd-rootless.sh[4836]: time="2024-06-15T20:28:35.181934663-05:00" level=info msg="Loading containers: start."                                                                                                                Jun 15 20:28:35 cleansurface dockerd-rootless.sh[4836]: time="2024-06-15T20:28:35.188621437-05:00" level=info msg="skipping firewalld management for rootless mode"                                                                                           Jun 15 20:28:35 cleansurface dockerd-rootless.sh[4836]: time="2024-06-15T20:28:35.454775851-05:00" level=info msg="Loading containers: done."                                                                                                                 Jun 15 20:28:35 cleansurface dockerd-rootless.sh[4836]: time="2024-06-15T20:28:35.478046681-05:00" level=warning msg="Not using native diff for overlay2, this may cause degraded performance for building images: running in a user namespace" storage-driver=overlay2                                                                                                                      Jun 15 20:28:35 cleansurface dockerd-rootless.sh[4836]: time="2024-06-15T20:28:35.478324256-05:00" level=warning msg="WARNING: Running in rootless-mode without cgroups. To enable cgroups in rootless-mode, you need to boot the system in cgroup v2 mode."  Jun 15 20:28:35 cleansurface dockerd-rootless.sh[4836]: time="2024-06-15T20:28:35.478386719-05:00" level=info msg="Docker daemon" commit=de5c9cf containerd-snapshotter=false storage-driver=overlay2 version=26.1.4                                          Jun 15 20:28:35 cleansurface dockerd-rootless.sh[4836]: time="2024-06-15T20:28:35.478856751-05:00" level=info msg="Daemon has completed initialization"                                                                                                       Jun 15 20:28:35 cleansurface systemd[4281]: Started Docker Application Container Engine (Rootless).                            Jun 15 20:28:35 cleansurface dockerd-rootless.sh[4836]: time="2024-06-15T20:28:35.682007093-05:00" level=info msg="API listen on /run/user/1000/docker.sock"                                                                                                  + DOCKER_HOST=unix:///run/user/1000/docker.sock /usr/bin/docker version                                                        Client: Docker Engine - Community                                                                                               Version:           26.1.3                                                                                                      API version:       1.45
 Go version:        go1.21.10
 Git commit:        b72abbb
 Built:             Thu May 16 08:33:29 2024
 OS/Arch:           linux/amd64
 Context:           defaultnstalled docker.service successfully.                                                                                  [INFO] To control docker.service, run: `systemctl --user (start|stop|restart) docker.service`                                  [INFO] To run docker.service on system startup, run: `sudo loginctl enable-linger jason`                                                                                                                                                                      [INFO] CLI context "rootless" already exists
[INFO] Using CLI context "rootless"
Current context is now "rootless"

[INFO] Make sure the following environment variable(s) are set (or add them to ~/.bashrc):
export PATH=/usr/bin:$PATH

[INFO] Some applications may require the following environment variable too:
export DOCKER_HOST=unix:///run/user/1000/docker.sock