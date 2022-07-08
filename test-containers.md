## Install docker

in case you haven't docker installed, please follow this instruction https://docs.docker.com/engine/install/ubuntu/

## Expose docker port

Create `daemon.json` file in `/etc/docker`, with content:

`{"hosts": ["tcp://127.0.0.1:2375", "unix:///var/run/docker.sock"]}`

## Restart docker daemon
`sudo systemctl daemon-reload` and then `sudo systemctl restart docker`
 or
`sudo service docker restart`

or just restart WSL2 by calling `wsl --shutdown`  in windows CMD/PowerShell and just open linux terminal once again

## Check that port are exposed
after restart docker daemon or restart wsl, run in linux terminal `netstat -nl | grep 2375` (install `net-tools` if you haven't it). You should see that port are open.

## Add environment variables

add the following properties in Windows Env Variables:

|Name|Value|
--|--
DOCKER_HOST|tcp://localhost:2375
DOCKER_TLS_VERIFY|0
DOCKER_CERT_PATH|\\\\wsl$\\home\\$USER_NAME\\.docker

:warning: seems like there is kind of an issue in WSL, some times `tcp//localhost:2375` should be replaced with `tcp://$wsl_ip:2375`, where `$wsl_ip` is IP of `ifconfig eth0` :warning:

:warning: port in `DOCKER_HOST` variable must be same as exposed above

:warning: `$USER_NAME` -replace this placeholder with your linux user

:warning: `.testcontainers.properties` - were empty

## Restart IDE
To apply environment variables you need restart IDE (in my case is Intellij Idea) OR just specify in `Run/Debug Configuration` these env variables directly.

## Check if port is exposed
Run on `Ubuntu` command `netstat -nl | grep 2375` -> it should return
![image](https://user-images.githubusercontent.com/27007646/177995535-7588b779-9bdf-41b9-9594-447217457d51.png)

