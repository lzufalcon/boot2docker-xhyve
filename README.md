# Boot2Docker running on xhyve hypervisor

## Features

- boot2docker v17.07.0-ce
- Disable TLS
- Expose the official IANA registered Docker port 2375
- Support NFS synced folder: /Users is NFS-mounted on the boot2docker VM.

## Requirements

- [xhyve](https://github.com/mist64/xhyve)
  - Mac OS X Yosemite 10.10.3 or later
  - A 2010 or later Mac (i.e. a CPU that supports EPT)

## Caution

- **Kernel Panic** will occur on booting, if VirtualBox (< v5.0) has run before.
- Pay attention to **exposing the port 2375 without TLS**, as you see the features.

## Installing xhyve

```
$ git clone https://github.com/mist64/xhyve
$ cd xhyve
$ make
$ cp build/xhyve /usr/local/bin/    # You may require sudo
```

or

```
$ brew install xhyve
```

## Setting up Boot2Docker images and tools

```
$ git clone https://github.com/ailispaw/boot2docker-xhyve
$ cd boot2docker-xhyve
$ make
```

## Booting Up

```
$ sudo ./xhyverun.sh

Core Linux
boot2docker login: 
```

or

```
$ make run    # You may be asked for your sudo password
Booting up...
```

- On Terminal.app: This will open a new window, then you will see in the window as below.
- On iTerm.app: This will split the current window, then you will see in the bottom pane as below.

```
Core Linux
boot2docker login: 
```

## Logging In

- ID: docker
- Password: tcuser (in most instances you will not be prompted for a password)

```
$ make ssh
docker@192.168.64.3's password:
                        ##         .
                  ## ## ##        ==
               ## ## ## ## ##    ===
           /"""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~~ ~ /  ===- ~~~
           \______ o           __/
             \    \         __/
              \____\_______/
 _                 _   ____     _            _
| |__   ___   ___ | |_|___ \ __| | ___   ___| | _____ _ __
| '_ \ / _ \ / _ \| __| __) / _` |/ _ \ / __| |/ / _ \ '__|
| |_) | (_) | (_) | |_ / __/ (_| | (_) | (__|   <  __/ |
|_.__/ \___/ \___/ \__|_____\__,_|\___/ \___|_|\_\___|_|
Boot2Docker version 17.09.0-ce, build HEAD : 06d5c35 - Wed Sep 27 23:22:43 UTC 2017
Docker version 17.09.0-ce, build afdb6d4
docker@boot2docker:~$ 
```

## Shutting Down

Use `halt` command to shut down in the VM:

```
docker@boot2docker:~$ sudo halt
docker@boot2docker:~$ reboot: System halted
$ 
```

or, use `make halt` on the host:

```
$ make halt
docker@192.168.64.3's password:
Shutting down...
```

## Using Docker

You can simply run Docker within the VM. However, if you install the Docker client on the host, you can use Docker commands natively on the host Mac. Install the Docker client as follows:

```
$ curl -L https://get.docker.com/builds/Darwin/x86_64/docker-latest -o docker
$ chmod +x docker
$ mv docker /usr/local/bin/    # You may require sudo
```

Alternatively install with Homebrew:

```
$ brew install docker
```

Then, in the VM, or on the host if you have installed the Docker client:

```
$ make env
export DOCKER_HOST=tcp://192.168.64.3:2375;
unset DOCKER_CERT_PATH;
unset DOCKER_TLS_VERIFY;
$ eval $(make env)

$ docker info
Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 0
Server Version: 17.09.0-ce
Storage Driver: overlay2
 Backing Filesystem: extfs
 Supports d_type: true
 Native Overlay Diff: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins:
 Volume: local
 Network: bridge host macvlan null overlay
 Log: awslogs fluentd gcplogs gelf journald json-file logentries splunk syslog
Swarm: inactive
Runtimes: runc
Default Runtime: runc
Init Binary: docker-init
containerd version: 06b9cb35161009dcb7123345749fef02f7cea8e0
runc version: 3f2f8b84a77f73d38244dd690525642a72156c64
init version: 949e6fa
Security Options:
 seccomp
  Profile: default
Kernel Version: 4.4.89-boot2docker
Operating System: Boot2Docker 17.09.0-ce (TCL 7.2); HEAD : 06d5c35 - Wed Sep 27 23:22:43 UTC 2017
OSType: linux
Architecture: x86_64
CPUs: 1
Total Memory: 995.8MiB
Name: boot2docker
ID: LZLG:5XY3:GINF:64NA:HESB:QGK2:LY2H:EAST:RTLM:XKWI:BNWY:BAHI
Docker Root Dir: /mnt/vda1/var/lib/docker
Debug Mode (client): false
Debug Mode (server): true
 File Descriptors: 18
 Goroutines: 26
 System Time: 2017-09-28T03:24:00.129917597Z
 EventsListeners: 0
Registry: https://index.docker.io/v1/
Experimental: false
Insecure Registries:
 127.0.0.0/8
Live Restore Enabled: false
```

## Upgrading Boot2Docker

When Boot2Docker is upgraded and boot2docker-xhyve is updated,

```
$ git pull origin master
$ make upgrade
```

## Resources

- /var/db/dhcpd_leases
- /Library/Preferences/SystemConfiguration/com.apple.vmnet.plist
  - Shared_Net_Address
  - Shared_Net_Mask
