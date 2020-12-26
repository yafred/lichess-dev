## Create a ubuntu 18.04 virtual machine
```
Vagrant.configure("2") do |config|
	config.vm.network "forwarded_port", guest: 9663, host: 9663
	config.vm.network "forwarded_port", guest: 9664, host: 9664
	config.vm.network "forwarded_port", guest: 27017, host: 27017
	config.vm.define "bionic64" do |bionic64|
		bionic64.vm.box = "ubuntu/bionic64"
		bionic64.vm.hostname = "bionic64"
		bionic64.disksize.size = "20GB"
		bionic64.vm.provider "virtualbox" do |v|
			v.memory = 12288
			v.cpus = 4
			v.name = "bionic64"
		end
	end
end
```
 ## Useful resources to monitor server
   * https://www.tecmint.com/check-linux-disk-usage-of-files-and-directories/
   * https://www.tecmint.com/how-to-check-disk-space-in-linux/
   * https://linoxide.com/monitoring-2/10-tools-monitor-cpu-performance-usage-linux-command-line/

## Initial space
```
vagrant@bionic64:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            5.9G     0  5.9G   0% /dev
tmpfs           1.2G  604K  1.2G   1% /run
/dev/sda1        20G  1.1G   19G   6% /
tmpfs           5.9G     0  5.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           5.9G     0  5.9G   0% /sys/fs/cgroup
vagrant         472G  113G  359G  24% /vagrant
tmpfs           1.2G     0  1.2G   0% /run/user/1000
vagrant@bionic64:~$ pwd
/home/vagrant
vagrant@bionic64:~$ du -h
4.0K    ./.cache
8.0K    ./.ssh
4.0K    ./.gnupg/private-keys-v1.d
8.0K    ./.gnupg
40K     .
```

## Prepare installations via apt-get
```
sudo apt-get update
sudo apt-get upgrade
```
```
vagrant@bionic64:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            5.9G     0  5.9G   0% /dev
tmpfs           1.2G  612K  1.2G   1% /run
/dev/sda1        20G  1.2G   19G   7% /
tmpfs           5.9G     0  5.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           5.9G     0  5.9G   0% /sys/fs/cgroup
vagrant         472G  114G  359G  24% /vagrant
tmpfs           1.2G     0  1.2G   0% /run/user/1000
vagrant@bionic64:~$ pwd
/home/vagrant
vagrant@bionic64:~$ du -k
4       ./.cache
8       ./.ssh
4       ./.gnupg/private-keys-v1.d
8       ./.gnupg
40      .
```
 
## Install openjdk
```
sudo apt update
sudo apt install default-jdk
java -version
```
```
vagrant@bionic64:~$ java -version
openjdk version "11.0.9.1" 2020-11-04
OpenJDK Runtime Environment (build 11.0.9.1+1-Ubuntu-0ubuntu1.18.04)
OpenJDK 64-Bit Server VM (build 11.0.9.1+1-Ubuntu-0ubuntu1.18.04, mixed mode, sharing)
```
```
vagrant@bionic64:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            5.9G     0  5.9G   0% /dev
tmpfs           1.2G  612K  1.2G   1% /run
/dev/sda1        20G  1.8G   18G   9% /
tmpfs           5.9G     0  5.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           5.9G     0  5.9G   0% /sys/fs/cgroup
vagrant         472G  115G  358G  25% /vagrant
tmpfs           1.2G     0  1.2G   0% /run/user/1000
vagrant@bionic64:~$ pwd
/home/vagrant
vagrant@bionic64:~$ du -h
4.0K    ./.cache
8.0K    ./.ssh
4.0K    ./.gnupg/private-keys-v1.d
8.0K    ./.gnupg
40K     .
```

## Install monitoring tool
```
sudo apt-get install glances
```
```
vagrant@bionic64:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            5.9G     0  5.9G   0% /dev
tmpfs           1.2G  620K  1.2G   1% /run
/dev/sda1        20G  1.9G   18G  10% /
tmpfs           5.9G     0  5.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           5.9G     0  5.9G   0% /sys/fs/cgroup
vagrant         472G  115G  358G  25% /vagrant
tmpfs           1.2G     0  1.2G   0% /run/user/1000
vagrant@bionic64:~$ pwd
/home/vagrant
vagrant@bionic64:~$ du -h
4.0K    ./.cache
8.0K    ./.ssh
4.0K    ./.gnupg/private-keys-v1.d
8.0K    ./.gnupg
40K     .
vagrant@bionic64:~$
```

## Install Node.js using nvm (https://github.com/nvm-sh/nvm)
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh | bash
. ~/.profile
nvm install 14.15.1
node --version
```
```
vagrant@bionic64:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            5.9G     0  5.9G   0% /dev
tmpfs           1.2G  620K  1.2G   1% /run
/dev/sda1        20G  2.1G   18G  11% /
tmpfs           5.9G     0  5.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           5.9G     0  5.9G   0% /sys/fs/cgroup
vagrant         472G  115G  357G  25% /vagrant
tmpfs           1.2G     0  1.2G   0% /run/user/1000
vagrant@bionic64:~$ pwd
/home/vagrant
vagrant@bionic64:~$ du -h --max-depth=1
135M    ./.nvm
4.0K    ./.cache
8.0K    ./.ssh
8.0K    ./.gnupg
135M    .
```

## Install yarn
```
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update
sudo apt-get install -y yarn
```
```
vagrant@bionic64:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            5.9G     0  5.9G   0% /dev
tmpfs           1.2G  620K  1.2G   1% /run
/dev/sda1        20G  2.1G   18G  11% /
tmpfs           5.9G     0  5.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           5.9G     0  5.9G   0% /sys/fs/cgroup
vagrant         472G  115G  357G  25% /vagrant
tmpfs           1.2G     0  1.2G   0% /run/user/1000
vagrant@bionic64:~$ pwd
/home/vagrant
vagrant@bionic64:~$ du -h --max-depth=1
135M    ./.nvm
4.0K    ./.cache
8.0K    ./.ssh
8.0K    ./.gnupg
135M    .
```

## Install sbt
```
echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
curl -sL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x2EE0EA64E40A89B84B2DF73499E82A75642AC823" | sudo apt-key add
sudo apt-get update
sudo apt-get install -y sbt
```
```
vagrant@bionic64:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            5.9G     0  5.9G   0% /dev
tmpfs           1.2G  620K  1.2G   1% /run
/dev/sda1        20G  2.1G   18G  11% /
tmpfs           5.9G     0  5.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           5.9G     0  5.9G   0% /sys/fs/cgroup
vagrant         472G  115G  357G  25% /vagrant
tmpfs           1.2G     0  1.2G   0% /run/user/1000
vagrant@bionic64:~$ pwd
/home/vagrant
vagrant@bionic64:~$ du -h --max-depth=1
135M    ./.nvm
4.0K    ./.cache
8.0K    ./.ssh
8.0K    ./.gnupg
135M    .
```

## Install parallel
```
sudo apt-get update -y
sudo apt-get install -y parallel
parallel --citation
```
```
vagrant@bionic64:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            5.9G     0  5.9G   0% /dev
tmpfs           1.2G  620K  1.2G   1% /run
/dev/sda1        20G  2.1G   18G  11% /
tmpfs           5.9G     0  5.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           5.9G     0  5.9G   0% /sys/fs/cgroup
vagrant         472G  115G  357G  25% /vagrant
tmpfs           1.2G     0  1.2G   0% /run/user/1000
vagrant@bionic64:~$ pwd
/home/vagrant
vagrant@bionic64:~$ du -h --max-depth=1
135M    ./.nvm
4.0K    ./.cache
8.0K    ./.ssh
8.0K    ./.gnupg
135M    .
```

## Install MongoDB
```
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
sudo apt-get update
sudo apt-get install -y mongodb-org
sudo systemctl start mongod
```
```
vagrant@bionic64:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            5.9G     0  5.9G   0% /dev
tmpfs           1.2G  620K  1.2G   1% /run
/dev/sda1        20G  2.6G   17G  14% /
tmpfs           5.9G     0  5.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           5.9G     0  5.9G   0% /sys/fs/cgroup
vagrant         472G  116G  357G  25% /vagrant
tmpfs           1.2G     0  1.2G   0% /run/user/1000
vagrant@bionic64:~$ pwd
/home/vagrant
vagrant@bionic64:~$ du -h --max-depth=1
135M    ./.nvm
4.0K    ./.cache
8.0K    ./.ssh
8.0K    ./.gnupg
135M    .
```

## Allow remote connections (to use Mongo Compass)
```
sudo vi /etc/mongod.conf
# network interfaces
net:
    port: 27017
    bindIp: 0.0.0.0   #default value is 127.0.0.1
```

## Install redis
```
sudo apt-get update
sudo apt-get install -y redis-server
```
```
vagrant@bionic64:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            5.9G     0  5.9G   0% /dev
tmpfs           1.2G  624K  1.2G   1% /run
/dev/sda1        20G  2.6G   17G  14% /
tmpfs           5.9G     0  5.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           5.9G     0  5.9G   0% /sys/fs/cgroup
vagrant         472G  116G  357G  25% /vagrant
tmpfs           1.2G     0  1.2G   0% /run/user/1000
vagrant@bionic64:~$ pwd
/home/vagrant
vagrant@bionic64:~$ du -h --max-depth=1
135M    ./.nvm
4.0K    ./.cache
8.0K    ./.ssh
8.0K    ./.gnupg
135M    .
```

## Clone lila-ws
```
git clone https://github.com/ornicar/lila-ws.git
```
```
vagrant@bionic64:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            5.9G     0  5.9G   0% /dev
tmpfs           1.2G  624K  1.2G   1% /run
/dev/sda1        20G  2.6G   17G  14% /
tmpfs           5.9G     0  5.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           5.9G     0  5.9G   0% /sys/fs/cgroup
vagrant         472G  116G  357G  25% /vagrant
tmpfs           1.2G     0  1.2G   0% /run/user/1000
vagrant@bionic64:~$ pwd
/home/vagrant
vagrant@bionic64:~$ du -h --max-depth=1
1.6M    ./lila-ws
135M    ./.nvm
4.0K    ./.cache
8.0K    ./.ssh
8.0K    ./.gnupg
137M    .
```

## Clone lila
```
git clone --recursive https://github.com/ornicar/lila.git
```
```
vagrant@bionic64:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            5.9G     0  5.9G   0% /dev
tmpfs           1.2G  624K  1.2G   1% /run
/dev/sda1        20G  2.9G   17G  15% /
tmpfs           5.9G     0  5.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           5.9G     0  5.9G   0% /sys/fs/cgroup
vagrant         472G  116G  356G  25% /vagrant
tmpfs           1.2G     0  1.2G   0% /run/user/1000
vagrant@bionic64:~$ pwd
/home/vagrant
vagrant@bionic64:~$ du -h --max-depth=1
1.6M    ./lila-ws
135M    ./.nvm
360M    ./lila
4.0K    ./.cache
8.0K    ./.ssh
8.0K    ./.gnupg
497M    .
```

## Build and start lila-ws (includes getting org.scala-sbt sbt 1.4.6)
```
cd lila-ws
sbt run
```
```
vagrant@bionic64:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            5.9G     0  5.9G   0% /dev
tmpfs           1.2G  640K  1.2G   1% /run
/dev/sda1        20G  3.2G   17G  17% /
tmpfs           5.9G     0  5.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           5.9G     0  5.9G   0% /sys/fs/cgroup
vagrant         472G  117G  356G  25% /vagrant
tmpfs           1.2G     0  1.2G   0% /run/user/1000
vagrant@bionic64:~$ pwd
/home/vagrant
vagrant@bionic64:~$ du -h --max-depth 1
58M     ./lila-ws
135M    ./.nvm
360M    ./lila
93M     ./.cache
8.0K    ./.ssh
8.0K    ./.gnupg
57M     ./.ivy2
123M    ./.sbt
825M    .
```

## Build lila ui
```
cd lila
./ui/build
```
```
vagrant@bionic64:~/lila$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            5.9G     0  5.9G   0% /dev
tmpfs           1.2G  640K  1.2G   1% /run
/dev/sda1        20G  3.7G   16G  19% /
tmpfs           5.9G     0  5.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           5.9G     0  5.9G   0% /sys/fs/cgroup
vagrant         472G  117G  355G  25% /vagrant
tmpfs           1.2G     0  1.2G   0% /run/user/1000
vagrant@bionic64:~$ pwd
/home/vagrant
vagrant@bionic64:~$ du -h --max-depth 1
12K     ./.parallel
58M     ./lila-ws
135M    ./.nvm
598M    ./lila
8.0K    ./.yarn
340M    ./.cache
8.0K    ./.ssh
8.0K    ./.gnupg
57M     ./.ivy2
123M    ./.sbt
1.3G    .
```

## Build and start lila
```
cd lila
./lila
run
```
```
vagrant@bionic64:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            5.9G     0  5.9G   0% /dev
tmpfs           1.2G  644K  1.2G   1% /run
/dev/sda1        20G  4.4G   15G  23% /
tmpfs           5.9G     0  5.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           5.9G     0  5.9G   0% /sys/fs/cgroup
vagrant         472G  117G  355G  25% /vagrant
tmpfs           1.2G     0  1.2G   0% /run/user/1000
vagrant@bionic64:~$ pwd
/home/vagrant
vagrant@bionic64:~$ du -h --max-depth 1
12K     ./.parallel
58M     ./lila-ws
135M    ./.nvm
913M    ./lila
8.0K    ./.yarn
462M    ./.cache
8.0K    ./.ssh
8.0K    ./.gnupg
57M     ./.ivy2
124M    ./.sbt
1.8G    .
```

## Remote access with vscode
```
vagrant@bionic64:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            5.9G     0  5.9G   0% /dev
tmpfs           1.2G  652K  1.2G   1% /run
/dev/sda1        20G  4.5G   15G  24% /
tmpfs           5.9G     0  5.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           5.9G     0  5.9G   0% /sys/fs/cgroup
vagrant         472G  117G  355G  25% /vagrant
tmpfs           1.2G     0  1.2G   0% /run/user/1000
vagrant@bionic64:~$ pwd
/home/vagrant
vagrant@bionic64:~$ du -h --max-depth 1
12K     ./.parallel
58M     ./lila-ws
135M    ./.nvm
913M    ./lila
8.0K    ./.yarn
462M    ./.cache
104M    ./.vscode-server
8.0K    ./.ssh
8.0K    ./.gnupg
57M     ./.ivy2
124M    ./.sbt
1.9G    .
```

## Open lila folder with vscode

### Increase this parm first
```
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```
### Download metals (vscode) and import build (new sbt workspace detected) ... be patient
```
vagrant@bionic64:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            5.9G     0  5.9G   0% /dev
tmpfs           1.2G  640K  1.2G   1% /run
/dev/sda1        20G  5.3G   15G  27% /
tmpfs           5.9G     0  5.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           5.9G     0  5.9G   0% /sys/fs/cgroup
vagrant         472G  118G  354G  25% /vagrant
tmpfs           1.2G     0  1.2G   0% /run/user/1000
vagrant@bionic64:~$ pwd
/home/vagrant
vagrant@bionic64:~$ du -h --max-depth 1
12K     ./.parallel
8.0K    ./.config
58M     ./lila-ws
135M    ./.nvm
1.3G    ./lila
8.0K    ./.yarn
16K     ./.local
837M    ./.cache
121M    ./.vscode-server
8.0K    ./.ssh
8.0K    ./.gnupg
57M     ./.ivy2
124M    ./.sbt
2.6G    .
```