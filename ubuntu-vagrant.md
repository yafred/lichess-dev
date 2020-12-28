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
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  1.1G   19G   6% /
4.0K    /home/vagrant/.cache
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
36K     /home/vagrant
```

## Prepare installations via apt-get
```
sudo apt-get update
sudo apt-get upgrade
```
```
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  1.2G   19G   7% /
4.0K    /home/vagrant/.cache
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
36K     /home/vagrant
```
 
## Install openjdk
```
sudo apt update
sudo apt install -y default-jdk
java -version
```
```
vagrant@bionic64:~$ java -version
openjdk version "11.0.9.1" 2020-11-04
OpenJDK Runtime Environment (build 11.0.9.1+1-Ubuntu-0ubuntu1.18.04)
OpenJDK 64-Bit Server VM (build 11.0.9.1+1-Ubuntu-0ubuntu1.18.04, mixed mode, sharing)
```
```
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  1.8G   18G   9% /
4.0K    /home/vagrant/.cache
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
36K     /home/vagrant
```

## Install monitoring tool
```
sudo apt-get update
sudo apt-get install -y glances
```
```
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  1.9G   18G  10% /
4.0K    /home/vagrant/.cache
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
36K     /home/vagrant
```

## Install Node.js using nvm (https://github.com/nvm-sh/nvm)
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh | bash
. ~/.profile
nvm install 14.15.1
node --version
```
```
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  2.1G   18G  11% /
135M    /home/vagrant/.nvm
4.0K    /home/vagrant/.cache
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
135M    /home/vagrant
```

## Install yarn
```
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update
sudo apt-get install -y yarn
```
```
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  2.1G   18G  11% /
135M    /home/vagrant/.nvm
4.0K    /home/vagrant/.cache
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
135M    /home/vagrant
```

## Install sbt
```
echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
curl -sL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x2EE0EA64E40A89B84B2DF73499E82A75642AC823" | sudo apt-key add
sudo apt-get update
sudo apt-get install -y sbt
```
```
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  2.1G   18G  11% /
135M    /home/vagrant/.nvm
4.0K    /home/vagrant/.cache
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
135M    /home/vagrant
```

## Install parallel
```
sudo apt-get update
sudo apt-get install -y parallel
parallel --citation
```
```
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  2.1G   18G  11% /
135M    /home/vagrant/.nvm
4.0K    /home/vagrant/.cache
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
135M    /home/vagrant
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
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  2.9G   17G  15% /
135M    /home/vagrant/.nvm
4.0K    /home/vagrant/.cache
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
135M    /home/vagrant
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
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  2.9G   17G  15% /
135M    /home/vagrant/.nvm
4.0K    /home/vagrant/.cache
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
135M    /home/vagrant
```

## Clone lila-ws
```
git clone https://github.com/ornicar/lila-ws.git
```
```
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  2.9G   17G  15% /
1.6M    /home/vagrant/lila-ws
135M    /home/vagrant/.nvm
4.0K    /home/vagrant/.cache
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
137M    /home/vagrant
```

## Clone lila
```
git clone --recursive https://github.com/ornicar/lila.git
```
```
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  3.2G   17G  17% /
1.6M    /home/vagrant/lila-ws
135M    /home/vagrant/.nvm
360M    /home/vagrant/lila
4.0K    /home/vagrant/.cache
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
497M    /home/vagrant
```

## Build and start lila-ws (includes getting org.scala-sbt sbt 1.4.6)
```
cd lila-ws
sbt run
```
```
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  3.5G   16G  19% /
58M     /home/vagrant/lila-ws
135M    /home/vagrant/.nvm
360M    /home/vagrant/lila
93M     /home/vagrant/.cache
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
57M     /home/vagrant/.ivy2
123M    /home/vagrant/.sbt
825M    /home/vagrant
```

## Build lila ui
```
cd lila
./ui/build
```
```
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  4.0G   16G  21% /
12K     /home/vagrant/.parallel
58M     /home/vagrant/lila-ws
135M    /home/vagrant/.nvm
596M    /home/vagrant/lila
8.0K    /home/vagrant/.yarn
318M    /home/vagrant/.cache
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
57M     /home/vagrant/.ivy2
123M    /home/vagrant/.sbt
1.3G    /home/vagrant
```

## Build and start lila
```
cd lila
./lila
run
```
```
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  4.4G   15G  23% /
12K     /home/vagrant/.parallel
58M     /home/vagrant/lila-ws
135M    /home/vagrant/.nvm
915M    /home/vagrant/lila
8.0K    /home/vagrant/.yarn
465M    /home/vagrant/.cache
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
57M     /home/vagrant/.ivy2
124M    /home/vagrant/.sbt
1.8G    /home/vagrant
```

## Remote access with vscode

### Stop lila and lila-ws

### Increase this parm first
```
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

### Connect to remote host
```
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  4.5G   15G  24% /
12K     /home/vagrant/.parallel
58M     /home/vagrant/lila-ws
135M    /home/vagrant/.nvm
916M    /home/vagrant/lila
8.0K    /home/vagrant/.yarn
465M    /home/vagrant/.cache
104M    /home/vagrant/.vscode-server
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
57M     /home/vagrant/.ivy2
124M    /home/vagrant/.sbt
1.9G    /home/vagrant
```

### Download metals (vscode) and ...
```
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  4.7G   15G  24% /
12K     /home/vagrant/.parallel
58M     /home/vagrant/lila-ws
135M    /home/vagrant/.nvm
916M    /home/vagrant/lila
8.0K    /home/vagrant/.yarn
552M    /home/vagrant/.cache
121M    /home/vagrant/.vscode-server
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
57M     /home/vagrant/.ivy2
124M    /home/vagrant/.sbt
2.0G    /home/vagrant
```

### ... import build (new sbt workspace detected) ... be patient 
```
vagrant@lichess:~$ df -h /dev/sda1; du -h --max-depth 1 /home/vagrant
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G  5.0G   15G  26% /
12K     /home/vagrant/.parallel
8.0K    /home/vagrant/.config
58M     /home/vagrant/lila-ws
135M    /home/vagrant/.nvm
997M    /home/vagrant/lila
8.0K    /home/vagrant/.yarn
16K     /home/vagrant/.local
826M    /home/vagrant/.cache
121M    /home/vagrant/.vscode-server
8.0K    /home/vagrant/.ssh
8.0K    /home/vagrant/.gnupg
57M     /home/vagrant/.ivy2
124M    /home/vagrant/.sbt
2.3G    /home/vagrant
```

## Useful .gitignore
```
.bloop
.bsp
.vscode
.metals
```

